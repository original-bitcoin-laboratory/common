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

> **Bitcoin is, fundamentally, the *design* Satoshi specified — the whitepaper's system,
> realised in the reference implementation. Its everyday referent — the running system with a
> ledger, networks, and a unit — is the family that shares the genesis block
> `000000000019d668…`; *which* member of that family, and *which* numbers, count as "Bitcoin"
> is convention.**

Two precise senses fall out of this:

- **Bitcoin (type)** — the design. LTC/DOGE *instantiate* it: they are *of the genus*, not
  the instance.
- **Bitcoin (the instance)** — the genesis‑rooted system: the shared root of BTC/BCH/BSV/XEC.
  Post‑fork it is a *branching continuant*; no single branch is uniquely it.

### The design anchor is fundamental; the instance anchor is a convention

The **fundamental invariant** — shared *without appeal to any convention* — is the **design**:
the pre‑release, v0.1.0, and every descendant realise it. The **instance** has a shared root
(v0.1.0's genesis) that the BTC/BCH/BSV/XEC family traces to, and that is what most people
mean. But two honest caveats keep this from privileging a codebase by fiat:

- Treating **v0.1.0's launch** as "the" Bitcoin — rather than the design, or another launch —
  is itself a **convention**, not a derived fact. Even "pre‑genesis" for the November code is
  defined *relative to* that popularly‑recognised block; it is a label, not neutral ground, so
  it cannot *prove* the January anchor without circularity.
- What is strictly **factual** is narrower: the pre‑release launched no running chain, and the
  network the world calls Bitcoin began in January 2009. "The pre‑release is Bitcoin not yet
  born" is a perfectly coherent reading — it simply places the design before the instance.

So the split is by sense: **type/design → the whitepaper (+ the earlier pre‑release), the
non‑circular invariant; instance → the conventionally‑recognised genesis‑rooted family.** The
design is the fixed point everyone shares; *which launch — and, as §5 shows, which numbers —
count as "Bitcoin" is convention.*

## 5. What a "satoshi" is

The code names **neither** "satoshi" **nor** "bitcoin." What
`static const int64 COIN = 100000000;` (v0.1.0 `main.h:18`) fixes is **structure, not names**:

- `nValue` is an `int64`, so value is carried in **indivisible integer units** — there is no
  fractional value in the protocol;
- `COIN` names a single **aggregate**: 1 `COIN` = 100,000,000 of those atomic units.

The words are conventions layered on afterward (the code's own identifier is `COIN`; "satoshi"
is a ~2010–11 coinage). So, precisely:

> **A satoshi is the atomic, indivisible integer unit of the ledger's value field (`nValue`).**
> That there *is* such an atomic unit, and an aggregate `COIN` above it, is artifact‑fixed and
> common to both codebases. **The ratio between them is not.**

The ratio is **convention‑relative**, and the pre‑release is the live proof: nov08's `COIN` is
**10⁶**, v0.1.0's is **10⁸**. So *"1 bitcoin = 10⁸ satoshi"* holds **only** if you anchor the
unit to v0.1.0; anchor it to the pre‑release and *"1 bitcoin = 10⁶ satoshi"* is equally,
internally correct. **There is no non‑circular way to prove 10⁸ over 10⁶** — the only thing
that selects 10⁸ is that the world's "bitcoin" is the v0.1.0‑launched instance, which is a
*social/historical convention* (the very "popularly known" one), not a fact derivable from the
design.

So, precisely: the **atomic‑unit structure is universal** (both codebases have one); the
**bitcoin↔satoshi ratio — and the names — are convention.** Across the *v0.1.0‑descended
family* (BTC/BCH/BSV/XEC) the 10⁸ ratio is shared, because all inherited it from that one
launch; step back to the pre‑release and it is not. **The satoshi is pinned only once you
*choose* an anchor — and choosing is convention, not derivation.**

## 6. Using the vocabulary — the two examples, resolved

- **"Bitcoin" (BTC‑person) vs "Bitcoin" (BSV‑person).** They *agree* on senses 1–3 and on the
  instance‑root (both trace to genesis `000000000019d668…`); they *differ* only on sense 4/5
  (which post‑fork branch). The framework converts "you're wrong about what Bitcoin is" into
  the precise, tractable "we mean the same root, different branch."
- **A regulation that says "Bitcoin."** It is pointing at sense 6 (the asset) on an
  unspecified sense‑4 network — usually the highest‑market‑cap continuation by social default.
  Naming that gap *is* the finding: the text under‑specifies its own referent.

## 7. What is definitive, and what is not

- **Definitive** (non‑circular, needs no convention): the **design** (the whitepaper's
  concept), which every version and descendant realises; and the **bare artifact facts** — the
  pre‑release's `COIN` is 10⁶, v0.1.0's is 10⁸, v0.1.0's genesis is `000000000019d668…`, and
  each ledger has an atomic indivisible unit.
- **Not definitive, and honestly so**: which *live network* deserves the bare word; which
  *codebase* defines "the" unit; and therefore the **bitcoin↔satoshi ratio itself** (10⁶ vs
  10⁸) and whether "Bitcoin" primarily means the design or a particular instance. These are
  real branchings and conventions in the world; no definition collapses them, and pretending to
  would be the overreach [`THESIS.md`](THESIS.md) disowns.

That distinction *is* the answer: a **non‑circular core** (the design + the artifact facts)
everyone can share, plus a vocabulary that makes the residual, genuinely‑conventional choices —
the network, the unit anchor, the ratio — **locatable and explicit** rather than smuggled in as
if they were fundamental. The reflex to treat "1 satoshi = 10⁻⁸ bitcoin" as bedrock is exactly
such a smuggle: it is true *by convention* (the v0.1.0 instance), not by derivation.

## 8. Can the definition be made *unique*? — the strongest counter‑questions

Three natural pushes try to escape convention and pin a single, beyond‑doubt "Bitcoin." Each
fails, and the primary artifacts say why.

**Does the whitepaper itself define a selection principle?** It defines **exactly one** —
proof‑of‑work: *"the longest chain, which has the greatest proof‑of‑work effort invested in
it"* is treated as correct. But that selects among competing **block‑histories under one fixed
rule set** (so double‑spends can't stand); it is **rule‑relative** — a node counts only work
valid under *its own* rules. At a **hard fork**, where the rule sets themselves diverge, the two
chains are mutually invalid and share no metric, so longest‑chain **cannot say which fork is
"Bitcoin."** The paper has a selector for the *ledger tip*, and **none for identity.**
("Most cumulative work across forks" is an *extrapolation* of the rule past where the paper
applies it — a convention in whitepaper clothing.)

**Is there a truer definition to switch to?** The two properties you'd want — *beyond doubt*
and *uniquely identifying* — pull in **opposite** directions:

| Definition (thin → thick) | Beyond doubt? | Uniquely identifies? |
|---|---|---|
| the **invention** — ledger consensus via PoW, no trusted third party | ✅ maximally | ❌ every PoW chain satisfies it |
| the **whitepaper design** | ✅ | ❌ BTC/BCH/BSV all satisfy it |
| **+ v0.1.0 reference** (Script set, `COIN`, consensus path) | ⚠️ carries contingent choices | ⚠️ discriminates, but the choices are contestable |
| the **genesis‑rooted chain** | ⚠️ | ❌ forks → which branch? |
| **most cumulative PoW / most‑adopted** | ❌ convention, time‑varying | ✅ but by popularity |

**Thin = beyond doubt but many satisfiers; thick = discriminating but contestable.** No
definition maximizes both — that is the whole trade, not a gap to close.

**So can "Bitcoin" be defined *and* uniquely pointed to, beyond doubt?** Two walls, both
*features* of the situation:

- **Uniqueness wall** — a definition is a *predicate*; predicates have **many satisfiers**; the
  artifacts contain **no identity‑selector** (above). So uniqueness of referent is *always*
  convention.
- **Completeness wall** — the design is an **incomplete specification** (it underdetermines
  Script, the unit, addresses, block size, the exact hashing). To *run*, an implementation must
  add contestable choices — so *"implement the design, nothing more"* is **unsatisfiable**: the
  moment it runs it is an *instance carrying choices*, not the design itself.

**The verdict, then, on "define it and point to / build it, beyond doubt":**

- **Beyond doubt is reachable** for the **design** (the type) and for **unique origin
  artifacts** — the hash‑verified v0.1.0 source and the genesis block `000000000019d668…`. You
  *can* point, uniquely and beyond doubt, to **the origin.**
- **Beyond doubt is *not* reachable** for a unique *"the Bitcoin"* (the ongoing network), nor
  for a fundamental *bitcoin↔satoshi ratio* — the whitepaper defines no identity‑selector, and
  a runnable build cannot be "only the design."

So the honest terminus: fix the definition at the design (beyond doubt, non‑unique), point
uniquely to the **origin** (beyond doubt), and — since no build can be "only the design" —
realise it as an instance whose every added choice is **maximally faithful to the origin and
disclosed as a choice** (see [`DEFINITIONAL_FIDELITY.md`](DEFINITIONAL_FIDELITY.md)). The
demand for a unique, beyond‑doubt "the Bitcoin" is not met — and the whitepaper is the reason.

## Boundary

Grounded only in the two hash‑verified origin artifacts ([`AUTHORITY.md`](AUTHORITY.md)); the
whitepaper occurrence counts are checkable against the archived PDF, and `COIN = 100000000` is
in the v0.1.0 source the lab reproduces. No external references, no chain privileged, no claim
about value or which network anyone *should* mean. Companion to
[`FINDINGS.md`](FINDINGS.md) (results), [`CLAIMS.md`](CLAIMS.md) (assertions), and
[`THESIS.md`](THESIS.md) (argument).
