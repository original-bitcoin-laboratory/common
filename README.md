# Original Bitcoin Laboratory

**An evidence‑first, *executable* reconstruction and neutral conformance study of the
earliest Bitcoin** — the November 2008 pre‑release and the January 2009 v0.1.0 — built
entirely from two hash‑verified archives, with nothing disabled and no chain privileged.

Most of Bitcoin's origin story is *prose*. This lab makes the earliest code **run**, and
lets you re‑derive it from scratch on your own machine.

**Start with the founding question:** [`WHAT_IS_BITCOIN.md`](WHAT_IS_BITCOIN.md) — a
definition of what "Bitcoin" (and a "satoshi") *is*, argued from the artifacts.

> **What it honestly claims** (the argument + what it refuses to claim in [`THESIS.md`](THESIS.md);
> full statement + hedges in [`CLAIMS.md`](CLAIMS.md)):
> the most complete executable reconstruction and neutral conformance study of the earliest
> Bitcoin *that we're aware of* — running the **complete original Script vocabulary with
> nothing disabled**, **differential‑verified against the unmodified released binary**, and
> measuring descendants' divergence from the origin with executed evidence under one neutral
> method.
>
> It is a **research microscope, not a coin.** The experimental chains are stamped *"not
> money"*: no premine, no sale, no promises.

## What we found

- **v0.1 was a general financial‑predicate engine, not "just money."** The full 106‑opcode
  Script, m‑of‑n escrow, hash‑locks, assurance/crowdfunding contracts — *and a shipped
  decentralized marketplace with a web‑of‑trust reputation.* All executed. (Satoshi, Jan 10
  2009: *"the network infrastructure can support a full range of escrow transactions and
  contracts."*)
- **The monetary constitution was set in January, not November.** Subsidy 100→50, halving
  100k→210k, block time 15→10 min, base unit `COIN` 1e6→1e8 — **the "satoshi" is
  genesis‑born**; November is denominated in coins/cents with a fixed `1*CENT` fee.
- **November's proof‑of‑work is a *different algorithm*** — `nBits` = leading‑zero *bits*,
  `MINPROOFOFWORK=20` ("ridiculously easy for testing," Satoshi's own comment), a primitive
  ±1‑bit retarget — not January's compact‑target + proportional retarget.
- **The genesis reproduces three independent ways** — the unmodified 2009 binary (run live),
  our C++ port, and our Python model all yield `000000000019d668…`.
- **Descendant divergence, measured neutrally + executed:** BTC disabled the broad vocabulary
  ~2010; BCH restored a subset; BSV restored nearly all (minus `OP_2MUL`/`OP_2DIV`). Only the
  origin carries the *literal* complete set — and v0.1 disabled exactly **one** functional
  opcode (`OP_NOTEQUAL`).
- **The origin was a working engine with the guardrails not yet installed** — no value‑overflow
  check, no block‑size cap, no script resource limits (all added in 2010). The two sharpest are
  *executed* as accept/reject divergences. Plus the attack‑surface maps: OpenSSL and Berkeley DB
  each once forked the chain; ECDSA/secp256k1 is the shared post‑quantum exposure.

**→ The full synthesis is [`FINDINGS.md`](FINDINGS.md)** — one page tying every result together.

## What's built (all headless, all tested)

The script engine (MODEL + C++ PORT), sighash, `OP_CHECKSIG`/`CHECKMULTISIG` on real
secp256k1, native instruments (escrow / hash‑lock / assurance), a UTXO `ConnectInputs`/
`ConnectBlock` ledger, a wallet, the P2P wire + chain sync, persistence, the neutral
6‑chain descendant matrix (BTC/LTC/DOGE + BSV **executed**, BCH/XEC execution‑bounded), the
commerce subsystem (signed listings + atoms reputation), a script **debugger**, and a
**full‑stack console** that drives it all.
Plus two live counterfactual networks — **NOV08‑X** and **JAN09‑X** — that mine, sync, and
transact contracts BTC can't express, each with the full vocabulary and nothing disabled.

One command re‑proves everything:

```bash
python genesis/scripts/reproduce.py        # every suite + regenerated artifacts
python genesis/scripts/verify_genesis.py   # both experimental genesis blocks re-derive from source
```

## Structure — three self‑contained repos

| Repo | What |
|---|---|
| [`common/`](.) (this one) | umbrella: [`WHAT_IS_BITCOIN.md`](WHAT_IS_BITCOIN.md), [`THESIS.md`](THESIS.md), [`FINDINGS.md`](FINDINGS.md), [`CLAIMS.md`](CLAIMS.md), [`AUTHORITY.md`](AUTHORITY.md), [`ROADMAP.md`](ROADMAP.md), the conformance/attack‑surface matrices, the NOV08‑X design ledger |
| `pre-genesis/` | **OBL‑NOV08** — the Nov 15 2008 pre‑release witness + inventory |
| `genesis/` | **OBL‑JAN09** — Bitcoin v0.1.0: the full executable reconstruction + derivatives |

```text
original-bitcoin-laboratory/     (container — not a repo)
└── lab/{ common/  pre-genesis/  genesis/ }     three independent git repos
```

## Provenance & method

Authority is **only** the two hash‑verified archives + the whitepaper
([`AUTHORITY.md`](AUTHORITY.md)); everything else — SNI‑as‑curator, mirrors, forks,
v0.1.3, BTC/BCH/BSV docs — is named *out of authority*. Canonical archive bytes are
fetched from the Nakamoto Institute, verified by hash, never committed, never edited. The
genesis blocks of the experimental chains are **deterministic** — anyone can regenerate the
identical block forever from source, no live node required. *Use the canonical archives to
authenticate; use the code to execute; measure everyone else neutrally, from the origin.*

## Licensing

Original Bitcoin source retains Satoshi Nakamoto's 2009 MIT notices. New laboratory
material is MIT © 2026 Parth Mauria Saxena.
