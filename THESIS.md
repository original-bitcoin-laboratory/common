# Thesis & scope — what this lab argues, and what it refuses to

`FINDINGS.md` is *what we found*; `CLAIMS.md` is *what we can assert about the artifact*.
This is the **argument** those support — stated at full strength, and, in the same
hostile‑reader discipline, bounded by the stronger claims we deliberately **do not** make.
The bounded version is the stronger one: it survives the skeptic who would otherwise rebut
us with our *own* executed evidence.

## The thesis (defensible)

> **Bitcoin's origin was more complete and more capable than the dominant narrative credits
> — a general financial‑predicate engine from day one, not a bare payment token that grew
> programmability later. Studying it neutrally from ground zero (a) recovers that lost
> completeness, (b) maps precisely where every later chain diverged from it, and (c) shows
> the early maturation was real safety engineering, not decoration. A minimal,
> dependency‑light reconstruction makes the origin's design legible and auditable — a
> reference, not a replacement.**

## Two layers, kept strictly separate

The whole argument turns on not conflating these:

- **The idea / design — essentially complete at the origin.** The whitepaper's insight (a
  proof‑of‑work‑secured, incentive‑aligned timechain that prevents double‑spending without a
  trusted third party) needed nothing invented "on top" to be whole; and v0.1 already shipped
  the full Script vocabulary, escrow, hash‑locks, assurance contracts, a UTXO state machine,
  and a marketplace. *This is the part the later narrative forgot, and the part we recover.*
- **The safe implementation at scale — not complete at the origin.** Our own
  [`conformance/CONSENSUS_SURFACE.md`](conformance/CONSENSUS_SURFACE.md) shows the origin was
  "a working consensus engine with the guardrails not yet installed": no `MoneyRange` (the
  value‑overflow the origin *accepts*, which we execute), no block‑size cap, no script
  resource limits, OpenSSL and Berkeley DB inside consensus (each caused a real chain split),
  bare‑P2PK keys exposed on‑chain. The 2010‑onward hardening fixed genuine holes.

**The origin was more complete in *design* and less safe in *implementation*.** Both are true;
neither cancels the other. Every over‑reach below comes from collapsing these two layers.

## Valuelessness is load‑bearing — and what that means for the live reconstructions

The second layer has a direct consequence. The origin ran **without** `MoneyRange`, without a
block‑size cap, without script resource limits — guardrails added from 2010 for real reasons.
Reconstructing the origin faithfully therefore means running **without** those guardrails, and that
is defensible **only** while the units carry no value: *"nothing disabled" is safe precisely because
there is nothing of value to steal.* Attach value and the same unguarded rules become a liability you
must defend — forcing the modern guardrails back in, at which point **it is no longer the origin.**
So the valuelessness is not a disclaimer bolted on; it is the **condition under which a faithful,
unguarded reconstruction can exist at all** — no premine, no sale, no assigned value, no solicited
market, stamped in the coinbase.

The two live reconstructions — **NOV08‑X** and **JAN09‑X** — are launched, operational, and joinable:
always‑on anchors, two independent client implementations (Python and Rust), reproducible genesis,
signed releases, a seed, one‑command Docker. Anyone can run a node and join. On the axis the lab
measures — **origin‑fidelity** — they are the reference‑closest (from the v0.1.0 origin, `JAN09‑X` = 1;
from the Nov‑2008 origin, `NOV08‑X` = 0), by construction at least as faithful to the earliest Bitcoin
as any live chain.

What they are **not** — and do not try to become — is "equal to" BTC/BCH/BSV as *secured, adopted,
monetary* networks. Those chains lead on hashpower, market, and adoption because they carry value;
this project carries none by design, and competing there would require attaching value, which un‑does
the reconstruction. Where the lab is at‑par‑or‑better, and where it puts its effort, is the axis it
actually measures: **fidelity to the origin, reproducibility, and honest neutrality** — kept
razor‑sharp. Participation is invited, not manufactured: anyone may join; whether others do is the
community's part, not something the lab stages. Best‑in‑class at *fidelity and honesty* is the real,
durable win — "the next Bitcoin" is the claim we keep declining.

