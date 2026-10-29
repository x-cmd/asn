# 地址、自治系统，以及发放它们的那五个机构

## 两种数字，回答不同的问题

大多数人对互联网的想象是一堆地址：1.1.1.1、192.168.1.1、`2606:4700:4700::1111`。地址说明**一个包应该送到哪里**。它是一个层级化的标识符，以块的形式层层下发，而且它是真正出现在网络线缆上的东西。

但互联网的路由根本不用地址工作。路由器的表里装的不是"到 1.1.1.1"，而是"到 AS13335，经由这个邻居"。所以底下还藏着**第二套编号体系**：**自治系统号（ASN）**。一个 ASN 标识一个网络 —— 通常是一个机构 —— 它向全球 BGP 系统广播自己的路由。互联网上每一个 IP 前缀，都恰好属于某一个 ASN。

这个区分值得记住，因为它解释了不少本来让人困惑的事实：

- **地址空间可以买卖、转让和转售。自治系统号不行** —— ASN 是标识符而不是资源，它跟着注册它的那个网络走。
- **一个 ASN 可以持有海量地址，而一个地址可以属于某个 ASN，而这个 ASN 自己又是在向大量客户转售的 ISP。** 两套编号体系的嵌套方式不同，分配的链条也不同。
- **同一个城市里的两个网络可以属于不同 ASN**，两个 ASN 也可以在每一个国家都有地址。

## 分配的链条

两种数字都不是直接分给最终用户的。链条是这样的：

```
IANA  ──►  区域互联网注册机构 (RIR)  ──►  国家/地区注册机构 (NIR 或 LIR)  ──►  ISP  ──►  最终客户
```

**IANA** 持有地址空间的顶端并委派给五个 **RIR**，真正持有资源的正是这五家机构。多数 RIR 再分配给**国家或地区注册机构**（NIR），NIR 分配给大型 ISP，ISP 再分配给客户。链条上的每一环都是一次委派，每一次委派都有记录 —— 这正是 delegated 统计文件存在的理由，也是这一页能被建立起来的基础。

## 五个注册机构

| 注册机构 | 区域 | IPv4 分配条目 | ASN |
|---|---|---|---|
| RIPE NCC | 欧洲、中东、中亚 | 100,774 | 40,114 |
| ARIN | 北美 | 81,326 | 33,123 |
| LACNIC | 拉丁美洲与加勒比 | 20,450 | 16,128 |
| APNIC | 亚太 | 58,336 | 13,981 |
| AFRINIC | 非洲 | 5,558 | 2,821 |

*（分配条目数取自各 RIR 的 delegated 统计文件，快照 2026-10-29）*

**RIPE NCC 两种资源都发得最多。** 这不是关于欧洲的陈述，而是关于互联网**如何被建造、又如何被重组**的陈述。IPv4 的分配史由转让与再委派主导，而 RIPE NCC 最终握住了两种资源中最大的份额。

**ARIN 的分配条目数排第二，但每个 ASN 摊到的地址空间排第一** —— 块更大，运营者更少。

**AFRINIC 在两列里都最小，而这恰恰是区域注册机构存在的全部理由。** 一个十亿多人的大陆，只有 2,821 个自治系统和一小部分地址空间，在互联网控制面里是薄薄的一条。区域注册机构的存在，正是为了让分配不必跨越大洋。

## 为什么 IPv6 改变了这套算术

IPv4 大约有 43 亿个地址。IPv6 大约有 (3.4 	imes 10^{38}) 个 —— 这个数字大到我们惯用的"稀缺"思维方式根本不再适用。关于 IPv6 有趣的问题不再是"够不够用"，而是"为什么用得这么少"，而那是一个部署与路由问题，不再是分配问题。

这一页留在 IPv4，因为分配的压力在那里才看得见。

## 同类的工具与网页：这个位置已经有多少人

