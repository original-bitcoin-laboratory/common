# The quantum row, made concrete — where ECDSA‑on‑secp256k1 actually breaks

The dependency matrix ends on the one cell shared by the origin **and** every descendant:
ECDSA on secp256k1 is not post‑quantum. This note makes that cell concrete — **objective
protocol/math analysis grounded in this repo's source**, no external references. It is not
a live‑exploit claim (no such quantum computer exists); it maps *where the exposure sits
and why*, and what a signature swap would change.

## Two very different quantum threats — one breaks, one only dents

Bitcoin's cryptography splits cleanly, and the two halves face very different quantum risk:

| Primitive | Where used | Quantum attack | Result |
|---|---|---|---|
| **ECDSA on secp256k1** (asymmetric) | ownership: signatures | **Shor's algorithm** solves the elliptic‑curve discrete‑log | **Broken** — private key recoverable from a **public key** |
| **SHA‑256 / RIPEMD‑160** (symmetric/hash) | PoW, sighash, addresses | **Grover's algorithm** — quadratic speedup on preimage search only | **Dented, not broken** — 256‑bit → ~128‑bit effective; still infeasible |

So the vulnerable half is **signatures**, not hashing. Proof‑of‑work and the address hash
survive (Grover just halves the exponent, and 2¹²⁸ is still out of reach). The whole
quantum question for Bitcoin reduces to one thing: **once a public key is known, its coins
are forgeable.** Everything below follows from that.

## The exposure rule: a public key must be *visible* to attack it

Shor needs the **public key** as input. Bitcoin outputs differ in whether they reveal it:

- **Bare P2PK** — `<pubkey> OP_CHECKSIG`. The public key sits **in the output script, in the
  clear, forever.** Exposed the moment it is mined; nothing to spend first.
- **P2PKH** — `OP_DUP OP_HASH160 <hash160(pubkey)> OP_EQUALVERIFY OP_CHECKSIG`. The output
  carries only `RIPEMD160(SHA256(pubkey))`. The public key is **hidden behind the hash**
  and revealed only **when the coin is spent** (it enters `scriptSig`). Until then a quantum
  attacker would have to invert the hash — a Grover problem (~2¹²⁸), infeasible. At the
  moment of spend the key hits the mempool and becomes a **harvest‑at‑spend race**.
- **Reused / already‑spent addresses** — the key was revealed by an earlier spend, so those
  coins are in the exposed class regardless of script type.

**Rule of thumb:** exposed public key ⇒ quantum‑forgeable; unspent, unreused P2PKH ⇒
protected by the hash until the instant it is spent.

## What our source says — the origin's coins are the *most* exposed

Verified in this repo (hash‑verified v0.1.0 source), the origin pays to **bare P2PK**, not
P2PKH:

- **Genesis coinbase** — `scriptPubKey = 5F1DF16B2B…4C3FBCF649B6… OP_CHECKSIG`
  (a 65‑byte public key + `OP_CHECKSIG`), captured in the executed witness.
- **Mining reward** — `CreateTransaction` / `CreateCoinBase` set
  `vout[0].scriptPubKey = CScript() << <pubkey> << OP_CHECKSIG`
  ([`genesis/extracted/bitcoin/src/main.cpp:1462`]).
- **Change / send‑to‑IP** — same bare‑P2PK form
  ([`genesis/extracted/bitcoin/src/main.cpp:1988`]).

P2PKH (the hashed form that would *hide* the key) is a **later** convention, not v0.1's
default path. Objective consequence: the **earliest coins — the whole v0.1‑era coinbase
set — carry their public keys in the clear on‑chain, permanently.** Under the exposure rule
above, they are the single most quantum‑exposed class of outputs in the Bitcoin lineage —
and this is a property of the **origin's own design** (the [A19] bare‑P2PK finding), not of
any descendant. Every descendant that still holds such outputs inherits the same exposure
on those coins; newer P2PKH/SegWit coins are better placed until spent.

## What a post‑quantum signature swap would change in the matrix

The exposure lives entirely in the **EC signatures** column. Replacing ECDSA‑on‑secp256k1
with a post‑quantum signature scheme rewrites exactly that one cell for every row:

- **Candidate families** — *hash‑based* (rest only on hash security, i.e. the Grover‑only,
  hard half — attractive because it reuses Bitcoin's strongest assumption) or *lattice‑based*
  (smaller signatures, newer assumptions). Verification would be a new opcode/script rule
  replacing `OP_CHECKSIG`'s ECDSA check.
- **What it does *not* fix** — a scheme swap protects *future* outputs. Coins whose public
  keys are **already exposed** (bare P2PK, reused addresses) cannot be retroactively hidden;
  they must be *moved* to a post‑quantum output **before** a quantum adversary exists — and
  a key whose owner is gone (e.g. the earliest coins) can never be moved. This is why the
  origin's bare‑P2PK cell is the sharp one: no swap reaches it after the fact.
- **Cost** — post‑quantum signatures are far larger than 64–72‑byte ECDSA (bytes → kilobytes),
  a consensus hard change, and a migration problem, not a drop‑in.

Objectively: the quantum row is a property of the **algorithm and the output format**, not
of any one chain — the curve is classically strong and the hashing survives. The exposure
concentrates exactly where a public key is visible, and this repo's source shows the origin
put its earliest keys there in the clear.

## Boundary

Objective protocol/math analysis + source‑verified facts from this repo (bare‑P2PK in
v0.1.0). No external references, no live exploit, no chain privileged — the exposure applies
identically to any lineage holding exposed‑key outputs. Cross‑ref: the quantum cell in
[`DEPENDENCY_MATRIX.md`](DEPENDENCY_MATRIX.md); the executed ECDSA cross‑check in
[`../../genesis/derivatives/crypto_conformance/`](../../genesis/derivatives/crypto_conformance/).
