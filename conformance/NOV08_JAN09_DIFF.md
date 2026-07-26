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

## 5. Monetary & timing constitution

The base unit and issuance schedule change materially between the two archives
(read from `main.cpp` / `main.h` in each edition's extracted, hash-verified tree).

| Parameter | NOV08 | JAN09 | anchor (nov08 / jan09) |
|---|---|---|---|
| base unit `COIN` | 1,000,000 (1e6) | 100,000,000 (1e8) | `main.h:34` / `main.h:18` |
| `CENT` | 10,000 | 1,000,000 | `main.h:35` / `main.h:19` |
| block subsidy | 100 coins (`10000 * CENT`) | 50 coins (`50 * COIN`) | `main.cpp:654` / `main.cpp:677` |
| halving interval | every 100,000 blocks (`for i=100000..; /=2`) | every 210,000 blocks (`>>= nBestHeight/210000`) | `main.cpp:655` / `main.cpp:680` |
| target spacing | 15 min (`15 * 60`) | 10 min (`10 * 60`) | `main.cpp:663` / `main.cpp:688` |
| retarget timespan | 30 days (≈2880 blocks) | 2 weeks (2016 blocks) | `main.cpp:662` / `main.cpp:687` |
| fixed tx fee | `1 * CENT` (`TRANSACTIONFEE`) | fee = inputs − outputs | `main.h:36` / — |
| coinbase value rule | must **equal** subsidy+fees (`!=` rejects) | must be **≤** subsidy+fees (`>` rejects) | `main.cpp:739` / `main.cpp:953` |

Two headline consequences:

- **The "satoshi" is genesis-born.** The 1e8 base unit (1 coin = 100,000,000
  units) first appears in January; November used a 1e6 base unit. A "satoshi" as
  the smallest unit is therefore a v0.1.0 (JAN09) property, not a pre-genesis one.
- **Issuance was re-scoped downward at launch:** initial subsidy 100 → 50 coins,
  halving stretched 100k → 210k blocks, block target tightened 15 → 10 min, and
  the coinbase value rule loosened from exact-equality to an upper bound. The
  familiar Bitcoin monetary schedule is set in **January, not November**.

## 6. Reading

On the preserved bytes, **NOV08 is an early architectural witness of the ledger +
networking layer, not a complete standalone Bitcoin** — the predicate engine, keys,
storage, commerce, and UI all first appear (or are first *defined*) in the January
v0.1.0 release. The January tree is where "original Bitcoin" becomes a runnable,
self-contained financial machine.

> Method note: this diff compares only the two frozen archives. It makes no claim
> about intermediate private drafts, and it treats the nov08 gaps as *absence from
> the preserved archive*, not proof the code never existed elsewhere.
