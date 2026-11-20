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

## 5b. The genesis block in each edition

Both editions hard-code a genesis block. Read from each edition's extracted, hash-verified `main.cpp`:

| Field | NOV08 | JAN09 | anchor (nov08 / jan09) |
|---|---|---|---|
| `hashGenesisBlock` | `000006b15d13…0cfaa646` | `000000000019d668…b60a8ce26f` | `main.cpp:39` / `main.cpp:24` |
| `nTime` | 1221069728 = 2008-09-10 18:02:08 UTC | 1231006505 = 2009-01-03 18:15:05 UTC | `main.cpp:1038` / `main.cpp:1468` |
| `nBits` | `20` | `0x1d00ffff` | `main.cpp:1039` / `main.cpp:1469` |
| `nNonce` | 141755 | 2083236893 | `main.cpp:1040` / `main.cpp:1470` |
| coinbase `scriptSig` | `CScript() << 247422313` | `486604799`, `CBigNum(4)`, the newspaper headline | `main.cpp:1031` / `main.cpp:1460` |
| output `nValue` | `10000` base units (0.01 coin at COIN = 1,000,000) | `50 * COIN` | `main.cpp:1032` / `main.cpp:1461` |
| output `scriptPubKey` | `OP_CODESEPARATOR`, `CBigNum("0x31D1…D404")`, `OP_CHECKSIG` | `CBigNum("0x5F1D…6704")`, `OP_CHECKSIG` | `main.cpp:1033` / `main.cpp:1462` |

**The output key is written the same way in both editions**, as a `CBigNum` hex literal pushed into the
script. `CScript << CBigNum` pushes `CBigNum::getvch()`, which in JAN09 returns the number's bytes
least-significant first (`bignum.h:245–255`; `bignum.h` is not among the four preserved NOV08 files).
Read that way, each 65-byte literal begins `0x04` and is a point on secp256k1:

| Edition | literal as pushed (bytes reversed) | point on secp256k1 | P2PKH form of that key |
|---|---|---|---|
| JAN09 | `04678afdb0fe…1d5f` | yes | `1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa` (the genesis address, as a control) |
| NOV08 | `04d451b0d7e5…088ad131` | yes | `1K8SfPfTkvjF8YHGG134uQvjcdNDHM6UfB` |

Read in the order the hex is written, neither literal begins with `0x04` and neither is a point on the
curve, so neither is a public key in that order. The NOV08 key is not the JAN09 key, and the NOV08 block
is not on the chain that JAN09 starts.

Reproduce (standard library; `hashlib` must provide RIPEMD-160):

```python
import hashlib, re
B58 = "123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz"
P = 2**256 - 2**32 - 977
def b58check(p):
    d = p + hashlib.sha256(hashlib.sha256(p).digest()).digest()[:4]
    n, s = int.from_bytes(d, "big"), ""
    while n: n, r = divmod(n, 58); s = B58[r] + s
    return "1" * (len(d) - len(d.lstrip(b"\0"))) + s
for path, line in (("nov08/main.cpp", 1033), ("v0.1.0/bitcoin/src/main.cpp", 1462)):
    hexlit = re.search(r'CBigNum\("0x([0-9A-F]+)"\)', open(path, encoding="latin-1").read().splitlines()[line - 1]).group(1)
    k = bytes.fromhex(hexlit)[::-1]                     # getvch(): least-significant byte first
    x, y = int.from_bytes(k[1:33], "big"), int.from_bytes(k[33:], "big")
    print(path, k[0] == 4 and (y * y - x ** 3 - 7) % P == 0,
          b58check(b"\0" + hashlib.new("ripemd160", hashlib.sha256(k).digest()).digest()))
```

## 6. Reading

On the preserved bytes, **NOV08 is an early architectural witness of the ledger +
networking layer, not a complete standalone Bitcoin** — the predicate engine, keys,
storage, commerce, and UI all first appear (or are first *defined*) in the January
v0.1.0 release. The January tree is where "original Bitcoin" becomes a runnable,
self-contained financial machine.

> Method note: this diff compares only the two frozen archives. It makes no claim
> about intermediate private drafts, and it treats the nov08 gaps as *absence from
> the preserved archive*, not proof the code did not exist elsewhere.
