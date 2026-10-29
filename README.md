# asn

**Address space and autonomous systems, as plain URLs.** Every chart below is a [x-cmd draw](https://github.com/x-cmd/draw) URL — the data lives in the link, server-rendered as SVG. Open any chart and you see its full parameters; paste it anywhere markdown images work.

> Languages: English · 中文（每页双语；es / ja / de 待补）

## Sneak peek

### Who hands out what — [full page](docs/2-registry/01-rir-allocation/en.md)

<a href="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CAPNIC%2CLACNIC%2CAFRINIC&values=100774%2C81326%2C58336%2C20450%2C5558&title=IPv4%20allocations%20by%20RIR&unit=blocks"><img src="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CAPNIC%2CLACNIC%2CAFRINIC&values=100774%2C81326%2C58336%2C20450%2C5558&title=IPv4%20allocations%20by%20RIR&unit=blocks" width="680" alt="who-hands-out-what"></a>

### Address space and ASNs, country by country — [full page](docs/2-registry/02-country-allocation/en.md)

<a href="https://x-cmd.com/draw?type=hbar&labels=CN%2CKR%2CJP%2CIN%2CSG%2CAU%2CTW%2CID%2CVN%2CHK%2CTH%2CPK%2CPH%2CMY&values=214719744%2C57649152%2C54388992%2C26313216%2C21224704%2C19878912%2C19008000%2C12987136%2C12063744%2C6200576%2C4043264%2C3815424%2C3424512%2C3337728&title=IPv4%20address%20space%20by%20country%20%28APNIC%20region%29&unit=addresses"><img src="https://x-cmd.com/draw?type=hbar&labels=CN%2CKR%2CJP%2CIN%2CSG%2CAU%2CTW%2CID%2CVN%2CHK%2CTH%2CPK%2CPH%2CMY&values=214719744%2C57649152%2C54388992%2C26313216%2C21224704%2C19878912%2C19008000%2C12987136%2C12063744%2C6200576%2C4043264%2C3815424%2C3424512%2C3337728&title=IPv4%20address%20space%20by%20country%20%28APNIC%20region%29&unit=addresses" width="680" alt="address-space-and-asns-country-by-country"></a>

## All pages

### overview

- [About this page](docs/1-overview/01-about/en.md)

### registry

- [Who hands out what](docs/2-registry/01-rir-allocation/en.md)
- [Address space and ASNs, country by country](docs/2-registry/02-country-allocation/en.md)

## 中文页

### overview

- [关于本页](docs/1-overview/01-about/cn.md)

### registry

- [谁在分配什么](docs/2-registry/01-rir-allocation/cn.md)
- [按国家看地址空间与 ASN](docs/2-registry/02-country-allocation/cn.md)

## How the charts work

Each figure is a plain `https://x-cmd.com/draw?type=…` URL — rendered to a standalone SVG by [x-cmd/draw](https://github.com/x-cmd/draw). No JS runs on this page, the SVG is static, printable, and AI-readable: the parameters *are* the data, so the same URL can be edited to query something else.

To make your own: run the CLI, then hand the output to draw — or let your AI agent do it with the [draw skill](https://raw.githubusercontent.com/x-cmd/draw/main/SKILL.md).

## Data sources

The five RIR delegated statistics exports — [RIPE NCC](https://ftp.ripe.net/pub/stats/ripencc/delegated-ripencc-latest) · [APNIC](https://ftp.apnic.net/stats/apnic/delegated-apnic-latest) · [ARIN](https://ftp.arin.net/pub/stats/arin/delegated-arin-extended-latest) · [LACNIC](https://ftp.lacnic.net/pub/stats/lacnic/delegated-lacnic-latest) · [AFRINIC](https://ftp.afrinic.net/pub/stats/afrinic/delegated-afrinic-latest) — snapshot 2026-10-29. Counts are *allocations* (blocks / ASNs), not address totals; per-country cuts use the APNIC region, which publishes per-country rows in the same export.
