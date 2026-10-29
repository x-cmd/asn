# asn

**Address space and autonomous systems, as plain URLs.**

Two kinds of number run the internet — **addresses** (where a packet goes) and **autonomous system numbers** (which network announces the route to it). They are allocated by five regional registries, down a chain that is documented in machine-readable files and drawn by almost nobody.

This site reads those files and renders them as charts whose data lives in the URL.

> Languages: English · 中文（每页双语；es / ja / de 待补）

## Sneak peek

### Addresses, autonomous systems, and the five registries that hand them out — [full page](docs/1-overview/01-about/en.md)

<a href="https://x-cmd.com/draw?type=hbar&labels=CN,JP,IN&values=214719744,54388992,26313216&title=IPv4%20by%20country"><img src="https://x-cmd.com/draw?type=hbar&labels=CN,JP,IN&values=214719744,54388992,26313216&title=IPv4%20by%20country" width="680" alt="addresses-autonomous-systems-and-the-five-registries-that-hand-them-out"></a>

### Who hands out what, and what that shape means — [full page](docs/2-registry/01-rir-allocation/en.md)

<a href="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CAPNIC%2CLACNIC%2CAFRINIC&values=100774%2C81326%2C58336%2C20450%2C5558&title=IPv4%20allocations%20by%20RIR&unit=blocks"><img src="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CAPNIC%2CLACNIC%2CAFRINIC&values=100774%2C81326%2C58336%2C20450%2C5558&title=IPv4%20allocations%20by%20RIR&unit=blocks" width="680" alt="who-hands-out-what-and-what-that-shape-means"></a>

### Address space and autonomous systems, country by country — [full page](docs/2-registry/02-country-allocation/en.md)

<a href="https://x-cmd.com/draw?type=hbar&labels=CN%2CKR%2CJP%2CIN%2CSG%2CAU%2CTW%2CID%2CVN%2CHK%2CTH%2CPK%2CPH%2CMY&values=214719744%2C57649152%2C54388992%2C26313216%2C21224704%2C19878912%2C19008000%2C12987136%2C12063744%2C6200576%2C4043264%2C3815424%2C3424512%2C3337728&title=IPv4%20address%20space%20by%20country%20%28APNIC%20region%29&unit=addresses"><img src="https://x-cmd.com/draw?type=hbar&labels=CN%2CKR%2CJP%2CIN%2CSG%2CAU%2CTW%2CID%2CVN%2CHK%2CTH%2CPK%2CPH%2CMY&values=214719744%2C57649152%2C54388992%2C26313216%2C21224704%2C19878912%2C19008000%2C12987136%2C12063744%2C6200576%2C4043264%2C3815424%2C3424512%2C3337728&title=IPv4%20address%20space%20by%20country%20%28APNIC%20region%29&unit=addresses" width="680" alt="address-space-and-autonomous-systems-country-by-country"></a>

## All pages

### Overview · 总览

- [Addresses, autonomous systems, and the five registries that hand them out](docs/1-overview/01-about/en.md)

### The registries · 注册机构

- [Who hands out what, and what that shape means](docs/2-registry/01-rir-allocation/en.md)
- [Address space and autonomous systems, country by country](docs/2-registry/02-country-allocation/en.md)

## 中文页

### 总览

- [地址、自治系统，以及发放它们的那五个机构](docs/1-overview/01-about/cn.md)

### 注册机构

- [谁在分配什么，以及这种形状意味着什么](docs/2-registry/01-rir-allocation/cn.md)
- [按国家看地址空间与自治系统](docs/2-registry/02-country-allocation/cn.md)

## How the charts work

Every figure is a plain `https://x-cmd.com/draw?type=…` URL — rendered to a standalone SVG by [x-cmd/draw](https://github.com/x-cmd/draw). No JavaScript runs on this page; the SVG is static, printable, and AI-readable, because the parameters *are* the data.

To make your own: pull the delegated files (or ask `x ip calc`), then hand the numbers to draw — or let your AI agent do it with the [draw skill](https://raw.githubusercontent.com/x-cmd/draw/main/SKILL.md).


## Data sources

The five RIR delegated statistics exports — [RIPE NCC](https://ftp.ripe.net/pub/stats/ripencc/delegated-ripencc-latest) · [APNIC](https://ftp.apnic.net/stats/apnic/delegated-apnic-latest) · [ARIN](https://ftp.arin.net/pub/stats/arin/delegated-arin-extended-latest) · [LACNIC](https://ftp.lacnic.net/pub/stats/lacnic/delegated-lacnic-latest) · [AFRINIC](https://ftp.afrinic.net/pub/stats/afrinic/delegated-afrinic-latest) — snapshot 2026-10-29. Counts are *allocations* (blocks / ASNs), not address totals; per-country cuts use the APNIC region, which publishes per-country rows in the same export.
