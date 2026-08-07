# What the January 2009 archive actually is

**Corrected 8 August 2026.** The archive this laboratory anchors to — distributed everywhere,
including by the Satoshi Nakamoto Institute, as **`bitcoin-0.1.0.rar`** — is **not the 8 January 2009
v0.1.0 release**. It is **`bitcoin-0.1.1.rar`**, built on 10 January 2009.

We inherited the label from the ecosystem and repeated it. This note records what the bytes say.

## Satoshi identifies the file himself, by size

Satoshi Nakamoto to Hal Finney, 10 January 2009, subject *"Re: Crash in bitcoin 0.1.0"*:

> *"The attached file: **bitcoin-0.1.1.rar (filesize 2,132,686)** is the version where I deleted the
> mapAddresses.count line, and that should be the safest version."*

Four hours earlier he had quoted the code he was about to remove:

```cpp
// make it try connecting sooner
CRITICAL_BLOCK(cs_mapAddresses)
    if (mapAddresses.count(addr.GetKey()))
        mapAddresses[addr.GetKey()].nLastFailed = 0;
```

## Six checks, all reproducible from the archive

```
archive size                 2,132,686 B   == the size Satoshi states for bitcoin-0.1.1.rar
serialize.h VERSION          101           (v0.1.0's was 100)
mapAddresses.count           ABSENT        the line he deleted FOR 0.1.1
CRITICAL_BLOCK(cs_mapAddresses)  ABSENT
bitcoin.exe PE TimeDateStamp 1231629360 -> 2009-01-10 23:16:00 UTC
files newer than 7 January   exactly three: irc.cpp, serialize.h, bitcoin.exe
readme banner                "BitCoin v0.01 ALPHA"  -- never bumped; cannot arbitrate
```

### The decisive one needs no custody chain

**The PE `TimeDateStamp` inside `bitcoin.exe` is 10 January 2009.** v0.1.0 was announced on
**8 January**. A binary linked on the 10th cannot be in the archive released on the 8th.

That is an internal property of bytes anyone can read. It depends on no archive, no mirror, and
nobody's account of custody.

### Calibrated against a control

The other historical archive we hold, `bitcoin-0.1.3.rar`, can be dated independently from the
correspondence:

```
                   serialize.h   readme                 bitcoin.exe PE stamp
0.1.0-labelled        101        BitCoin v0.01 ALPHA    2009-01-10 23:16:00 UTC
bitcoin-0.1.3.rar     103        BitCoin v0.1.3 ALPHA   2009-01-12 05:20:24 UTC
```

Satoshi to Finney, **11 January 2009, 9:31 PM Pacific**: *"0.1.3 exe attached."* That binary's stamp
is **11 January 21:20:24 Pacific** — linked **eleven minutes** before the mail attaching it.

**On the one archive whose build time is independently known, the method lands within eleven
minutes.** And `VERSION` tracks the point release exactly on both samples, so `101` ↔ v0.1.1 is
demonstrated correspondence rather than inference.

## What this changes, and what it does not

**The entire 0.1.0 → 0.1.1 delta is `irc.cpp` and `serialize.h`. Both are networking.** `main.cpp`,
`main.h` and `script.cpp` — where every consensus rule lives — are dated **7 January** and are
untouched. `V0_1_0_TO_V0_1_3_DIFF.md` reached the same reading from the other end: *"network
hardening, not base-layer restriction."*

```
genesis re-derivation from the unmodified binary       UNAFFECTED
two-node mine / relay / reorg witness                  UNAFFECTED
every rule in CONSENSUS_SURFACE.md                     UNAFFECTED
NOV08-X                                                UNAFFECTED — different archive entirely
JAN09-X and Bitcoin consensus behaviour                UNAFFECTED
"an unmodified January 2009 Satoshi binary"            STILL TRUE
"the 8 January v0.1.0 release"                         FALSE
```

**No executed result changes. No hash changes. No code changes.** What changes is a version label and
the sentences that turned it into a claim about *which release*.

**One substantive correction beyond the label:** `serialize.h VERSION = 101` is **v0.1.1's** protocol
version. v0.1.0's was `100`. Anywhere the lab described `101` as v0.1.0's wire version, that was
wrong.

## The filename stays

The artifact is called `bitcoin-0.1.0.rar` by every host that serves it, and its sha256
`8b17eb9a…` is published in our manifests and cited in our releases. **Renaming it would break every
published checksum and help nobody.** What we correct is the description, not the filename.

## And the part worth recording plainly

**We ran this binary and it told us what it was.** `lab/genesis/r3-findings/run1/FINDINGS.md`, from
the witnessed 26 July 2026 run:

> *"The About box reads **"version 0.1.1 Alpha"** … So the binary shipped in the *0.1.0* archive
> self-identifies as 0.1.1"*

We wrote the conclusion down and filed it as a curiosity of the run, between a GUI crash note and the
pay-to-IP dialog. `V0_1_0_TO_V0_1_3_DIFF.md` then inverted the causality outright — *"v0.1.0's
VERSION=101 is why its About box reads '0.1.1 Alpha'"* — explaining the artifact's own statement away
as a quirk needing explanation.

This laboratory's stated method is that **authority attaches to exact bytes, never to a name**. The
software announced its own version, on screen, in a witnessed run, and the filename won anyway.
Recording that is the point of this note.

## Where the real v0.1.0 is

Missing, on present evidence. It was the public download for four days; SourceForge's copy was
replaced in place on 12 January 2009 with v0.1.3's bytes. No surviving copy, no contemporaneous
hash, no byte size. It differs from what we hold in `irc.cpp` and `serialize.h`, and in nothing else.
