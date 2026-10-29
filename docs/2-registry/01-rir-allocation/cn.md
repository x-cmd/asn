# 谁在分配什么，以及这种形状意味着什么

## 两种资源，两种不同的形状

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

把两张并排放，两条曲线**形状并不相同** —— 这是第一件值得注意的事。

## 怎么读这个对照

**RIPE NCC 分配的 ASN 最多（40,114），IPv4 块也最多（100,774）。** 这是全世界路由环境最密的地方：基本上一个 ISP 或一个大型企业网络就对应一个自治系统。欧洲、中东、中亚挤满了拥有自己 ASN 的中型网络，这也正是早期商业互联网和各国运营商成长起来的地方。

**ARIN 的 ASN 比 RIPE 少（33,123），IPv4 块却几乎一样多（81,326）。** 块更大，运营者更少。北美的模式不一样：少数几个非常大的网络 —— 一级传输商、超大规模云 —— 各自握着巨大的地址空间，而长尾从它们那里租。

**LACNIC 的 ASN 比 APNIC 还多（16,128 对 13,981），IPv4 块却只有 APNIC 的三分之一。** 拉美在一个相对小的地址池上做了很细的路由划分：自治系统多，每个能分到的地址少。这是一个 ISP 市场有竞争、而地址供给有限的区域的形状。

**AFRINIC 的数字最小但并不小到可以忽略：5,558 个块、2,821 个自治系统。** 对一个十多亿人的大陆来说，这是很薄的 —— 也是区域注册机构为何存在的最有力论据。AFRINIC 成立之前（2005 年），非洲的网络必须从几千公里之外的 ARIN 拿到地址空间，走一套为北美机构设计的流程。

## 为什么 ASN 数量是更有信息量的那个数字

分配计数只是代理指标；真正有意思的故事在**有多少个自治系统**里，因为 ASN 是一个路由决策，不只是一条记账记录。

- 一个 ASN 是在某个网络打算广播自己的路由时发出的。某个区域的 ASN 越多，独立路由的网络就越多 —— 路由多样性更高，失效点更多，流量走上意外路径的地方也更多。
- 反过来，ASN 少、块大的区域路由拓扑更扁平：效率更高，但也更脆弱 —— 上游出一次问题，影响的地址空间份额就很大。
- **ASN 与地址数的比值是碎片化程度的代理指标。** RIPE 大约是每 0.4 个块一个 ASN，LACNIC 大约 0.8，AFRINIC 大约 0.5。裸计数低估了这些区域之间结构上的差异。

## 二级市场不在这两张图里

关于 IPv4 分配，有一件最大的事在上面这些计数里看不见：**地址空间会流动。** 大约在 2015 到 2022 年间，转让 IPv4 块的市场每年价值数十亿美元，大块地址在不同 RIR 之间易手 —— 通常是从有富余的地区流向有需求的地区。

delegated 文件记录的是那些转让的*结果*（显示当前持有情况），而不是流动本身。但读任何分配图表时都值得记住：**这些数字是一个市场的快照，而不是一份历史遗产。** ARIN 在 IPv4 上的主导地位，不是因为 1990 年北美分到了更多，而是因为块被转走了。

## 注意事项

- **这些是分配条目数，不是地址数。** 发出 100,774 个块的注册机构，发的块可能比发出 58,336 个的更大。要直接比较地址空间，得把 `value` 列加起来；而且那量的是*已委派*空间，而不是*已路由*空间 —— 已委派的 IPv4 里有相当大一部分从来没有在 BGP 里被宣告过。
- **ASN 不等于网络。** 一个机构可以持有许多 ASN（在多地运营的云服务商常有几十个）；一个 ASN 也可以覆盖许多机构（LIR 分配给客户）。
- **数字在动。** 委派每天都在发生。

> 复现：`curl -s https://ftp.ripe.net/pub/stats/ripencc/delegated-ripencc-latest`
> （apnic / arin / lacnic / afrinic 同理），然后取 `*|ipv4|*|<n>|summary`
> 和 `*|asn|*|<n>|summary` 两行。
