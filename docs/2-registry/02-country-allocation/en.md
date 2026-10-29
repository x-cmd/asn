# Address space and autonomous systems, country by country

## A caveat that comes before the chart

"A country's IPv4 space" is a slippery phrase, and it is worth being precise about what it means.

Address space is allocated **down a chain**: from an RIR to a national registry to an ISP to a customer. What the delegated files record is the link *from the registry to the next holder down*, which in APNIC's export is tagged with a country code — the country of the **registry's customer**, not the country where the addresses are used.

So the number for a country is: **the address space that the region's registry has delegated to organisations registered in that country.** It is a fact about where the allocation paperwork sits, and only loosely a fact about where the machines are.

The same applies to ASNs, and it applies even more strongly: an ASN is registered to an organisation, and that organisation may announce from anywhere in the world.

## IPv4 address space by country (APNIC region)

```text
https://x-cmd.com/draw?type=hbar&labels=China%2CSouth%20Korea%2CJapan%2CIndia%2CSingapore%2CAustralia%2CTaiwan%2CIndonesia%2CVietnam%2CHong%20Kong%2CThailand%2CPakladesh%2CPhilippines%2CMalaysia%2CNew%20Zealand%2CBangladesh&values=214719744%2C57649152%2C54393088%2C26313216%2C21224704%2C19878912%2C19008000%2C12987136%2C12063744%2C6200576%2C4043264%2C3815424%2C3424512%2C3337728%2C2222848%2C1478656&title=IPv4%20address%20space%20by%20country%20%28APNIC%20region%29&unit=addresses
```

<a href="https://x-cmd.com/draw?type=hbar&labels=China%2CSouth%20Korea%2CJapan%2CIndia%2CSingapore%2CAustralia%2CTaiwan%2CIndonesia%2CVietnam%2CHong%20Kong%2CThailand%2CPakladesh%2CPhilippines%2CMalaysia%2CNew%20Zealand%2CBangladesh&values=214719744%2C57649152%2C54393088%2C26313216%2C21224704%2C19878912%2C19008000%2C12987136%2C12063744%2C6200576%2C4043264%2C3815424%2C3424512%2C3337728%2C2222848%2C1478656&title=IPv4%20address%20space%20by%20country%20%28APNIC%20region%29&unit=addresses"><img src="https://x-cmd.com/draw?type=hbar&labels=China%2CSouth%20Korea%2CJapan%2CIndia%2CSingapore%2CAustralia%2CTaiwan%2CIndonesia%2CVietnam%2CHong%20Kong%2CThailand%2CPakladesh%2CPhilippines%2CMalaysia%2CNew%20Zealand%2CBangladesh&values=214719744%2C57649152%2C54393088%2C26313216%2C21224704%2C19878912%2C19008000%2C12987136%2C12063744%2C6200576%2C4043264%2C3815424%2C3424512%2C3337728%2C2222848%2C1478656&title=IPv4%20address%20space%20by%20country%20%28APNIC%20region%29&unit=addresses" width="780" alt="address-space-and-autonomous"></a>

## ASNs per country (APNIC region)

```text
https://x-cmd.com/draw?type=hbar&labels=China%2CSouth%20Korea%2CJapan%2CIndia%2CSingapore%2CAustralia%2CTaiwan%2CIndonesia%2CVietnam%2CHong%20Kong%2CThailand%2CPakladesh%2CPhilippines%2CMalaysia%2CNew%20Zealand%2CBangladesh&values=6397%2C1149%2C1267%2C6253%2C682%2C2956%2C419%2C4027%2C1049%2C1180%2C604%2C470%2C656%2C390%2C694%2C2022&title=Autonomous%20systems%20per%20country%20%28APNIC%20region%29&unit=ASNs
```

