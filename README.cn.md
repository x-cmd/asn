# asn —— 地址空间与自治系统

> 🌐 **English version: [README.md](./README.md)**.
>
> **📑 目录**
>
> - [互联网是怎么被编号的](#about)
> - [谁在分配什么](#rir-allocation)
> - [按国家看地址空间与 ASN](#country-allocation)
>
> **开发者文档** —— 仓库结构、命名规则、CI：[`CONTRIBUTING.md`](./CONTRIBUTING.md)
> **数据使用** —— 命令、schema、脚本：[`SKILL.md`](./SKILL.md)

互联网跑在两种数字上：**地址**（包该送到哪里）和**自治系统号**（哪个网络在广播通往它的路由）。它们由五个区域注册机构沿着一条产业链分配。

这个站点读那些注册机构自己发布的统计文件，把它们画成「数据在 URL 里」的图。

## 页面

### 地址、自治系统，以及发放它们的那五个机构 — [全文](docs/1-overview/01-about/cn.md)

### 谁在分配什么，以及这种形状意味着什么 — [全文](docs/2-registry/01-rir-allocation/cn.md)

<a href="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CAPNIC%2CLACNIC%2CAFRINIC&values=100774%2C81326%2C58336%2C20450%2C5558&title=IPv4%20allocations%20by%20RIR&unit=blocks"><img src="https://x-cmd.com/draw?type=hbar&labels=RIPE%20NCC%2CARIN%2CAPNIC%2CLACNIC%2CAFRINIC&values=100774%2C81326%2C58336%2C20450%2C5558&title=IPv4%20allocations%20by%20RIR&unit=blocks" width="680" alt=""></a>

### 按国家看地址空间与自治系统 — [全文](docs/2-registry/02-country-allocation/cn.md)

<a href="https://x-cmd.com/draw?type=hbar&labels=China%2CSouth%20Korea%2CJapan%2CIndia%2CSingapore%2CAustralia%2CTaiwan%2CIndonesia%2CVietnam%2CHong%20Kong%2CThailand%2CPakladesh%2CPhilippines%2CMalaysia%2CNew%20Zealand%2CBangladesh&values=214719744%2C57649152%2C54393088%2C26313216%2C21224704%2C19878912%2C19008000%2C12987136%2C12063744%2C6200576%2C4043264%2C3815424%2C3424512%2C3337728%2C2222848%2C1478656&title=IPv4%20address%20space%20by%20country%20%28APNIC%20region%29&unit=addresses"><img src="https://x-cmd.com/draw?type=hbar&labels=China%2CSouth%20Korea%2CJapan%2CIndia%2CSingapore%2CAustralia%2CTaiwan%2CIndonesia%2CVietnam%2CHong%20Kong%2CThailand%2CPakladesh%2CPhilippines%2CMalaysia%2CNew%20Zealand%2CBangladesh&values=214719744%2C57649152%2C54393088%2C26313216%2C21224704%2C19878912%2C19008000%2C12987136%2C12063744%2C6200576%2C4043264%2C3815424%2C3424512%2C3337728%2C2222848%2C1478656&title=IPv4%20address%20space%20by%20country%20%28APNIC%20region%29&unit=addresses" width="680" alt=""></a>

## 图是怎么工作的

每一张图都是一个明文的 `https://x-cmd.com/draw?type=…` URL —— 由 [x-cmd/draw](https://github.com/x-cmd/draw) 渲染成独立 SVG。页面上不跑任何 JavaScript；SVG 是静态的、可打印的、机器可读的，因为**参数本身就是数据**。

## 许可证

本仓库自身的��容以 Apache-2.0 授权（见 [LICENSE](./LICENSE)）。`data/` 下的派生数据来自各注册机构与公开 API，来源与日期见各文件头部。
