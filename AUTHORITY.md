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
| `bitcoin.pdf` (whitepaper) | — | `b1674191…` | bitcoin.org, captured locally |

Nothing else is authority. All four are fetched from the Nakamoto Institute CDN
and **independently hash-verified in-repo** (see each edition's
`manifests/EXPECTED_CHECKSUMS.json`); the JAN09 `.rar`↔`.tgz` trees were confirmed
**byte-identical**, giving independent-witness confidence without trusting any
third-party mirror.

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
