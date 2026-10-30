# 地址、自治系统，以及发放它们的那五个机构

## 两种数字，两个问题

大多数人想象互联网，就是一堆地址：1.1.1.1、192.168.1.1、`2606:4700:4700::1111`。地址说明一个包该送到哪里。它是层级化的标识符，以块的形式层层下发，也是真正出现在网络线缆上的东西。

但路由根本不用地址工作。路由器的表里不写「到 1.1.1.1」，写的是「到 AS13335，经由这个邻居」。底下还藏着第二套编号体系，就是**自治系统号**。一个 ASN 标识一个网络，通常是某个机构，它向全球 BGP 系统广播自己的路由。互联网上每一个 IP 前缀，都恰好属于某一个 ASN。

这个区分能解释不少看起来莫名其妙的事。地址空间可以买卖、转让和转售，ASN 不行，因为它是标识符而不是资源，一直跟着注册它的那家走。一个 ASN 可以持有海量地址，而一个地址可以属于某个 ASN，这个 ASN 自己又是个向成千上万家客户转售的 ISP。同一座城市里的两个网络可以分属不同 ASN，两个 ASN 也可以在每个国家都有地址。

## 分配的链条

两种数字都不会直接落到最终用户手里：

```
IANA  ──►  区域互联网注册机构 (RIR)  ──►  国家/地区注册机构 (NIR 或 LIR)  ──►  ISP  ──►  最终客户
```

IANA 持有地址空间的顶端，委派给五个 RIR，真正持有资源的正是这五家。多数 RIR 再分配给国家或地区注册机构，后者分配给大型 ISP，ISP 再分配给客户。链条上每一环都是一次委派，每一次委派都有记录。delegated 统计文件就是这么来的，这一页也是从它们算出来的。

## 五个注册机构

| 注册机构 | 区域 | IPv4 分配条目 | ASN |
|---|---|---|---|
| RIPE NCC | 欧洲、中东、中亚 | 100,774 | 40,114 |
| ARIN | 北美 | 81,326 | 33,123 |
| LACNIC | 拉丁美洲与加勒比 | 20,450 | 16,128 |
| APNIC | 亚太 | 58,336 | 13,981 |
| AFRINIC | 非洲 | 5,558 | 2,821 |

*（分配条目数取自各 RIR 的 delegated 统计文件，快照 2026-10-29）*

RIPE NCC 两种资源都发得最多，这与其说是关于欧洲的事，不如说是关于互联网如何被建造、又如何被重组。IPv4 的分配史由转让和再委派主导，最后 RIPE NCC 握住了两种资源中最大的份额。

ARIN 的分配条目数排第二，但每个 ASN 摊到的地址空间排第一：块更大，运营者更少。

AFRINIC 两项都最小，区域注册机构存在的理由就在这儿。一个十多亿人的大陆只有 2,821 个自治系统、只分到很小一份地址空间，在互联网控制面里是薄薄的一条。区域注册机构的存在，就是为了让分配不必跨越大洋。

## 为什么 IPv6 改变了这套算术

IPv4 大约 43 亿个地址，IPv6 大约 (3.4 	imes 10^{38}) 个，大到我们惯用的「稀缺」这套思考方式根本用不上了。关于 IPv6 有趣的问题不再是「够不够」，而是「为什么用得这么少」，而那是一个部署与路由问题，不再是分配问题。

这一页留在 IPv4，因为分配的压力在那里才看得见。

## 这件事已经有人做了

问「这个 IP 是谁的」「这个 ASN 是谁」，不难。现成的服务有四大家族，各自回答的问题并不一样。知道什么时候该用哪一个，能省下不少时间。

### 注册机构自己

五个 RIR 是权威源头，东西本来就是他们分出去的。每家都提供 whois、RDAP，以及一份机器可读的全量导出。

