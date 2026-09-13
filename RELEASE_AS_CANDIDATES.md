# Releasing NOV08‑X / JAN09‑X — as candidates, honestly

The lab holds two reconstructions that carry the **full original vocabulary** ("nothing
disabled") under each origin's constitution: `NOV08‑X` (November) and `JAN09‑X` (January
genesis). This document states — in the discipline of [`THESIS.md`](THESIS.md),
[`DEFINITIONAL_FIDELITY.md`](DEFINITIONAL_FIDELITY.md), and
[`WHAT_IS_BITCOIN.md`](WHAT_IS_BITCOIN.md) — **how they can be released to the world, and how
they must not be.**

## The one honest claim

> They are released as **candidates** — reconstructions whose **fidelity to a chosen origin is
> measured and reproducible** — for anyone to run and verify.
> They are **not** "the real Bitcoin."

Why this is the only defensible claim: *which* thing "is" Bitcoin has **no factual answer**
(identity is convention — WHAT_IS_BITCOIN §3). What *is* a fact is **conformance to a stated
origin**: under `nov08`, `NOV08‑X` measures distance **0**; under `v0.1.0`, `JAN09‑X` measures
**1** (it differs only by re‑opening the one opcode v0.1 disabled). So we put them on the table
with their distances shown, and leave any convention about names to others.

## "A Bitcoin," not "the Bitcoin" — and why no instance is privileged

The claim is deliberately *"a Bitcoin,"* not *"the Bitcoin."* Because no fact selects a unique
referent (WHAT_IS_BITCOIN §3), **every instance — BTC, BSV, NOV08‑X, JAN09‑X — can be measured for its distance from the
origin, and none is privileged by fact.** The measurement ranks fidelity only; which instance the
word "Bitcoin" denotes is left to convention.

They stand as **different *kinds* of candidate — and that is *why* they're equal**:

- **BTC / BSV are continuity candidates** — branches of the actually‑launched chain (they trace
  to genesis `000000000019d668`). Claim: *"I am what that chain became."*
- **NOV08‑X / JAN09‑X are fidelity candidates** — new instances (own genesis) that re‑realise
  the origin's constitution *undrifted* (JAN09‑X is distance 1 from v0.1.0; BTC is 9). Claim:
  *"I am what that design specifies."*

Neither claim is a fact — *which* lens (continuity vs fidelity) counts is the **contextual
choice**, and choosing is convention. So the reconstructions are candidates alongside the
continuity chains, ranked by neither, **precisely because the definition is ambiguous**: pick
continuity and the reconstructions do not apply; pick fidelity and they measure closer. The origin‑distance tracker
([`genesis/derivatives/tracker/`](https://github.com/original-bitcoin-laboratory/genesis/tree/main/derivatives/tracker/))
shows both directions.

## Definition and access are different questions

"Candidate" is a claim about **definition**; *"as openly accessible as a BTC or a BSV"* is
a claim about **infrastructure** — not the same thing. The definitional point is real now;
access has three senses (below): publishing gives inspection, a joinable network gives
participation, and trading as money is excluded — not deferred. Whether any network is money is a
convention outside this measurement; here "not tradable" means nothing is sold, offered, priced or
traded, and it ranks nothing.

## Three senses of "release" — the accessibility‑parity ladder (only two are safe)

| Release | What it is | Verdict |
|---|---|---|
| **A · Artifact** | publish the source; anyone runs the MODEL and re‑derives the exact X‑genesis blocks | **done — published** |
| **B · Non‑monetary network** | a live, joinable, **"not money"** experimental network | **done — live; see `genesis/docs/ANNOUNCE.md`** |
| **C · Value‑bearing money** | coins people buy/sell/hold as an asset | **excluded — not a later step** |

- **A — done.** [`verify_genesis.py`](https://github.com/original-bitcoin-laboratory/genesis/blob/main/scripts/verify_genesis.py)
  already makes both X‑genesis blocks **reproducible from source, with no live node required**.
  Publishing the repos turns the reconstructions into durable, world‑runnable artifacts +
  measured candidates. The repositories are published.
- **B — done.** A running network people can join needed three things the
  MODEL lacked: a **hardened production node** (not lab Python), **public infrastructure**
  (seeds/discovery), and **participants who keep mining it**. Framed explicitly as *not money*,
  this is the defensible way to make them *live*. It is a genuine engineering project
  (harden → audit → launch), not a switch.
- **C — the line we hold.** The MODEL is unaudited and mines at regtest difficulty; it is not offered
  as an asset, and nothing is sold, offered, priced or traded. `NO value‑bearing mainnet on
  MODEL code.`

## Two senses of permanent

Only one of them is within the maintainers' power:

- **Artifact permanence.** The genesis blocks re‑derive from source with no live node required; the
  published repositories are what that rests on.
- **Network permanence.** Satoshi's genesis persists because a community *kept mining the chain*.
  Persistence of a *live network* is a **social fact conferred by use** — a candidate can be
  **offered**; it cannot be **declared** permanent. The recipe is published; whether a network
  endures is not the laboratory's to decide.

## Path B is live; the standing conditions

The concrete engineering plan — the MODEL→joinable‑node gap, a staged roadmap, and *why the
non‑monetary status is what makes the origin's "nothing disabled" safe to run publicly* — is
[`genesis/docs/PUBLIC_TESTNET_SCOPE.md`](https://github.com/original-bitcoin-laboratory/genesis/blob/main/docs/PUBLIC_TESTNET_SCOPE.md).
The non‑negotiables:

- **Stamped "not money"** everywhere — no premine, no sale, no yield, no price talk. The maintainers
  assign the units no value and solicit no market; mining issues valueless experimental units, and
  whether third parties value them is outside any software's control, but nothing in the design invites it.
- **Hardened first** — a production node with real proof‑of‑work (the Python and Rust nodes in
  `genesis/derivatives/`, cross‑checked byte‑for‑byte); no external security review is claimed. The
  MODEL is a research microscope, not a validator for value.
- **Distinct identity** — its own genesis/magic/ports; **units are not satoshis**; no inherited
  balances. (Already true of the X‑chains by construction.)
- **Candidate framing intact** — measured fidelity, not "the real Bitcoin."

## What is not done

- Present a MODEL as money, or imply investment value.
- Claim either reconstruction **is** "the real Bitcoin" (identity is convention).
- Claim to have made a network permanent (only continued use can).
- Ship path B without hardening + the "not money" framing.

## The through‑line

This is [`THESIS.md`](THESIS.md) and [`DEFINITIONAL_FIDELITY.md`](DEFINITIONAL_FIDELITY.md) put
into action: **put the origin, seen clearly and measurably, on the table — as candidates — and
leave the rest to others.** They can be made *permanent artifacts* and *measured candidates*
today; a *permanent network* is something only others can choose to build on top of what is
released. The laboratory publishes the artifacts and the measurement; nothing further is claimed.
