# Addresses, autonomous systems, and the five registries that hand them out

## Two kinds of number, and they answer different questions

Most people picture the internet as a pile of addresses: 1.1.1.1, 192.168.1.1, `2606:4700:4700::1111`. An address says where a packet should be delivered. It is a hierarchical identifier handed down in blocks, and it is the thing that actually appears on the wire.

Routing, though, does not work with addresses at all. A router's table does not say "to 1.1.1.1"; it says "to AS13335, via this neighbour". Underneath sits a second numbering system, the **Autonomous System Number**. An ASN identifies a network, usually an organisation, that announces its own routes into the global BGP system. Every IP prefix on the internet belongs to exactly one ASN.

The distinction explains a lot that otherwise looks strange. Address space can be bought, transferred and resold; an ASN cannot, because it is an identifier rather than a resource, and it stays with whoever registered it. One ASN can hold an enormous number of addresses, and one address can belong to an ASN that is itself an ISP reselling to thousands of customers. Two networks in the same city can sit in different ASNs, and two ASNs can have addresses in every country.

## The allocation chain

Neither kind of number goes straight to the person who ends up using it:

```
IANA  ──►  Regional Internet Registry (RIR)  ──►  National IR (or LIR)  ──►  ISP  ──►  end customer
```

IANA holds the top of the address space and delegates to the five RIRs, which are the institutions that actually hold the resource. Most RIRs then allocate to national or regional registries, which allocate to large ISPs, which allocate to their customers. Every link in that chain is a delegation, and every delegation is recorded. That is why the delegated statistics files exist, and why this page can be built from them.

## The five registries

| Registry | Region | IPv4 allocations | ASNs |
|---|---|---|---|
| RIPE NCC | Europe, Middle East, Central Asia | 100,774 | 40,114 |
| ARIN | North America | 81,326 | 33,123 |
| LACNIC | Latin America & Caribbean | 20,450 | 16,128 |
| APNIC | Asia-Pacific | 58,336 | 13,981 |
| AFRINIC | Africa | 5,558 | 2,821 |

*(allocation counts from the RIR delegated statistics files, snapshot 2026-10-29)*

RIPE NCC hands out the most of both, and that is not a fact about Europe so much as about how the internet was built and then reorganised. The history of IPv4 allocation has been dominated by transfers and re-delegations, and RIPE NCC ended up holding the largest share of each resource.

ARIN comes second in allocations but first in address space per ASN: big blocks, fewer operators.

AFRINIC is smallest in both columns, which is the entire reason regional registries exist. A continent of well over a billion people with 2,821 autonomous systems and a small share of the address space is a thin slice of the internet's control plane. Regional registries exist so that allocation does not have to happen across an ocean.

## Why IPv6 changes the arithmetic

IPv4 has about 4.3 billion addresses. IPv6 has about (3.4 	imes 10^{38}), a number large enough that the usual way of thinking about scarcity stops applying at all. The interesting question about IPv6 stopped being "is there enough of it" and became "why is so little of it in use", which is a deployment and routing problem rather than an allocation problem.

This page stays on IPv4, because that is where the allocation pressure is visible.

## The tools that already do this

Asking "who owns this IP" or "who is this ASN" is easy. There are four families of services that answer it, and they answer rather different questions. Knowing which one to reach for saves a lot of time, so here they are.

### The registries themselves

The five RIRs are the authoritative source, because they are the ones who did the allocating. Each publishes a whois server, an RDAP endpoint, and a machine-readable dump of everything it has delegated.

