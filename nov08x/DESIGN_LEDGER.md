# NOV08‑X — R8 experimental‑network design + provenance ledger

Roadmap **R8** ("Modern Safe Experimental Nodes"), NOV08 branch. This is the design
contract for **NOV08‑X**: a runnable, provenance‑controlled electronic‑cash network
whose *constitution* is derived from the surviving **November 2008 pre‑release**
source, executed on the minimum reconstructed machinery.

It is a **cross‑edition design artifact** (NOV08 semantics on a JAN09‑reconstructed
substrate), so it lives in the umbrella `common/` repo. It cites only the two
hash‑verified archives and our own `conformance/NOV08_JAN09_DIFF.md`; authority is
governed by [`../AUTHORITY.md`](../AUTHORITY.md). Nothing outside the authority set
is ever cited as origin.

---

## 0. What NOV08‑X is — and is not

**Is:** a *counterfactual* network that executes November's monetary + ledger
constitution on the smallest reconstruction needed to run. A software‑archaeology
experiment answering: *what complete electronic‑cash system is latent in the last
surviving pre‑genesis source state, if it is independently completed instead of
inheriting January wholesale?*

**Is not:** recovered history; "the hidden true Bitcoin"; a continuation of the real
chain. NOV08‑X mints a **new genesis**, uses **new network magic / ports / address
version**, has **no inherited BTC/BCH/BSV balances**, and its units are **not**
historical bitcoins or satoshis. Even where it preserves an earlier design more
closely, it is a newly instantiated descendant — treated as neutrally as any other
candidate in the descendant matrix.

---

## 1. What actually survives in NOV08 (the authoritative constitution)

Per our verified manifest, the NOV08 archive is **5 files / 5,005 lines**:
`main.cpp`, `main.h`, `node.cpp`, `node.h`, `readme.txt`. It is an **early witness of
the ledger + networking main loop only**. Absent (all first defined in JAN09):
`script.*` (opcodetype/CScript/EvalScript/SIGHASH), `key.*` (EC keys/ECDSA),
`db.*` (Berkeley‑DB storage + `CTxIndex`/`CDiskBlockIndex`), `bignum.h`, `base58.h`,
`sha.*`, `serialize.h`, `irc.*` (discovery), `ui.*`, `market.*`.

Consequence, stated honestly: NOV08‑X's **distinctive DNA is narrow** — the
`main.*`/`node.*` ledger‑and‑net loop plus the monetary constitution (§3). The rest
is reconstructed substrate (§4). We do not pretend otherwise.

---

## 2. Provenance classes (the ledger's vocabulary)

Every line of the NOV08‑X build carries exactly one class. This is the mechanism
that keeps "November wins where November specifies" auditable and stops the project
silently becoming January Bitcoin.

| Class | Meaning | Authority |
|---|---|---|
| **N‑ORIG** | Behaviour specified in the surviving NOV08 source (cite `main/node:line`). **November wins here, unconditionally.** | Tier 0 (NOV08 archive) |
| **N‑IFACE** | Machinery NOV08 *references but does not define* (e.g. `CScript`, `OP_CHECKSIG`, keys). Reconstructed from the interface NOV08 relies on. | Tier 0 interface + Tier 4 reconstruction |
| **J‑DONOR** | NOV08 is silent and any faithful reconstruction would just re‑derive JAN09; imported from JAN09 semantics, labelled, never overriding an N‑ORIG rule. | Tier 0 (JAN09) as donor |
| **NEW‑EXP** | A deliberate new decision for the experimental network (genesis, magic, ports, unit name). **Never a semantics change** to N‑ORIG. | New, disclosed |
| **INSTR** | Observation‑only instrumentation (logging, evidence export). | New, non‑consensus |

**Override rule:** an N‑ORIG rule may never be silently overwritten by a J‑DONOR
import. Where January implemented the same thing differently, November's value stands
and the difference is recorded.

---

## 3. Rule A — "November wins": the N‑ORIG constitution

Read from NOV08's extracted, hash‑verified tree (anchors from
`conformance/NOV08_JAN09_DIFF.md`). These are the settings NOV08‑X **must** run; each
differs from our JAN09 headless stack (`genesis/derivatives`).

