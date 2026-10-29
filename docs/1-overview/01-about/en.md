# About this page

The internet is not one address space — it is **five regional registries**, and inside them, a few hundred thousand **autonomous systems** that each announce their own routes.

| Registry | Region | IPv4 allocations | ASNs |
|---|---|---|---|
| RIPE NCC | Europe, Middle East, Central Asia | 100,774 | 40,114 |
| ARIN | North America | 81,326 | 33,123 |
| LACNIC | Latin America & Caribbean | 20,450 | 16,128 |
| APNIC | Asia-Pacific | 58,336 | 13,981 |
| AFRINIC | Africa | 5,558 | 2,821 |

*(allocation counts from the RIR delegated statistics files, 2026-10-29)*

Two things are worth noticing before the charts:

**RIPE NCC hands out more ASNs than anyone, but ARIN hands out far more IPv4 space per ASN.** ARIN's ASNs cover a continent; RIPE's cover a smaller region split across more autonomous systems. Address space and routing autonomy are not the same thing.

**AFRINIC is small in both columns, and that is the whole point of a regional registry.** It is the reason an African ISP can get an allocation without leaving the continent.

Everything here is built from the RIRs' own `delegated-*-latest` files — the authoritative registry exports — not from a third-party mirror.

> CLI: `x ip calc` · `x ip cidr` · `x ip map` — see [x-cmd.com/mod/ip](https://x-cmd.com/mod/ip)
> Data: [ftp.ripe.net](https://ftp.ripe.net/pub/stats/ripencc/) · [ftp.apnic.net](https://ftp.apnic.net/stats/apnic/) · [ftp.arin.net](https://ftp.arin.net/pub/stats/arin/) · [ftp.lacnic.net](https://ftp.lacnic.net/pub/stats/lacnic/) · [ftp.afrinic.net](https://ftp.afrinic.net/pub/stats/afrinic/)
