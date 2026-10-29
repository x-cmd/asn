# 按国家看地址空间与自治系统

## 「一个国家的地址空间」到底指什么

这个说法有点滑，得先讲准。地址空间是沿着一条链分配的：RIR 到国家注册机构，到 ISP，到客户。delegated 文件记录的是从注册机构到下一手的委派，在 APNIC 的导出里带着国家代码，指的是**注册机构客户所在的国家**，不是地址被使用的地方。

所以一个国家的数字是：该区域的注册机构分配给「注册在该国的组织」的地址空间。这是关于分配文书落在哪里的事实，和机器在哪里只有松散的关系。

ASN 同理，而且更明显，因为 ASN 属于一个组织，而这个组织可能在全世界任何地方广播。

## APNIC 区域 IPv4 地址空间按国家

```text
https://x-cmd.com/draw?type=hbar&labels=China%2CSouth%20Korea%2CJapan%2CIndia%2CSingapore%2CAustralia%2CTaiwan%2CIndonesia%2CVietnam%2CHong%20Kong%2CThailand%2CPakladesh%2CPhilippines%2CMalaysia%2CNew%20Zealand%2CBangladesh&values=214719744%2C57649152%2C54393088%2C26313216%2C21224704%2C19878912%2C19008000%2C12987136%2C12063744%2C6200576%2C4043264%2C3815424%2C3424512%2C3337728%2C2222848%2C1478656&title=IPv4%20address%20space%20by%20country%20%28APNIC%20region%29&unit=addresses
```

<a href="https://x-cmd.com/draw?type=hbar&labels=China%2CSouth%20Korea%2CJapan%2CIndia%2CSingapore%2CAustralia%2CTaiwan%2CIndonesia%2CVietnam%2CHong%20Kong%2CThailand%2CPakladesh%2CPhilippines%2CMalaysia%2CNew%20Zealand%2CBangladesh&values=214719744%2C57649152%2C54393088%2C26313216%2C21224704%2C19878912%2C19008000%2C12987136%2C12063744%2C6200576%2C4043264%2C3815424%2C3424512%2C3337728%2C2222848%2C1478656&title=IPv4%20address%20space%20by%20country%20%28APNIC%20region%29&unit=addresses"><img src="https://x-cmd.com/draw?type=hbar&labels=China%2CSouth%20Korea%2CJapan%2CIndia%2CSingapore%2CAustralia%2CTaiwan%2CIndonesia%2CVietnam%2CHong%20Kong%2CThailand%2CPakladesh%2CPhilippines%2CMalaysia%2CNew%20Zealand%2CBangladesh&values=214719744%2C57649152%2C54393088%2C26313216%2C21224704%2C19878912%2C19008000%2C12987136%2C12063744%2C6200576%2C4043264%2C3815424%2C3424512%2C3337728%2C2222848%2C1478656&title=IPv4%20address%20space%20by%20country%20%28APNIC%20region%29&unit=addresses" width="780" alt=""></a>

## APNIC 区域 ASN 数量按国家

```text
https://x-cmd.com/draw?type=hbar&labels=China%2CSouth%20Korea%2CJapan%2CIndia%2CSingapore%2CAustralia%2CTaiwan%2CIndonesia%2CVietnam%2CHong%20Kong%2CThailand%2CPakladesh%2CPhilippines%2CMalaysia%2CNew%20Zealand%2CBangladesh&values=6397%2C1149%2C1267%2C6253%2C682%2C2956%2C419%2C4027%2C1049%2C1180%2C604%2C470%2C656%2C390%2C694%2C2022&title=Autonomous%20systems%20per%20country%20%28APNIC%20region%29&unit=ASNs
```