| # | Rule | NOV08‑X value (N‑ORIG) | source | JAN09 stack default |
|---|---|---|---|---|
| A1 | base unit `COIN` | **1,000,000** (1e6) — *no "satoshi"* | `main.h:34` | 100,000,000 |
| A2 | `CENT` | **10,000** | `main.h:35` | 1,000,000 |
| A3 | block subsidy | **100 coins** (`10000 * CENT`) | `main.cpp:654` | 50 coins |
| A4 | halving interval | **every 100,000 blocks** | `main.cpp:655` | 210,000 |
| A5 | target spacing | **15 min** (`15*60`) | `main.cpp:663` | 10 min |
| A6 | retarget timespan | **30 days** (≈2,880 blocks) | `main.cpp:662` | 2 weeks (2,016) |
| A7 | transaction fee | **fixed `1 * CENT`** (`TRANSACTIONFEE`) | `main.h:36` | dynamic (inputs − outputs) |
| A8 | coinbase value rule | must **equal** subsidy+fees (`!=` rejects) | `main.cpp:739` | must be ≤ (`>` rejects) |
| A9 | networking layer | `node.*` classes (`CMessageHeader`/`CAddress`/`CInv`/`CRequestTracker`/`CNode`) as they stood pre‑rename | `node.{h,cpp}` | `net.*` (renamed + expanded) |

Observable payoff: NOV08‑X issues a **100‑coin reward every ~15 minutes, halving at
100k blocks, denominated in millionths, with a fixed 0.01‑coin fee and an
exact‑equality coinbase rule** — a materially different monetary machine from JAN09,
executed rather than merely diffed.

> A9 note: `node.*` → `net.*` was a rename + expansion of the *same five classes*
> (not a redesign), so NOV08‑X maps to our `derivatives/p2p` wire, keeping the
> pre‑rename class identities and any behaviour the NOV08 `node.*` bytes specify;
> expansions unique to JAN09 `net.*` are J‑DONOR only where NOV08 is silent.

---

## 4. Rule B — reconstructed substrate (where November is silent)

