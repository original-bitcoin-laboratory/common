# Dependency / attack‑surface matrix — where risk lives, by version & descendant

A neutral map of the **security‑critical dependencies** of each Bitcoin codebase — the
places where a fault or a vulnerability can reside (in the *dependency*, not necessarily
the chain). Same discipline as the conformance matrix: **source‑verified** where we hold
the code (NOV08 / v0.1.0 / v0.1.3), **documented** for the modern descendants. It maps
*attack surface and historical incident class* — **not** a claim any chain is currently
exploitable.

> Read with care: "a dependency had incidents" ≠ "this chain is vulnerable now." Most of
> these were *fixed*; the value is seeing **which dependency carried the risk, and who
> still carries it.**

## The matrix

Legend: **[S]** = verified from this repo's hash‑verified source; **[D]** = documented
from public record (not source‑verified here).

| Function → | EC signatures | RNG (key entropy) | Hashing (SHA‑256 / RIPEMD‑160) | Script arithmetic | Database | GUI | Networking |
|---|---|---|---|---|---|---|---|
| **NOV08** (pre‑release) | OpenSSL `BN_*` refs; no key.* in snapshot **[S]** | — (absent in snapshot) | `SHA256` refs (sha.h absent) **[S]** | OpenSSL `BIGNUM` (unbounded) **[S]** | Berkeley DB (`CDB` refs) **[S]** | none (no GUI) **[S]** | Winsock **[S]** |
| **v0.1.0** (genesis) | **OpenSSL** `EC_KEY` (general EC) **[S]** | **OpenSSL** `RAND_bytes` **[S]** | **OpenSSL** SHA‑256 + own `sha.cpp`; RIPEMD‑160 **[S]** | OpenSSL **`BIGNUM`, unbounded** (full vocab) **[S]** | **Berkeley DB** (`db_cxx`) **[S]** | **wxWidgets 2.8** (+png/jpeg/tiff/zlib) **[S]** | Winsock **[S]** |
| **v0.1.3** | OpenSSL `EC_KEY` (identical to v0.1.0) **[S]** | OpenSSL `RAND_bytes` **[S]** | OpenSSL + `sha.cpp` **[S]** | OpenSSL `BIGNUM`, unbounded **[S]** | Berkeley DB **[S]** | wxWidgets 2.8 **[S]** | Winsock (hardened reconnect) **[S]** |
| **BTC** (modern Core) | **libsecp256k1** **[D]** | own (OS entropy + ChaCha20; dropped OpenSSL) **[D]** | own SHA‑256/RIPEMD‑160 (SIMD) **[D]** | **bounded** `CScriptNum` (broad vocab disabled) **[D]** | **LevelDB** (chainstate); BDB→SQLite for wallet **[D]** | Qt **[D]** | own P2P **[D]** |
| **LTC** | libsecp256k1 **[D]** | own **[D]** | **Scrypt** PoW + SHA‑256 tx **[D]** | bounded `CScriptNum` (disabled) **[D]** | LevelDB **[D]** | Qt **[D]** | own **[D]** |
| **DOGE** | libsecp256k1 **[D]** | own **[D]** | **Scrypt** (AuxPoW w/ LTC) **[D]** | bounded `CScriptNum` (disabled) **[D]** | LevelDB **[D]** | Qt **[D]** | own **[D]** |
| **BCH** | libsecp256k1 **[D]** | own **[D]** | own SHA‑256/RIPEMD‑160 **[D]** | bounded, **restored subset** (with limits) **[D]** | LevelDB **[D]** | Qt **[D]** | own **[D]** |
| **XEC** (eCash) | libsecp256k1 **[D]** | own **[D]** | own **[D]** | bounded, BCH subset **[D]** | LevelDB **[D]** | Qt **[D]** | own + Avalanche **[D]** |
| **BSV** | libsecp256k1 **[D]** | own **[D]** | own **[D]** | **near‑original**, configurable limits **[D]** | LevelDB **[D]** | Qt **[D]** | own **[D]** |

## The pattern this reveals

Two dependencies that sat **inside consensus** in the origin — **OpenSSL** and
**Berkeley DB** — each caused a **real chain split**, and **every descendant removed both
from consensus**:

- **OpenSSL (crypto) → the July 2015 BIP66 fork.** Lenient, version‑dependent signature
  parsing → a ~6‑block split. Fixed by strict‑DER + moving to **libsecp256k1**
  (see `genesis/inventory/THE_OPENSSL_THREAD.md`, executed in
  `genesis/derivatives/crypto_conformance/`).
- **Berkeley DB (storage) → the March 2013 fork.** Berkeley DB's default lock limit
  couldn't process a large block, so v0.7 (BDB) nodes rejected a block v0.8 (LevelDB)
  accepted → a ~24‑block split, resolved by miners downgrading. It **triggered the move to
  LevelDB** for the chainstate. A *configuration of a dependency* forked the chain.
- **RNG (key entropy).** Not a Bitcoin fork, but the same class: weak randomness = stolen
  coins (the 2008 Debian OpenSSL RNG weakening; the 2013 Android `SecureRandom` bug). This
  motivated **deterministic nonces (RFC 6979)** in libsecp256k1 — no reliance on RNG at
  signing time.

**Lesson (the through‑line):** the origin placed consensus‑critical behavior in
**general‑purpose third‑party libraries** (OpenSSL, Berkeley DB) whose behavior varied by
version/platform/config; each produced a real split; the whole ecosystem's maturation was
**pulling those in‑house** (libsecp256k1, LevelDB, bounded `CScriptNum`, strict‑DER). This
is the same spine as the Script matrix and the OpenSSL essay, one layer down.

## The one risk shared by *everyone* — original and all descendants

**ECDSA on secp256k1 is not post‑quantum.** A sufficiently large quantum computer running
**Shor's algorithm** solves the elliptic‑curve discrete‑log problem and recovers a private
key from a public key — breaking signatures for **NOV08, v0.1.0, and every descendant
alike** (they all use the same curve + ECDSA). The *curve itself* is classically strong
and well‑chosen (transparent, non‑NIST parameters); the shared long‑term exposure is the
**algorithm**, not the implementation. This is the single cell where "secp256k1 is a weak
point of *all* of them" is true — and it's the frontier post‑quantum cryptography targets.

This cell is made concrete — Shor vs Grover, which outputs expose their public keys, and why
the origin's **bare‑P2PK** coins are the sharpest case — in [`QUANTUM_EXPOSURE.md`](QUANTUM_EXPOSURE.md).
The curve's own classical structure — the derivable GLV endomorphism (a ~0.79‑bit tax, not a backdoor),
the textbook safety checks, the one un‑derivable constant (the generator `G`), and the twist's
point‑validation caveat — is derived from the published constants in
[`CURVE_STRUCTURE.md`](CURVE_STRUCTURE.md).

## Boundary

Source‑verified rows (NOV08 / v0.1.0 / v0.1.3) are ground truth from this repo. Modern rows
are **documented** from the public record and would need each chain's source to promote to
[S] — the same "executed vs documented" honesty as the conformance matrix, applied equally.
Nothing here asserts a live exploit; it maps *where risk has historically lived and who
still carries which dependency.*

The companion inventory [`CONSENSUS_SURFACE.md`](CONSENSUS_SURFACE.md) maps the *code‑level*
bounds (money range, block size, script limits) the origin left unbounded — the guardrails
this dependency map's incidents later motivated.