## On the origin artifacts — the "most fundamental primitive" question, honestly

Anchoring on the **Nov 2008 pre‑release source** and the **whitepaper** is a *defensible
choice of the earliest Satoshi artifacts* — it is **not** a proven unique bottom, and we do
not pretend it is:

- **The pre‑release is a partial snapshot** (ledger + net loop; no Script engine, keys,
  crypto, DB, or serialization). A full system **cannot** be reconstructed from it alone —
  the runnable substrate is necessarily imported from v0.1.0 (classed `J‑DONOR`). So the
  reconstruction is honestly *"v0.1‑substrate under the pre‑release's constitution,"* not
  *"the pre‑release, made whole from itself."*
- **v0.1.0 is the better‑provenanced origin** — it carries the Hal Finney hash chain, it
  runs, and it is our behavioural oracle. Epistemically it is a *stronger* anchor than the
  pre‑release, not a weaker one.
- **More fundamental primitives exist upstream** — the whitepaper itself cites them
  (Hashcash, Merkle trees, b‑money, the Haber–Stornetta timestamp chain). Bitcoin is the
  *synthesis*; there is no single bottom rock. We remain open to digging further upstream.
- **The whitepaper is a technical design document, not a legal one.** Neither artifact is a
  legal definition; we do not frame them that way.

## What we deliberately do NOT claim

- ❌ **"The original is the most secure / longest‑lasting Bitcoin."** Contradicted by our own
  executed evidence — v0.1's `CheckTransaction` accepts the 184‑billion‑BTC overflow. A
  version without those bounds is *less* safe, not more.
- ❌ **"Going back to the original yields a more secure system."** A system that is minimal
  *and* secure would be **origin design + the essential bounds** — which is no longer the
  pure original, but the original with its lessons learned. That is the honest frontier; we
  don't disguise it as "just the original."
- ❌ **"The world was distracted and evaded the real solution."** The record shows engineers
  competently closing real holes the origin left open. Presuming blindness or evasion is
  unfalsifiable, trivially rebutted, and trades away the neutrality that is this lab's moat.
- ❌ **"The pre‑release + whitepaper are sufficient to reconstruct a full Bitcoin."** They are
  not — a partial snapshot and a design paper with a thin intersection.
- ❌ **Any unfalsifiable superlative** ("the only," "the most secure in the world," "what
  everyone missed"). Precision is the moat; superlatives invite easy rebuttal and read as
  marketing.
- ❌ **"These networks are, or should become, at par with BTC/BCH/BSV as secured/adopted money."**
  They carry no value by design; matching value‑driven chains on hashpower or market would require
  attaching value, which re‑forces the guardrails and un‑does the reconstruction (see above). The
  lab competes on fidelity, reproducibility, and neutrality — never on metrics that presuppose value.

## What we DO put out

The **evidence** (hash‑verified archives, executed reconstruction, reproducible recipe), the
**neutral divergence map** across the lineage, and a **minimal reference reconstruction** that
makes the origin's design auditable. We argue the origin was more capable than remembered and
we show exactly where the tree grew from it — no more, no less. Whether the world finds a
minimal, ground‑up design worth choosing is the community's call, not ours; our part is to put
the origin, seen clearly and checkably, on the table.

## Why the bounded thesis is the strong one

The same logic as [`CLAIMS.md`](CLAIMS.md): a skeptic cannot rebut *"the origin was more
complete than remembered, here is the executed proof, and here is where each chain diverged."*
They *can* demolish *"the original is the most secure Bitcoin"* in one line — by pointing at
the overflow bug **we executed ourselves.** We refuse the claim that refutes itself, and keep
the one that doesn't.
