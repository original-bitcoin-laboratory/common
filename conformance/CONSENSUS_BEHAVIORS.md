# Era-authentic consensus behaviors — the executable is the spec, bugs included

[`CONSENSUS_SURFACE.md`](CONSENSUS_SURFACE.md) maps which consensus *bounds* the origin shipped vs left
for the 2010 hardening. This inventory maps a different thing: the **behaviors** — the specific ways
v0.1's *executed* code diverges from the tidy paraphrase of "what Bitcoin does," where the divergence is
itself the consensus rule. A faithful reconstruction must reproduce each of these **as written, bug and
all**; "correcting" any of them changes validity and forks the chain, so the corrected version is the
consensus-*invalid* one. This is the operational meaning of the lab's thesis that *the executable, not
the whitepaper or the intent, is the specification.*

Objective source inspection only — no external references, no persons, no chain privileged. Each row is
anchored to the v0.1 source this repo holds ([`extracted/bitcoin/src/`](https://github.com/original-bitcoin-laboratory/genesis/tree/main/extracted/bitcoin/src))
and to the OBL engine(s) that **execute** it, with a one-line machine-checkable statement. "Executed"
means a test in this project runs the behavior and asserts the result.

## The behaviors

| # | Behavior (the rule *is* the quirk) | Kind | v0.1 source | Executed in OBL |
|---|---|---|---|---|
| 1 | **CHECKMULTISIG off-by-one** — pops one item too many, so every multisig unlocking script must prepend a dummy `OP_0` | consensus | `script.cpp:727‑785` (extra pop `while (i-- > 0)` at `:783‑784`) | `validator-rs/src/eval.rs:567‑614`; `model/evalscript_model.py`; port `checksig_e2e.cpp` |
| 2 | **SIGHASH_SINGLE returns the constant `1`** — when the input index ≥ the number of outputs, the sighash is `1`, not the transaction | consensus | `script.cpp:818‑854` (`return 1` at `:823` and `:851‑854`) | `validator-rs/src/sighash.rs:38‑73`; `model/tx_sighash.py`; port `sighash.cpp` |
| 3 | **Merkle duplicates the last node on odd levels** — `[A,B,C]` and `[A,B,C,C]` share a root | consensus | `main.h:868‑882` (`i2 = min(i+1, nSize-1)` at `:878`) | `validator-rs/src/lib.rs:173‑177`; node `node_port.cpp`; `p2p/p2p.py:109` |
| 4 | **No output-sum / MoneyRange check** — `CheckTransaction` bounds only `nValue < 0` per output; a two-output int64 sum can wrap past the `inputs ≥ outputs` check | consensus | `main.h:442` (`nValue < 0` only) | `overflow/overflow.py` (v0.1 accepts the block-74638 tx; the 2010 rule rejects it) |
| 5 | **Retarget fencepost** — a 2016-block window's timespan is measured over only 2015 intervals, so real spacing settles at `2016/2015 × 600 = 600.30 s` (a hair **slow**, not fast) | consensus | `main.cpp:685‑728` (walk-back loop `i < nInterval-1` at `:701`) | `retarget/retarget.py` (fencepost fixed point + the timewarp boundary) |

Each engine is cross-checked against the others: the Rust validator's golden vectors are generated from
the Python model, and the C++/OpenSSL port is differential-tested against the model — so a behavior in
the table above holds identically across the Python, Rust, and C++ reconstructions, not in one alone.

## Per-behavior notes

**1 — CHECKMULTISIG off-by-one.** `OP_CHECKMULTISIG` computes `i = 3 + nKeys + nSigs` and then
`while (i-- > 0) stack.pop_back()` (`script.cpp:783‑784`), popping the sigs, the keys, the two count
items, **and one extra** — the item the design never accounted for. Every real multisig spend therefore
prepends a dummy `OP_0` to feed that pop. The lab reproduces the extra pop verbatim
(`eval.rs:613`, comment *"pop nsigs+nkeys+2 + the off-by-one"*) and its multisig vectors carry the dummy.

**2 — SIGHASH_SINGLE returns 1.** `SignatureHash` answers two error conditions — input index out of
range (`:823`) and, under `SIGHASH_SINGLE`, output index ≥ output count (`:851‑854`) — by returning the
256-bit value `1` instead of aborting. A signature made in that state commits to a constant and can be
replayed onto any other transaction that also triggers it. The lab returns the same sentinel
(`sighash.rs:38‑42 one()`, `:71‑73`).

**3 — Merkle odd-level duplication.** `BuildMerkleTree` pairs node `i` with node `min(i+1, nSize-1)`
(`main.h:878`); on an odd level the last node is paired **with itself**, so a distinct transaction list
that duplicates the final entry yields the same root (and the same block hash) though SHA-256 is sound.
The lab duplicates the final hash identically (`lib.rs:176`, *"duplicate the final hash (Satoshi's
merkle)"*).

**4 — No output-sum check.** `CheckTransaction` (`main.h:442`) rejects only negative outputs; it has no
`MoneyRange` and no check on the output **sum**. Two large-but-positive outputs can overflow a signed
64-bit accumulator to a small value that passes the downstream `inputs ≥ outputs` comparison — the
CVE-2010-5139 shape. `overflow/overflow.py` runs the exact block-74638 amounts through the v0.1 rule
(accept) and the Aug-2010 `MoneyRange` rule (reject) side by side.

**5 — Retarget fencepost.** `GetNextWorkRequired` walks `pindexFirst` back `nInterval-1 = 2015` blocks
(`main.cpp:701`) and divides the resulting 2015-interval timespan by a 2016-interval budget, so the
fixed point is `2015·τ = 2016·600 → τ = 600.30 s` — blocks run ~0.05% **slow**, permanently. (Direction
matters: the code under-measures elapsed time, not over-measures, so it is *slow*, not the sometimes-cited
"599.7 s, fast".) `retarget/retarget.py` derives the fixed point from the ported function and also
exhibits the boundary-only measurement that a timewarp would exploit.

## Scope & boundary

- **These are v0.1 behaviors, so a faithful reconstruction reproduces them, not their later fixes.**
  Adding MoneyRange, a strict CHECKMULTISIG, a BIP143 sighash, a merkle fix, or a retarget fix would be
  a hard fork — a *drift*, not a fidelity improvement (see
  [`../../genesis/docs/AUDIT_SCOPE.md`](https://github.com/original-bitcoin-laboratory/genesis/blob/main/docs/AUDIT_SCOPE.md)).
- **BIP30 duplicate-coinbase is deliberately *not* in the table.** v0.1 keys the UTXO set by outpoint
  `(txid, n)` and has **no** duplicate-coinbase guard, so a coinbase-txid collision would overwrite the
  earlier entry — a real latent behavior of the v0.1 code. But the *rule* that addresses it (BIP30, 2012)
  and its structural successor (BIP34) are **post-v0.1**, outside this repo's reconstruction scope; they
  are recorded here as an era-map fact, not modeled as a v0.1 behavior.
- **Latent boundaries baked into v0.1 (era-authentic, computed).** Two more "there all along" facts the
  origin's own types/schedule fix forever: (a) the block header's `nTime` is a **uint32**, so it
  overflows at `2³²` seconds after the epoch — **2106-02-07 06:28:16 UTC** (like the BIP34 fencepost, a
  future boundary, not a present flaw); and (b) the subsidy halving uses integer `>>=`, so the true
  issued cap is **20,999,999.9769 BTC** (< the folklore 21,000,000), before the ~100 BTC BIP30 loss and
  the unspendable genesis 50. Both follow by arithmetic from the v0.1 constants.
- **The merkle layer has a second ambiguity beyond odd-duplication.** A transaction serialized to
  exactly 64 bytes has a `txid` equal to an interior merkle node over its two 32-byte halves (leaves and
  nodes share one double-SHA-256) — a leaf/node ambiguity enabling forged SPV proofs, executed in
  [`../../genesis/derivatives/hash_structure/`](https://github.com/original-bitcoin-laboratory/genesis/tree/main/derivatives/hash_structure/)
  and written up in [`HASH_STRUCTURE.md`](HASH_STRUCTURE.md); the defence is to reject 64-byte txs.
- **Later consensus events are history, not reconstruction.** The 2013 Berkeley-DB lock fork, the 2015
  BIP66 SPV-mining fork, and CVE-2018-17144 (2018) post-date v0.1 and are out of scope; they appear in
  the lab only as dependency/timeline notes ([`DEPENDENCY_MATRIX.md`](DEPENDENCY_MATRIX.md)).
- "Executed" is a `MODEL`/port claim (Python/Rust/C++ reconstructions), not the original binary running;
  the executed-binary evidence is the separate `r3`/`r4` witness bundles. No chain is privileged and no
  identity is asserted; every row is a tool, never authority (`common/AUTHORITY.md`).
