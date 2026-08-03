# Wire surface — what the origin's protocol could say, and what it could not

Companion to [`CONSENSUS_SURFACE.md`](CONSENSUS_SURFACE.md). That document asks what the origin
*bounded*; this one asks what the origin could *express*. Both are source-verified against the
January 2009 tree (`extracted/bitcoin/src`, archive SHA256
`8b17eb9a5707f2519defda4cdf8d14fa1b8dee630e11e6ef85ff9f5547555b56`), with line references.

The distinction matters because consensus and transport fail differently. A consensus gap lets a bad
block through. A transport gap means two implementations that agree perfectly on every rule still
cannot exchange a single message — and will look, from the outside, like a network partition with no
cause.

## A. The framing — 20 bytes, and no integrity check at all

```
CMessageHeader            net.h:56
  pchMessageStart[4]      net.h:54   { 0xf9, 0xbe, 0xb4, 0xd9 }
  pchCommand[12]          net.h:61   ascii, NUL-padded
  nMessageSize            net.h:62   uint32
                          --------
                          20 bytes, then the payload
```

**There is no checksum field, and no checksum anywhere in the protocol.** `grep -i checksum` over the
whole tree returns nothing. A corrupted payload is not detected by the framing; it is handed to the
deserializer, which fails or does not.

This is a header-shape fact with a hard consequence: **any implementation whose frame is not exactly
these 20 bytes cannot talk to the origin binary at all.** A 24-byte frame (magic | command | size |
checksum, as later Bitcoin uses) misreads the first four payload bytes as a checksum and rejects
every message. The handshake never completes; nothing distinguishes it from an unreachable peer.

## B. `version` — matches later Bitcoin, which is why the gap is easy to miss

```
PushMessage("version", VERSION, nLocalServices, nTime, addr)      net.h:493
  ->  nVersion[4 LE] | nServices[8 LE] | nTime[8 LE] | CAddress
```

The payload layout is unchanged from what later versions send. An implementation can therefore get
`version` byte-perfect and still fail at the frame, or succeed at both and fail at the first `addr`.
The failure is staged, not immediate — worth knowing before diagnosing one.

## C. `CAddress` — 26 bytes on the wire, and structurally IPv4

```
CAddress                  net.h:135-138
  nServices               uint64
  pchReserved[12]         net.h:130   pchIPv4 = {0,0,0,0,0,0,0,0,0,0,0xff,0xff}
  ip                      unsigned int   <- 32 bits
  port                    unsigned short
```

`nVersion` and `nTime` are serialized **only** under `SER_DISK` (`net.h`, `IMPLEMENT_SERIALIZE`), so
they are absent from the network form. On the wire a `CAddress` is exactly **26 bytes**, and `addr`
is a `compact_size`-prefixed `vector<CAddress>` (`main.cpp:1748`, `vRecv >> vAddr` at `:1751`).

**Byte order is the trap.** Bitcoin serializes integers little-endian, but `ip` and `port` already
hold *network* byte order (they come from `inet_addr` / `htons`). Writing a network-order value
little-endian puts the dotted quad on the wire **in order**, and the port **big-endian**. Reasoning
about this from the serializer alone gives the wrong answer in both fields; it has to be checked.

## The sharpest one — the origin cannot express an IPv6 peer

`ip` is `unsigned int`. Thirty-two bits. Every `CAddress` constructor `memcpy`s `pchIPv4` into
`pchReserved` (`net.h:149`, `:159`), so the twelve reserved bytes are not an address field — they are
a constant.

There is no arrangement of this struct that carries a 128-bit address. IPv6 entered Bitcoin's `addr`
message only later, when `pchReserved` was absorbed into a full 16-byte address.

The consequence is not a limitation to be worked around — it is a boundary of what the protocol can
mean. A v0.1-conformant network can carry IPv6 *transport* (nothing stops a node listening on `::`
and a peer dialling it), but it can never *gossip* an IPv6 peer, because there is no way to say one.
Discovery is IPv4 by construction. An implementation that "adds IPv6 support" to `addr` has stopped
being v0.1-conformant, whatever else it preserves.

## Established by execution, not only by reading

These are not inferred from the source alone. `genesis/derivatives/netnode` carries both framings and
both `addr` encodings, selected per chain (`ChainConfig.wire_checksum`, `ChainConfig.addr_v01`), with
tests that assert the v0.1 forms byte-for-byte against the structures above **and** that the two
framings do not interoperate. The 20-byte frame and the 26-byte `CAddress` are what a node must emit
to be read by the original binary.

## Scope & boundary

This describes v0.1 only. It says nothing about which later protocol change happened when, and it
does not claim the origin's transport was adequate — the absent checksum and the unbounded message
size are exactly the sort of thing a hardened node must add back, and `PUBLIC_TESTNET_SCOPE.md`
treats them as such. What it establishes is narrower and firmer: **what the January 2009 protocol can
and cannot say**, so that anyone building against it knows where conformance ends and divergence
begins.
