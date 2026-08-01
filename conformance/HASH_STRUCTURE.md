# Hash structure — the algebra and margins of Bitcoin's hash layer

[`CURVE_STRUCTURE.md`](CURVE_STRUCTURE.md) maps the elliptic curve (the signature layer). This maps the
**hash layer** — SHA-256 (proof-of-work, txids, the merkle tree) and RIPEMD-160 (addresses) — from the
published constants and the functions themselves. Executed in
[`genesis/derivatives/hash_structure/`](https://github.com/original-bitcoin-laboratory/genesis/tree/main/derivatives/hash_structure/)
(`python -m pytest`, 5 tests). Pure computation; no chain privileged, no identity claim.

## The facts

| # | Fact | Machine check | Result |
|---|---|---|:--:|
| 1 | **SHA-256 constants are NUMS** — `K[0..63]` = frac(cuberoot of the first 64 primes)·2³²; `H[0..7]` = frac(sqrt of the first 8 primes)·2³² | reproduce from the primes by integer `icbrt`/`isqrt` | **64/64 + 8/8** |
| 2 | **64-byte-tx merkle ambiguity** — a 64-byte transaction's `txid` equals the interior-node hash of its two 32-byte halves (same double-SHA-256 for leaves and nodes) | `dsha256(L‖R)` == `txid(L‖R)` | **identical** |
| 3 | **RIPEMD-160 is the floor** — `HASH160 = RIPEMD-160(SHA-256(x))` is 160-bit ⇒ ~2⁸⁰ collision, below SHA-256's 2¹²⁸ and secp256k1's ~2¹²⁷ | bit-length + a birthday demo | **2⁸⁰ weakest** |

## What each means

- **(1) The hash layer is *more* transparent than the curve.** SHA-256's "magic numbers" are the
  fractional parts of the cube/square roots of the small primes — the archetypal nothing-up-my-sleeve
  construction — and they reproduce exactly from the primes here. Applying the same derivability test
  used on the curve ([`CURVE_STRUCTURE.md`](CURVE_STRUCTURE.md)): SHA-256 leaves **no** room for a hidden
  relation, whereas secp256k1's generator `G` is the one constant that is **not** derivable. So across
  Bitcoin's entire cryptographic base, `G` is the **single irreducible trust-atom**; everything else —
  the endomorphism, the field constant `977`, the hash constants — is forced or derivable.

- **(2) A structural merkle collision with no broken hash.** Because leaves and interior nodes share one
  hash function, a 64-byte transaction is indistinguishable from an interior node over two child hashes,
  which enables forged SPV inclusion proofs. It sits beside the odd-level duplication
  ([`CONSENSUS_BEHAVIORS.md`](CONSENSUS_BEHAVIORS.md)) as a merkle ambiguity; the defence is to reject
  64-byte transactions / bind tree depth to the transaction count.

- **(3) The real security floor.** The address hash's 160-bit width caps collision resistance at ~2⁸⁰ —
  the lowest margin in the system. Infeasible today and only relevant to adversarial same-`HASH160`
  constructions, but it is the honest floor, stated precisely rather than rounded up to "256-bit."

## Scope & boundary

- **MODEL / pure computation** — over the published constants and the standard hash functions (the
  SHA-256 verified is confirmed standard via `sha256("")`). Documents the hash layer's structure and
  margins, inherited by every version that keeps these primitives.
- Not a break or backdoor claim: SHA-256 is clean NUMS; the merkle ambiguity is defended by rejecting
  64-byte transactions; the 2⁸⁰ margin is astronomically far off. A tool, never authority
  (`common/AUTHORITY.md`).
