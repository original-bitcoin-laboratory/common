# NOV08 → JAN09 structural diff (R1)

Cross-edition comparison of the two frozen profiles, from their extracted,
hash-verified source trees. This is umbrella (cross-edition) material; per-edition
detail lives in each repo's `inventory/SOURCE_INVENTORY.md`.

- **OBL-NOV08** — 4 source units + readme, **5,005 lines**.
- **OBL-JAN09** — 26 source units, **19,820 lines** (≈4×), plus a runnable
  `bitcoin.exe` and OpenSSL DLL.

## 1. File-level presence

| Module | NOV08 | JAN09 | Change |
|---|:--:|:--:|---|
| `main.{h,cpp}` | ✓ | ✓ | present in both (heavily revised) |
| networking | `node.{h,cpp}` | `net.{h,cpp}` | **renamed** `node` → `net` |
| `script.{h,cpp}` | — | ✓ | **added** (used but undefined in nov08) |
| `key.h` | — | ✓ | added (EC keys / ECDSA) |
| `db.{h,cpp}` | — | ✓ | added (Berkeley DB storage) |
| `market.{h,cpp}` | — | ✓ | added (commerce experiment) |
| `irc.{h,cpp}` | — | ✓ | added (peer discovery) |
| `sha.{h,cpp}` | — | ✓ | added (SHA-256) |
| `ui.*`, `uibase.*` | — | ✓ | added (wxWidgets GUI) |
| `base58.h`, `bignum.h`, `serialize.h`, `uint256.h`, `util.*`, `headers.h` | — | ✓ | added (support/utility) |

Everything except the ledger core (`main`) and networking arrives in January.

## 2. Networking: `node.*` → `net.*`

Same five classes on both sides — `CMessageHeader`, `CAddress`, `CInv`,
`CRequestTracker`, `CNode` — so this is a **rename + expansion**, not a redesign.

| Pair | added | removed |
|---|--:|--:|
| `node.h` → `net.h` | 148 | 38 |
| `node.cpp` → `net.cpp` | 303 | 145 |

## 3. Ledger core: `main.*` revised in place

Same class spine in both; January **adds two disk-index classes**
`CTxIndex` and `CDiskBlockIndex` (paired with the new `db.*` layer). Otherwise the
transaction/block model (COutPoint, CTxIn, CTxOut, CTransaction, CMerkleTx,
CWalletTx, CBlock, CBlockIndex, CBlockLocator) is common to both.

| Pair | added | removed |
|---|--:|--:|
| `main.h` → `main.h` | 384 | 203 |
| `main.cpp` → `main.cpp` | 1357 | 958 |

## 4. Script: present-but-undefined → fully realised

The single most important structural change.

- **NOV08:** `main.cpp` uses `CScript() << OP_CODESEPARATOR << <pubkey> <<
  OP_CHECKSIG`, but the four preserved files contain **no** `opcodetype` enum,
  `class CScript`, `EvalScript`, or `SIGHASH`. Only two opcodes are referenced
  (`OP_CHECKSIG`, `OP_CODESEPARATOR`).
- **JAN09:** a dedicated `script.{h,cpp}` defines the full **106-opcode** vocabulary
  (94 implemented in `EvalScript`, incl. the later-BTC-disabled `OP_CAT`, `OP_MUL`,
  `OP_DIV`, `OP_LSHIFT`, `OP_INVERT`, …), the four `SIGHASH` modes, multisig, and
  the template `Solver`. See `genesis/inventory/OPCODES.md`.

## 5. Reading

On the preserved bytes, **NOV08 is an early architectural witness of the ledger +
networking layer, not a complete standalone Bitcoin** — the predicate engine, keys,
storage, commerce, and UI all first appear (or are first *defined*) in the January
v0.1.0 release. The January tree is where "original Bitcoin" becomes a runnable,
self-contained financial machine.

> Method note: this diff compares only the two frozen archives. It makes no claim
> about intermediate private drafts, and it treats the nov08 gaps as *absence from
> the preserved archive*, not proof the code never existed elsewhere.
