# 按国家看地址空间与 ASN

**按国家划分的 IPv4 地址空间（APNIC 区域）** —— 是地址数，不是块数。

```text
https://x-cmd.com/draw?type=hbar&labels=CN%2CKR%2CJP%2CIN%2CSG%2CAU%2CTW%2CID%2CVN%2CHK%2CTH%2CPK%2CPH%2CMY&values=214719744%2C57649152%2C54388992%2C26313216%2C21224704%2C19878912%2C19008000%2C12987136%2C12063744%2C6200576%2C4043264%2C3815424%2C3424512%2C3337728&title=IPv4%20address%20space%20by%20country%20%28APNIC%20region%29&unit=addresses
```

<a href="https://x-cmd.com/draw?type=hbar&labels=CN%2CKR%2CJP%2CIN%2CSG%2CAU%2CTW%2CID%2CVN%2CHK%2CTH%2CPK%2CPH%2CMY&values=214719744%2C57649152%2C54388992%2C26313216%2C21224704%2C19878912%2C19008000%2C12987136%2C12063744%2C6200576%2C4043264%2C3815424%2C3424512%2C3337728&title=IPv4%20address%20space%20by%20country%20%28APNIC%20region%29&unit=addresses"><img src="https://x-cmd.com/draw?type=hbar&labels=CN%2CKR%2CJP%2CIN%2CSG%2CAU%2CTW%2CID%2CVN%2CHK%2CTH%2CPK%2CPH%2CMY&values=214719744%2C57649152%2C54388992%2C26313216%2C21224704%2C19878912%2C19008000%2C12987136%2C12063744%2C6200576%2C4043264%2C3815424%2C3424512%2C3337728&title=IPv4%20address%20space%20by%20country%20%28APNIC%20region%29&unit=addresses" width="780" alt="asn"></a>

**按国家划分的 ASN 数量（APNIC 区域）** —— 每个国家持有多少个自治系统。

```text
https://x-cmd.com/draw?type=hbar&labels=CN%2CIN%2CID%2CAU%2CBD%2CJP%2CHK%2CKR%2CVN%2CNZ%2CSG%2CPH%2CTH%2CPK&values=6397%2C6253%2C4027%2C2956%2C2022%2C1267%2C1180%2C1149%2C1049%2C694%2C682%2C656%2C604%2C470&title=ASNs%20per%20country%20%28APNIC%20region%29&unit=ASNs
```

<a href="https://x-cmd.com/draw?type=hbar&labels=CN%2CIN%2CID%2CAU%2CBD%2CJP%2CHK%2CKR%2CVN%2CNZ%2CSG%2CPH%2CTH%2CPK&values=6397%2C6253%2C4027%2C2956%2C2022%2C1267%2C1180%2C1149%2C1049%2C694%2C682%2C656%2C604%2C470&title=ASNs%20per%20country%20%28APNIC%20region%29&unit=ASNs"><img src="https://x-cmd.com/draw?type=hbar&labels=CN%2CIN%2CID%2CAU%2CBD%2CJP%2CHK%2CKR%2CVN%2CNZ%2CSG%2CPH%2CTH%2CPK&values=6397%2C6253%2C4027%2C2956%2C2022%2C1267%2C1180%2C1149%2C1049%2C694%2C682%2C656%2C604%2C470&title=ASNs%20per%20country%20%28APNIC%20region%29&unit=ASNs" width="780" alt="asn"></a>

两种形状，讲的是两个故事：

- **中国拿下该区域一半以上的 IPv4 空间**（在这个前 14 名的切面里，2.147 亿对约 3.5 亿），但只有 6,397 个 ASN —— 地址池很大，路由运营者相对少。分配块大、集中度高。
- **印度恰好是镜像**：6,253 个 ASN，几乎和中国一样多，却只分到 2,631 万个地址。自治系统多，每个的地址少。这就是一个被切得很碎的大型 ISP 市场的样子。
- **日本（1,267）和韩国（1,149）**相对各自的地址空间，比印度还要密。
- **孟加拉国（2,022 个 ASN）和印尼（4,027）**是同一模式的小号版本。

ASN 数与地址数的比值，是衡量一个国家路由市场碎片化程度的好指标 —— 也就等于说，这个国家的**互联网控制面**有多少落在那儿。

> 范围说明：这里的切法限定 APNIC 区域，因为 APNIC 在同一个 delegated 导出里发布逐国家的行；ARIN 和 RIPE NCC 的逐国家行在各自另外的文件里。
> 复现：`curl -s https://ftp.apnic.net/stats/apnic/delegated-apnic-latest`，把每个 `<cc>|ipv4|<start>|<count>|...` 行的 `value` 列加起来。
