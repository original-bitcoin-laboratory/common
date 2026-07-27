# Releasing NOV08‑X / JAN09‑X — as candidates, honestly

The lab holds two reconstructions that carry the **full original vocabulary** ("nothing
disabled") under each origin's constitution: `NOV08‑X` (November) and `JAN09‑X` (January
genesis). This document states — in the discipline of [`THESIS.md`](THESIS.md),
[`DEFINITIONAL_FIDELITY.md`](DEFINITIONAL_FIDELITY.md), and
[`WHAT_IS_BITCOIN.md`](WHAT_IS_BITCOIN.md) — **how they can be released to the world, and how
they must not be.**

## The one honest claim

> They are released as **candidates** — reconstructions whose **fidelity to a chosen origin is
> measured and reproducible** — for anyone to run, verify, and, if they ever choose, adopt.
> They are **not** "the real Bitcoin."

Why this is the only defensible claim: *which* thing "is" Bitcoin has **no factual answer**
(identity is convention — WHAT_IS_BITCOIN §3). What *is* a fact is **conformance to a stated
origin**: under `nov08`, `NOV08‑X` measures distance **0**; under `v0.1.0`, `JAN09‑X` measures
**1** (it differs only by re‑opening the one opcode v0.1 disabled). So we put them on the table
with their distances shown and let **adoption — which is convention — decide.** That is
exactly "the world decides."

## Three senses of "release" — only two are safe

| Release | What it is | Verdict |
|---|---|---|
| **A · Artifact** | publish the source; anyone runs the MODEL and re‑derives the exact X‑genesis blocks | ✅ **safe, and largely already true** |
| **B · Non‑monetary network** | a live, joinable, **"not money"** experimental network | ✅ possible — **after hardening** |
| **C · Value‑bearing money** | coins people buy/sell/hold as an asset | ❌ **no — not on MODEL code** |

- **A — do this now.** [`verify_genesis.py`](https://github.com/original-bitcoin-laboratory/genesis/blob/main/scripts/verify_genesis.py)
  already makes both X‑genesis blocks **deterministically reproducible from source, forever**.
  Publishing the repos turns the reconstructions into permanent, world‑runnable artifacts +
  measured candidates. This needs **no new code** — it is the pending publish step.
- **B — feasible, but real work.** A running network people can join needs three things the
  MODEL lacks: a **hardened production node** (not lab Python), **public infrastructure**
  (seeds/discovery), and **participants who keep mining it**. Framed explicitly as *not money*,
  this is the defensible way to make them *live*. It is a genuine engineering project
  (harden → audit → launch), not a switch.
- **C — the line we hold.** Shipping the unaudited, regtest‑easy MODEL as an asset would harm
  adopters and resurrect the identity overreach the lab disowns. `NO value‑bearing mainnet on
  MODEL code.`

## On "permanent" — what you can and cannot confer

Two different permanences, and only one is in your gift:

- **Artifact‑permanent (yours now).** The genesis blocks re‑derive identically from source in
  2026, 2100, forever. Publishing makes that public and immutable. Done.
- **Network‑permanent (the world's, not yours).** Satoshi's genesis is permanent because a
  community *kept mining the chain*. Permanence of a *live network* is a **social fact conferred
  by adoption** — you can **offer** a permanent‑capable candidate; you cannot **declare** it
  permanent. So: *the recipe is eternal; whether a network endures is the world's choice.* Which
  is precisely what "let the world decide" means.

## If it goes live (path B), the non‑negotiables

- **Stamped "not money"** everywhere — no premine, no sale, no yield, no price talk.
- **Hardened first** — a production node with real proof‑of‑work and a security review; the
  MODEL is a research microscope, not a validator for value.
- **Distinct identity** — its own genesis/magic/ports; **units are not satoshis**; no inherited
  balances. (Already true of the X‑chains by construction.)
- **Candidate framing intact** — measured fidelity, never "the real Bitcoin."

## What we do NOT do

- ❌ Present a MODEL as money, or imply investment value.
- ❌ Claim either reconstruction **is** "the real Bitcoin" (identity is convention).
- ❌ Claim to have made a network permanent (only adoption can).
- ❌ Ship path B without hardening + the "not money" framing.

## The through‑line

This is [`THESIS.md`](THESIS.md) and [`DEFINITIONAL_FIDELITY.md`](DEFINITIONAL_FIDELITY.md) put
into action: **put the origin, seen clearly and measurably, on the table — as candidates — and
leave adoption to the world.** We can make them *permanent artifacts* and *measured candidates*
today; a *permanent network* is something only the world can choose to build on top of what we
release. Our part is to release it honestly. The rest is not ours to decide.
