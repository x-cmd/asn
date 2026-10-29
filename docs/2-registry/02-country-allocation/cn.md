# 按国家看地址空间与自治系统

## 一个必须放在图前面的说明

"某个国家的 IPv4 空间"是个 slippery 的说法，值得先把它的确切含义讲清楚。

地址空间是**沿着一条链**分配的：RIR → 国家注册机构 → ISP → 客户。delegated 文件记录的是**从注册机构到下一手的委派**，在 APNIC 的导出里带着国家代码 —— 那是**注册机构客户所在的国家**，而不是地址**被使用**的地方。

所以一个国家的数字是：**该区域的注册机构分配给"注册在该国的组织"的地址空间。** 这是关于"分配文书落在哪里"的事实，和"机器在哪里"只有松散的关系。

ASN 同理，而且更明显：ASN 注册给一个组织，而这个组织可能在全世界任何地方广播。

## APNIC 区域 IPv4 地址空间按国家

```text
https://x-cmd.com/draw?type=hbar&labels=CN%2CKR%2CJP%2CIN%2CSG%2CAU%2CTW%2CID%2CVN%2CHK%2CTH%2CPK%2CPH%2CMY&values=214719744%2C57649152%2C54388992%2C26313216%2C21224704%2C19878912%2C19008000%2C12987136%2C12063744%2C6200576%2C4043264%2C3815424%2C3424512%2C3337728&title=IPv4%20address%20space%20by%20country%20%28APNIC%20region%29&unit=addresses
```

<a href="https://x-cmd.com/draw?type=hbar&labels=CN%2CKR%2CJP%2CIN%2CSG%2CAU%2CTW%2CID%2CVN%2CHK%2CTH%2CPK%2CPH%2CMY&values=214719744%2C57649152%2C54388992%2C26313216%2C21224704%2C19878912%2C19008000%2C12987136%2C12063744%2C6200576%2C4043264%2C3815424%2C3424512%2C3337728&title=IPv4%20address%20space%20by%20country%20%28APNIC%20region%29&unit=addresses"><img src="https://x-cmd.com/draw?type=hbar&labels=CN%2CKR%2CJP%2CIN%2CSG%2CAU%2CTW%2CID%2CVN%2CHK%2CTH%2CPK%2CPH%2CMY&values=214719744%2C57649152%2C54388992%2C26313216%2C21224704%2C19878912%2C19008000%2C12987136%2C12063744%2C6200576%2C4043264%2C3815424%2C3424512%2C3337728&title=IPv4%20address%20space%20by%20country%20%28APNIC%20region%29&unit=addresses" width="780" alt=""></a>

## APNIC 区域 ASN 数量按国家

```text
https://x-cmd.com/draw?type=hbar&labels=CN%2CIN%2CID%2CAU%2CBD%2CJP%2CHK%2CKR%2CVN%2CNZ%2CSG%2CPH%2CTH%2CPK&values=6397%2C6253%2C4027%2C2956%2C2022%2C1267%2C1180%2C1149%2C1049%2C694%2C682%2C656%2C604%2C470&title=ASNs%20per%20country%20%28APNIC%20region%29&unit=ASNs
```

<a href="https://x-cmd.com/draw?type=hbar&labels=CN%2CIN%2CID%2CAU%2CBD%2CJP%2CHK%2CKR%2CVN%2CNZ%2CSG%2CPH%2CTH%2CPK&values=6397%2C6253%2C4027%2C2956%2C2022%2C1267%2C1180%2C1149%2C1049%2C694%2C682%2C656%2C604%2C470&title=ASNs%20per%20country%20%28APNIC%20region%29&unit=ASNs"><img src="https://x-cmd.com/draw?type=hbar&labels=CN%2CIN%2CID%2CAU%2CBD%2CJP%2CHK%2CKR%2CVN%2CNZ%2CSG%2CPH%2CTH%2CPK&values=6397%2C6253%2C4027%2C2956%2C2022%2C1267%2C1180%2C1149%2C1049%2C694%2C682%2C656%2C604%2C470&title=ASNs%20per%20country%20%28APNIC%20region%29&unit=ASNs" width="780" alt=""></a>

## 两种形状，讲两个故事

**中国拿下该区域一半以上的 IPv4 空间** —— 在这个前 14 名的切面里，2.147 亿地址对约 3.5 亿 —— 但只有 6,397 个 ASN。地址池极大，而握在其中的路由运营者相对少。分配块大、集中度高：一个只有少数几家大运营商和云厂商握着空间、其余都是租用的市场，形状就是这样。

**印度恰好是镜像。** 6,253 个 ASN，几乎和中国一样多，却只分到 2,631 万地址。自治系统多，每个的地址少。这就是一个被切得很碎的 ISP 市场的样子：成千上万家区域运营商，每家一块不大的地址、一套自己的路由。

**日本（1,267）和韩国（1,149）**相对各自的地址空间，比印度还要密：大型运营商加上发育良好的对等生态，以及大量各自路由的企业网络。

**孟加拉国（2,022 个 ASN）和印尼（4,027）**是同一模式的小号版本 —— 一个年轻、竞争激烈的 ISP 市场，独立运营者很多，而地址空间并不多。

## 有意思的是那个比值

把两张图合起来看，会掉出第三个数字：**每百万地址对应多少个 ASN**。它是一个国家路由市场碎片化程度的粗糙代理指标，也就是**互联网控制面**有多少落在那里。

- 中国：每百万地址约 30 个 ASN。
- 印度：每百万约 238 个。
- 日本：每百万约 21 个。

按地址算，印度的路由市场比中国**碎约八倍**。这有实际后果：独立运营者更多意味着路径更多样（对韧性有利），但也意味着路由出错导致流量黑洞的地方更多（对可靠性不利），而且需要保持修补的、由独立主体运营的基础设施表面大得多。

## 为什么对 ASN 来说"在哪里"是个错问题

因为 ASN 属于一个组织，而组织是全球运营的，上面按国家统计的 ASN 数字描述的是**运营者注册在哪里**，而不是它的网络在哪里。一家在某国注册、ASN 也在该国的云服务商，可能在为每个大洲广播前缀。

所以诚实地使用 ASN 那张图的方式，是把它当成关于**结构**的图：一个国家的地址空间分属多少个独立网络，因此有多少个组织有能力对这些地址做路由决策。这是一个治理问题，而且值得问。

## 注意事项

- **只限 APNIC 区域。** 这一刀覆盖亚太注册机构，因为 APNIC 是那个在同一个 delegated 导出里发布逐国家行的注册机构。ARIN 和 RIPE NCC 的逐国家行在另外的文件里，国别覆盖范围也不同。
- **没有 IPv6。** 同一个导出的 IPv6 行只有汇总。
- **已委派 ≠ 已路由。** 相当大一部分已分配的 IPv4 从来没在 BGP 里被宣告过；那些地址只存在于纸面上。
- **快照。** 2026-10-29。

> 复现：`curl -s https://ftp.apnic.net/stats/apnic/delegated-apnic-latest`，
> 然后把每个 `<cc>|ipv4|<start>|<count>|...` 行的 `value` 列加起来
> （ASN 行同理）。
