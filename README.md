# Original Bitcoin Laboratory

Evidence-first reconstruction and conformance study of the earliest Bitcoin code,
split into two **self-contained** editions:

| Edition | Repo | Source basis |
|---|---|---|
| **Pre-Genesis** (`OBL-NOV08`) | `pre-genesis/` | November 15, 2008 pre-release source witness |
| **Genesis** (`OBL-JAN09`) | `genesis/` | January 2009 Bitcoin v0.1.0 — first public release |

Each edition is an **independent git repository**: independently fetched,
verified, built, and publishable under `github.com/original-bitcoin-laboratory`.

This `common/` folder is its **own** (umbrella) repository. It holds material
shared across both editions:

- `ROADMAP.md` — the R0–R9 program spanning both editions.
- `conformance/` — future cross-edition and descendant (BTC / BCH / BSV)
  comparison work, produced only after both original profiles are frozen.
- The design log and original bootstrap archive live one level up in
  `../../context/`.

## Layout

```text
original-bitcoin-laboratory/        (container — not a repo)
├── context/                        design log + original bootstrap zip
└── lab/
    ├── common/                     GIT REPO — umbrella (this folder)
    ├── pre-genesis/                GIT REPO — OBL-NOV08
    └── genesis/                    GIT REPO — OBL-JAN09
```

## Provenance & licensing

Canonical archive bytes are fetched from the Nakamoto Institute and verified by
hash inside each repo; they are never committed and never edited. Original Bitcoin
source retains Satoshi Nakamoto's 2009 MIT notices. New laboratory material is
MIT © 2026 Parth Mauria Saxena. The only external dependency of either repo is the
Nakamoto Institute code archive (and bitcoin.org for the whitepaper, captured
locally under `provenance/`).
