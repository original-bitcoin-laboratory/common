# What "Bitcoin" is — a definition from the artifacts

Everyone says the word; people mean different things by it. This resolves the ambiguity from
first principles, grounded only in the lab's hash‑verified origin artifacts (the whitepaper
and the v0.1.0 reference implementation). The result is a **universal definitional core**
everyone already shares, plus a **precise vocabulary** that makes the *remaining* disagreement
locatable instead of invisible. In the discipline of [`THESIS.md`](THESIS.md) and
[`CLAIMS.md`](CLAIMS.md), it states what is *definitively* answerable and what is
*irreducibly conventional* — and does not pretend the second is the first.

## 1. What the origin actually names (evidence first)

Checked against the archived whitepaper PDF:

- **"Bitcoin" occurs exactly twice** — the **title** ("Bitcoin: A Peer‑to‑Peer Electronic
  Cash System") and the URL **`www.bitcoin.org`**. **Zero times in the body as a noun.**
- The body names a **system** ("electronic cash system") and defines its **unit** as a
  **"coin"** — *"We define an electronic coin as a chain of digital signatures"* ("coin"
  appears ~10×).
- **"satoshi" never appears as a unit** — the sole occurrence is the **author's name**.

So at the origin, **"Bitcoin" is the name of the *system/project*, not of a thing you hold.**
The unit is the *coin*. Neither "a bitcoin" (the asset) nor "a satoshi" (the sub‑unit) is
reified in the paper at all. The ambiguity people feel is therefore not sloppiness — it is
that a single project name later got stretched across several distinct *kinds* of object.

## 2. The seven senses of "Bitcoin"

The one word points at seven ontologically different objects; speakers silently pick one:

| # | Sense | Kind of thing | How many? |
|---|---|---|---|
| 1 | **Design / concept** — the whitepaper's system | abstract *type* | one |
| 2 | **Reference implementation** — the v0.1.0 code | concrete artifact | one |
| 3 | **Protocol / consensus rules** | abstract specification | one origin, many since |
| 4 | **Network** — a live P2P system of nodes | concrete, dynamic | **many** (BTC, BCH, BSV, XEC, …) |
| 5 | **Ledger / chain** — the block data itself | concrete data | **many** |
| 6 | **Unit / asset** — "a bitcoin" (the priced thing) | ledger‑relative quantity | many |
| 7 | **Genus / lineage** — the whole descendant family | category | one |

"BTC vs BSV" is a **senses‑4/5** disagreement. A regulation's "Bitcoin" is almost always
**sense 6** (the asset) on an *unspecified* **sense‑4** network — which is exactly why such
text is under‑specified: it names a genus/asset without pinning the ledger.

## 3. Why "which chain is *the* Bitcoin" has no factual answer

BTC, BCH, BSV, XEC share **one genesis block and one history up to their fork points** — they
are **branches of a single tree**. (LTC/DOGE differ: they have their **own** genesis blocks —
separate *instances of the design*, not branches of the instance.)

When one continuant **branches into two**, identity logic is unambiguous: *neither branch is
uniquely "the original"; the original is their common ancestor, and each branch is a distinct
continuant.* It is the twin / ship‑of‑Theseus result. Therefore **"which network really is
Bitcoin" is not a fact recoverable from the artifacts** — it is fixed only by social/market
convention. A claim that one branch *is* the real Bitcoin is rhetorical, not derivable. (This
is why this lab privileges no chain: neutrality is the *correct ontology*, not mere manners.)

## 4. The invariant — what Bitcoin definitively **is**

Though "which branch" has no factual answer, exactly one object is shared by **every**
claimant, cryptographically unique and unforgeable, and traced back to by all of them: the
**genesis block** (`000000000019d668…` — reproduced three independent ways by the lab: the
unmodified 2009 binary, the C++/OpenSSL port, and the Python model). Hence:

> **Bitcoin is the specific system Satoshi originated: the design fixed by the whitepaper plus
> the mechanism fixed by the reference implementation, uniquely anchored by the genesis block.
> Everything called "Bitcoin" downstream is a *continuation of that one rooted instance.***

Two precise senses fall out of this:

- **Bitcoin (type)** — the design. LTC/DOGE *instantiate* it: they are *of the genus*, not
  the instance.
- **Bitcoin (the instance)** — the genesis‑rooted system: the shared root of BTC/BCH/BSV/XEC.
  Post‑fork it is a *branching continuant*; no single branch is uniquely it.

The **universal thing Bitcoin *is*** is the **origin, anchored at genesis** — the only
non‑arbitrary fixed point, and the thing *even two disagreeing people already share*: a
BTC‑holder and a BSV‑holder both trace to the same genesis block.

### Why the instance is anchored at January (genesis), not the November pre‑release

This is **not** a preference for one codebase over another — it is *when the thing being
defined began to exist.* The genesis block is a **January (v0.1.0)** artifact; the pre‑release
is, by name and by fact, **pre‑genesis** — no genesis block was mined from it, no chain
launched, no ledger ran, no balance ever existed under it. An instance cannot be defined from
an artifact that **predates the instance.** So the two artifacts anchor **different senses**:

- **Type / design** (the abstract concept) → the **whitepaper + the pre‑release**, the earlier
  sources; here November is legitimately the *more original*.
- **Instance** (the running system — genesis, ledger, network, unit, asset) → **v0.1.0's
  genesis**, *necessarily*, because that is when it came into being.

The pre‑release is *Bitcoin's design before launch*; it is not, and cannot be, the launched
instance. Any sense of "Bitcoin" that has a genesis, a ledger, a network, or a unit is a
January fact — by existence, not by ranking.

## 5. What a "satoshi" is

The code names **neither** "satoshi" **nor** "bitcoin." What
`static const int64 COIN = 100000000;` (v0.1.0 `main.h:18`) fixes is **structure, not names**:

- `nValue` is an `int64`, so value is carried in **indivisible integer units** — there is no
  fractional value in the protocol;
- `COIN` names a single **aggregate**: 1 `COIN` = 100,000,000 of those atomic units.

The words are conventions layered on afterward (the code's own identifier is `COIN`; "satoshi"
is a ~2010–11 coinage). So, precisely:

> **A satoshi is the atomic, indivisible integer unit of the ledger's value field (`nValue`); a
> bitcoin is 10⁸ of them (the code's `COIN`).** The *referents* — the atomic unit, the
> aggregate, and the 10⁸ ratio — are artifact‑fixed; the *names* are convention.

This 10⁸ is specifically a **January (v0.1.0)** fact: November's `COIN` was **10⁶**, so the
atomic‑unit‑to‑coin ratio there was 10⁻⁶. **The "10⁻⁸ satoshi" does not exist in the
pre‑release at all** — it is a different unit. Far from an arbitrary preference for v0.1.0, the
satoshi is *forced* to be a genesis‑era fact: it is the only place it exists (see §4's
type‑vs‑instance note).

The symmetry with §4: this atomic‑unit + 10⁸‑ratio structure is **invariant across every
genesis‑sharing fork** (all inherited it from the origin); only a specific *balance* is
ledger‑relative. **The structure is universal; the instance is contingent.**

## 6. Using the vocabulary — the two examples, resolved

- **"Bitcoin" (BTC‑person) vs "Bitcoin" (BSV‑person).** They *agree* on senses 1–3 and on the
  instance‑root (both trace to genesis `000000000019d668…`); they *differ* only on sense 4/5
  (which post‑fork branch). The framework converts "you're wrong about what Bitcoin is" into
  the precise, tractable "we mean the same root, different branch."
- **A regulation that says "Bitcoin."** It is pointing at sense 6 (the asset) on an
  unspecified sense‑4 network — usually the highest‑market‑cap continuation by social default.
  Naming that gap *is* the finding: the text under‑specifies its own referent.

## 7. What is definitive, and what is not

- **Definitive** (artifact‑fixed, shared by all): the one universal thing Bitcoin *is* — the
  genesis‑rooted origin (whitepaper design + reference implementation) — and what a satoshi
  *is* — the 10⁻⁸ atomic ledger unit.
- **Not definitive, and honestly so**: which *live network* deserves the bare word. That is a
  real branching in the world; no definition collapses it, and pretending to would be the very
  overreach [`THESIS.md`](THESIS.md) disowns.

That distinction *is* the answer: a universal definitional core everyone shares, plus a
vocabulary that makes the residual, genuinely‑social disagreement **locatable** rather than a
source of people talking past each other while using the same word.

## Boundary

Grounded only in the two hash‑verified origin artifacts ([`AUTHORITY.md`](AUTHORITY.md)); the
whitepaper occurrence counts are checkable against the archived PDF, and `COIN = 100000000` is
in the v0.1.0 source the lab reproduces. No external references, no chain privileged, no claim
about value or which network anyone *should* mean. Companion to
[`FINDINGS.md`](FINDINGS.md) (results), [`CLAIMS.md`](CLAIMS.md) (assertions), and
[`THESIS.md`](THESIS.md) (argument).
