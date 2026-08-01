# Consensus surface — what the origin bounded vs left unbounded

The Script matrix maps the origin's **vocabulary** (which opcodes were enabled/disabled) and
the crypto cross‑check maps its **signatures**. This inventory maps the third foundational
layer: the **bounds** — the resource, value, and timing limits a consensus system needs so a
transaction or block can't overflow, bloat, or stall it. The finding, source‑verified in the
v0.1 code this repo holds: **the origin shipped the full consensus *machinery* but almost
none of the *bounds* — they were added later, in the 2010 hardening.**

Objective source inspection only — no external references, no chain privileged. "Absent"
means *not present in this repo's source*, i.e. attack surface / hardening‑timeline, **not** a
claim of a live exploit today (most were fixed years ago).

> This inventory maps the *bounds*. Its companion
> [`CONSENSUS_BEHAVIORS.md`](CONSENSUS_BEHAVIORS.md) maps the *behaviors* — the specific v0.1 quirks
> (CHECKMULTISIG off‑by‑one, `SIGHASH_SINGLE → 1`, merkle odd‑node duplication, the output‑sum
> overflow, the retarget fencepost) that a faithful reconstruction must reproduce bug‑for‑bug, each
> anchored to the source and to the OBL engine that executes it.

## The two halves

**A method note first:** *consensus* rules decide which blocks are valid (every node must
agree); *policy* rules are node‑local (relay, standardness, mempool — nodes may differ, e.g.
Cluster Mempool). This inventory is consensus/anti‑DoS bounds; the one policy row is marked.

### A. Present in v0.1 — the machinery *was* there (source‑verified)

| Rule / bound | Kind | v0.1.0 | Source |
|---|---|---|---|
| PoW difficulty retarget — 2‑week window, `nInterval = 2016`, clamp [¼×, 4×] | consensus | ✅ | `main.cpp:687‑711` |
| Coinbase maturity — `COINBASE_MATURITY = 100` | consensus | ✅ | `main.h:20` |
| Median‑time‑past timestamp rule (+ CheckBlock future‑2h) | consensus | ✅ **executed** | `main.h:1086`, `main.cpp:1164`,`1206` |
| Transaction finality — `IsFinal` / `nLockTime` (**height‑only**, no time threshold) | consensus | ✅ **executed** | `main.h:226`, `main.h:363` |
| Chain reorganization — `Reorganize` (disconnect/reconnect) | consensus | ✅ | `main.cpp` (11 refs) |
| Merkle‑root binding — `BuildMerkleTree` | consensus | ✅ | `main.cpp:1186` |
| Serialization size cap — `MAX_SIZE = 0x02000000` (32 MB) | anti‑DoS | ✅ | `main.h:17` |
| Reject **negative** output values (`nValue < 0`) | consensus | ✅ (partial — see B) | `main.h:442` |

The temporal rows are **executed** — median‑time‑past, the two block‑timestamp checks, and
transaction finality (with v0.1's **height‑only** `nLockTime`) run in
[`genesis/derivatives/temporal/`](https://github.com/original-bitcoin-laboratory/genesis/tree/main/derivatives/temporal/). Difficulty
retarget, merkle, subsidy and coinbase maturity are executed in the C++/OpenSSL PORT
(`genesis/derivatives/node/`) and the ledger; reorg in chain‑sync/persist.

### B. Absent in v0.1 — the bounds that came later (the sharp findings)

| Missing bound | Kind | v0.1.0 | Introduced later (hardening) |
|---|---|---|---|
| **Money range / overflow** — no `MAX_MONEY`, no `MoneyRange`, no output‑**sum** overflow check | consensus | ❌ (only `nValue < 0`) | 0.3.1, **Aug 2010** — the value‑overflow fix |
| **Block‑size cap** — no `MAX_BLOCK_SIZE` (1 MB); only the 32 MB serialization cap | consensus | ❌ | **2010** — the 1 MB limit |
| **Script element‑size limit** — no 520‑byte push cap (`MAX_SCRIPT_ELEMENT_SIZE`) | anti‑DoS | ❌ | 2010 script hardening |
| **Script op‑count limit** — no ~201‑op ceiling | anti‑DoS | ❌ | 2010 script hardening |
| **Stack‑size cap** — no 1000‑element ceiling (only *underflow* guards `if (stack.size() < N)`) | anti‑DoS | ❌ | 2010 script hardening |
| **Signature‑op count** — no `MAX_BLOCK_SIGOPS` per‑block limit | anti‑DoS | ❌ | 2010 |
| **Standardness** — no `IsStandard` | *policy* | ❌ | 2010 (node‑local policy, not consensus) |

The element/op/stack ceilings are **executed** — the lab's real v0.1 interpreter validates
scripts with a 600‑byte element, 250 ops, and a 1500‑deep stack, each of which the 2010 rule
rejects: [`genesis/derivatives/script_limits/`](https://github.com/original-bitcoin-laboratory/genesis/tree/main/derivatives/script_limits/).

## The sharpest one, in the origin's own words

v0.1's entire value sanity check is `CheckTransaction` ([`main.h:442`]):

```cpp
// Check for negative values
foreach(const CTxOut& txout, vout)
    if (txout.nValue < 0)
        return error(".. txout.nValue negative");
```

It rejects a **negative** output but never checks an **upper bound**, and never checks that
the **sum** of outputs doesn't overflow a signed 64‑bit integer. That is exactly the surface
of the **value‑overflow bug**: two enormous outputs can individually pass `nValue < 0` yet
sum past the wrap point. This surface existed continuously from the origin until it was
patched (Aug 2010) — the check that closes it, `MoneyRange`, has **0 occurrences** in the
v0.1 source. Same for the 1 MB block cap and every Script resource limit: **not yet written.**

This one is **executed** — a faithful port of this rule and the 2010 fix, run on the exact
block‑74638 amounts, showing v0.1 accept while the hardened rule rejects:
[`genesis/derivatives/overflow/`](https://github.com/original-bitcoin-laboratory/genesis/tree/main/derivatives/overflow/).

## The through‑line

The lab has now mapped three sides of the same maturation:

- **Vocabulary** — the broad opcode set, later disabled (Script matrix).
- **Arithmetic** — unbounded OpenSSL `BIGNUM`, later bounded `CScriptNum` (the OpenSSL essay).
- **Values & resources** — *this* inventory: money range, block size, and script limits, all
  **absent in the origin, all added in 2010**.

The origin was a working consensus engine with the guardrails **not yet installed**. That is a
neutral, source‑grounded statement about the code — not a verdict on any descendant, all of
which inherited these bounds once they existed.

## Scope & boundary

Source‑verified against this repo's hash‑verified **v0.1.0**; **v0.1.3** is consensus
byte‑identical (see `V0_1_0_TO_V0_1_3_DIFF.md`), so every row is identical there. **NOV08** is
a partial 5‑file snapshot (no `main.h` / `script.cpp`), so header‑ and script‑defined bounds
are simply *not in that snapshot* — absence there is not evidence about the pre‑release code.
"Later hardening" dates are from the public record; the *presence/absence* facts and line
numbers are from source. Cross‑ref: [`DEPENDENCY_MATRIX.md`](DEPENDENCY_MATRIX.md) (the library
layer) and the Script conformance matrix (the vocabulary layer).
