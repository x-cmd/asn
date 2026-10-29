# Who hands out what, and what that shape means

**IPv4 allocations by RIR**, the number of IPv4 blocks each registry has delegated.

```text
https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CAPNIC%2CLACNIC%2CAFRINIC&values=100774%2C81326%2C58336%2C20450%2C5558&title=IPv4%20allocations%20by%20RIR&unit=blocks
```

<a href="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CAPNIC%2CLACNIC%2CAFRINIC&values=100774%2C81326%2C58336%2C20450%2C5558&title=IPv4%20allocations%20by%20RIR&unit=blocks"><img src="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CAPNIC%2CLACNIC%2CAFRINIC&values=100774%2C81326%2C58336%2C20450%2C5558&title=IPv4%20allocations%20by%20RIR&unit=blocks" width="780" alt="who-hands-out-what-and-what-"></a>

**ASNs allocated by RIR**, how many autonomous systems each registry has handed out.

```text
https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CLACNIC%2CAPNIC%2CAFRINIC&values=40114%2C33123%2C16128%2C13981%2C2821&title=ASNs%20allocated%20by%20RIR&unit=ASNs
```

<a href="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CLACNIC%2CAPNIC%2CAFRINIC&values=40114%2C33123%2C16128%2C13981%2C2821&title=ASNs%20allocated%20by%20RIR&unit=ASNs"><img src="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CLACNIC%2CAPNIC%2CAFRINIC&values=40114%2C33123%2C16128%2C13981%2C2821&title=ASNs%20allocated%20by%20RIR&unit=ASNs" width="780" alt="who-hands-out-what-and-what-"></a>

Put the two side by side and they are not the same shape.

RIPE NCC allocates the most of both: 40,114 ASNs and 100,774 IPv4 blocks. This is the densest routing environment in the world, where a large ISP or enterprise network typically gets an ASN of its own. Europe, the Middle East and Central Asia are full of medium-sized networks, and this is also where the early commercial internet and the national carriers grew up.

ARIN allocates fewer ASNs than RIPE, 33,123 against 40,114, but nearly as many IPv4 blocks. The North American model looks different: a handful of very large networks, tier-one transit providers and hyperscale clouds, each holding enormous address space, with the long tail renting from them.

LACNIC has more ASNs than APNIC, 16,128 against 13,981, while holding a third of APNIC's IPv4 blocks. Latin America is routed finely over a comparatively small address pool, which is what a competitive ISP market with limited address supply looks like.

AFRINIC is smallest at 5,558 blocks and 2,821 autonomous systems. For a continent of well over a billion people that is thin, and it is the clearest argument for why regional registries exist at all. Before AFRINIC was created in 2005, African networks obtained address space from ARIN, thousands of kilometres away, through a process designed for North American institutions.

## The ASN count is the more revealing number

Allocation counts are a proxy. The more interesting story is in how many autonomous systems exist, because an ASN is a routing decision rather than an accounting entry.

An ASN is issued when a network intends to announce its own routes. More ASNs in a region means more independently-routed networks, which means more routing diversity and more places for traffic to take an unexpected path. A region with few ASNs and large blocks has a flatter topology: efficient, but fragile, since one upstream problem affects a large share of the address space.

The ratio between them is a decent measure of fragmentation. RIPE NCC sits near 0.4 ASNs per block, LACNIC near 0.8, AFRINIC near 0.5. The raw counts understate how differently these regions are built.

## The secondary market is invisible here

One of the biggest facts about IPv4 allocation does not show up in the counts above. Address space moves. Between roughly 2015 and 2022 the market for transferred IPv4 blocks was worth billions of dollars a year by some estimates, and large blocks changed hands between RIRs, usually from a region with surplus to a region with demand.

The delegated files record the result of those transfers, since they show current holdings, but not the flow. It is worth remembering when reading any allocation chart that the numbers are a snapshot of a market rather than a historical inheritance. ARIN's dominance in IPv4 is not because North America was allocated more in 1990; it is partly because blocks have moved.

## Caveats

These are allocation counts, not address counts, and a registry that issued 100,774 blocks may have issued larger ones than a registry that issued 58,336. To compare address space directly you need the sum of the `value` column, and even that measures delegated space rather than routed space, since a large share of delegated IPv4 is never announced in BGP.

ASNs are not the same as networks. One organisation can hold many of them, a cloud provider operating in many regions will have dozens, and one ASN can cover many organisations, an LIR allocating to its customers.

The counts move. Delegations happen daily.

> Reproduce: `curl -s https://ftp.ripe.net/pub/stats/ripencc/delegated-ripencc-latest`, then take the `*|ipv4|*|<n>|summary` and `*|asn|*|<n>|summary` rows.
