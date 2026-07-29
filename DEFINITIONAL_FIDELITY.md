# Definitional fidelity — the strongest *true* claim, and its one contestable premise

This states the strongest claim the lab can make **scientifically**: not that our
reconstruction *is* Bitcoin (impossible — see below), but that it **maximally satisfies an
explicit, defended definition of Bitcoin** — falsifiably, neutrally, at least as fully as any
existing candidate. It reads in the discipline of [`THESIS.md`](THESIS.md) and
[`CLAIMS.md`](CLAIMS.md) and rests on the result of
[`WHAT_IS_BITCOIN.md`](WHAT_IS_BITCOIN.md).

## Two claims — only one is possible

- **(A) Identity** — *"this **is** the Bitcoin; nobody can argue against it."*
- **(B) Fidelity** — *"this **satisfies the definition** of Bitcoin at least as fully as
  anything else."*

A definition specifies a **predicate**: *x is Bitcoin iff x realises [the design / the
origin]*. The load‑bearing logical fact: **many x can satisfy one predicate.** Satisfying it
makes you *a* Bitcoin‑by‑this‑definition — never *the* Bitcoin. Uniqueness of referent needs a
**selection principle on top of the predicate**, and the only ones that exist are conventions
(earliest launch, most‑adopted, the popularly‑recognised genesis). Hence:

> **Uniqueness of referent is convention; predicate‑satisfaction is fact.**

**(A) is therefore unachievable**, and it is *our own* logic that forecloses it: the
branching‑identity result (no branch is uniquely "the original"), and the 10⁶‑vs‑10⁸
demonstration (even the unit can only be *selected*, not derived). A critic can *always* argue
against (A) — not because the work is weak, but because (A) asks for a fact where only a
convention exists, and forcing it would smuggle a convention in as truth. Two artifact‑level
nails: the thing *most faithful to the origin is the origin*, which **already exists and runs**
(v0.1.0 was executed); a new build can only **tie** it and — new genesis — is a *new member of
the genus*, not the origin's unique continuation.

**(B) is achievable, and is the actual science.** The rest of this document is (B).

## The definition (explicit, defended — not smuggled)

> **Bitcoin ≝ the whitepaper design, with v0.1.0 — the author's own first realisation — as its
> reference implementation.**

Why *this* definition:

- **The design alone is too thin to discriminate.** The whitepaper fixes a PoW timechain that
  prevents double‑spending without a trusted third party — which BTC, BCH, BSV *all* satisfy.
  To give the predicate teeth you need the reference implementation (Script set, monetary
  rules, consensus path), which the paper leaves open.
- **v0.1.0 is the principled reference**: the author's own artifact, the earliest *running*
  realisation, hash‑verified, and this lab's executed behavioural oracle.
- **Neutrality**: everything is measured *from* this origin, uniformly, no chain privileged
  (including our own NOV08‑X/JAN09‑X).

This is a **definitional stance** — defensible, but a choice, not a forced fact. It is the one
place a critic can stand (see the last section), and we say so plainly.

## The metric (falsifiable, reproducible)

For each candidate — our reconstruction, BTC, LTC, DOGE, BCH, XEC, BSV — measure **divergence
from the reference** on explicit, checkable criteria, each source‑verified or executed:

| Criterion | How measured | Where |
|---|---|---|
| **Opcode vocabulary** (enabled / disabled / altered) | executed / execution‑bounded per chain | the neutral conformance matrix |
| **Monetary constitution** (`COIN`, subsidy, halving, spacing, coinbase rule) | source‑verified | the NOV08→JAN09 diff + consensus modules |
| **Consensus rules** (PoW, difficulty retarget, validation path, timestamps, finality) | C++/OpenSSL PORT + executed ports | `derivatives/node`, `derivatives/temporal` |
| **Cryptography** (ECDSA on secp256k1, malleability) | executed vs libsecp256k1 | `derivatives/crypto_conformance` |

