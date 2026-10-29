# Addresses, autonomous systems, and the five registries that hand them out

## Two kinds of number, and they answer different questions

Most people who think about the internet picture it as a set of addresses: 1.1.1.1, 192.168.1.1, `2606:4700:4700::1111`. An address says **where a packet should be delivered**. It is a hierarchical identifier, handed down in blocks, and it is the thing that actually appears on the wire.

But routing the internet does not work with addresses at all. Routers do not carry "to 1.1.1.1" in their tables; they carry "to AS13335, via this neighbour". So there is a **second numbering system** underneath: the **Autonomous System Number (ASN)**. An ASN identifies a network — an organisation, usually — that announces its own routes into the global BGP system. Every IP prefix on the internet belongs to exactly one ASN.

The distinction is worth holding onto, because it explains a lot of otherwise confusing facts:

- **Address space can be bought, transferred and resold.** Autonomous system numbers cannot — an ASN is an identifier, not a resource, and it stays with the network that registered it.
- **One ASN can hold enormous numbers of addresses, and one address can belong to an ASN that is itself an ISP reselling to many customers.** The two numbering systems nest differently and are allocated by different chains.
- **Two networks in the same city can be in different ASNs**, and two ASNs can have addresses in every country.

## The allocation chain

Neither kind of number is allocated directly to the end user. There is a chain:

```
IANA  ──►  Regional Internet Registry (RIR)  ──►  National IR (or LIR)  ──►  ISP  ──►  end customer
```

The **IANA** holds the top of the address space and delegates to the five **RIRs**, which are the institutions that actually hold the resource. Most RIRs then allocate to **national or regional registries** (NIRs), which allocate to large ISPs, which allocate to their customers. Each link in the chain is a delegation, and each delegation is recorded — which is exactly why the delegated statistics files exist and why this page can be built from them.

## The five registries

| Registry | Region | IPv4 allocations | ASNs |
|---|---|---|---|
| RIPE NCC | Europe, Middle East, Central Asia | 100,774 | 40,114 |
| ARIN | North America | 81,326 | 33,123 |
| LACNIC | Latin America & Caribbean | 20,450 | 16,128 |
| APNIC | Asia-Pacific | 58,336 | 13,981 |
| AFRINIC | Africa | 5,558 | 2,821 |

*(allocation counts from the RIR delegated statistics files, snapshot 2026-10-29)*

**RIPE NCC hands out the most of both.** That is not a statement about Europe; it is a statement about how the internet was built and then reorganised. The allocation history of IPv4 has been dominated by transfers and re-delegations, and RIPE NCC has ended up holding the largest share of both resources.

**ARIN is second in allocations but first in address space per ASN** — a big block, fewer operators.

**AFRINIC is smallest in both columns, and that is the entire point of a regional registry.** A continent of a billion-plus people, with 2,821 autonomous systems and a small fraction of the address space, is a thin slice of the internet's control plane. Regional registries exist precisely so that allocation does not have to happen across an ocean.

## Why IPv6 changes the arithmetic

IPv4 has about 4.3 billion addresses. IPv6 has about (3.4 	imes 10^{38}) — a number so large that the usual way of thinking about scarcity stops applying at all. The interesting question about IPv6 stopped being "is there enough of it" and became "why is so little of it in use", which is a deployment and routing problem rather than an allocation problem.

This page stays on IPv4, because that is where the allocation pressure is visible.

## 同类的工具与网页：这个位置已经有多少人

查「这个 IP 属于谁」和「这个 ASN 是谁」这件事，有四条完全不同的技术路径，而每条路径上都已经挤满了服务。理清这四条路，才知道 x-cmd/asn 站在哪里。

### 第一条路：注册机构自己（权威，但难用）

五个 RIR 各自提供 whois、RDAP 和批量导出：

