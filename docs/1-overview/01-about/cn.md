# 关于本页

互联网不是一个地址空间 —— 它是**五个区域注册机构**，而它们之下是几十万个各自广播路由的**自治系统**。

| 注册机构 | 区域 | IPv4 分配条目 | ASN |
|---|---|---|---|
| RIPE NCC | 欧洲、中东、中亚 | 100,774 | 40,114 |
| ARIN | 北美 | 81,326 | 33,123 |
| LACNIC | 拉丁美洲与加勒比 | 20,450 | 16,128 |
| APNIC | 亚太 | 58,336 | 13,981 |
| AFRINIC | 非洲 | 5,558 | 2,821 |

*（分配条目数取自各 RIR 的 delegated 统计文件，2026-10-29）*

看图之前有两件事值得先说清楚：

**RIPE NCC 发出的 ASN 最多，但 ARIN 每个 ASN 摊到的 IPv4 空间大得多。** ARIN 的一个 ASN 要覆盖一整块大陆；RIPE 的一个 ASN 只覆盖更小的区域，却被切得更碎。地址空间和路由自主性不是一回事。

**AFRINIC 在两列里都最小，而这恰恰是区域注册机构存在的意义** —— 非洲的 ISP 不必离开本大陆就能拿到地址段。

这里的一切都基于各 RIR 自己的 `delegated-*-latest` 导出文件 —— 也就是注册机构的权威数据源，而不是第三方镜像。

> CLI：`x ip calc` · `x ip cidr` · `x ip map` —— 见 [x-cmd.com/mod/ip](https://x-cmd.com/mod/ip)
> 数据：[ftp.ripe.net](https://ftp.ripe.net/pub/stats/ripencc/) · [ftp.apnic.net](https://ftp.apnic.net/stats/apnic/) · [ftp.arin.net](https://ftp.arin.net/pub/stats/arin/) · [ftp.lacnic.net](https://ftp.lacnic.net/pub/stats/lacnic/) · [ftp.afrinic.net](https://ftp.afrinic.net/pub/stats/afrinic/)
