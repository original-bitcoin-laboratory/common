# Curve structure — the algebra of secp256k1, the curve the origin chose

[`QUANTUM_EXPOSURE.md`](QUANTUM_EXPOSURE.md) maps where the signature layer breaks under a quantum
attacker; [`DEPENDENCY_MATRIX.md`](DEPENDENCY_MATRIX.md) maps secp256k1 as a *dependency*. This inventory
maps the **classical algebraic structure of the curve itself** — the properties every Bitcoin descendant
inherits by choosing secp256k1, each derived from the published SEC2 / libsecp256k1 constants with
pure-integer arithmetic and re-evaluable identically by anyone. Executed in
[`genesis/derivatives/curve_structure/`](https://github.com/original-bitcoin-laboratory/genesis/tree/main/derivatives/curve_structure/)
(`python -m pytest`, 8 tests). Its hash-layer companion — SHA-256's NUMS constants, the merkle 64-byte
ambiguity, and the RIPEMD-160 margin — is [`HASH_STRUCTURE.md`](HASH_STRUCTURE.md).

Objective derivation only — no chain privileged, no identity claim. The point is neutral: **the curve's
"suspicious" constants are derivable (so they cannot hide a backdoor), and the one constant that is not
derivable is a benign generator.**

## The facts

| # | Fact | Machine check | Result |
|---|---|---|:--:|
| 1 | **GLV endomorphism** — `β, λ` are the nontrivial cube roots of unity mod `p`/`n`, forced by the curve order (`j = 0`), not author-chosen | derive `β,λ` from `p,n`; `[λ]G == (β·Gx, Gy)`; `1+λ+λ² ≡ 0 (mod n)` | **derivable, holds** |
| 2 | **Endomorphism tax** — the √6 rho speedup makes the best generic classical attack `~2¹²⁷·⁰³`, ~0.79 bit below NIST P-256 (`~2¹²⁷·⁸³`) | `½·log₂(π·n/2) − ½·log₂(6)` vs P-256's `−½·log₂(2)` | **0.79-bit tax, not a break** |
| 3 | **Textbook safety** — `p` prime, `n` prime, cofactor 1, non-anomalous, MOV embedding degree > 200, `G` on curve | six checks | **all pass** |
| 4 | **The trust atom** — `G` has no published NUMS derivation (`Gx ≠ sha256(seed) mod p`, exhaustively) | test obvious seeds + a wide counter sweep | **G un-derivable** |
| 5 | **Twist** — small factors `{3², 13², 3319, 22639}` leak ~33 key bits to a non-validating implementation; large cofactor (~2²²⁰) prime | factor the twist order; verify it reconstructs exactly | **~33-bit leak; validate points** |
| 6 | **`977` is forced, not chosen** — the smallest `c` giving prime `p`, `p≡3 (mod 4)`, `p≡1 (mod 3)`, and a **prime-order** curve (263 → composite order; 361 → `p≡2 mod 3`) | CM point-count (validated on secp256k1) over `c=1..1000` | **977 is minimal ⇒ forced** |

## What each means

- **(1)+(2) The endomorphism is a feature, not a backdoor.** `β` and `λ` — the constants the origin-grift
  points at as "suspicious" — are *forced* by the curve order and re-derived here from `p` and `n` alone;
  they carry no author entropy. Their only real consequence is the √6 Pollard-rho speedup, a **~0.79-bit
  tax** that has been public since SEC2 (2000). `~2¹²⁷` is unbroken, and Shor moots the signature layer
  regardless (`QUANTUM_EXPOSURE.md`). The object praised for GLV speed and the object feared as a backdoor
  are the same thing.

- **(3)+(4) Where "verify, don't trust" bottoms out.** Every algebraic-safety property is checkable and
  passes, and the "backdoor" constants are *derivable* — so no chosen relationship among them can be a
  master key (the general test for a planted backdoor is **derivability**, not vocabulary: only
  un-derivable constants with a hidden relation can hide one). The single un-derivable constant is the
  generator `G`, whose provenance must be trusted, not reproduced. For the discrete log this is benign —
  any generator of the prime-order group is equivalent — but it is the one irreducible trust atom under
  the whole "verify" regress.

- **(6) The `977` field constant is *forced*, not chosen — the wrinkle inverts.** `977` is not the
  minimal prime constant (`263` is), which looked like unexplained rigidity. But a self-contained CM
  point-count (valid because `j=0`; validated by reproducing secp256k1's published order) shows `977` is
  the **smallest** `c` for which `p = 2²⁵⁶−2³²−c` is prime, `p ≡ 3 (mod 4)` (fast √), `p ≡ 1 (mod 3)`
  (the GLV endomorphism exists), **and** the curve `y²=x³+7` has **prime order** (cofactor 1). The
  smaller candidates fail: `263` gives a **composite** curve order; `361` has `p ≡ 2 (mod 3)` (no
  endomorphism, even order). So `977` is transparently determined by the design requirements — the
  *opposite* of a planted magic number. Executed in `curve_structure/` (`field_constant_minimality`).

- **(5) The twist is a "handle with care".** secp256k1 is not twist-secure in the clean sense; an
  implementation that skips point validation leaks ~33 bits. libsecp256k1 validates points, so Bitcoin is
  unaffected — but the requirement is real and part of using this curve correctly.

## Scope & boundary

- **MODEL** — pure-integer derivations from the published constants; documents the curve's *structure*,
  distinct from v0.1's runtime ECDSA *behavior* (malleability/low-S), which is
  [`crypto_conformance/`](https://github.com/original-bitcoin-laboratory/genesis/tree/main/derivatives/crypto_conformance/).
- These are properties of **secp256k1 itself**, inherited by every descendant that keeps the curve — not
  a v0.1-specific behavior and not a claim about any chain or person.
- Not a break or backdoor claim: the endomorphism is a ~0.79-bit tax; the trust atom is benign after
  15+ years; the twist is mitigated by point validation. A tool, never authority (`common/AUTHORITY.md`).
