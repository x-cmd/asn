---
name: asn
description: Address space and autonomous system allocation data, derived from the five RIR delegated statistics files. Use when the user asks about "IP allocation", "ASN", "autonomous system", "RIR", "address space", "which country has the most IPs", "how many ASNs does X have", or wants per-country IPv4 / ASN counts.
metadata: type=reference, source=rir-delegated-stats, schema=yaml-2-doc, refresh=manual, license=apache-2.0, scope=ip-asn-allocation
---

# x-cmd/asn — reading the allocation data

Two consumption paths. Pick whichever fits.

## 1. Read the YAML (no install)

```sh
# RIR-level allocation counts (IPv4 blocks + ASNs)
curl -fsSL https://raw.githubusercontent.com/x-cmd/asn/main/data/rir-allocation.yaml

# Per-country, APNIC region (IPv4 addresses + ASNs)
curl -fsSL https://raw.githubusercontent.com/x-cmd/asn/main/data/apnic-country.yaml
```

```yaml
rir:
  - registry: RIPE NCC
    region: Europe, Middle East, Central Asia
    ipv4_allocations: 100774
    asns: 40114
```

## 2. Use the `x ip` module

```bash
x ip cidr 10.0.0.1/24       # CIDR / special-use ranges
x ip addr                     # local interfaces
x ip map                    # local address map
x ip -h                     # full help
```

## Rebuilding from source

```sh
# The delegated exports are the authoritative source
curl -s https://ftp.ripe.net/pub/stats/ripencc/delegated-ripencc-latest
curl -s https://ftp.apnic.net/stats/apnic/delegated-apnic-latest

# Take the *|ipv4|*|<n>|summary and *|asn|*|<n>|summary rows for the RIR table,
# and sum the value column of each <cc>|ipv4|<start>|<count>|... row for countries.
```

## Turning any of it into a chart

Every figure on the site is a plain draw URL. Hand the numbers over:

```
https://x-cmd.com/draw?type=hbar&labels=CN,JP,IN&values=214719744,54388992,26313216&title=IPv4%20by%20country
```

## Watch out

- **Allocations are not addresses.** A registry that issued 100,774 blocks may have
  issued larger blocks than one that issued 58,336. Compare the `value` sums, not
  the row counts, when the question is about address space.
- **Delegated is not routed.** A large share of allocated IPv4 is never announced
  in BGP.
- **Per-country data is the APNIC region only** — APNIC is the one registry that
  publishes per-country rows inside the same export.
- The ASN-to-address ratio is the interesting derived number: it measures how
  fragmented a region's routing market is.