- [RIPE whois](https://www.ripe.net/manage-ips-and-asns/db/) and [rdap.db.ripe.net](https://rdap.db.ripe.net/) — the European, Middle Eastern and Central Asian allocations
- [ARIN RDAP](https://rdap.arin.net/registry/ip/8.8.8.8) and `whois.arin.net/rest/net/…` — North America
- [APNIC whois](https://wq.apnic.net/) — Asia-Pacific. Note the parameter is `?searchtext=`, not `?ip=`
- LACNIC and AFRINIC, same shape

Whois is the oldest of these and still the most useful: a telnet port and a query syntax, output meant for a human to read. RDAP is its successor, the same information over HTTP as JSON, and it is the standard IANA requires.

The third option is the one almost nobody uses, and it is the most interesting for this page: the `delegated-*-latest` file each RIR publishes. Machine-readable, complete, updated daily, no key and no rate limit. RIPE's is 18 MB, ARIN's 13 MB, APNIC's 9 MB.

### Routing aggregators

These answer a different question — what is actually being announced right now, and by whom.

| Service | What it has | Key | Notes |
|---|---|---|---|
| [RIPEstat](https://stat.ripe.net/) | 60+ datasets: routing table size, announced prefixes, ASN overview, neighbours and their history, RPKI validation, CAIDA relationships, address transfers | none | Free, and RIPE's own wording is "no limit on the amount of requests". Eight concurrent connections per IP |
| [CAIDA AS Rank](https://asrank.caida.org/) | ASes ranked by traffic, cone size broken into provider/peer/customer, and an AS relationship graph | none | GraphQL v2, 122,962 ASes |
| [RouteViews](https://www.routeviews.org/) | Global routing table snapshots and raw MRT dumps back to 2001 | none | JSON API. Guest access is 1 req/s; 10 req/s with a key |
| [PeeringDB](https://www.peeringdb.com/api/net) | Which networks exist, and where they interconnect. 35,779 networks, 1,332 IXPs | none to read | Anonymous 20/min |
| [RADb](https://www.radb.net/) | The IRR object database, with RPKI ROAs mirrored in | none for whois | whois on port 43 is free and anonymous; the REST search API is a $595/yr subscription |
| [bgp.tools](https://bgp.tools/) | The best ASN pages on the web: prefixes, upstreams, peers, RPKI, IRR | for the interactive pages | See below |
| [bgp.he.net](https://bgp.he.net/) | Hurricane Electric's toolkit, with 11 internet statistics reports | none | See below |

Two of these deserve a note.

**bgp.tools has a login wall on its interactive pages** — `/as/`, `/prefix/` and `/api/query.js` all redirect to `/login?must-login`. Three data endpoints still answer without authentication: `bgp.tools/table.jsonl` (23.5 MB, 451,159 rows), `bgp.tools/asns.csv` (4.5 MB), and `map.bgp.tools/`, an interactive map of the whole IPv4 space. Their robots.txt disallows ClaudeBot, GPTBot, OAI-SearchBot and others outright, so this site does not touch their interactive layer or their API.

**bgp.he.net draws good charts** — six of them, plus a server-rendered AS-path graph — but it has no official API, no bulk export, and no clear data licence. Third parties are already scraping it commercially. When you want BGP data, use RIPEstat, RADb and RouteViews instead.

One more from this family is worth knowing: [iptoasn.com](https://iptoasn.com/) publishes the full IP-to-ASN mapping as a download, refreshed hourly. Its public API was retired in 2020, but the files are still there and still free.

### Commercial IP data APIs

For "where is this IP and who is it", these are the fastest route.

| Service | Key | Free tier | What comes back |
|---|---|---|---|
| [ip.guide](https://ip.guide/) | none | no published limit | Country, city, ASN, org, coordinates — the full set |
| [ipwho.is](https://ipwho.is/8.8.8.8) | none | 1,000/day per client IP | Country, city, ASN, ISP, coordinates, timezone, currency |
| [ip-api.com](http://ip-api.com/json/8.8.8.8) | none | 45 req/min per IP | Country, region, city, ISP, AS, AS name, latency. HTTP only, non-commercial use only |
| [ipinfo.io](https://ipinfo.io/8.8.8.8/json) | free token for Lite | Lite is unlimited; the old keyless endpoint is 50k/month and returns less | ASN, org, location, postcode, timezone, company |
| [ipdata.co](https://ipdata.co/) | `ipdata.free` works keyless | 1,000 city lookups/day; country and ASN are unlimited | ASN, org, city |
| [ipapi.co](https://ipapi.co/) | email registration | 30,000/month but ≤1,000/day | ASN and org |

They return nearly identical fields, because they are all reading the same GeoIP databases underneath.

### Command line

The reference implementation is [github.com/nitefood/asn](https://github.com/nitefood/asn) — a 1,944-star Bash script, MIT licensed, still updated in October 2026, and packaged for Alpine, Debian, Homebrew, Nix, AUR and FreeBSD. It does more than anything else here: AS numbers, prefixes, ISPs, countries, upstream chains, and RPKI validation on every hop, with 16 command-line options and 18 data sources behind it.

What it costs you is eight system packages: `curl whois bind9-host mtr-tiny jq ipcalc grepcidr nmap ncat aha`.

Others worth a look:

| Tool | Language | Stars | What it does |
|---|---|---|---|
| [nexttrace](https://github.com/nxtrace/NTrace-core) | Go | 8,254 | The most-starred tool in this space. Traceroute with the ASN and location of every hop; `--from<country>` picks probes through Globalping |
| [ttl](https://github.com/lance0/ttl) | Rust | 1,489 | "A better mtr", with ECMP detection and MPLS label parsing |
| [asnmap](https://github.com/projectdiscovery/asnmap) | Go | 1,145 | The reverse direction: ASN or org to CIDR. Now needs a ProjectDiscovery API key |
| [asname](https://github.com/flyingllama87/asname) | Go | new | Offline LC-trie, IP or hostname to ASN and AS name, `--rest` to serve it |
| [aslookup](https://github.com/nieldk/aslookup) | C | new | No external command dependencies at all — just libcurl and libcjson |

And the one already on your machine is the most underrated. `whois` against Team Cymru returns a full record, free and forever, with no registration:

```sh
whois -h whois.cymru.com " -v 8.8.8.8"
# 15169 | 8.8.8.0/24 | US | arin | 2023-12-28 | GOOGLE - Google LLC, US
```

Team Cymru also speaks DNS (TXT records) and bulk mode. What it does not give you is city-level geography, and its country code comes from the registry record rather than from where the address is actually used — sometimes a long way apart.

### What all of them are missing

Put the four families together and the same gap shows up in each.

They are built to answer one question at a time. Every one of them takes an input and returns a record. Asking "what does France's IPv4 space look like" or "which country's routing market is most fragmented" is not a query any of them accepts — you would have to take a few thousand lines of whois output and push it through a spreadsheet.

Their output is a table or JSON, never a picture. That follows from the data model rather than from laziness: a query returns a record, and a record has no shape worth drawing.

And the authoritative data is kept somewhere else entirely. The only bulk, machine-readable, authoritative source is the RIR delegated files, and using them means writing your own parser.

There is one more thing to be honest about: several sites already draw AS-level topology graphs — Internet Health Report, BGPTrace, as-graph, bgpmap.net, APNIC REx, ISOC Pulse, CleanIP, SecViz and CAIDA's own AS Core and AS Reach projects. RIPEstat alone ships 86 embeddable widgets. So the gap is not that nobody draws this.

The gap is that those are all interactive applications. Change a parameter and you get a fresh page; you never get a URL for the picture you are looking at. And the one product that does the distribution shape properly, ipinfo's "Summarize IPs", only works on the list of addresses you paste in yourself — there is no version of it for "what does this RIR's address structure look like".

Which is exactly what a chart URL is. Change `type=hbar` to `type=pie` and you have a different picture of the same data; change the query and you have a different question. The parameter *is* the question, and it stays in the address bar.

### One boundary worth knowing

Draw ships thirteen chart types: `pie bar hbar line area radar gauge scatter combo sankey qr archmap mindmap`. There is no force-directed node-link graph among them, which rules out self-hosting the AS-path topology diagrams. That work stays with bgpmap.net, whose Graphviz output can be embedded as-is.

`sankey` is the interesting one, because the AS relationship data is already in exactly that shape. CAIDA publishes it monthly as `provider|customer|-1` and `peer|peer|0` — a directed graph with a natural three-way split, which is what a sankey draws.

### Three sources worth bookmarking

| Source | Key | What you get |
|---|---|---|
| [bgpmap.net](https://bgpmap.net/) | none, no registration | `POST /api/v1/snapshots {"resource":"1.1.1.0/24"}` returns origin ASes, upstreams, RPKI state, and a ready-made Graphviz SVG and PNG. Batch up to 100 |
| [api.routeviews.org](https://api.routeviews.org/) | optional | `/asn/{n}`, `/prefix/{cidr}` with reporting peers and AS paths, `/rib/collectors`, `/collector/` with coordinates |
| [CAIDA AS Relationships](https://publicdata.caida.org/datasets/as-relationships/serial-1/) | none, public download | A monthly file, 1998-01 to date, of AS relationships — the raw material for a sankey |

## If you only want one record, use these

| You want | Use | Key needed? |
|---|---|---|
| The registration record for an IP | each RIR's whois / RDAP | no |
| Where an IP is, who owns it, its ASN | [ip.guide](https://ip.guide/) · [ipwho.is](https://ipwho.is/) · [ipinfo.io](https://ipinfo.io/) | no |
| How big an ASN is, and its rank | [CAIDA AS Rank](https://asrank.caida.org/) | no |
| Who announces a prefix, and its RPKI state | [RIPEstat](https://stat.ripe.net/) · [RouteViews](https://www.routeviews.org/) | no |
| What the AS path to an IP looks like | [bgpmap.net](https://bgpmap.net/) · [BGPTrace](https://bgptrace.com/) | no |
| How networks and IXPs interconnect | [PeeringDB](https://www.peeringdb.com/) · [RADb](https://www.radb.net/) | no |

This page does a different job. Those tools give you one record; this gives you a distribution.

## How to add a page

The whole mechanism of this site is one thing: turn data into a URL with the parameters written out.

```sh
# 1. get the data
curl -s https://ftp.apnic.net/stats/apnic/delegated-apnic-latest

# 2. turn it into a chart URL — the parameters are plain text, so an AI can build one too
```

```text
https://x-cmd.com/draw?type=hbar&labels=CN,JP,IN&values=214719744,54388992,26313216&title=IPv4%20by%20country
```

<a href="https://x-cmd.com/draw?type=hbar&labels=CN,JP,IN&values=214719744,54388992,26313216&title=IPv4%20by%20country"><img src="https://x-cmd.com/draw?type=hbar&labels=CN,JP,IN&values=214719744,54388992,26313216&title=IPv4%20by%20country" width="780" alt="addresses-autonomous-systems"></a>

```sh
# 3. or let an AI agent do it
#    with the draw skill: https://raw.githubusercontent.com/x-cmd/draw/main/SKILL.md
```

The replaceable parameters are the point of the whole thing. Change `product:nginx` to `org:yourcompany` and the same template becomes a picture of your own estate. So what a page here gives you is not a finished chart but a template you can take apart and change.

The repository layout, the naming rules, how routes get generated and what CI checks are in [CONTRIBUTING.md](https://github.com/x-cmd/asn/blob/main/CONTRIBUTING.md). The notes for AI agents are in [SKILL.md](https://github.com/x-cmd/asn/blob/main/SKILL.md).

## Where the numbers come from

Everything here is computed from the RIRs' own `delegated-*-latest` exports, the authoritative registry files rather than a third-party mirror. The per-country cuts use the APNIC region, because APNIC is the registry that publishes per-country rows inside the same export; ARIN and RIPE NCC publish them separately.

One caution applies to every table on this site. These are counts of allocations, not of addresses. A registry that handed out 100,774 blocks may have handed out larger blocks than one that handed out fewer. The next page puts the two RIR resources side by side, which makes the distinction visible.

> CLI: `x ip cidr` · `x ip map` — see [x-cmd.com/mod/ip](https://x-cmd.com/mod/ip)
> Data: [ftp.ripe.net](https://ftp.ripe.net/pub/stats/ripencc/) · [ftp.apnic.net](https://ftp.apnic.net/stats/apnic/) · [ftp.arin.net](https://ftp.arin.net/pub/stats/arin/) · [ftp.lacnic.net](https://ftp.lacnic.net/pub/stats/lacnic/) · [ftp.afrinic.net](https://ftp.afrinic.net/pub/stats/afrinic/)
