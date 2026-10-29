# 谁在分配什么，以及这种形状意味着什么

**各 RIR 的 IPv4 分配条目数**，也就是每个注册机构已经委派出去的 IPv4 块数量。

```text
https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CAPNIC%2CLACNIC%2CAFRINIC&values=100774%2C81326%2C58336%2C20450%2C5558&title=IPv4%20allocations%20by%20RIR&unit=blocks
```

<a href="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CAPNIC%2CLACNIC%2CAFRINIC&values=100774%2C81326%2C58336%2C20450%2C5558&title=IPv4%20allocations%20by%20RIR&unit=blocks"><img src="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CAPNIC%2CLACNIC%2CAFRINIC&values=100774%2C81326%2C58336%2C20450%2C5558&title=IPv4%20allocations%20by%20RIR&unit=blocks" width="780" alt=""></a>

**各 RIR 分配出的 ASN 数量**，每个注册机构发出了多少个自治系统。

```text
https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CLACNIC%2CAPNIC%2CAFRINIC&values=40114%2C33123%2C16128%2C13981%2C2821&title=ASNs%20allocated%20by%20RIR&unit=ASNs
```

<a href="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CLACNIC%2CAPNIC%2CAFRINIC&values=40114%2C33123%2C16128%2C13981%2C2821&title=ASNs%20allocated%20by%20RIR&unit=ASNs"><img src="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CLACNIC%2CAPNIC%2CAFRINIC&values=40114%2C33123%2C16128%2C13981%2C2821&title=ASNs%20allocated%20by%20RIR&unit=ASNs" width="780" alt=""></a>

两张图并排放，形状并不一样。

RIPE NCC 两种都发得最多：40,114 个 ASN、100,774 个 IPv4 块。这是全世界路由环境最密的地方，一个大型 ISP 或企业网络通常就有一个自己的 ASN。欧洲、中东、中亚挤满了中型网络，早期的商业互联网和各国运营商也是在这儿长起来的。

ARIN 的 ASN 比 RIPE 少（33,123 对 40,114），IPv4 块却几乎一样多。北美的模式不一样：少数几个非常大的网络，一级传输商和超大规模云，各自握着巨大的地址空间，长尾从它们那儿租。

LACNIC 的 ASN 比 APNIC 多（16,128 对 13,981），IPv4 块却只有 APNIC 的三分之一。拉美在一个相对小的地址池上做了很细的路由划分，这正是一个 ISP 市场有竞争、而地址供给有限的地区该有的样子。

AFRINIC 最小，5,558 个块、2,821 个自治系统。对一个十多亿人的大陆来说是很薄的，这也是区域注册机构为何存在的最有力论据。AFRINIC 在 2005 年成立之前，非洲的网络要从几千公里外的 ARIN 拿地址空间，走一套为北美机构设计的流程。

## ASN 数量是更有信息量的那个数字

分配计数只是代理指标。真正有意思的故事在有多少个自治系统，因为 ASN 是一个路由决策，不只是一条记账记录。

一个 ASN 是在某个网络打算广播自己的路由时发出的。某个区域的 ASN 越多，独立路由的网络就越多，路由多样性更高，流量走上意外路径的地方也更多。ASN 少、块大的区域拓扑更扁平：效率更高，但也更脆弱，上游出一次问题就影响很大一块地址空间。

两者的比值是衡量碎片化程度的好指标。RIPE NCC 大约每 0.4 个块一个 ASN，LACNIC 约 0.8，AFRINIC 约 0.5。裸计数低估了这几个区域之间结构上的差异。

## 二级市场在图里看不见

关于 IPv4 分配，有件最大的事在上面这些计数里看不到：地址空间会流动。大约 2015 到 2022 年间，转让 IPv4 块的市场每年价值数十亿美元，大块地址在不同 RIR 之间易手，通常从有富余的地区流向有需求的地区。

delegated 文件记录的是转让的结果，因为它们显示当前持有情况，但不记录流动本身。读任何分配图表时都值得记住，这些数字是一个市场的快照，不是一份历史遗产。ARIN 在 IPv4 上的主导地位不是因为 1990 年北美分到了更多，部分原因是块被转走了。

## 注意事项

这些是分配条目数，不是地址数。发出 100,774 个块的注册机构，发的块可能比发出 58,336 个的那家更大。要直接比较地址空间，得把 `value` 列加起来；而且那量的是已委派空间，不是已路由空间，因为已委派的 IPv4 里有相当大一部分从来没在 BGP 里被宣告过。

ASN 和网络也不是一回事。一个机构可以持有许多个，在多地运营的云服务商常有几十个；一个 ASN 也可以覆盖许多机构，比如 LIR 分配给自己的客户。

数字在动。委派每天都在发生。

> 复现：`curl -s https://ftp.ripe.net/pub/stats/ripencc/delegated-ripencc-latest`，然后取 `*|ipv4|*|<n>|summary` 和 `*|asn|*|<n>|summary` 两行。