<a href="https://x-cmd.com/draw?type=hbar&labels=China%2CSouth%20Korea%2CJapan%2CIndia%2CSingapore%2CAustralia%2CTaiwan%2CIndonesia%2CVietnam%2CHong%20Kong%2CThailand%2CPakladesh%2CPhilippines%2CMalaysia%2CNew%20Zealand%2CBangladesh&values=6397%2C1149%2C1267%2C6253%2C682%2C2956%2C419%2C4027%2C1049%2C1180%2C604%2C470%2C656%2C390%2C694%2C2022&title=Autonomous%20systems%20per%20country%20%28APNIC%20region%29&unit=ASNs"><img src="https://x-cmd.com/draw?type=hbar&labels=China%2CSouth%20Korea%2CJapan%2CIndia%2CSingapore%2CAustralia%2CTaiwan%2CIndonesia%2CVietnam%2CHong%20Kong%2CThailand%2CPakladesh%2CPhilippines%2CMalaysia%2CNew%20Zealand%2CBangladesh&values=6397%2C1149%2C1267%2C6253%2C682%2C2956%2C419%2C4027%2C1049%2C1180%2C604%2C470%2C656%2C390%2C694%2C2022&title=Autonomous%20systems%20per%20country%20%28APNIC%20region%29&unit=ASNs" width="780" alt=""></a>

## 两种形状

上面两张图用的是同一批国家、同一个顺序，按 IPv4 地址空间降序。这是刻意的，也正是把它们放在一起看的意义：一个国家在一张图里靠前、在另一张图里靠后，本身就在说明问题。请把它们当成一对来看。

中国拿下该区域一半以上的 IPv4 空间，在这个切面里是 2.147 亿地址对约 3.5 亿，却只有 6,397 个 ASN。地址池极大，握在其中的路由运营者却相对少。分配块大、集中度高，这是少数几家大运营商和云厂商握着空间、其余都是租用的市场的形状。

印度恰好是镜像。6,253 个 ASN，几乎和中国一样多，却只分到 2,631 万地址。它按地址空间排第 4，按自治系统数排第 2。自治系统多，每个的地址少，这是被切得很碎的 ISP 市场：成千上万家区域运营商，每家一块不大的地址、一套自己的路由。

印尼把这个特点推得更极端，1,300 万地址撑 4,027 个 ASN，按地址空间第 8，按自治系统数第 3。

孟加拉国是最极端的一例。147 万地址撑 2,022 个 ASN，按地址空间第 16，而按 ASN 数比日本、韩国、台湾三家加起来还多。一个年轻、竞争激烈的 ISP 市场，地址空间并不多。

同一根轴的另一端是日本的 1,267、台湾的 419 和韩国的 1,149。各自几千万地址，却只握着一两百到一千个自治系统。这是该区域里最不碎片化的路由市场，少数几家大运营商握着空间，并且自己路由。

## 有意思的是那个比值

把两张图合起来看，会掉出第三个数字：每百万地址对应多少个 ASN，一个国家路由市场碎片化程度的粗略指标，也就是互联网控制面有多少落在那里。

- 中国：每百万地址约 30 个 ASN
- 印度：约 238 个
- 日本：约 23 个
- 孟加拉国：约 1,367 个，全区域最碎

按地址算，印度的路由市场比中国碎约八倍，后果是实在的。独立运营者多意味着路径更多样，对韧性有利；但也意味着路由出错导致流量黑洞的地方更多，而且需要保持修补的、由独立主体运营的基础设施表面大得多。

## 对 ASN 来说，「在哪里」是个错问题

ASN 属于一个组织，而组织是全球运营的，上面按国家统计的 ASN 描述的是运营者注册在哪里，不是它的网络在哪里。一家在某国注册、ASN 也在该国的云服务商，可能在为每个大洲广播前缀。

所以诚实地使用 ASN 那张图是把它当成关于结构的图：一个国家的地址空间分属多少个独立网络，因此有多少个组织有能力对这些地址做路由决策。这是个治理问题，值得问。

## 注意事项

这一刀覆盖亚太注册机构，因为 APNIC 是在同一个 delegated 导出里发布逐国家行的那一家。ARIN 和 RIPE NCC 另有文件，国别覆盖范围也不同。

没有 IPv6，因为同一个导出的 IPv6 行只有汇总。已委派不等于已路由，已分配的 IPv4 里有相当一部分从来没在 BGP 里被宣告过。以上全部是 2026-10-29 的快照。

> 复现：`curl -s https://ftp.apnic.net/stats/apnic/delegated-apnic-latest`，然后把每个 `<cc>|ipv4|<start>|<count>|...` 行的 `value` 列加起来，ASN 行同理。
