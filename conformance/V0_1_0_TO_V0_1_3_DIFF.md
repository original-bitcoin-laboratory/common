# v0.1.0 → v0.1.3 diff — the first evolution of Bitcoin (SCAFFOLD)

**Status: PENDING ARCHIVE.** This is the ready‑to‑fill scaffold for a cross‑version
diff of Bitcoin **v0.1.0** (Jan 2009, our genesis baseline) against **v0.1.3 ALPHA**
(2009) — the third Satoshi codebase the Satoshi Nakamoto Institute hosts, and the *first
few point releases after the genesis release*. It answers one sharp question:

> **What did Satoshi change first?** — the earliest divergence from the origin, by the
> origin's own author.

## Framing (why this is a diff, not a third edition)

v0.1.3 is **out of authority** (`../AUTHORITY.md`): v0.1.0 is the behavioral oracle;
v0.1.3 is a *successor*, treated here as a **measured divergence point** — the same
neutral method the descendant matrix uses, applied to Satoshi's own next steps. It does
**not** become a co‑equal edition unless the diff surfaces something that earns one.

## How to fill it (once you have the archive)

1. **Fetch** `bitcoin-0.1.3` from the Nakamoto Institute code page
   (`satoshi.nakamotoinstitute.org/code/`).
2. **Hash‑verify** it against SNI's published MD5/SHA‑1 (and any SHA‑256). Record the
   values here — *pin them before reading a single line* (same discipline as
   `genesis/manifests/EXPECTED_CHECKSUMS.json`).

   | Artifact | md5 | sha1 | sha256 |
   |---|---|---|---|
   | `bitcoin-0.1.3.___` | _pending_ | _pending_ | _pending_ |

3. **Extract read‑only** next to the v0.1.0 tree (gitignored), then generate the
   inventories with the existing tooling and diff them:
   ```bash
   # from genesis/ , pointed at each extracted tree
   python scripts/inventory-symbols.py <v0.1.3 src>   # opcodes / SIGHASH / functions
   python scripts/inventory-tree.py    <v0.1.3 src>   # file/class map
   # then diff against the committed v0.1.0 inventories (inventory/OPCODES.json etc.)
   ```
4. **Drop me the two inventories** and I'll produce the tables below, source‑anchored.

## Findings tables (PENDING — placeholders)

### 1. File‑level presence
| Module | v0.1.0 | v0.1.3 | Change |
|---|:--:|:--:|---|
| _pending_ | | | |

### 2. Opcode / SIGHASH vocabulary
- v0.1.0 baseline: **106 opcodes, 94 implemented, only `OP_NOTEQUAL` disabled**
  (`genesis/inventory/OPCODES.md`).
- v0.1.3: _pending_ — did the disabled set change? new opcodes? (This is the headline
  question: is any of the broad‑vocabulary disabling already present by v0.1.3, or does
  it all come later in 2010?)

### 3. Monetary & timing constitution
| Parameter | v0.1.0 | v0.1.3 | anchor |
|---|---|---|---|
| subsidy / halving / spacing / retarget / coinbase rule | (as `NOV08_JAN09_DIFF.md`) | _pending_ | |

### 4. Consensus / networking / wallet deltas
- _pending_ — added classes, `main.*`/`net.*` changes, db/wallet changes, IRC discovery
  tweaks, difficulty handling.

## Reading (to be written)

_One paragraph, once filled: what the earliest post‑genesis evolution reveals — whether
Satoshi's first changes were bug‑fixes, hardening, or feature work, and whether any
vocabulary narrowing had begun by v0.1.3 or is entirely a later (2010) event._

> Method note: like every diff here, this compares only **hash‑verified archives**, treats
> gaps as *absence from the preserved artifact* (not proof of non‑existence), and cites the
> exact `src:line`. v0.1.3 is a measured successor, never presented as "the original."
