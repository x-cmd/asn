# Addresses, autonomous systems, and the five registries that hand them out

## Two kinds of number, and they answer different questions

Most people who think about the internet picture it as a set of addresses: 1.1.1.1, 192.168.1.1, `2606:4700:4700::1111`. An address says **where a packet should be delivered**. It is a hierarchical identifier, handed down in blocks, and it is the thing that actually appears on the wire.

But routing the internet does not work with addresses at all. Routers do not carry "to 1.1.1.1" in their tables; they carry "to AS13335, via this neighbour". So there is a **second numbering system** underneath: the **Autonomous System Number (ASN)**. An ASN identifies a network — an organisation, usually — that announces its own routes into the global BGP system. Every IP prefix on the internet belongs to exactly one ASN.

The distinction is worth holding onto, because it explains a lot of otherwise confusing facts:

- **Address space can be bought, transferred and resold.** Autonomous system numbers cannot — an ASN is an identifier, not a resource, and it stays with the network that registered it.
- **One ASN can hold enormous numbers of addresses, and one address can belong to an ASN that is itself an ISP reselling to many customers.** The two numbering systems nest differently and are allocated by different chains.
- **Two networks in the same city can be in different ASNs**, and two ASNs can have addresses in every country.

## The allocation chain

Neither kind of number is allocated directly to the end user. There is a chain:

```
IANA  ──►  Regional Internet Registry (RIR)  ──►  National IR (or LIR)  ──►  ISP  ──►  end customer
```

The **IANA** holds the top of the address space and delegates to the five **RIRs**, which are the institutions that actually hold the resource. Most RIRs then allocate to **national or regional registries** (NIRs), which allocate to large ISPs, which allocate to their customers. Each link in the chain is a delegation, and each delegation is recorded — which is exactly why the delegated statistics files exist and why this page can be built from them.

## The five registries

| Registry | Region | IPv4 allocations | ASNs |
|---|---|---|---|
| RIPE NCC | Europe, Middle East, Central Asia | 100,774 | 40,114 |
| ARIN | North America | 81,326 | 33,123 |
| LACNIC | Latin America & Caribbean | 20,450 | 16,128 |
| APNIC | Asia-Pacific | 58,336 | 13,981 |
| AFRINIC | Africa | 5,558 | 2,821 |

*(allocation counts from the RIR delegated statistics files, snapshot 2026-10-29)*

**RIPE NCC hands out the most of both.** That is not a statement about Europe; it is a statement about how the internet was built and then reorganised. The allocation history of IPv4 has been dominated by transfers and re-delegations, and RIPE NCC has ended up holding the largest share of both resources.

**ARIN is second in allocations but first in address space per ASN** — a big block, fewer operators.

**AFRINIC is smallest in both columns, and that is the entire point of a regional registry.** A continent of a billion-plus people, with 2,821 autonomous systems and a small fraction of the address space, is a thin slice of the internet's control plane. Regional registries exist precisely so that allocation does not have to happen across an ocean.

## Why IPv6 changes the arithmetic

IPv4 has about 4.3 billion addresses. IPv6 has about (3.4 	imes 10^{38}) — a number so large that the usual way of thinking about scarcity stops applying at all. The interesting question about IPv6 stopped being "is there enough of it" and became "why is so little of it in use", which is a deployment and routing problem rather than an allocation problem.

This page stays on IPv4, because that is where the allocation pressure is visible.

{{SURVEY}}

## Where the numbers come from

Everything here is computed from the RIRs' own `delegated-*-latest` exports — the authoritative registry files, not a third-party mirror. The per-country cuts use the APNIC region, because APNIC is the registry that publishes per-country rows inside the same export; ARIN and RIPE NCC publish them in separate files.

One caution that applies to every table on this site: **these are counts of allocations, not of addresses.** A registry that handed out 100,774 blocks may have handed out larger or smaller blocks than one that handed out fewer. The next page makes that distinction visible by putting the two RIR resources side by side.

> CLI: `x ip calc` · `x ip cidr` · `x ip map` — see [x-cmd.com/mod/ip](https://x-cmd.com/mod/ip)
> Data: [ftp.ripe.net](https://ftp.ripe.net/pub/stats/ripencc/) · [ftp.apnic.net](https://ftp.apnic.net/stats/apnic/) · [ftp.arin.net](https://ftp.arin.net/pub/stats/arin/) · [ftp.lacnic.net](https://ftp.lacnic.net/pub/stats/lacnic/) · [ftp.afrinic.net](https://ftp.afrinic.net/pub/stats/afrinic/)
