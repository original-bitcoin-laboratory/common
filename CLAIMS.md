# What this project can honestly claim (and what it can't)

Claims here are written to **survive a hostile reader**. Every one is verifiable from
the repo; the hedges are load‑bearing, not decoration. Superlatives about "the world"
are avoided on purpose — you cannot prove a global negative, and unfalsifiable claims
read as marketing and invite easy rebuttal.

## The headline claim (defensible)

> **The most complete executable reconstruction and neutral conformance study of the
> earliest Bitcoin — the November 2008 pre‑release and the January 2009 v0.1.0 — that we
> are aware of. It runs the complete original Script vocabulary with nothing disabled, is
> differential‑verified against the unmodified released binary, and measures descendants'
> divergence from the origin with executed evidence under a single neutral method.**

Note the two hedges that keep it true: **"that we are aware of"** (not "in the world"),
and **"under a single neutral method"** (not "every descendant" — see below).

## Sub‑claims, each individually true

1. **Fidelity.** We execute v0.1's *literal* enabled‑opcode set — including
   `OP_2MUL`/`OP_2DIV` and the byte‑index `OP_SUBSTR/LEFT/RIGHT` that **BSV** drops or
   replaces with `OP_SPLIT`. JAN09‑X even re‑opens `OP_NOTEQUAL`, the one opcode v0.1
   itself disabled. → *"the most literal executable match to v0.1's opcode set we know of."*
2. **Verification.** The exact genesis (`000000000019d668…`) is reproduced three
   independent ways — the unmodified 2009 binary (JAN09‑EXECUTED, `r3‑findings/run1`), the
   C++ PORT, and the Python MODEL — and cross‑checked.
3. **Neutrality + method.** Descendants are measured *from the origin*, equally, by
   execution where an independent implementation exists.
4. **Counterfactual.** Both source states run: v0.1 (JAN09‑X) *and* the Nov‑2008
   pre‑release constitution (NOV08‑X), as live isolated chains with per‑line provenance.

## On "every descendant" — the honest scope

We do **not** measure *every* descendant, and shouldn't claim to (new forks appear
faster than anyone can census). What we have is an **extensible, neutral matrix** applied
uniformly:

| Descendant | Status | How |
|---|---|---|
| **BTC** | **executed** | `python-bitcoinlib` (independent impl) |
| **BSV** | **executed** | `bitcoinx` (independent BSV impl) |
| **BCH**, **XEC** | **documented** | that chain's own consensus spec, same method |
| any other (LTC, DOGE, …) | **addable** | drop in its rules / an installable impl |

So the honest phrasing is *"a neutral, extensible conformance matrix — two chains
cross‑checked by execution today, the rest documented under the same method, and any
candidate addable on the same footing"* — **not** "every descendant." Coverage grows as
independent implementations are installed; the *method* is what's uniform, and no chain is
privileged (including our own NOV08‑X/JAN09‑X).

## What we do NOT claim

- ❌ *"Closest to the original in the world"* — the original v0.1.0 is **preserved and
  runnable**; it isn't something to be "close" to. We execute/reconstruct/measure it.
- ❌ *"The only full Bitcoin"* — BSV runs nearly the full original Script on a live chain,
  and anyone can compile v0.1.0 itself.
- ❌ *A better financial system than BTC/BCH/BSV/XEC/LTC/DOGE* — they are hardened,
  economically‑live networks; this is a MODEL‑level research lab.
- ❌ Anything about **money/value** — the experimental chains are stamped "not money";
  no premine, no sale, no promises.

## Why the hedged claim is stronger

A skeptic can't rebut "the most complete reconstruction *we're aware of*, running the full
original vocabulary, verified against the released binary." They *can* trivially rebut
"closest in the world" (point at BSV, or the v0.1.0 archive) or "measures every descendant"
(name one you didn't). Precision is the moat here — it's exactly the evidence‑first stance
the lab is built on.
