# v0.1.0 → v0.1.3 diff — the first evolution of Bitcoin

Cross‑version diff of Bitcoin **v0.1.0** (Jan 2009, our genesis baseline) against
**v0.1.3 ALPHA** — the third Satoshi codebase the Nakamoto Institute hosts, and the
first point releases *after* the genesis release. One question:

> **What did Satoshi change first?**

**Answer: almost nothing but the network.** The consensus engine, the monetary
constitution, and the full Script vocabulary are **byte‑for‑byte untouched**; the
changes are networking/peer‑discovery robustness plus a version bump.

## Provenance

`bitcoin-0.1.3.rar` fetched from the canonical CDN (same path as nov08 / v0.1.0):
`https://cdn.nakamotoinstitute.org/code/bitcoin-0.1.3.rar` (there is **no `.tgz`** for
0.1.3). Recorded on acquisition (2026‑07‑27):

| Artifact | size | md5 | sha256 |
|---|---|---|---|
| `bitcoin-0.1.3.rar` | 2,127,418 B | `9a73e0826d5c069091600ca295c6d224` | `3d73b1a80ce775e0cec7f9476644a6bf9b361e99567fd143807ad1d1c81b1756` |

**Out of authority** (`../AUTHORITY.md`): v0.1.0 is the behavioral oracle; v0.1.3 is a
successor, treated here as a *measured divergence point* — not a co‑equal edition. Bytes
are not committed (extracted read‑only for the diff, like every archive).

## 1. File‑level presence — no change

Same **48‑file** tree as v0.1.0 (`bitcoin.exe` + DLLs + the 26 `src/` units + resources).
**No files added or removed.** All differences are *inside* files.

## 2. Per‑file churn (changed lines)

| File | changed lines | nature |
|---|--:|---|
| `net.cpp` | 141 | networking / peer connection logic |
| `irc.cpp` | 57 | IRC peer‑discovery reconnection + backoff |
| `util.cpp` | 8 | utility tweaks |
| `net.h` | 4 | `CAddress` default port; `IsRoutable` fix |
| `db.cpp` | 4 | minor |
| `main.cpp` | 2 | a more verbose `version` debug print (no logic) |
| `serialize.h` | 2 | **`VERSION 101 → 103`** |

**Byte‑identical** (verified with `cmp`): `script.cpp`, `script.h`, `key.h`, `main.h`.

## 3. Consensus, vocabulary, money — all unchanged

- **Script vocabulary: identical.** `script.cpp`/`script.h` are byte‑identical, so v0.1.3
  still runs the whole broad vocabulary (`OP_CAT`, `OP_MUL`, `OP_DIV`, `OP_LSHIFT`,
  `OP_INVERT`, `OP_2MUL/2DIV`, …) and disables exactly **one** functional opcode —
  `OP_NOTEQUAL` — with the same comment. **The 2010 disabling had not begun by v0.1.3.**
- **Monetary constitution: identical.** `COIN = 1e8`, `CENT = 1e6`, subsidy `50 * COIN`,
  halving `>>= nBestHeight/210000`, spacing `10*60`, timespan `14 days` — all unchanged
  from v0.1.0 (`main.h`/`main.cpp` consensus lines identical).

## 4. What actually changed (networking)

- **IRC discovery robustness** (`irc.cpp`): a new `Wait(nSeconds)` reconnect helper with a
  countdown, `nErrorWait`/`nRetryWait` backoff, and normal thread priority — i.e. *retry
  IRC seeding gracefully instead of hammering it.*
- **`IsRoutable` fix** (`net.h:265`): now also excludes `127.x` (loopback) and `0.x`
  alongside `10.x`/`192.168.x` — a correctness fix so the node doesn't advertise loopback /
  null as its routable address. (Refines the exact quirk the R3 isolated‑network recipe
  works around.)
- **`CAddress` default port** = `DEFAULT_PORT` (`net.h:156`).
- **`VERSION 101 → 103`** (`serialize.h`) — the protocol/serialization version. (v0.1.0's
  `VERSION=101` is why its About box reads "0.1.1 Alpha"; 0.1.3 → `103`.)
- **Verbose `version` log** (`main.cpp:1737`): prints peer address + version — diagnostics,
  no behavior change.

## 5. Reading

**Bitcoin's earliest evolution was network hardening, not base‑layer restriction.** In the
weeks after launch Satoshi's first changes were making peer discovery resilient (IRC
reconnect/backoff) and fixing address routability — while leaving the **consensus, the
money, and the full Script vocabulary byte‑for‑byte alone.** This directly refutes any
notion that the base layer was progressively narrowed from the start: the broad vocabulary
was still fully live in v0.1.3, and its disabling is a distinct, **later (2010)** event. It
also corroborates the lab thesis — the *capability* was stable and complete at the origin;
what moved early was the plumbing.

> Method note: compares only hash‑verified archives; gaps are *absence from the preserved
> artifact*, not proof of non‑existence; `src:line` anchors throughout. v0.1.3 is a measured
> successor, never presented as "the original." A single diff earns v0.1.3 no edition of its
> own — the finding is precisely that there is *nothing consensus‑level to enshrine.*
