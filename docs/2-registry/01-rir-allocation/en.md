# Who hands out what, and what that shape means

## Two resources, two different shapes

**IPv4 allocations by RIR** — the number of IPv4 blocks each registry has delegated.

```text
https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CAPNIC%2CLACNIC%2CAFRINIC&values=100774%2C81326%2C58336%2C20450%2C5558&title=IPv4%20allocations%20by%20RIR&unit=blocks
```

<a href="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CAPNIC%2CLACNIC%2CAFRINIC&values=100774%2C81326%2C58336%2C20450%2C5558&title=IPv4%20allocations%20by%20RIR&unit=blocks"><img src="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CAPNIC%2CLACNIC%2CAFRINIC&values=100774%2C81326%2C58336%2C20450%2C5558&title=IPv4%20allocations%20by%20RIR&unit=blocks" width="780" alt="who-hands-out-what-and-what-"></a>

**ASNs allocated by RIR** — how many autonomous systems each registry has handed out.

```text
https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CLACNIC%2CAPNIC%2CAFRINIC&values=40114%2C33123%2C16128%2C13981%2C2821&title=ASNs%20allocated%20by%20RIR&unit=ASNs
```

<a href="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CLACNIC%2CAPNIC%2CAFRINIC&values=40114%2C33123%2C16128%2C13981%2C2821&title=ASNs%20allocated%20by%20RIR&unit=ASNs"><img src="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CLACNIC%2CAPNIC%2CAFRINIC&values=40114%2C33123%2C16128%2C13981%2C2821&title=ASNs%20allocated%20by%20RIR&unit=ASNs" width="780" alt="who-hands-out-what-and-what-"></a>

Put them next to each other and the two curves are not the same shape, which is the first thing worth noticing.

## Reading the pairing

**RIPE NCC allocates the most ASNs (40,114) and the most IPv4 blocks (100,774).** This is the densest routing environment in the world: one autonomous system per ISP or large enterprise network, by and large. Europe, the Middle East and Central Asia are full of medium-sized networks with their own ASNs, and this is also where the early commercial internet and its national carriers grew up.

**ARIN allocates fewer ASNs than RIPE (33,123) but nearly as many IPv4 blocks (81,326).** Bigger blocks, fewer operators. The North American model is different: a handful of very large networks — tier-one transit providers, hyperscale clouds — each holding enormous address space, with the long tail rented from them.

**LACNIC has more ASNs than APNIC (16,128 vs 13,981) while holding a third of APNIC's IPv4 blocks.** Latin America is routed finely over a comparatively small address pool: many autonomous systems, each with less to work with. This is the shape of a region with a competitive ISP market and limited address supply.

**AFRINIC's numbers are the smallest but not trivial: 5,558 blocks and 2,821 autonomous systems.** For a continent of well over a billion people, that is thin — and it is the clearest argument for why regional registries exist. Before AFRINIC was created in 2005, African networks had to obtain address space from ARIN, thousands of kilometres away, through a process designed for North American institutions.

## Why the ASN count is the more revealing number

Allocation counts are a proxy; the interesting story is in how many **autonomous systems** exist, because an ASN is a routing decision, not just an accounting entry.

- An ASN is issued when a network intends to announce its own routes. More ASNs in a region means more independently-routed networks — more routing diversity, more points of failure, more places for traffic to take an unexpected path.
- Conversely, a region with few ASNs and large blocks has a flatter routing topology, which is efficient but fragile: a single upstream problem affects a large share of the address space.
- **The ASN-to-address ratio is a proxy for fragmentation.** RIPE's ratio is roughly 0.4 ASNs per block; LACNIC's is roughly 0.8; AFRINIC's roughly 0.5. The raw counts understate how differently these regions are structured.

## The secondary market is not in these charts

One of the biggest facts about IPv4 allocation is invisible in the counts above: **address space moves.** Between roughly 2015 and 2022 the market for transferred IPv4 blocks was, by some estimates, worth billions of dollars annually, and large blocks changed hands between RIRs — usually from a region with surplus to a region with demand.

The delegated files record the *result* of those transfers (they show current holdings), not the flow. But it is worth remembering when reading any allocation chart: **the numbers are a snapshot of a market, not a historical inheritance.** ARIN's dominance in IPv4 is not because North America was allocated more in 1990; it is partly because blocks have moved.

## Caveats

- **These are allocation counts, not address counts.** A registry that issued 100,774 blocks may have issued larger blocks than one that issued 58,336. To compare address space directly you need the sum of the `value` column, and even that measures *delegated* space rather than *routed* space — a large share of delegated IPv4 is never announced in BGP.
- **ASNs are not the same as networks.** One organisation can hold many ASNs (a cloud provider operating in many regions will have dozens); one ASN can cover many organisations (an LIR allocating to its customers).
- **The counts move.** Delegations happen daily.

> Reproduce: `curl -s https://ftp.ripe.net/pub/stats/ripencc/delegated-ripencc-latest`
> (same for apnic / arin / lacnic / afrinic), then take the `*|ipv4|*|<n>|summary`
> and `*|asn|*|<n>|summary` rows.
