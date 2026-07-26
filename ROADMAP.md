# Roadmap

## R0 — Provenance Freeze

- Acquire NOV08 RAR/TGZ and JAN09 RAR/TGZ.
- Verify published MD5/SHA-1 and available SHA-256 values.
- Generate local SHA-256 and file metadata manifests.
- Extract archives into read-only working copies.
- Compare archive-pair source trees where applicable.
- Record provenance and custody limitations.

## R1 — Source Inventory

- Enumerate every source file, class, function, opcode, network message, transaction field, and build dependency.
- Produce a NOV08/JAN09 structural diff.
- Label missing, dormant, UI-only, networking, policy, and consensus paths.

## R2 — Historical Build Reconstruction

- Reconstruct period-appropriate build environments.
- Execute the preserved JAN09 binary in an isolated VM.
- Attempt a reproducible JAN09 build.
- Determine the maximum executable reconstruction possible for NOV08.

## R3 — Node and Chain Laboratory

- Run isolated peers.
- Mine blocks and relay transactions.
- Test synchronization, orphan handling, competing branches, and reorganizations.
- Capture deterministic evidence bundles.

## R4 — Script and Transaction Conformance

- Positive, negative, boundary, malformed, and context tests for every opcode.
- Signature-hash vectors.
- Serialization, accounting, maturity, locktime, sequence, and double-spend vectors.

## R5 — Native Financial Experiments

- Payments, multisig, escrow, deposits, bonds, conditional revelation, collaborative funding, delayed refunds, and transaction-state graphs.
- Each example includes raw bytes, scripts, execution trace, block witness, UTXO result, and limitations.

## R6 — Commercial Subsystem Audit

- Product, order, review, reputation, subscription, and related P2P message paths.
- Classify each as operational, reachable, partial, dormant, or absent.

## R7 — Transaction Studio

- Script debugger, stack trace, transaction composer, graph viewer, signer, miner, UTXO viewer, and evidence exporter.

## R8 — Modern Safe Experimental Nodes

- Separate NOV08-derived and JAN09-derived nodes.
- New experimental genesis blocks.
- Modern tooling and security hardening with historical semantic profiles.
- Differential testing against the historical oracle.

> **NOV08-X design + provenance ledger:** [`nov08x/DESIGN_LEDGER.md`](nov08x/DESIGN_LEDGER.md)
> — the contract for a counterfactual network that executes November's constitution
> (`main.*`/`node.*` + the monetary rules A1–A9) on the reconstructed substrate, with
> the **full original opcode vocabulary and nothing disabled**, every line
> provenance-classed (N-ORIG / N-IFACE / J-DONOR / NEW-EXP). Build is headless on the
> `genesis/derivatives` stack — no period-toolchain compile.

## R9 — Descendant Conformance

- Open-ended candidate registry.
- Test BTC, BCH, BSV, other descendants, and reimplementations.
- Classify behavior as preserved, disabled, restricted, restored, modified, added, or unproven.