| 来源 | 提供什么 | 门槛 | 实测（2026-10-29） |
|---|---|---|---|
| [RIPE whois / RDAP](https://www.ripe.net/manage-ips-and-asns/db/) · [rdap.db.ripe.net](https://rdap.db.ripe.net/) | 该 RIR 分配的一切记录 | 免 key，但要懂查询语法 | ✅ 可用；RIPE DB 有明确配额（每 IP 1000/24h、最多 3 并发） |
| [ARIN RDAP](https://rdap.arin.net/registry/ip/8.8.8.8) · `whois.arin.net/rest/net/…` | 北美的地址段与 ASN | 免注册 | ✅ 可用；官方 PPM 写明约 10 qps，超限查询被 tar-pit 隔离 |
| [APNIC whois](https://wq.apnic.net/) | 亚太的分配记录 | 免 key | ✅ 可用（参数是 `?searchtext=`，不是 `?ip=`） |
| LACNIC / AFRINIC whois | 各自的记录 | 免 key | ✅ 可用 |
| 五个 RIR 的 `delegated-*-latest` | **批量、机器可读、权威、每日更新** | 直接下载，无限流 | ✅ RIPE 18 MB / ARIN 13 MB / APNIC 9 MB / LACNIC 4.6 MB / AFRINIC 1 MB |

whois 是最古老也最耐用的那一条：它没有 API，只有一个 telnet 端口和一条查询语法，输出是给人看的文本。RDAP 是它的继任者，把同样的东西变成了 HTTP + JSON，并且是 IANA 强制的标准。

**而批量导出的那一条几乎没人用** —— 这正是这个站点存在的起点。

### 第二条路：路由数据聚合（看「现在实际在跑什么」）

| 服务 | 提供什么 | 门槛 | 实测（2026-10-29） |
|---|---|---|---|
| [RIPEstat](https://stat.ripe.net/) | 60+ 数据集：路由表规模、宣告前缀、ASN 概览、邻居与**邻居历史**、RPKI 校验、**BGPlay 动画 AS-path**、CAIDA 关系、地址转让历史… | **完全免费、免 key、官方原文「无请求总量限制」**，每 IP 8 并发 | ✅ 直接可用 |
| [bgp.tools](https://bgp.tools/) | 最好用的 ASN 页面：前缀、上下游、peer、RPKI、IRR、IXP | ⚠️ **交互层已登录化**（`/as/`、`/prefix/`、`/api/query.js` 全 307 → `/login?must-login`） | ❌ 交互需登录 |
| [bgp.he.net](https://bgp.he.net/)（HE BGP Toolkit） | ASN 前缀表、peers、upstreams、**RPKI + ASPA**、IRR、反向 whois、11 份互联网统计报告 | 免费，**免注册** | ⚠️ **无官方 API**（`bgp.he.net/AS13335.json` → 404，网传端点并不存在） |
| [PeeringDB](https://www.peeringdb.com/api/net) | 网络 35,779 / IXP 1,332 / 设施 5,885 / 运营商 288，含 IXP switchboard | 读免注册 | ✅ 匿名 20 次/分；认证用 `Authorization: Api-Key <KEY>` |
| [RADb](https://www.radb.net/) | IRR 对象库，**RPKI ROA 镜像也进 IRR** | whois 43 端口**免费、匿名、免注册** | ✅ REST 校验端点免认证可用（`/search` 需付费订阅 $595/年） |
| [RouteViews](https://www.routeviews.org/) | 全球路由表快照、MRT 原始转储（2001.10 至今）、采集器清单 | ✅ 免认证 JSON API | ✅ guest **1 req/s**，带 key 10 req/s |
| [CAIDA AS Rank](https://asrank.caida.org/) | 按流量给 AS 排名、cone 分解、**AS 关系图（带 relationship + numberPaths）** | 免 key，GraphQL v2 | ✅ 可用（122,962 个 AS） |
| [bgpview.io](https://bgpview.io/) | — | — | ❌ **已死**：三个解析器均 0 A 记录，GitHub 仓 404，MISP 记为 2025-11-26 永久关闭 |

> **三个必须记下的事实：**
>
> **① bgp.tools 的登录墙不是全站。** 交互查询确实要登录，但三个数据端点**仍然免认证返回 200**：
> `https://bgp.tools/table.jsonl`（**23.5 MB / 451,159 行**，字段 `CIDR`/`ASN`/`Hits`）、`https://bgp.tools/asns.csv`（**4.5 MB**，`asn,name,class,cc`）、`https://map.bgp.tools/`（**全 IPv4 空间可平移缩放的交互地图**）。
>
> **② 但 bgp.tools 的 robots.txt 对 AI agent 是明确封杀的**：对 `ClaudeBot`、`ChatGPT-User`、`GPTBot`、`OAI-SearchBot`、`Amazonbot` 等一律 `Disallow: /`，站点还要求 UA 携带 LLM 标识与联系邮箱。**这是合规红线 —— 不碰它的交互层和 API。** 真要用，只用上面那两个静态数据文件，并在文档里写明出处与快照日期。
>
> **③ bgp.he.net 有图，但没有 API。** 它页面里确实有 **6 张 Chart.js 图表**（IPv4/IPv6 Peer Count、Prefixes Announced、Prefixes Originated）和服务端渲染的 Graph v4/v6 AS-path 连通图 —— 但**没有官方 API，也没有批量导出，数据许可也不明确**。第三方已经在商业化抓取它。**要 BGP 数据就用 RIPEstat + RADb + RouteViews 替代。**

### 第三条路：商业 IP 情报 API（查一个 IP 的属性）

| 服务 | 免 key？ | 免费额度（已查证） | 返回什么 |
|---|---|---|---|
| [ip.guide](https://ip.guide/) | ✅ | 无公布上限 | 国家、城市、ASN、org、经纬度 —— **四段齐全** |
| [ipwho.is](https://ipwho.is/8.8.8.8) | ✅ | **1,000 次/天/客户端 IP** | 国家、城市、ASN、ISP、经纬度、时区、货币 |
| [ip-api.com](http://ip-api.com/json/8.8.8.8) | ✅ | **45 req/min/IP** | 国家、地区、城市、ISP、AS、AS 名、延迟、时区。**仅 HTTP、仅非商用** |
| [ipinfo.io](https://ipinfo.io/8.8.8.8/json) | ⚠️ Lite 需免费 token | **Lite 无限**（含 country + basic ASN）；老接口免 token 50k/月但返回受限数据 | ASN、org、地理位置、邮编、时区、公司 |
| [ipdata.co](https://ipdata.co/) | ✅ `ipdata.free` | **1,000 次城市级/天**（国家 + ASN 无限） | ASN + org + 城市 |
| [ipquery.io](https://ipquery.io/) | ✅ | 宣称无限（无数值） | ISP、ASN、org |
| [ipapi.co](https://ipapi.co/) | ❌ 429 门禁 | 试用 30,000/月但 ≤1,000/天，需**工作邮箱**申请 | ASN + org |
| [ipstack](https://ipstack.com/) / [ipapi.com](https://ipapi.com/) | ❌ 必须注册 | **100 次/月**，免费档**没有 ASN 字段**、仅 HTTP | 地理位置 |
| [IPQualityScore](https://www.ipqualityscore.com/) | ❌ 必须注册 | 1,000 credits/月 + 35 次/天 | ASN（字段在 IP Reputation 响应里） |
| [MaxMind](https://www.maxmind.com/) | ❌ 需 license key | GeoLite2 ASN 免费库仍在 | ⚠️ **付费的「GeoIP2 ASN Database」页面已 404**，ASN 数据并入 **GeoIP ISP Database**；且 **MaxMind 没有 ASN web service**（`geoip.maxmind.com/geoip/v2.1/asn/…` → 404，ASN 字段在 `traits` 里） |

这一路解决的是「这个 IP 在哪、属于谁」，而不是「这个地址段被谁分配过」。它们的返回结构高度相似，因为大家抄的是同一份 GeoIP 数据源。

**还有一个免费但少人用的宝藏**：[iptoasn.com](https://iptoasn.com/) 的全量数据文件（`ip2asn-combined.tsv.gz` 9 MB / v4 7 MB / v6 2 MB，**每小时更新**）。它的公共 API 已于 2020-12-31 下线，只剩下载 —— 但下载本身完全免费免注册。

### 第四条路：命令与库

**参照物是 [github.com/nitefood/asn](https://github.com/nitefood/asn) —— 它不是 Go 库，而是一个 1,944★ 的 Bash 脚本**（Shell 98.4%），MIT 许可，2026-10 仍在更新，被 Alpine / Debian / Homebrew / Nix / AUR 打包，是这个领域的事实标准。它的能力最全（ASN、前缀、ISP、国家、上游链、**每跳 RPKI 校验**），选项有 16 项（`-t` trace / `-n` 只查 / `-s` Shodan 扫描 / `-l` server 模式 / `-j` JSON…），数据源接了 18 个服务。

**但它要装 8 个系统包**：`curl whois bind9-host mtr-tiny jq ipcalc grepcidr nmap ncat aha`。而且它**只给查询，不给分布**。

其他值得知道的：

| 工具 | 语言 | ★ | 做什么 |
|---|---|---|---|
| [nexttrace](https://github.com/nxtrace/NTrace-core) | Go | **8,254** | 本领域 star 最高。traceroute，每跳给 IP / 时延 / ASN / 地理位置；`--from<国家>` 走 Globalping 选点 |
| [ttl](https://github.com/lance0/ttl) | Rust | 1,489 | 「更好的 mtr」，每跳 ASN + geo，ECMP 检测、MPLS 解析 |
| [asnmap](https://github.com/projectdiscovery/asnmap) | Go | **1,145** | **反向**：ASN/ORG → CIDR。⚠️ **现已需 PDCP API key** |
| [asname](https://github.com/flyingllama87/asname) | Go | 0（2026-08 新建） | 离线 LC-trie，IP/主机 → ASN + AS 名 + 国家，`--rest` 可起服务。**形态最接近「一条命令四合一」** |
| [aslookup](https://github.com/nieldk/aslookup) | C | 0 | **零外部命令依赖**（libcurl + libcjson + libresolv） |
| [asnlookup](https://github.com/viktb/asnlookup) | Go | 21 | 纯离线 IP→ASN，LC-trie |
| [pyasn](https://github.com/hadiasghari/pyasn) | Python | 330 | IP→ASN，还支持**基于历史 BGP dump 的历史归属查询** |

系统自带里最有价值的一条是 **`whois`**：

```sh
whois -h whois.cymru.com " -v 8.8.8.8"
# 15169 | 8.8.8.0/24 | US | arin | 2023-12-28 | GOOGLE - Google LLC, US
```

Team Cymru 免费、永久、免注册，还有 DNS（TXT）和 bulk 三种接口。**但它只给到国家级，没有城市级地理**，而且官方明确说明国家码来自注册机构记录，与 IP 实际使用地常差很多。

x-cmd 侧已经有 `x ip`（`cidr` / `map` / `iter` / `addr` / `query` / `scan`）、`x dns`、`x ping` / `x tping`、`x route`、`x nets`。**没有 `x asn`，也没有 `x whois`** —— 这正是这个站点要补的那一块。

---

### 它们的共同短板 —— x-cmd/asn 站在这里

把上面四条路放在一起看，会看到一个一致的形状：

**① 它们几乎全是「查一行」，不是「看分布」。** 每一个服务的交互模型都是「给我一个 IP，我给你一个答案」。而「法国的 IPv4 空间长什么样」「哪个国家最碎片化」「谁持有最多地址段」这类问题，只能靠你自己把几千行 whois 输出丢进表格工具里。

**② 它们的输出是表格或 JSON，不是一张能贴出去的图。** 而且这是结构性的，不是偷懒：这些服务的数据模型是「一个查询对应一条记录」，没有聚合的形状。

**③ 一个完整的视图要跨好几个站。** 想看「我这段地址在 BGP 里由谁宣告」要开 RIPEstat；想看「这个 ASN 有多活跃」要开 bgp.tools；想看「这个 IP 在哪」要开 ipinfo。三家，三套账号，三种限制。

**④ 权威数据和易用数据是分开的。** 唯一批量、机器可读、权威的源是五个 RIR 的 delegated 文件 —— 但那需要你自己写解析器。

### 但要说清楚：**做可视化的站点其实不少**

这一点必须诚实。做 **AS 级拓扑图**（力导向、节点连线）的站点至少有九个：Internet Health Report（Plotly + D3 + **Sankey**）、BGPTrace（vis-network + Chart.js + ECharts）、as-graph（Graphviz→JS）、bgpmap.net（Graphviz SVG/PNG）、APNIC REx（D3）、ISOC Pulse（Vega + Leaflet）、CleanIP、SecViz、以及 CAIDA 的 AS Core / AS Reach。RIPEstat 自己就有 **86 个可嵌入 widget** 加 BGPlay 动画 AS-path 图。bgp.he.net 也有 6 张 Chart.js 图。

**所以真正的空白不是「没人画 ASN 图」，而是下面这三条：**

> **① A 类（拓扑图）全是交互式应用，不是可分享的静态产物。** 用户看到一张「我的 AS 路径图」后想改一个参数，只能刷新页面重新交互 —— **拿不到「这张图的 URL」**。
>
> **② B 类（分布图）只有 ipinfo 一家做对了形态**，而它只服务于「你自己粘进去的 IP 列表」，**没有面向「某个 RIR 的地址结构」「某个 ASN 的全球分布」「AS 关系图」这类公共议题的版本**。
>
> **③ 没有任何一个把 CAIDA AS Rank / AS Relationships / RIPEstat / RIR delegated 这些权威数据变成「参数即 URL 的公共图」。**

而 x-cmd/draw 的模型恰恰是 `https://x-cmd.com/draw?type=…&labels=…&values=…` —— **改一个参数就是换一张图**，SVG 内嵌 `<metadata>{generator,template,version,spec}</metadata>` 保证数据无损往返。**这就是「可分享、可复现、可参数化」的公共图**，而上面九家一个都不是。

### 一条必须守住的技术边界

**draw 支持的 13 种图是**：`pie bar hbar line area radar gauge scatter combo sankey qr archmap mindmap`。**它没有力导向节点连线图** —— 也就是上面 A 类那一种。

所以分工很清楚：

- ✅ **B 类（分布图）是 draw 的主场** —— pie / hbar / radar / scatter / combo 全部现成。
- ✅ **`sankey` 正好对应「provider / peer / customer」的三分关系** —— CAIDA AS Relationships 的原生格式就是 `provider|customer|-1` 和 `peer|peer|0`，**它天生就是这个数据的形状**。
- ⚠️ **A 类（AS-path 拓扑图）draw 做不了** —— 想自研要另起渲染器，成本远高于收益。**只引用不自研**：把 bgpmap.net 现成的 Graphviz SVG 嵌进文档就行。

**所以 x-cmd/asn 应主攻分布图与关系 sankey，拓扑图只做引用。**

### 本次挖到的三个免费优质数据源

| 源 | 免 key？ | 产出 | 适合哪张图 |
|---|---|---|---|
| **[bgpmap.net](https://bgpmap.net/)** | ✅ **免注册免 key** | `POST /api/v1/snapshots {"resource":"1.1.1.0/24"}` → origin_asns / upstreams / **rpki_state** / **自带 Graphviz SVG + PNG**；批量 ≤100 | AS-path 拓扑（**直接引用它的 SVG**） |
| **[api.routeviews.org](https://api.routeviews.org/)** | ✅ guest | `/asn/{n}`、`/prefix/{cidr}`（含 `origin_asn` / `rpki_state` / `reporting_peers[].as_path`）、`/rib/collectors`、`/collector/`（含经纬度）、`/rib/adjacent-asn` | 前缀宣告、可见度、采集器地理 |
| **[CAIDA AS Relationships](https://publicdata.caida.org/datasets/as-relationships/serial-1/)** | ✅ 公开下载 | 月度 `YYYYMMDD.as-rel.txt.bz2`（**1998-01 至今**），格式 `provider|customer|-1` / `peer|peer|0` | **`sankey` 的原生输入** |

---

## 我们还应该建哪些

既然一个「x-cmd.com/<name> 子页面 + 一个 x-cmd/<name> 仓库」已经跑通三套（[ping](https://github.com/x-cmd/ping) · [shodan](https://github.com/x-cmd/shodan) · [asn](https://github.com/x-cmd/asn)），那下一个该建什么？

判据只有三条，缺一条就别做：

1. **有公开、可复现、不需要 key 的数据源** —— 否则第一次刷新就要求人。
2. **有用户真的会问的问题** —— 而不是「我们有数据所以做一页」。
3. **用 draw 能画出形状，而不只是列表** —— 表格型数据做不成子页面。

### 值得做的

| 候选 | 数据源 | 为什么成立 | 优先级 |
|---|---|---|---|
| **x-cmd.com/asn 续建** | 5 个 RIR delegated + CAIDA AS Rank（GraphQL）+ CAIDA AS Relationships + RIPEstat `announced-prefixes` | 数据源已验证免费免 key；**新增三章**：一个 AS 的形状（cone/degree 三分 → `pie`）、谁在上游（provider/customer/peer → **`sankey`**）、宣告前缀的时间线（`announced-prefixes` 直接带 `timelines` → `line`） | **最高** |
| **x-cmd.com/bgp** | RIPEstat + RouteViews + bgpmap.net + RADb + CAIDA | 「这个前缀是谁宣告的 / RPKI 状态如何 / 可见度多少」都是真问题 | 高。⚠️ **但定位必须收窄**：不做实时 looking glass（要自建 BGP 采集，成本量级完全不同），只做「用公开可复现数据能回答的问题」 |
| **x-cmd.com/whois**（或作为 asn 的子组） | 五个 RIR whois + RDAP | RDAP 返回结构化对象 → 「注册链条」（IANA→RIR→LIR→ISP→客户）的 `sankey` 是教科书级的图 | 中高。💡 **更省事的是并入 asn 作为一组**：数据源重叠、读者重叠、搜索意图重叠，拆站会稀释 |
| **x-cmd.com/ip** | 无需外部数据 —— `x ip` 本身 | **完全不需要新数据源**，纯算法 | 中。⚠️ **前提是先修 `x ip calc`**（见下）。定位应是「地址空间的形状」（IPv6 的分配算术、CGNAT 为什么存在、地址分类各占多少），不是「CIDR 计算教程」 |
| **x-cmd.com/kev** | CISA KEV CSV | 一份**监管指令清单**，图表化价值独立于 Shodan | 中。⚠️ **建议并入 x-cmd.com/cve 作为一组** —— `/cve` 已存在且已有 7 篇，另起 `/kev` 会抢同一批读者和关键词 |

### 谨慎的

| 候选 | 为什么存疑 |
|---|---|
| **x-cmd.com/dns** | ⚠️ **内容与模块能力不匹配**。`x-cmd/dns` 仓**没有 docs/**（是 workspace 配置仓），而 `mod/dns` **不做解析查询** —— 只配 resolver + 拉 provider 目录。要么先给 `mod/dns` 加解析能力，要么把选题改成「DNS 生态与公共 resolver 目录」（不需要新模块） |
| **x-cmd.com/uptime** · **/seo** | 仓库已存在（都带 README.cn.md / SKILL.md / docs/）但**尚未上线**（线上 404）。先确认内容是否还活，再决定救活还是重做 |
| **x-cmd.com/geolocation** | 商业 API 的字段几乎完全同质，做不出差异 |
| **x-cmd.com/rdap**（独立站） | 与 whois / asn 三方重叠，并入即可 |

### 不该做的

这一条同样重要：

- **每天/每次都变、且无法固定快照的数据** —— 「今日热门 IP」这类页面明天就过期，而子页面的价值恰恰来自**可以引用**（「2019 年的这张图」有意义，「今天的这张图」没有）。
- **上游已有成熟可视化且比我们做得好** —— scorecard.dev、GitHub 原生图、ipinfo 的 Summarize IPs、CAIDA AS Core。**边际价值低。**
- **本质是命令手册而非数据** —— traceroute / mtr / host / route / nets 这类。traceroute 结果是机器相关的一次性测量（从哪跳、ICMP 被墙与否、每个中间路由器的负载），**不可复现，画出来没有意义**。它们属于 `x-cmd.com/mod/<name>`，不是子页面。
- **需要 key / 付费墙才能复现的数据** —— 除非是给合作方专用的子页面（像 shodan 那样），否则等于把内容锁死。
- **数据源的 robots.txt 明确禁止 AI agent** —— bgp.tools 就是。
- **数据源没有批量导出或许可不明** —— bgp.he.net 就是。
- **超出 draw 的图表能力（力导向拓扑图）** —— 只引用不自研。

> **目前已上线的 x-cmd.com 子页面**：`dbnomics` · `cve` · `cwe` · `gpg` · `ratelimit` · `skill0` · `ai` · `action` · `install` · `mod` · `pkg` · `theme`
> （另有 `x-cmd/uptime`、`x-cmd/seo`、`x-cmd/terminal`、`x-cmd/browser` 四个内容仓建好但未上线。）

---

## 怎么自己加一页

这个站点的全部机制就是一件事：**把数据变成一条明文参数 URL**。

```sh
# 1. 拿到数据
curl -s https://ftp.apnic.net/stats/apnic/delegated-apnic-latest

# 2. 变成图表 URL —— 参数是明文的，AI 也能拼
```text
https://x-cmd.com/draw?type=hbar&labels=CN,JP,IN&values=214719744,54388992,26313216&title=IPv4%20by%20country
```

<a href="https://x-cmd.com/draw?type=hbar&labels=CN,JP,IN&values=214719744,54388992,26313216&title=IPv4%20by%20country"><img src="https://x-cmd.com/draw?type=hbar&labels=CN,JP,IN&values=214719744,54388992,26313216&title=IPv4%20by%20country" width="780" alt="addresses-autonomous-systems"></a>

# 3. 或者让 AI agent 来
#    用 draw skill：https://raw.githubusercontent.com/x-cmd/draw/main/SKILL.md
```

参数可替换是这套东西的核心：**把 `product:nginx` 换成 `org:你的公司`，同一个模板就成了你自己的图**。所以每个子页面给出的都不只是成品，而是**一个用户可以拿去改的模板**。

仓库结构、命名规则、路由生成方式和 CI 见 [`CONTRIBUTING.md`](https://github.com/x-cmd/asn/blob/main/CONTRIBUTING.md)；给 AI agent 的消费说明见 [`SKILL.md`](https://github.com/x-cmd/asn/blob/main/SKILL.md)。

## Where the numbers come from

Everything here is computed from the RIRs' own `delegated-*-latest` exports — the authoritative registry files, not a third-party mirror. The per-country cuts use the APNIC region, because APNIC is the registry that publishes per-country rows inside the same export; ARIN and RIPE NCC publish them in separate files.

One caution that applies to every table on this site: **these are counts of allocations, not of addresses.** A registry that handed out 100,774 blocks may have handed out larger or smaller blocks than one that handed out fewer. The next page makes that distinction visible by putting the two RIR resources side by side.

> CLI: `x ip cidr` · `x ip map` — see [x-cmd.com/mod/ip](https://x-cmd.com/mod/ip)
> Data: [ftp.ripe.net](https://ftp.ripe.net/pub/stats/ripencc/) · [ftp.apnic.net](https://ftp.apnic.net/stats/apnic/) · [ftp.arin.net](https://ftp.arin.net/pub/stats/arin/) · [ftp.lacnic.net](https://ftp.lacnic.net/pub/stats/lacnic/) · [ftp.afrinic.net](https://ftp.afrinic.net/pub/stats/afrinic/)