**Fewer divergences from the reference ⇒ higher fidelity.** The whole measurement reproduces
from source:
[`reproduce.py`](https://github.com/original-bitcoin-laboratory/genesis/blob/main/scripts/reproduce.py).

## The result

- **At‑par with the origin.** The reconstruction is **differential‑verified behaviourally
  identical** to v0.1.0 — MODEL ≡ C++/OpenSSL PORT ≡ the executed genesis
  `000000000019d668…`. Its fidelity is **maximal: it ties the origin.** The origin is the
  **ceiling** — nothing can exceed fidelity‑to‑itself.
- **Ahead of the divergent descendants.** The matrix *measures* their divergences: BTC disabled
  the broad opcode set; LTC/DOGE changed the proof‑of‑work; every chain altered monetary or
  consensus rules. On *origin‑fidelity* they are strictly further from the reference.

> **Under the stated definition, this reconstruction realises Bitcoin at least as completely as
> any existing candidate — tying the origin, ahead of every chain that disabled or altered an
> origin feature — and the measurement is reproducible.**

That is the science‑grade form of "fits the definition better, or at‑par": it ignores market
cap and social consensus entirely and scores predicate‑satisfaction against a stated
definition.

## How to falsify it

The claim is only worth stating because it can be broken. Any one of these refutes it:

1. **Name a behaviour** where the reconstruction diverges from v0.1.0 → breaks *at‑par*.
2. **Name a chain with zero measured divergence** from the reference → it ties us; or show a
   descendant's divergence count is *lower* than ours → breaks *ahead*.
3. **Reject the definition** itself — the only refutation that needs no divergence at all.

The first two are objective and checkable against the repo. The third is the real fault line.

## The one contestable premise — named openly

Separate (A) from (B) and notice *where* disagreement can live: **not in the measurement**
(objective) but in **the choice of definition.** A critic who defines Bitcoin as *"the BTC
chain"* or *"the most‑adopted network"* is unbound by our result — they picked a different
predicate. We defend the origin‑definition on principled grounds (the author's own artifacts,
the earliest running realisation, neutrality), but it remains a **definitional stance, not a
derivable fact.** Conceding that is exactly what makes the rest bulletproof: *everything
downstream of the definition is objective; only the definition is a choice.* Nobody can argue
with the measurement; anyone can choose a different definition — and then they are measuring a
different thing, and should say which.

## What this does **not** claim

- ❌ *"This is **the** Bitcoin."* Identity is convention; unachievable, and disowned.
- ❌ *"This **beats** the origin."* It ties it — the origin is the fidelity ceiling.
- ❌ *"Better / more secure / longer‑lasting."* Fidelity ≠ quality; that superlative is
  disowned in [`THESIS.md`](THESIS.md).
- ❌ *"The Bitcoin the world has been waiting for."* A claim about *desire*, not *definition* —
  outside this frame entirely.
- ❌ *That anyone must adopt it.* Adoption is social; the claim is definitional.

## Network parameters vs. consensus fidelity (why the live nets mine "easy")

One divergence deserves an explicit line so it is never mistaken for a fidelity gap: the live
experimental networks **NOV08‑X / JAN09‑X mine at regtest‑easy difficulty** with a short block
spacing, whereas the unmodified 2009 client and the reproduced historical genesis run at the
**real** difficulty (`0x1d00ffff`). This is deliberate, disclosed, and **confined**:

- It is a **network/operational parameter, not a change to any consensus *rule*.** The retarget
  *algorithm*, the proof‑of‑work *check*, the block structure, and the monetary rules are all
  faithful; only the target *value* on the isolated toy net is set easy.
- It is the choice **Bitcoin's own `regtest` makes**, for the same reasons: a valueless, isolated
  network with one or two CPUs cannot sustain mainnet difficulty, and there is **nothing of value
  to secure** (so PoW difficulty would buy nothing). It also keeps the experimental genesis
  **deterministically re‑minable** — `verify_genesis.py` re‑derives it in a fraction of a second.
- It **does not touch the fidelity claims.** Those rest on the real genesis `000000000019d668…`
  and the consensus rule *content*, reproduced at real difficulty by the C++/OpenSSL port and the
  unmodified binary — never on the easy toy nets. A live `min_bits` floor
  (`derivatives/netnode/difficulty.py`) lets an operator require real work on an X‑network without
  ever touching the faithful genesis.

In short: **real binary / real genesis → real difficulty (faithful); the throwaway NEW‑EXP
networks → easy difficulty (a disclosed operational choice, security‑irrelevant because valueless).**

## Boundary

The definition is stated and defended, not assumed; the metric is source‑verified or executed;
the result reproduces from the repo; the sole contestable premise (the definition) is named,
not hidden. Companion to [`WHAT_IS_BITCOIN.md`](WHAT_IS_BITCOIN.md) (what the word denotes),
[`THESIS.md`](THESIS.md) (the argument), [`CLAIMS.md`](CLAIMS.md) (the assertions), and the
neutral conformance matrix (the measurement).
