# NOV08‑Full — the counterfactual completion (interpretation, walled off)

> **Read this warning first.** NOV08‑Full is **interpretation, not evidence.** It is a
> *counterfactual completion* of the November 2008 pre‑release toward a full financial
> system — it is **not recovered Satoshi code**, **not** "the hidden true Bitcoin",
> and **not** authoritative about what November *was*. Authority stops at the two
> hash‑verified archives (`../AUTHORITY.md`). Everything here is a disclosed design
> choice, labelled, and must never be presented as historical fact.

## Where it sits

Roadmap R8, build‑order step 7 (`DESIGN_LEDGER.md`) — considered **only after**
NOV08‑Minimal was complete, which it now is. NOV08‑Minimal executes November's
**constitution** (subsidy / PoW / retarget / coinbase / denomination — all **N‑ORIG**,
source‑anchored) on a reconstructed substrate, as a live two‑node network that
transacts. NOV08‑Full is the step from "the constitution runs" to "**the whole
financial machine runs on that constitution**."

## What NOV08‑Full *is*, concretely

It is the **assembled full‑capability system on November's rules** — and it already
has an executable form: `genesis/derivatives/console/` run under `Rules.load("nov08")`.
That session drives, on the November constitution:

- coinbase issuance (100 coins, the N‑ORIG schedule) + UTXO `ConnectBlock`;
- a wallet with real secp256k1 (`CreateTransaction` / `SignSignature`);
- **the full opcode vocabulary, nothing disabled** — incl. contracts BTC cannot run
  (an `OP_CAT` hash‑lock, created and spent);
- the **commerce layer** (signed `CProduct`/`CReview`, the atoms web‑of‑trust) that
  v0.1 itself shipped (R6);
- a deterministic evidence bundle.

So NOV08‑Full is not new *code* so much as the **honest wiring together** of the
lab's executed pieces under November's constitution — the answer to *"what complete
electronic‑cash system is latent in the last surviving pre‑genesis source state?"*

## The completion decisions (each disclosed, class NEW‑EXP / N‑IFACE)

November's 4 files define the ledger + net main loop and the monetary constitution,
but **not** Script, keys, storage, or commerce. NOV08‑Full supplies them, and every
choice is one of:

| Decision | Choice | Class | Why it's honest |
|---|---|---|---|
| Script engine | the lab's full‑vocabulary EvalScript (`../../genesis/derivatives/model`) | N‑IFACE | November *references* `CScript`/`OP_CHECKSIG`; we reconstruct from that interface, disabling nothing (Nov predates the Script file that disabled `OP_NOTEQUAL`) |
| keys / sighash | real secp256k1 + pre‑BIP143 SignatureHash | N‑IFACE | implied by `<pubkey> OP_CHECKSIG` |
| storage | `CDiskBlockIndex` model (`../../genesis/derivatives/persist`) | J‑DONOR | November is silent; January's form, labelled |
| commerce | `CProduct`/`CReview`/atoms (R6) | J‑DONOR | shipped in v0.1; November had no commerce files — importing it is an explicit counterfactual choice |
| network identity | new genesis/magic/ports (`../../genesis/derivatives/nov08x/net.py`) | NEW‑EXP | a new experimental chain, never mainnet |

The single hard rule (from the ledger): **a J‑DONOR/NEW‑EXP import may never silently
overwrite an N‑ORIG rule.** Where November specifies behaviour (its monetary
constitution, its leading‑zero‑bits PoW, its exact‑equality coinbase), November wins —
and it does, in `consensus.py` / `nov08x`.

## What NOV08‑Full is *not*

- Not a claim that November *contained* a marketplace or a Script file — it did not
  (R1 / the NOV08→JAN09 diff prove their absence). Importing them is a **counterfactual
  design act**, disclosed here.
- Not recovered code, not "true Bitcoin", not money. Its units are not satoshis; it
  has no inherited balances; its genesis is freshly minted.
- Not more authoritative than NOV08‑Minimal. If you want *only* what the surviving
  source compels, that is NOV08‑Minimal; NOV08‑Full is the interpretive envelope
  around it.

## Boundary

NOV08‑Full is intentionally the lab's **one interpretive artifact**. It is kept in the
umbrella repo, fenced by this document, and its executable form is clearly the
*console driven under November's rules* — reusing pieces whose provenance is already
classified. No new consensus behaviour is invented; the interpretation is only in
*which* absent machinery gets supplied, and that is enumerated above.
