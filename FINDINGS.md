# Findings — the Original Bitcoin Laboratory

A one‑page synthesis of what the lab found, and how. The lab is an **evidence‑first,
executable** reconstruction and **neutral conformance study** of the earliest Bitcoin: the
**15 Nov 2008 pre‑release** and the **Jan 2009 v0.1.0** genesis client. Everything below is
grounded in two **hash‑verified archives + the whitepaper** — the sole authority
([`AUTHORITY.md`](AUTHORITY.md)) — and re‑derived here, independent of any outside account.

**How to check any of it:** `python genesis/scripts/reproduce.py` — **18 steps, 265 tests**,
all green; it re‑runs every suite and regenerates the derived matrices from source. Evidence
is labelled on a ladder: *declared → implemented → reachable → consensus‑relevant → executed
→ mined → wallet‑exposed*. The strongest rung, **JAN09‑EXECUTED**, is the original
`bitcoin.exe` run on a Windows host reconstructing the exact genesis
`000000000019d668…` — everything else is **MODEL** (Python port) or **PORT** (C++/OpenSSL),
always marked.

## 1. The central finding — v0.1 was already a general financial‑predicate engine

The genesis client is not a stripped payment token that "grew" programmability later. At
inception it shipped: the **full Script vocabulary** (106 opcodes, 94 implemented; **only
`OP_NOTEQUAL` disabled**), a **UTXO state machine**, **all SIGHASH modes**, and even an
**off‑chain marketplace** (`market.cpp`: signed products/reviews, a web‑of‑trust reputation
system). The programmability critics say Bitcoin "never had" was **native at the origin**
(executed: [`genesis/derivatives/model/`](../genesis/derivatives/model/),
[`ledger/`](../genesis/derivatives/ledger/), [`market/`](../genesis/derivatives/market/)).

## 2. The through‑line — a working engine with the guardrails not yet installed

The single most unifying result: the origin had the full consensus **machinery** but almost
none of the **bounds**. Bitcoin's early maturation (mostly **2010**) is the story of adding
them — and it has **four faces**, all documented, the last three **executed** as accept/reject
divergences on the same engine:

| Face | Origin | Later | Executed proof |
|---|---|---|---|
| **Vocabulary** | full opcode set, only `OP_NOTEQUAL` off | broad set disabled 2010 | the neutral matrix ([`conformance/MATRIX.md`](conformance/MATRIX.md)) |
| **Arithmetic** | unbounded OpenSSL `BIGNUM` | bounded `CScriptNum` | MODEL == C++/OpenSSL PORT, 63 vectors |
| **Values** | no `MoneyRange` (only `nValue<0`) | overflow check, Aug 2010 | [`overflow/`](../genesis/derivatives/overflow/) — v0.1 accepts the block‑74638 tx; hardened rejects |
| **Resources** | no element/op/stack ceilings | 520 B / 201 / 1000, 2010 | [`script_limits/`](../genesis/derivatives/script_limits/) — v0.1 validates a 600 B element / 250 ops / 1500 stack; hardened rejects |

The **present machinery** v0.1 *did* ship is executed too: difficulty retarget, merkle,
subsidy and coinbase maturity (PORT + ledger); reorg (chain‑sync/persist); and the temporal
rules — median‑time‑past, the two block‑timestamp checks, and transaction finality —
in [`temporal/`](../genesis/derivatives/temporal/), which surfaces that v0.1's `nLockTime` is
**height‑only** (no time/height `LOCKTIME_THRESHOLD`; that split is a later refinement). The
full map: [`conformance/CONSENSUS_SURFACE.md`](conformance/CONSENSUS_SURFACE.md).

## 3. The monetary constitution was set in January, not November

Re‑derived from the lab's own archives: the parameters that define Bitcoin‑the‑money changed
between the pre‑release and genesis — **COIN 1e6 → 1e8** (the "satoshi" is **genesis‑born**),
**subsidy 100 → 50**, **halving 100k → 210k blocks**, **spacing 15 → 10 min**, and the
coinbase value rule **`==` → `≤`**. November was cent‑denominated with a 100‑coin reward; the
constitution we call Bitcoin is a January decision.

## 4. The attack‑surface layer — where risk has lived

Three source‑grounded maps ([`conformance/`](conformance/)):

- **Dependencies** ([`DEPENDENCY_MATRIX.md`](conformance/DEPENDENCY_MATRIX.md)) — two
  general‑purpose libraries sat **inside consensus** in the origin, and **each caused a real
  chain split**: **OpenSSL** (the July 2015 BIP66 fork) and **Berkeley DB** (the March 2013
  lock‑limit fork that triggered the move to LevelDB). Every descendant removed both from
  consensus (libsecp256k1, LevelDB, bounded `CScriptNum`, strict‑DER).
- **Quantum** ([`QUANTUM_EXPOSURE.md`](conformance/QUANTUM_EXPOSURE.md)) — ECDSA‑on‑secp256k1
  is not post‑quantum (**Shor** breaks signatures; **Grover** only dents the hashes, so PoW
  and the address hash survive). Because a public key must be *visible* to attack, v0.1's
  **bare‑P2PK** coinbases — pubkeys in the clear on‑chain forever — are the most exposed coin
  class, a property of the origin's own design.
- **Consensus bounds** — §2 above.

## 5. Neutrality is the method, not a footnote

The descendant matrix treats **v0.1 as the sole executed baseline** and **every** descendant
identically — BTC/LTC/DOGE (Bitcoin Core's engine), BCH/XEC, BSV — none privileged. Cells are
executed where a real interpreter exists (**BTC** via `python-bitcoinlib`, **BSV** via
`bitcoinx`, which is backed by real **libsecp256k1**) and **execution‑bounded** otherwise,
stated plainly. The disabling of opcodes was mostly legitimate **defensive** security in
immature 2009–10 code; the lab's claim is not that anything was sinister — it is that the
**origin's capability was real and is now recoverable and checkable**. Neutrality is the moat.

## 6. The counterfactuals — "nothing disabled," realised

[`NOV08‑X`](../genesis/derivatives/nov08x/) and [`JAN09‑X`](../genesis/derivatives/jan09x/)
carry the **complete original vocabulary live**, each under its own constitution (November's
design vs January's release), as **two isolated networks that synchronise end‑to‑end**. They
are explicitly **not** "true Bitcoin": distinct genesis/magic/ports, **units are not
satoshis**, **no inherited balances**. They answer the question the matrix raises — *what does
Bitcoin with nothing disabled actually run like* — executably.

## 7. What this is **not**

Honest boundaries, held throughout: the executed layers are **MODEL/PORT** except the one
JAN09‑EXECUTED genesis witness; this is **not** recovered history or "the only full Bitcoin in
the world" (the v0.1.0 archive itself is the original, and BSV runs a nearly‑full vocabulary
live); **no value‑bearing mainnet** runs on MODEL code; and nothing here asserts a **live
exploit** — the surfaces mapped were mostly fixed years ago. The defensible public claim is in
[`CLAIMS.md`](CLAIMS.md); permanence is the **reproducible recipe + public evidence**, not a
coin.

---

*Reproduce:* `python genesis/scripts/reproduce.py`. *Authority:* the two hash‑verified
archives + whitepaper ([`AUTHORITY.md`](AUTHORITY.md)). *License:* MIT © 2026 Parth Mauria
Saxena for the lab tooling; the historical Bitcoin sources keep Satoshi's 2009 MIT notice.
