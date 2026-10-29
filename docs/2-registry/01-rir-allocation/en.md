# Who hands out what

**IPv4 allocations by RIR** — the number of IPv4 blocks each registry has delegated.

```text
https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CAPNIC%2CLACNIC%2CAFRINIC&values=100774%2C81326%2C58336%2C20450%2C5558&title=IPv4%20allocations%20by%20RIR&unit=blocks
```

<a href="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CAPNIC%2CLACNIC%2CAFRINIC&values=100774%2C81326%2C58336%2C20450%2C5558&title=IPv4%20allocations%20by%20RIR&unit=blocks"><img src="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CAPNIC%2CLACNIC%2CAFRINIC&values=100774%2C81326%2C58336%2C20450%2C5558&title=IPv4%20allocations%20by%20RIR&unit=blocks" width="780" alt="who-hands-out-what"></a>

**ASNs allocated by RIR** — how many autonomous systems each registry has handed out.

```text
https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CLACNIC%2CAPNIC%2CAFRINIC&values=40114%2C33123%2C16128%2C13981%2C2821&title=ASNs%20allocated%20by%20RIR&unit=ASNs
```

<a href="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CLACNIC%2CAPNIC%2CAFRINIC&values=40114%2C33123%2C16128%2C13981%2C2821&title=ASNs%20allocated%20by%20RIR&unit=ASNs"><img src="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CLACNIC%2CAPNIC%2CAFRINIC&values=40114%2C33123%2C16128%2C13981%2C2821&title=ASNs%20allocated%20by%20RIR&unit=ASNs" width="780" alt="who-hands-out-what"></a>

Read the two together and a structural difference shows up:

- **RIPE NCC allocates the most ASNs** (40,114) — the densest routing environment, one per ISP or large network.
- **ARIN allocates fewer ASNs than RIPE but far more IPv4** — 81,326 blocks spread over 33,123 autonomous systems. Bigger allocations, fewer operators.
- **LACNIC has more ASNs than APNIC** (16,128 vs 13,981) while holding a fifth of APNIC's IPv4 space — Latin America is routed finely over a small address pool.
- **AFRINIC's numbers are the smallest but not trivial** — 2,821 autonomous systems for a continent of a billion people is thin, and it is the clearest argument for regional registries.

```
RIPE NCC   100774 ipv4   40114 asn
ARIN        81326 ipv4   33123 asn
LACNIC      20450 ipv4   16128 asn
APNIC       58336 ipv4   13981 asn
AFRINIC      5558 ipv4    2821 asn
```

> Reproduce: `curl -s https://ftp.ripe.net/pub/stats/ripencc/delegated-ripencc-latest` (same for apnic / arin / lacnic / afrinic),
> then take the `*|ipv4|*|<n>|summary` and `*|asn|*|<n>|summary` rows.