查「这个 IP 属于谁」和「这个 ASN 是谁」这件事，有四条完全不同的技术路径，而每条路径上都已经挤满了服务。理清这四条路，才知道 x-cmd/asn 站在哪里。

### 第一条路：注册机构自己（权威，但难用）

五个 RIR 各自提供 whois、RDAP 和批量导出：

| 来源 | 提供什么 | 门槛 |
|---|---|---|
| [RIPE whois / RDAP](https://www.ripe.net/manage-ips-and-asns/db/) · [rdap.db.ripe.net](https://rdap.db.ripe.net/) | 该 RIR 分配的一切记录 | 免 key，但要懂查询语法 |
| [ARIN RDAP](https://rdap.arin.net/registry/ip/8.8.8.8) | 北美的地址段与 ASN | 免 key |
| [APNIC / LACNIC / AFRINIC whois](https://wq.apnic.net/) | 各自的分配记录 | 免 key |
| 五个 RIR 的 `delegated-*-latest` 文件 | **批量、机器可读、权威** | 直接下载，无限流 |

whois 是最古老也最耐用的那一条：它没有 API，只有一个 telnet 端口和一条查询语法，输出是给人看的文本。RDAP 是它的继任者，把同样的东西变成了 HTTP + JSON，并且是 IANA 强制的标准。

**而批量导出的那一条几乎没人用** —— 这正是这个站点存在的起点。

### 第二条路：路由数据聚合（看「现在实际在跑什么」）

| 服务 | 提供什么 | 门槛 | 实测（2026-10-29） |
|---|---|---|---|
| [RIPEstat](https://stat.ripe.net/) | RIPE 的大规模统计 API：路由表规模、宣告前缀、ASN 概览、RIR 分配、BGP 可见性…… 数十个 data call | **完全免费、免 key、无速率限制** | ✅ 直接可用 |
| [bgp.tools](https://bgp.tools/) | 最好用的 ASN 页面：前缀、上下游、RPKI、IRR | ⚠️ **现在跳登录页**（307 → `/login?must-login`） | ❌ 需登录 |
| [bgp.he.net](https://bgp.he.net/) | Hurricane Electric BGP Toolkit：ASN → 前缀 → 互联网全貌 | ⚠️ **同样跳登录页** | ❌ 需登录 |
| [PeeringDB](https://www.peeringdb.com/api/net) | 网络与 IXP 的机读数据库（谁和谁互联） | 免 key，JSON | ✅ 可用 |
| [CAIDA AS Rank](https://asrank.caida.org/) | 按流量给 AS 排名 | 免 key | ✅ 可用 |

> **值得注意的一个变化**：bgp.tools 和 bgp.he.net 这两个最经典的 BGP 查询站，现在都要求登录了。它们的免费价值曾经是它们被广泛引用的原因，而这个价值正在消失。**任何不依赖登录的替代方案因此变得更有价值** —— RIPEstat 就是其中最好的一个，而且它完全不需要 key。

### 第三条路：商业 IP 情报 API（查一个 IP 的属性）

| 服务 | 免 key？ | 免费额度 | 返回什么 |
|---|---|---|---|
| [ipwho.is](https://ipwho.is/8.8.8.8) | ✅ | 无明确限制 | 国家、城市、ASN、ISP、经纬度、时区、货币 |
| [ip-api.com](http://ip-api.com/json/8.8.8.8) | ✅ | 免费层 45 次/分钟 | 国家、地区、城市、ISP、AS、AS 名称、延迟、时区 |
| [ipinfo.io](https://ipinfo.io/8.8.8.8/json) | ⚠️ 基础查询可以 | 每月 5 万次（需 token 才有完整字段） | ASN、组织、地理位置、邮编、时区、公司 |
| [ipapi.co](https://ipapi.co/) · [ipdata.co](https://ipdata.co/) · [ipquery.io](https://ipquery.io/) | 多数有免费层 | 各不相同 | 同类字段 |

这一路解决的是「这个 IP 在哪、属于谁」，而不是「这个地址段被谁分配过」。它们的返回结构高度相似，因为大家抄的是同一份 GeoIP 数据源。

### 第四条路：命令本身

系统自带和开源生态里已经有的：

| 工具 | 做什么 |
|---|---|
| `whois` | 查询注册记录（各 RIR 的语法不同） |
| `host` / `dig` | DNS 解析与 PTR 反查 —— **PTR 反查常常就是最快的 ASN 归属线索** |
| `mtr` / `traceroute` | 路径，以及沿途的 ASN |
| `ip`（iproute2） | 本地地址与路由表 |
| [github.com/nitefood/asn](https://github.com/nitefood/asn) | Go 写的 ASN 查询库 —— **Adriano 自己的那个**，也是这个站点存在的原因 |

x-cmd 侧已经有对应的模块：`x ip`（`calc` / `cidr` / `map` / `addr` / `scan`）、`x whois`、`x dns`、`x ping` / `x tping`、`x route`、`x nets`。**但没有 `x asn`** —— 而这正是这个站点要补的那一块。

---

### 它们的共同短板 —— x-cmd/asn 站在这里

把上面四条路放在一起看，会看到一个一致的形状：

**① 它们几乎全是「查一行」，不是「看分布」。** 每一个服务的交互模型都是「给我一个 IP，我给你一个答案」。而「法国的 IPv4 空间长什么样」「哪个国家最碎片化」「谁持有最多地址段」这类问题，只能靠你自己把几千行 whois 输出丢进表格工具里。

**② 它们的输出是表格或 JSON，不是一张能贴出去的图。** 而且这是结构性的，不是偷懒：这些服务的数据模型是「一个查询对应一条记录」，没有聚合的形状。

**③ 一个完整的视图要跨好几个站。** 想看「我这段地址在 BGP 里由谁宣告」要开 RIPEstat；想看「这个 ASN 有多活跃」要开 bgp.tools；想看「这个 IP 在哪」要开 ipinfo。三家，三套账号，三种限制。

**④ 权威数据和易用数据是分开的。** 唯一批量、机器可读、权威的来源是五个 RIR 的 delegated 文件 —— 但那需要你自己写解析器。

**所以 x-cmd/asn 的位置不是「又一个 whois」，而是：**

> 把最权威、最难拿到的那份数据（五个 RIR 的 delegated 导出），变成**不需要写解析器就能看懂的图**，并且让每一张图的**查询可以自己换**。

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
| **x-cmd.com/whois**（或 /rdap） | 五个 RIR 的 whois + RDAP，免 key | 和 asn 同一批数据源的另一面；**RDAP 是 IANA 强制的标准，whois 是事实上的标准**，而两者现在都需要手工拼查询串 | 高 |
| **x-cmd.com/ip** | 无需外部数据 —— `x ip calc` / `cidr` / `map` 本身 | **完全不需要新数据源**，纯算法；CIDR 切分、子网划分、VLSM、地址换算这类题目网上全是，但没有一个把「怎么算」讲清楚的图解 | 高 |
| **x-cmd.com/dns** | `x dns` + 公开解析器 | 仓库已存在（x-cmd/dns），只差内容；全球解析一致性、DNS 记录类型、TTL 行为都是真实问题 | 中高 |
| **x-cmd.com/kev** | CISA KEV CSV（公开、免 key、每天更新） | 已经作为 shodan 站的一页存在；单独成站也成立 —— 它是一份**监管指令清单**，用图表化呈现的价值独立于 Shodan | 中 |
| **x-cmd.com/bgp** | RIPEstat（免 key）+ 各 RIR 的 route 导出 | BGP 路由表规模、宣告前缀数、可见性 —— 全部是公开可复现的，而且**有天然的时间序列**（每月一个点） | 中 |
| **x-cmd.com/ossinsight** | GitHub 公开事件流 | `x ossinsight` 已经能吐 CSV；开源生态的年度/语言/仓库排行是真实需求 | 中 |

### 谨慎的

| 候选 | 为什么存疑 |
|---|---|
| **x-cmd.com/cve** | **已经存在**（[x-cmd/cve](https://github.com/x-cmd/cve) → x-cmd.com/cve，“CVE for AI builders”），不要重复建 |
| **x-cmd.com/uptime** · **/seo** | 仓库已存在但子页面**尚未上线**（线上 404）；先确认它们的内容是否还是活的，再决定是救活还是重做 |
| **x-cmd.com/geolocation** | 商业 API 的字段几乎完全同质，做不出差异 |
| **x-cmd.com/traceroute** | 路径数据没有稳定的公开批量来源；每一跳的时延也无法复现 |

### 不该做的

这一条同样重要：

- **每天都在变、且无法固定快照的数据** —— 一次性的「今日热门 IP」这类页面，明天就过期，而子页面的价值恰恰来自**可以引用**（「2019 年的这张图」是有意义的，「今天的这张图」不是）。
- **只有列表、没有形状的数据** —— 例如一个 5000 条的 whois 全表。能画出来不代表值得画。
- **需要 key 才能复现的数据** —— 除非是给合作方专用的子页面（像 shodan 那样），否则等于把内容锁死。
- **和已有页面重复的** —— 做之前先看 `x-cmd.com` 上已经有什么：目前已上线的是 `dbnomics`、`cve`、`cwe`、`gpg`、`ratelimit`、`skill0`、`ai`、`action`、`install`、`mod`、`pkg`、`theme`。

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

<a href="https://x-cmd.com/draw?type=hbar&labels=CN,JP,IN&values=214719744,54388992,26313216&title=IPv4%20by%20country"><img src="https://x-cmd.com/draw?type=hbar&labels=CN,JP,IN&values=214719744,54388992,26313216&title=IPv4%20by%20country" width="780" alt=""></a>

# 3. 或者让 AI agent 来
#    用 draw skill：https://raw.githubusercontent.com/x-cmd/draw/main/SKILL.md
```

参数可替换是这套东西的核心：**把 `product:nginx` 换成 `org:你的公司`，同一个模板就成了你自己的图**。所以每个子页面给出的都不只是成品，而是**一个用户可以拿去改的模板**。

仓库结构、命名规则、路由生成方式和 CI 见 [`CONTRIBUTING.md`](https://github.com/x-cmd/asn/blob/main/CONTRIBUTING.md)；给 AI agent 的消费说明见 [`SKILL.md`](https://github.com/x-cmd/asn/blob/main/SKILL.md)。

## 数字从哪来

这里的一切都是从各 RIR 自己的 `delegated-*-latest` 导出文件算出来的 —— 权威的注册机构文件，而不是第三方镜像。按国家的切法用的是 APNIC 区域，因为 APNIC 是那个在同一个导出文件里发布逐国家行的注册机构；ARIN 和 RIPE NCC 的逐国家行在另外的文件里。

有一条适用于本站每一张表的注意事项：**这些数字数的是分配条目，不是地址数。** 发出 100,774 个块的注册机构，发的块可能比发出更少块的机构更大或更小。下一页把两种 RIR 资源并排放，让这个区别变得可见。

> CLI：`x ip calc` · `x ip cidr` · `x ip map` —— 见 [x-cmd.com/mod/ip](https://x-cmd.com/mod/ip)
> 数据：[ftp.ripe.net](https://ftp.ripe.net/pub/stats/ripencc/) · [ftp.apnic.net](https://ftp.apnic.net/stats/apnic/) · [ftp.arin.net](https://ftp.arin.net/pub/stats/arin/) · [ftp.lacnic.net](https://ftp.lacnic.net/pub/stats/lacnic/) · [ftp.afrinic.net](https://ftp.afrinic.net/pub/stats/afrinic/)
