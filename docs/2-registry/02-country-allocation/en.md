# Address space and ASNs, country by country

**IPv4 address space by country (APNIC region)** — addresses delegated, not block counts.

```text
https://x-cmd.com/draw?type=hbar&labels=CN%2CKR%2CJP%2CIN%2CSG%2CAU%2CTW%2CID%2CVN%2CHK%2CTH%2CPK%2CPH%2CMY&values=214719744%2C57649152%2C54388992%2C26313216%2C21224704%2C19878912%2C19008000%2C12987136%2C12063744%2C6200576%2C4043264%2C3815424%2C3424512%2C3337728&title=IPv4%20address%20space%20by%20country%20%28APNIC%20region%29&unit=addresses
```

<a href="https://x-cmd.com/draw?type=hbar&labels=CN%2CKR%2CJP%2CIN%2CSG%2CAU%2CTW%2CID%2CVN%2CHK%2CTH%2CPK%2CPH%2CMY&values=214719744%2C57649152%2C54388992%2C26313216%2C21224704%2C19878912%2C19008000%2C12987136%2C12063744%2C6200576%2C4043264%2C3815424%2C3424512%2C3337728&title=IPv4%20address%20space%20by%20country%20%28APNIC%20region%29&unit=addresses"><img src="https://x-cmd.com/draw?type=hbar&labels=CN%2CKR%2CJP%2CIN%2CSG%2CAU%2CTW%2CID%2CVN%2CHK%2CTH%2CPK%2CPH%2CMY&values=214719744%2C57649152%2C54388992%2C26313216%2C21224704%2C19878912%2C19008000%2C12987136%2C12063744%2C6200576%2C4043264%2C3815424%2C3424512%2C3337728&title=IPv4%20address%20space%20by%20country%20%28APNIC%20region%29&unit=addresses" width="780" alt="address-space-and-asns-count"></a>

**ASNs per country (APNIC region)** — how many autonomous systems each country holds.

```text
https://x-cmd.com/draw?type=hbar&labels=CN%2CIN%2CID%2CAU%2CBD%2CJP%2CHK%2CKR%2CVN%2CNZ%2CSG%2CPH%2CTH%2CPK&values=6397%2C6253%2C4027%2C2956%2C2022%2C1267%2C1180%2C1149%2C1049%2C694%2C682%2C656%2C604%2C470&title=ASNs%20per%20country%20%28APNIC%20region%29&unit=ASNs
```

<a href="https://x-cmd.com/draw?type=hbar&labels=CN%2CIN%2CID%2CAU%2CBD%2CJP%2CHK%2CKR%2CVN%2CNZ%2CSG%2CPH%2CTH%2CPK&values=6397%2C6253%2C4027%2C2956%2C2022%2C1267%2C1180%2C1149%2C1049%2C694%2C682%2C656%2C604%2C470&title=ASNs%20per%20country%20%28APNIC%20region%29&unit=ASNs"><img src="https://x-cmd.com/draw?type=hbar&labels=CN%2CIN%2CID%2CAU%2CBD%2CJP%2CHK%2CKR%2CVN%2CNZ%2CSG%2CPH%2CTH%2CPK&values=6397%2C6253%2C4027%2C2956%2C2022%2C1267%2C1180%2C1149%2C1049%2C694%2C682%2C656%2C604%2C470&title=ASNs%20per%20country%20%28APNIC%20region%29&unit=ASNs" width="780" alt="address-space-and-asns-count"></a>

Two shapes, two different stories:

- **China holds more than half the region's IPv4 space** (214.7M of ~350M addresses in this top-14 cut) but only 6,397 ASNs — a large address pool with relatively few routing operators. Large allocations, concentrated.
- **India is the mirror image**: 6,253 ASNs — nearly as many as China — on 26.3M addresses. Many autonomous systems, fewer addresses each. That is what a large, fragmented ISP market looks like.
- **Japan (1,267) and Korea (1,149)** run even denser than India relative to their address space.
- **Bangladesh (2,022 ASNs)** and **Indonesia (4,027)** run the same pattern at a smaller scale.

The ratio of ASNs to addresses is a decent proxy for how fragmented a country's routing market is — and therefore how much of the internet's *control plane* sits there.

> Scope note: this cut is the APNIC region, because APNIC publishes per-country rows in the same delegated export. ARIN and RIPE NCC publish per-country rows in separate files.
> Reproduce: `curl -s https://ftp.apnic.net/stats/apnic/delegated-apnic-latest`, sum the `value` column of each `<cc>|ipv4|<start>|<count>|...` row.