NOV08 references these through its interface but does not define them. Reconstructed
from our **provenance‑clean** engines (the lab's MODEL/PORT), not by copy‑pasting
JAN09 files.

| Component | NOV08 status | NOV08‑X source | class |
|---|---|---|---|
| Script engine | *used* (`CScript`, `OP_CHECKSIG`, `OP_CODESEPARATOR`) but undefined | `genesis/derivatives/model` + `…/port` — **full original vocabulary** (§5) | N‑IFACE |
| Keys / ECDSA | implied by `<pubkey> OP_CHECKSIG` | real secp256k1 (our `tx_sighash`/PORT) | N‑IFACE |
| Sighash | implied by signing | pre‑BIP143 `SignatureHash` (our model, pinned) | N‑IFACE |
| Persistence | absent (no `db.*`, no `CDiskBlockIndex`) | our `derivatives/persist` (CDiskBlockIndex model) | J‑DONOR |
| Disk index `CTxIndex`/`CDiskBlockIndex` | JAN09‑added | our persist model | J‑DONOR |
| `bignum`/`base58`/`sha`/`serialize` | absent | reconstructed | N‑IFACE / J‑DONOR |
| Peer discovery | `irc.*` absent | our `derivatives/r3/mini_ircd` (isolated) | J‑DONOR |
| Chain sync / relay | in `node.*` main loop | our `derivatives/p2p` + `chainsync` | N‑ORIG where node.* specifies, else J‑DONOR |
| UTXO connect / validation | in `main.*` ledger core | our `derivatives/node/chain_port` | N‑ORIG (params A1–A8) on our connect logic |

---

## 5. The "nothing disabled" mandate

The lab's mission is to expose *everything the original code can do*. NOV08‑X's Script
engine (reconstructed under N‑IFACE) therefore carries the **complete original
vocabulary with nothing disabled** — the full splice/bitwise/arithmetic set
(`OP_CAT`, `OP_SUBSTR/LEFT/RIGHT`, `OP_INVERT`, `OP_AND/OR/XOR`, `OP_MUL/DIV/MOD`,
`OP_LSHIFT/RSHIFT`, `OP_2MUL/2DIV`) that BTC later removed and that our descendant
matrix shows only BSV largely restored. This is the concrete point of the whole
exercise: NOV08‑X (and, symmetrically, a **JAN09‑X** built the same way) run the
origin's full expressive power.

- Because NOV08 **predates** the Script file entirely, it never disabled any opcode —
  so including even `OP_NOTEQUAL` (which *JAN09* disabled) is defensible here. That
  choice is **NEW‑EXP**, disclosed: NOV08‑X enables the full vocabulary including
  `OP_NOTEQUAL`, on the ground that the disabling was a later January decision, not a
  November one. (A stricter variant that mirrors JAN09's single disable is available
  behind a flag for differential study.)

---

## 6. Network identity (all NEW‑EXP — new decisions, never semantics)

Minted fresh so NOV08‑X can never be confused with, or collide with, any historical
or live chain. Exact values fixed at genesis‑mint time; candidates below.

| Item | NOV08‑X (candidate) | must differ from |
|---|---|---|
| genesis block | freshly mined; new timestamp + **new** coinbase message (not the Times headline) | JAN09 genesis `000000000019d668…` |
| network magic | distinct 4 bytes (≠ `f9 be b4 d9`) | BTC/BCH/BSV magics |
| default P2P port | e.g. `18008` (≠ 8333) | 8333 |
| address version byte | distinct (≠ `0x00`, so not a `1…` address) | BTC/descendants |
| initial difficulty | low, CPU‑mineable (regtest‑style, e.g. `0x207fffff`) | mainnet difficulty |
| unit name | experimental (e.g. "novcoin‑lab unit"); **not** bitcoin/satoshi | — |
| initial balances | **none** | any inherited ledger |

---

## 7. Build approach (headless — no period‑toolchain compile)

We do **not** compile the incomplete 2008 source (that is R2's separate, toolchain‑
hard "reproducible build" goal, and it would force wholesale JAN09 imports). Instead
NOV08‑X is assembled **headlessly on the derivatives stack we already have**:

```
genesis/derivatives/{node/chain_port, model+port (script), wallet, p2p+chainsync, persist}
        + Rule A swap table (§3, N‑ORIG)      → November's constitution
        + Rule B substrate  (§4, N‑IFACE/J‑DONOR)
        + §6 network identity (NEW‑EXP)
        + §5 full vocabulary, nothing disabled
        = NOV08‑X node (headless, provenance‑labelled)
```

Every module the build touches emits its provenance class into a machine‑readable
`PROVENANCE.json` so the "November‑wins" invariant is testable, not just asserted.

*Placement decision (deferred to the build step):* the executable NOV08‑X profile
should live where it can reuse the JAN09 engines without breaking repo self‑
containment — most likely a `nov08x/` profile inside the `genesis` repo that imports
the reconstructed engines and applies this ledger, with this design doc as its spec.

---

## 8. Differential‑test plan (vs the JAN09 oracle)

NOV08‑X is validated by **difference**, against our headless JAN09 stack:

- **Issuance:** first N blocks pay 100 coins (A3), halve at 100k (A4) — vs JAN09's 50 / 210k.
- **Timing/retarget:** 15‑min spacing (A5), 30‑day window (A6) — vs 10‑min / 2‑week.
- **Denomination:** balances/amounts in 1e6 units (A1/A2) — vs 1e8.
- **Coinbase rule:** a coinbase paying *less* than subsidy+fees is **rejected** by
  NOV08‑X (A8 exact‑equality) but **accepted** by JAN09 — a crisp behavioural fork.
- **Fee:** fixed `1*CENT` (A7) vs dynamic.
- **Vocabulary:** the full opcode set executes in both (§5), confirming NOV08‑X
  disables nothing.
- **Isolation:** NOV08‑X genesis/magic/ports are provably distinct (§6).

Each result is captured as a deterministic evidence bundle (same discipline as
`r3-findings/`).

---

## 9. Build order (R8)

1. **[this doc]** design + provenance ledger. ✅
2. Freeze the exact NOV08 semantic set as a machine‑readable `rules_nov08.json` (A1–A9 with source anchors).
3. Build **NOV08‑Minimal**: the derivatives stack under the Rule A swap table + Rule B substrate, emitting `PROVENANCE.json`.
4. Differential‑test NOV08‑Minimal vs the JAN09 stack (§8); capture the bundle.
5. Mint the **NOV08‑X** genesis + identity (§6); bring up two isolated headless nodes.
6. Evidence bundle: issuance/timing/coinbase‑rule/vocabulary differences, live.
7. *(optional, walled‑off)* **NOV08‑Full** — completion toward the broader design.
   This is **interpretive**, must be labelled speculation, and must never be
   presented as recovered code. Considered only after Minimal is complete.

---

## 10. Guardrails (non‑negotiable)

- **November wins** where November specifies (§2 override rule); J‑DONOR never
  overwrites N‑ORIG.
- **No overclaim:** NOV08‑X is a new experimental descendant, not recovered history
  and not "true Bitcoin"; units are not historical satoshis.
- **Provenance or it didn't happen:** every added line carries a class; the invariant
  is machine‑checked via `PROVENANCE.json`.
- **NOV08‑Full is interpretation, not evidence** — deferred and clearly fenced.
- **Independence:** authority is only the hash‑verified archives + whitepaper
  (`../AUTHORITY.md`); no external lineage is cited as origin.
