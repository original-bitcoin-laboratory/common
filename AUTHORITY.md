# Authority & scope

This project deliberately anchors to **ground zero** — the earliest hash-verifiable
Satoshi artifacts — and treats everything else as discovery, mirror, or derivative.
The point is to cut through the ecosystem's noise: authority attaches to *exact
bytes with custody*, never to a website's or repository's reputation.

## The authority set (all of it)

| Artifact | Profile | sha256 | Custody |
|---|---|---|---|
| `bitcoin-nov08.rar` | OBL-NOV08 | (md5/sha1 per SNI) | earliest preserved pre-release package |
| `bitcoin-0.1.0.rar` | OBL-JAN09 | `8b17eb9a…` | 2012 Hal Finney recovery |
| `bitcoin-0.1.0.tgz` | OBL-JAN09 | `ce9da465…` | 2012 Hal Finney recovery (same source tree) |
| `bitcoin.pdf` (whitepaper, **2009 revision**) | — | `b1674191…` | bitcoin.org, captured locally — **see the caveat below** |

Nothing else is authority. All four are fetched from the Nakamoto Institute CDN
and **independently hash-verified in-repo** (see each edition's
`manifests/EXPECTED_CHECKSUMS.json`); the JAN09 `.rar`↔`.tgz` trees were confirmed
**byte-identical**, giving independent-witness confidence without trusting any
third-party mirror.

### The whitepaper is a weaker authority than the archives, and here is why

The two code archives are authority in the full sense: exact bytes, recovered custody, independently
hash-verified, and the JAN09 `.rar` and `.tgz` trees confirmed byte-identical to each other.

`bitcoin.pdf` is not in that class, and this table used to imply it was.

Its bytes are solid — `b1674191…` is served identically by **five independent sources** (bitcoin.org
today, the Nakamoto Institute, two Internet Archive captures, and this repository). What it is *not*
is the document announced on 31 October 2008:

- its own `/Info` dictionary reads `/CreationDate D:20090324113315-06'00'` — **created 24 March
  2009**, 144 days after the announcement and 80 after the genesis;
- the file behind the October 2008 link is preserved in **no** archive. The Internet Archive's
  earliest capture is 2010-07-04; Common Crawl's `CC-MAIN-2008-2009` index — a crawl that ran during
  the window — never visited the domain; the announcement **linked** the paper rather than attaching
  it, so no mail archive holds one; and no hash of it was ever published;
- and the text **demonstrably changed**. The abstract archived on bitcoin.org on **2009-01-31** still
  read *"without the burdens of going through financial institutions"* and *"as long as honest nodes
  control the most CPU power"*. By **2009-03-03** the site read *"a majority of CPU power is
  controlled by nodes that are not cooperating to attack the network"*. Both states are dated by the
  Internet Archive, not by us.

So this artifact is authority for **what bitcoin.org has served since 2009**, and for the design as
Satoshi last stated it. It is *not* a witness to October 2008, and nothing in this project should
lean on it as one. The October text that does survive is the abstract quoted inline in the
announcement, timestamped by the `cryptography@metzdowd.com` list server.

The distinction matters because it is the same one the whole method turns on: an artifact's authority
comes from what anchors it, not from how canonical it has become through repetition.

Note the one asymmetry we preserve: **`bitcoin-nov08.tgz` is SNI-compressed**, a
convenience companion — *not* an independently recovered original. It is recorded
as `"SNI-compressed companion"` and used only for browsing, never as a second
witness. (JAN09 `.rar` and `.tgz`, by contrast, are both recovered packages.)

## Out of authority (named, and why)

These are useful for *finding* and *cross-checking*, and are explicitly **not**
part of the evidence base:

| Resource | Role here | Why not authority |
|---|---|---|
| Satoshi Nakamoto Institute (site/GitHub) | curator / acquisition index | mirror + rendering; we verify each artifact ourselves |
| CitadelXBT | correspondence discovery corpus | its "verified"/annotation labels are editorial; schema inconsistent |
| `trottier/original-bitcoin` | adjacent executable | it is **v0.1.3**, not the v0.1.0 baseline |
| `benjiqq/bitcoinArchive` | research mirror | copy of the same bytes (see verified mirror below) |
| `0xMagnuz/Bitcoin-v0.1` | browsing | **fork of** `benjiqq` — not independent |
| `Fiach-Dubh/*` | availability/redundancy | **forks of** `benjiqq` / `portlandhodl` |
| `portlandhodl/BitCoin-0.01` | candidate source checkout | single non-Satoshi commit; verify against archive before any use |
| SourceForge → Git (v0.1.5+) | later lineage | a *different* provenance regime (Evolution Track), not ground zero |
| bitaddress.org, UTXO Engineer | modern apps / BSV tooling | later software; comparison objects, not original evidence |

The only legitimate use of any mirror is **byte cross-checking against the
authority set**. A mirror is never promoted above "matches canonical".

## Evidence tiers

```text
Tier 0  Canonical raw artifacts      <- the authority set above (we hash + preserve)
Tier 1  Custody-bearing witnesses    <- Hal Finney recovery, release announcements
Tier 2  Verified mirrors             <- bytes proven == a Tier 0 artifact
Tier 3  Curated archives / indexes   <- SNI pages, Citadel JSON, GitHub collections
Tier 4  Interpretations / modern     <- Citadel annotations, BTC/BCH/BSV docs, ours
```

## Known mirror graph

```text
benjiqq/bitcoinArchive
├── 0xMagnuz/Bitcoin-v0.1        (fork)
└── Fiach-Dubh/bitcoinArchive    (fork)

portlandhodl/BitCoin-0.01
└── Fiach-Dubh/bitcoin-0.1.0     (fork)

SNI bitcoin-nov08.rar
└── SNI bitcoin-nov08.tgz        (SNI-compressed companion, not independent)
```

## Verified mirrors (Tier 2)

| Mirror | Artifact | sha256 | Result | Date |
|---|---|---|---|---|
| `benjiqq/bitcoinArchive` (`master`) | `bitcoin-0.1.0.tgz` | `ce9da465…` (2,935,983 B) | **== canonical** | 2026-07-26 |

Belt-and-suspenders confirmation only: it changes no authority — it shows an
independent GitHub archive carries the exact recovered bytes. We still fetch the
canonical archives from SNI and verify them ourselves.

## The stance, in one line

> Use mirrors/indexes to **discover**. Use the two hash-verified archives to
> **authenticate**. Use the canonical code to **execute**. Use the conformance
> lab to **conclude**. Nothing outside the authority set is ever cited as origin.