- [RIPE whois](https://www.ripe.net/manage-ips-and-asns/db/) 与 [rdap.db.ripe.net](https://rdap.db.ripe.net/)，欧洲、中东、中亚
- [ARIN RDAP](https://rdap.arin.net/registry/ip/8.8.8.8) 与 `whois.arin.net/rest/net/…`，北美
- [APNIC whois](https://wq.apnic.net/)，亚太。注意参数是 `?searchtext=`，不是 `?ip=`
- LACNIC 与 AFRINIC，结构相同

whois 是这几家里最老也最耐用的：一个 telnet 端口、一套查询语法，输出是给人读的。RDAP 是它的继任者，同样的信息，走 HTTP 和 JSON，而且是 IANA 强制的标准。

第三种选择几乎没人用，却恰恰是这一页最感兴趣的东西：各 RIR 发布的 `delegated-*-latest` 文件。机器可读、完整、每天更新，不要 key，也没有限流。RIPE 的 18 MB，ARIN 13 MB，APNIC 9 MB。

### 路由数据聚合

这一族回答的是另一个问题：此刻实际在宣告路由的是谁。

| 服务 | 有什么 | 要 key | 备注 |
|---|---|---|---|
| [RIPEstat](https://stat.ripe.net/) | 60+ 数据集：路由表规模、宣告前缀、ASN 概览、邻居及其历史、RPKI 校验、CAIDA 关系、地址转让记录 | 不要 | 免费，RIPE 自己的原话是「无请求总量限制」。每 IP 8 个并发 |
| [CAIDA AS Rank](https://asrank.caida.org/) | 按流量给 AS 排名，cone 拆成 provider/peer/customer，还有 AS 关系图 | 不要 | GraphQL v2，122,962 个 AS |
| [RouteViews](https://www.routeviews.org/) | 全球路由表快照与 MRT 原始转储，可回溯到 2001 年 | 不要 | JSON API。游客 1 req/s，带 key 10 req/s |
| [PeeringDB](https://www.peeringdb.com/api/net) | 有哪些网络、怎么互联。35,779 个网络、1,332 个 IXP | 读不要 | 匿名 20 次/分 |
| [RADb](https://www.radb.net/) | IRR 对象库，RPKI 的 ROA 也镜像在里面 | whois 不要 | 43 端口的 whois 免费且匿名；REST 搜索 API 是每年 595 美元的订阅 |
| [bgp.tools](https://bgp.tools/) | 全网最好用的 ASN 页面：前缀、上下游、peer、RPKI、IRR | 交互页需要 | 见下 |
| [bgp.he.net](https://bgp.he.net/) | Hurricane Electric 的工具箱，另有 11 份互联网统计报告 | 不要 | 见下 |

有两家值得单独说一句。

**bgp.tools 的交互页有登录墙**，`/as/`、`/prefix/`、`/api/query.js` 全都跳 `/login?must-login`。但有三个数据端点仍然免认证返回：`bgp.tools/table.jsonl`（23.5 MB、451,159 行）、`bgp.tools/asns.csv`（4.5 MB），以及 `map.bgp.tools/`，一个可以平移缩放的完整 IPv4 空间地图。他们的 robots.txt 对 ClaudeBot、GPTBot、OAI-SearchBot 等一律封杀，所以这个站点不碰他们的交互层和 API。

**bgp.he.net 的图其实画得不错**，六张图，外加一张服务端渲染的 AS-path 连通图。但它没有官方 API，没有批量导出，数据许可也不明确，已经有第三方在商业化抓取。要 BGP 数据，用 RIPEstat、RADb 和 RouteViews 替代。

这一族里还有一个容易被忽略的：[iptoasn.com](https://iptoasn.com/) 把完整的 IP→ASN 映射做成下载文件，每小时刷新。它的公共 API 在 2020 年就下线了，但文件还在，依然免费。

### 商业 IP 数据 API

要「这个 IP 在哪、属于谁」，走这一族最快。

| 服务 | 要 key | 免费额度 | 返回什么 |
|---|---|---|---|
| [ip.guide](https://ip.guide/) | 不要 | 无公布上限 | 国家、城市、ASN、org、经纬度，四段齐全 |
| [ipwho.is](https://ipwho.is/8.8.8.8) | 不要 | 每客户端 IP 每天 1,000 次 | 国家、城市、ASN、ISP、经纬度、时区、货币 |
| [ip-api.com](http://ip-api.com/json/8.8.8.8) | 不要 | 每 IP 每分钟 45 次 | 国家、地区、城市、ISP、AS、AS 名、时延。**仅 HTTP、仅非商用** |
| [ipinfo.io](https://ipinfo.io/8.8.8.8/json) | Lite 版需免费 token | Lite 无限；老的免 token 接口 5 万/月，返回字段更少 | ASN、org、地理位置、邮编、时区、公司 |
| [ipdata.co](https://ipdata.co/) | `ipdata.free` 可免 key | 城市级每天 1,000 次；国家和 ASN 无限 | ASN、org、城市 |
| [ipapi.co](https://ipapi.co/) | 需用工作邮箱注册 | 试用 3 万/月，但每天不超过 1,000 | ASN 与 org |

各家返回的字段几乎一样，因为底层读的是同一批 GeoIP 数据库。

### 命令行

这个领域最常被引用的实现是 [github.com/nitefood/asn](https://github.com/nitefood/asn)：1,944 星的 Bash 脚本，MIT 许可，2026 年 10 月还在更新，Alpine、Debian、Homebrew、Nix、AUR、FreeBSD 都打包了它。它做的事比这里任何工具都多，AS 号、前缀、ISP、国家、上游链，每一跳都做 RPKI 校验，16 个命令行选项，背后接了 18 个数据源。

代价是八个系统包：`curl whois bind9-host mtr-tiny jq ipcalc grepcidr nmap ncat aha`。

其他几个值得一试：

| 工具 | 语言 | Star | 做什么 |
|---|---|---|---|
| [nexttrace](https://github.com/nxtrace/NTrace-core) | Go | 8,254 | 这一领域 star 最高的工具。traceroute，每一跳都给 ASN 和位置；`--from<国家>` 通过 Globalping 选探针 |
| [ttl](https://github.com/lance0/ttl) | Rust | 1,489 | 「更好的 mtr」，能识别 ECMP、能解 MPLS 标签 |
| [asnmap](https://github.com/projectdiscovery/asnmap) | Go | 1,145 | 反方向：ASN 或 org → CIDR。现在需要 ProjectDiscovery 的 API key |
| [asname](https://github.com/flyingllama87/asname) | Go | 新 | 离线 LC-trie，IP 或主机名 → ASN 与 AS 名，`--rest` 还能起服务 |
| [aslookup](https://github.com/nieldk/aslookup) | C | 新 | 零外部命令依赖，只要 libcurl 和 libcjson |

而你机器上早就有的那个 `whois`，其实最被低估。对着 Team Cymru 查，免费、永久、无需注册，就能拿到完整记录：

```sh
whois -h whois.cymru.com " -v 8.8.8.8"
# 15169 | 8.8.8.0/24 | US | arin | 2023-12-28 | GOOGLE - Google LLC, US
```

Team Cymru 也支持 DNS（TXT 记录）和 bulk 模式。它给不了城市级的地理信息，而且它的国家码来自注册机构记录，不是地址实际被使用的地方，两者有时差得很远。

### 共同的缺口

把四大家族放在一起看，同样的缺口在每一家里都出现。

**它们都是一次问一句的工具。** 每一家都是「给我一个输入，还你一条记录」。而「法国的 IPv4 空间长什么样」「哪个国家的路由市场最碎」不是它们任何一个能接受的查询，你得把几千行 whois 输出拽进表格工具里自己算。

**输出是表格或 JSON，从来不是一张图。** 这是数据模型决定的，不是偷懒：一次查询返回一条记录，而记录本身没有值得画的形状。

**权威数据又放在别的地方。** 唯一批量、机器可读、权威的源是各 RIR 的 delegated 文件，用它就得自己写解析器。

还有一点得说实话：做 AS 级拓扑图的站点并不少，Internet Health Report、BGPTrace、as-graph、bgpmap.net、APNIC REx、ISOC Pulse、CleanIP、SecViz，还有 CAIDA 自己的 AS Core 和 AS Reach。RIPEstat 一家就提供 86 个可嵌入的 widget。所以缺口不在「没人画」。

缺口在于：那些全是交互式应用。改一个参数就得到一个新页面，**你永远拿不到当前这张图的 URL**。而唯一把分布形状做对了的产品，ipinfo 的「Summarize IPs」，只服务于你自己粘进去的那串地址，没有一个版本是给「这个 RIR 的地址结构长什么样」用的。

而图表 URL 恰恰就是这个。`type=hbar` 改成 `type=pie`，同一份数据换个形状；换掉查询，就是换个问题。**参数就是问题本身，而且它一直留在地址栏里。**

### 一条需要知道的边界

draw 一共十三种图：`pie bar hbar line area radar gauge scatter combo sankey qr archmap mindmap`。其中没有力导向的节点连线图，也就是说 AS-path 拓扑图我们不自己画，交给 bgpmap.net，它生成的 Graphviz 图直接嵌进来就行。

`sankey` 才是有意思的那个，因为 AS 关系数据天生就是这个形状。CAIDA 每月发布一次，格式是 `provider|customer|-1` 和 `peer|peer|0`，一个带三分法的有向图，而三分法正是 sankey 画的。

### 三个值得收藏的源

| 源 | 要 key | 能拿到什么 |
|---|---|---|
| [bgpmap.net](https://bgpmap.net/) | 不要，免注册 | `POST /api/v1/snapshots {"resource":"1.1.1.0/24"}` 返回 origin AS、upstream、RPKI 状态，以及现成的 Graphviz SVG 与 PNG。批量一次最多 100 个 |
| [api.routeviews.org](https://api.routeviews.org/) | 可选 | `/asn/{n}`、`/prefix/{cidr}`（含上报 peer 与 AS 路径）、`/rib/collectors`、`/collector/`（含坐标） |
| [CAIDA AS Relationships](https://publicdata.caida.org/datasets/as-relationships/serial-1/) | 不要，公开下载 | 每月一份，1998-01 至今，AS 关系边表，画 sankey 的原料 |

## 只想查一条记录？用这些

| 你要的 | 用什么 | 要 key？ |
|---|---|---|
| 这个 IP 的登记记录 | 各 RIR 的 whois / RDAP | 不要 |
| 这个 IP 在哪、属于谁、ASN 多少 | [ip.guide](https://ip.guide/) · [ipwho.is](https://ipwho.is/) · [ipinfo.io](https://ipinfo.io/) | 不要 |
| 这个 ASN 有多大、排第几 | [CAIDA AS Rank](https://asrank.caida.org/) | 不要 |
| 这个前缀谁在宣告、RPKI 状态如何 | [RIPEstat](https://stat.ripe.net/) · [RouteViews](https://www.routeviews.org/) | 不要 |
| 到这个 IP 的 AS 路径长什么样 | [bgpmap.net](https://bgpmap.net/) · [BGPTrace](https://bgptrace.com/) | 不要 |
| 网络与 IXP 怎么互联 | [PeeringDB](https://www.peeringdb.com/) · [RADb](https://www.radb.net/) | 不要 |

这一页做的是另一件事。上面那些工具给你一条记录，这里给你一片分布。

## 怎么自己加一页

这个站点的全部机制就是一件事：把数据变成一条把参数写出来的 URL。

```sh
# 1. 拿到数据
curl -s https://ftp.apnic.net/stats/apnic/delegated-apnic-latest

# 2. 变成图表 URL，参数是明文的，AI 也能拼
```

```text
https://x-cmd.com/draw?type=hbar&labels=CN,JP,IN&values=214719744,54388992,26313216&title=IPv4%20by%20country
```

<a href="https://x-cmd.com/draw?type=hbar&labels=CN,JP,IN&values=214719744,54388992,26313216&title=IPv4%20by%20country"><img src="https://x-cmd.com/draw?type=hbar&labels=CN,JP,IN&values=214719744,54388992,26313216&title=IPv4%20by%20country" width="780" alt=""></a>

```sh
# 3. 或者让 AI agent 来
#    用 draw skill：https://raw.githubusercontent.com/x-cmd/draw/main/SKILL.md
```

参数可以替换，正是整套东西的关键。把 `product:nginx` 换成 `org:你的公司`，同一个模板就成了你自己资产的那张图。所以这里每一页给的不只是成品图，**而是一个可以拆开改的模板**。

仓库结构、命名规则、路由怎么生成、CI 查什么，都写在 [CONTRIBUTING.md](https://github.com/x-cmd/asn/blob/main/CONTRIBUTING.md) 里；给 AI agent 的说明在 [SKILL.md](https://github.com/x-cmd/asn/blob/main/SKILL.md)。

## 数字从哪来

这里的一切都是从各 RIR 自己的 `delegated-*-latest` 导出文件算出来的，用的是权威的注册机构文件，不是第三方镜像。按国家的切法用 APNIC 区域，因为 APNIC 是在同一个导出里发布逐国家行的那一家，ARIN 和 RIPE NCC 另有文件。

有一条适用于本站每张表的注意事项。这些数字数的是分配条目，不是地址数。发出 100,774 个块的注册机构，发的块可能比发出更少块的那家更大。下一页把两种 RIR 资源并排放，这个区别就看得见了。

> CLI：`x ip cidr` · `x ip map` —— 见 [x-cmd.com/mod/ip](https://x-cmd.com/mod/ip)
> 数据：[ftp.ripe.net](https://ftp.ripe.net/pub/stats/ripencc/) · [ftp.apnic.net](https://ftp.apnic.net/stats/apnic/) · [ftp.arin.net](https://ftp.arin.net/pub/stats/arin/) · [ftp.lacnic.net](https://ftp.lacnic.net/pub/stats/lacnic/) · [ftp.afrinic.net](https://ftp.afrinic.net/pub/stats/afrinic/)
