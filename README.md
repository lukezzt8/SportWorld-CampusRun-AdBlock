# 运动世界校园 · 去广告规则

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Rules](https://img.shields.io/badge/rules-128-blue.svg)](#规则构成)
[![No Root](https://img.shields.io/badge/root-not%20required-green.svg)](#为什么不需要-root)

针对 **运动世界校园**(`com.zjwh.android_wh_physicalfitness`)的广告域名拦截规则。

规则从 APK 静态逆向提取,并经手机实测日志验证。**不修改 APK、不需要 Root。**

---

## 规则构成

| 类型 | 条数 | 说明 |
|---|---|---|
| `DOMAIN-SUFFIX` 父域规则 | 99 | 一条覆盖整棵子树,抗动态子域 |
| `DOMAIN` 精确规则 | 29 | 补齐父域规则未覆盖的部分 |
| **合计** | **128** | |

覆盖的广告平台:

| 平台 | 归属 | 代表域名 |
|---|---|---|
| 腾讯优量汇 | 腾讯 | `gdt.qq.com`、`sdk.e.qq.com` |
| 快手磁力引擎 | 快手 | `kuaishou.com`、`kskwai.com` |
| 穿山甲 / GroMore | 字节跳动 | `snssdk.com`、`pstatp.com` |
| 百度 Mobads | 百度 | `mobads.baidu.com`、`cpro.baidustatic.com` |
| 阿里天目 | 阿里 | `tianmu.mobi` |
| 京东广告 | 京东 | `xlog.jd.com`、`dsp-x.jd.com` |
| YFAN AdX | 第三方 | `yfanads.com` |
| AdGain | 第三方 | `adgain.cn` |
| 友盟统计 | 阿里 | `umeng.com`、`umengcloud.com` |
| OTS 线下推广 | 第三方 | `zztfly.com`、`idazhe.net` |
| 厂商广告 SDK | 华为/荣耀/OPPO/小米 | `ads-adnet-drcn.cloud.honor.com` 等 |

---

## 快速开始

### 方式一:直接导入(不需要代理)

用 [`configs/standalone.yaml`](configs/standalone.yaml) —— 含一个直连占位节点,
导入后除广告域名被拦截外,其余流量全部直连。

适用于:只想屏蔽广告,不需要科学上网。

### 在线订阅地址(URL 方式)

仓库已托管在 GitHub,可以直接用 URL 引用,不用手动下载:

**Clash rule-provider 地址(GitHub 源):**

`
https://raw.githubusercontent.com/lukezzt8/SportWorld-CampusRun-AdBlock/main/rules/campusrun.yaml
`

**国内推荐用 jsDelivr CDN 镜像**(GitHub raw 在国内经常无法访问,CDN 更快更稳):

`
https://cdn.jsdelivr.net/gh/lukezzt8/SportWorld-CampusRun-AdBlock@main/rules/campusrun.yaml
`

**hosts 文件地址:**

`
https://cdn.jsdelivr.net/gh/lukezzt8/SportWorld-CampusRun-AdBlock@main/rules/hosts.txt
`

使用 CDN 地址作为 rule-provider 的示例:

`yaml
rule-providers:
  campusrun:
    type: http
    behavior: domain
    format: yaml
    interval: 86400
    url: 'https://cdn.jsdelivr.net/gh/lukezzt8/SportWorld-CampusRun-AdBlock@main/rules/campusrun.yaml'
`

> 用 URL 方式的好处:规则更新后客户端会自动拉取,不用手动替换文件。

> **关于缓存**:jsDelivr 与 GitHub raw 都有 CDN 缓存(通常几分钟到几小时)。
> 如果想确保拿到最新版本,可以在 URL 后加一个查询参数强制刷新,例如:
> `
> https://cdn.jsdelivr.net/gh/lukezzt8/SportWorld-CampusRun-AdBlock@main/rules/campusrun.yaml?v=1
> `
> 或者直接按 commit 哈希引用(永久有效、无缓存延迟):
> `
> https://cdn.jsdelivr.net/gh/lukezzt8/SportWorld-CampusRun-AdBlock@<commit>/rules/campusrun.yaml
> `

---

### 方式二:作为规则集叠加(推荐)

用 [`rules/campusrun.yaml`](rules/campusrun.yaml) 作为 Clash 的 `rule-provider`:

```yaml
rule-providers:
  campusrun:
    type: file
    behavior: domain
    format: yaml
    path: ./ruleset/campusrun.yaml

rules:
  - 'RULE-SET,campusrun,REJECT'   # 必须放在 rules 最前面
  - 'MATCH,你的代理组'             # 你原有的兜底规则保持在最后
```

### 方式三:合并进现有配置

用 [`configs/prepend-rules.yaml`](configs/prepend-rules.yaml) ——
适合 Clash Verge Rev 的「规则覆写」、mihomo 的 mixin/patch 功能。

> **如果你用 AI 帮忙合并配置**,请参考 [`docs/ai-prompt.md`](docs/ai-prompt.md),
> 里面写了完整的约束条件(不改节点、不加 MATCH 兜底、不许改成关键词匹配等),
> 可以避免 AI 改坏你的配置。

### 其他格式

- [`rules/domains.txt`](rules/domains.txt) —— 纯域名清单,适合 AdGuard / 路由器 / Pi-hole
- [`rules/hosts.txt`](rules/hosts.txt) —— hosts 格式,适合 AdAway / NetGuard / RethinkDNS

---

## ⚠️ 使用须知

**1. 必须开启 TUN 或 VPN 模式**

只开「系统代理」时,App 的流量不经过 Clash,规则不会生效。这是最常见的失败原因。

**2. 规则必须放在最前面**

Clash 的规则是**按顺序匹配**的。如果广告规则排在后面,前面的规则先命中了,拦截就不会生效。

**3. 不要添加 `MATCH` 兜底规则**

如果你用叠加方式,`prepend` 里**不要**加 `MATCH,xxx`,否则会覆盖你自己配置的兜底规则,
导致所有流量直连、无法代理。本项目提供的 `prepend-rules.yaml` 已经刻意不含兜底规则。

**4. 已刻意剔除的内容(避免误伤)**

- **裸父域**:`baidu.com`、`qq.com`、`sogou.com`、`xiaomi.com` —— 只保留具体广告子域
- **共享基础设施域**:`tracking.miui.com`、`metrics*.data.hicloud.com` 等 10 个
  —— 这些在小米/华为手机上属于系统级上报通道,屏蔽可能连累系统推送
- **全部业务域**:该 App 的 `*.iydsj.com`、高德地图、个推推送、登录认证域名

---

## 实测效果

单次会话日志统计(31 秒):

| 指标 | 数值 |
|---|---|
| 被拦截的广告连接 | **459 次** |
| 被拦截的广告主机 | **41 个** |
| 放行的业务连接 | 80 次 |
| **拦截率** | **85%** |

被拦截的域名示例:

```
sdk.e.qq.com                            86 次   腾讯优量汇主接口
open.e.kuaishou.com                     57 次   快手磁力
tracker.yfanads.com                     42 次   YFAN 广告追踪
api-access.pangolin-sdk-toutiao.com     38 次   穿山甲
v2mi.gdt.qq.com / mi.gdt.qq.com         63 次   优量汇曝光监测
mobads.baidu.com                        18 次   百度 Mobads
```

---

## 为什么不需要 Root

Android 从 4.0 起提供 **`VpnService`** 机制,普通应用即可创建本地 VPN 接管全部流量,
无需 Root、无需解锁。Clash Meta for Android、Clash Verge Rev 等客户端都基于此机制运行。

本方案**不修改 APK 的任何字节**,因此:

- 应用签名不变,加固方案的完整性校验不会触发
- 可随时一键撤销,不留痕迹
- 应用升级后规则依然有效

---

## 规则是怎么来的

| 来源 | 性质 |
|---|---|
| `res/xml/network_security_config.xml` | 开发者自己明文放行的 10 个广告域名 |
| `res/values/arrays.xml` → `adBlockUrl` | **应用自带的广告黑名单** |
| `assets/gdt_plugin/gdtadv2.jar` | 腾讯优量汇 SDK 本体(未加密) |
| `assets/bdxadsdk.jar` | 百度 Mobads SDK 本体(未加密) |
| `assets/ksad_idc.json` | 快手磁力 SDK 的完整 IDC 域名表 |
| `classes.dex` 明文字符串表 | 其余 SDK 域名与 API 路径 |
| **手机实测日志** | 补充静态分析拿不到的域名(如京东广告链路) |

详细的逆向过程见 [`docs/analysis.md`](docs/analysis.md)。

---

## 技术边界

**这个规则集拦截不了「服务端下发」的广告。**

如果广告数据是随正常接口响应一起返回的,那么它与业务数据走**同一个 HTTPS 连接、同一个域名**。
域名规则只能判断「连不连这个域名」,无法判断「这个连接返回的内容是广告还是正常数据」。

而该 App 的业务域是**不能屏蔽的** —— 登录、跑步记录、上传全部依赖它。

对于这类广告,需要使用**界面层**工具(如 [GKD](https://github.com/gkd-kit/gkd))在无障碍服务层面自动点击跳过。

**网络层(本规则集)+ 界面层(GKD)的组合,是免 Root、不改 APK 前提下的最优解。**

---

## 免责声明

- 本项目仅提供域名拦截规则,用于个人减少广告干扰。
- 规则来源于对公开分发的 APK 包的静态分析,不包含任何反编译代码或 SDK 内容。
- 本项目不修改目标应用,不破解任何付费功能,不绕过任何账号或会员限制。
- 请遵守目标应用的服务条款。使用本规则产生的任何后果由使用者自行承担。

## License

[MIT](LICENSE)