<a href="https://x-cmd.com/draw?type=hbar&labels=China%2CSouth%20Korea%2CJapan%2CIndia%2CSingapore%2CAustralia%2CTaiwan%2CIndonesia%2CVietnam%2CHong%20Kong%2CThailand%2CPakladesh%2CPhilippines%2CMalaysia%2CNew%20Zealand%2CBangladesh&values=6397%2C1149%2C1267%2C6253%2C682%2C2956%2C419%2C4027%2C1049%2C1180%2C604%2C470%2C656%2C390%2C694%2C2022&title=Autonomous%20systems%20per%20country%20%28APNIC%20region%29&unit=ASNs"><img src="https://x-cmd.com/draw?type=hbar&labels=China%2CSouth%20Korea%2CJapan%2CIndia%2CSingapore%2CAustralia%2CTaiwan%2CIndonesia%2CVietnam%2CHong%20Kong%2CThailand%2CPakladesh%2CPhilippines%2CMalaysia%2CNew%20Zealand%2CBangladesh&values=6397%2C1149%2C1267%2C6253%2C682%2C2956%2C419%2C4027%2C1049%2C1180%2C604%2C470%2C656%2C390%2C694%2C2022&title=Autonomous%20systems%20per%20country%20%28APNIC%20region%29&unit=ASNs" width="780" alt="address-space-and-autonomous"></a>

## Two shapes, two different stories

**Both charts above list the same countries in the same order — sorted by IPv4 address space.** That is deliberate, and it is the point of showing them side by side: a country that sits near the top of one chart and far down the other is telling you something. Read them as a pair, not as two separate rankings.

**China holds more than half the region's IPv4 space** — 214.7M addresses against roughly 350M in this cut — but only 6,397 ASNs. A very large address pool, held by a comparatively small number of routing operators. Large allocations, concentrated: this is the shape of a market where a handful of large carriers and clouds hold the space and everything else rents from them.

**India is the mirror image.** 6,253 ASNs — nearly as many as China — on 26.3M addresses. It sits **fourth by address space and second by autonomous systems**. Many autonomous systems, fewer addresses each: a fragmented ISP market of thousands of regional operators, each with a modest block and its own routing.

**Indonesia makes the same point more sharply** — 4,027 ASNs on 13.0M addresses. Eighth by address space, third by autonomous systems.

**Bangladesh is the extreme case.** 2,022 ASNs on 1.5M addresses: sixteenth by address space, yet by ASN count it is ahead of Japan, South Korea and Taiwan **combined**. A young, competitive ISP market with very little address space to go around.

**At the other end of the same axis sit Japan (1,267), Taiwan (419) and South Korea (1,149)** — tens of millions of addresses each, held by only a few hundred or a thousand autonomous systems. These are the *least* fragmented routing markets in the region: a handful of large carriers hold the space and route it themselves.

## The ratio is the interesting part

Put the two charts together and a third number falls out: **ASNs per address**. It is a rough proxy for how fragmented a country's routing market is, and therefore for how much of the internet's *control plane* sits there.

- China: ~30 ASNs per million addresses.
- India: ~238 per million.
- Japan: ~23 per million.
- Bangladesh: ~1,367 per million — the most fragmented in the region.

India's routing market is roughly **eight times more fragmented** than China's, per address. That has real consequences: more independent operators means more diverse paths (good for resilience), but also more places where a routing mistake can blackhole traffic (bad), and a much larger surface of independently-operated infrastructure to keep patched.

## Why "where" is the wrong question for ASNs

Because an ASN belongs to an organisation and an organisation operates globally, the per-country ASN counts above describe **where the operator is registered**, not where its network is. A cloud provider registered in one country with ASNs in that country may originate prefixes for every continent.

The honest way to use the ASN chart is therefore about **structure**: how many independent networks does a country's address space belong to, and therefore how many organisations are in a position to make routing decisions about it. That is a governance question, and it is one worth asking.

## Caveats

- **APNIC region only.** This cut covers the Asia-Pacific registry, because APNIC is the registry that publishes per-country rows inside the same delegated export. ARIN and RIPE NCC publish them in separate files, and the country coverage there is different.
- **IPv6 is absent.** The same export's IPv6 rows are summary-only.
- **Delegated ≠ routed.** A substantial share of allocated IPv4 is never announced in BGP; those addresses exist on paper only.
- **Snapshot.** 2026-10-29.

> Reproduce: `curl -s https://ftp.apnic.net/stats/apnic/delegated-apnic-latest`,
> then sum the `value` column of every `<cc>|ipv4|<start>|<count>|...` row
> (and the same for `<cc>|asn|...` rows).
