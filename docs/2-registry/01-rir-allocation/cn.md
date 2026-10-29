# 谁在分配什么

**各 RIR 的 IPv4 分配条目数** —— 每个注册机构已经委派出去的 IPv4 块数量。

```text
https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CAPNIC%2CLACNIC%2CAFRINIC&values=100774%2C81326%2C58336%2C20450%2C5558&title=IPv4%20allocations%20by%20RIR&unit=blocks
```

<a href="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CAPNIC%2CLACNIC%2CAFRINIC&values=100774%2C81326%2C58336%2C20450%2C5558&title=IPv4%20allocations%20by%20RIR&unit=blocks"><img src="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CAPNIC%2CLACNIC%2CAFRINIC&values=100774%2C81326%2C58336%2C20450%2C5558&title=IPv4%20allocations%20by%20RIR&unit=blocks" width="780" alt=""></a>

**各 RIR 分配出的 ASN 数量** —— 每个注册机构发出了多少个自治系统。

```text
https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CLACNIC%2CAPNIC%2CAFRINIC&values=40114%2C33123%2C16128%2C13981%2C2821&title=ASNs%20allocated%20by%20RIR&unit=ASNs
```

<a href="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CLACNIC%2CAPNIC%2CAFRINIC&values=40114%2C33123%2C16128%2C13981%2C2821&title=ASNs%20allocated%20by%20RIR&unit=ASNs"><img src="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CLACNIC%2CAPNIC%2CAFRINIC&values=40114%2C33123%2C16128%2C13981%2C2821&title=ASNs%20allocated%20by%20RIR&unit=ASNs" width="780" alt=""></a>

把两张放在一起看，一个结构性差异就显出来了：

- **RIPE NCC 分配的 ASN 最多**（40,114）—— 路由环境最密，通常一个 ISP 或大型网络就占一个 ASN。
- **ARIN 的 ASN 比 RIPE 少，IPv4 空间却多得多** —— 81,326 个块分布在 33,123 个自治系统里。分配块更大，运营者更少。
- **LACNIC 的 ASN 比 APNIC 还多**（16,128 对 13,981），手头的 IPv4 空间却只有 APNIC 的五分之一 —— 拉美在一个很小的地址池上做了很细的路由划分。
- **AFRINIC 的数字最小，但并不小到可以忽略** —— 十几亿人的大陆只有 2,821 个自治系统，这个厚度本身就是区域注册机构最有力的论据。

```
RIPE NCC   100774 ipv4   40114 asn
ARIN        81326 ipv4   33123 asn
LACNIC      20450 ipv4   16128 asn
APNIC       58336 ipv4   13981 asn
AFRINIC      5558 ipv4    2821 asn
```

> 复现：`curl -s https://ftp.ripe.net/pub/stats/ripencc/delegated-ripencc-latest`（apnic / arin / lacnic / afrinic 同理），
> 然后取 `*|ipv4|*|<n>|summary` 与 `*|asn|*|<n>|summary` 两行。
