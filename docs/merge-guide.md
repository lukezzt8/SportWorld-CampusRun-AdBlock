# 合并指南

本项目提供三种使用方式,按你的情况选择。

---

## 一、直接导入(不需要科学上网)

用 `configs/standalone.yaml`。

里面有一个名为 `DIRECT-PLACEHOLDER` 的直连占位节点 —— 它仅用于通过客户端对
`proxies` 字段的校验,不提供任何代理能力。

导入后:除广告域名被拒绝外,其余流量全部直连。

**步骤**

1. 把 `configs/standalone.yaml` 传到手机(建议重命名为纯英文,如 `campusrun.yaml`)
2. Clash Meta for Android →「配置」→「+」→「从文件导入」→ 选它
3. 点中该配置 → 打开 VPN 开关
4. 模式选择「**规则**」

---

## 二、作为 rule-provider(推荐)

用 `rules/campusrun.yaml`。这是最干净的方式 —— 规则与你的配置分离,更新规则不用改主配置。

**步骤**

1. 把 `rules/campusrun.yaml` 放到 Clash 配置目录的 `ruleset/` 下
2. 在主配置里声明 provider:

```yaml
rule-providers:
  campusrun:
    type: file
    behavior: domain
    format: yaml
    path: ./ruleset/campusrun.yaml
```

3. 在 `rules:` 的**最前面**加入引用:

```yaml
rules:
  - 'RULE-SET,campusrun,REJECT'
  # ... 你原有的规则保持不动
  - 'MATCH,你的代理组'    # 你的兜底规则保持在最后
```

4. 重载配置

---

## 三、叠加 / 合并进现有配置

用 `configs/prepend-rules.yaml`。

适用场景:

- **Clash Verge Rev**:「订阅」→「规则(Rules)覆写」→ 把 `prepend:` 下的条目粘进去
- **mihomo**:使用 mixin / patch 功能
- **手动合并**:把 128 条规则粘到你配置 `rules:` 的第一行下面

> ⚠️ **不要**把 `MATCH` 兜底规则一起粘进去。本项目提供的叠加文件已刻意不含兜底规则,
> 就是为了避免覆盖你自己的兜底规则(那会导致所有流量直连、无法代理)。

---

## 四、其他工具

| 工具 | 用哪个文件 |
|---|---|
| AdGuard / AdAway / NetGuard / RethinkDNS | `rules/hosts.txt` |
| 路由器(Pi-hole / OpenWrt / AdGuard Home) | `rules/domains.txt` |
| 支持通配符的 DNS 过滤 | `rules/domains.txt`(父域部分) |

---

## 五、验证是否生效

打开目标 App 翻几页,然后在 Clash 里查看:

### 方法一:「连接」页(最直接)

搜索 `snssdk`、`gdt.qq.com`、`kuaishou`、`mobads`、`yfanads`,看**代理列是否显示 REJECT**。

### 方法二:「日志」页

先确认日志级别是 `info`(`warning` 级别不记录成功匹配),然后搜索 `REJECT`。
命中时日志格式如下:

```
[TCP] 127.0.0.1:xxxxx --> i.snssdk.com:443 match DomainSuffix(snssdk.com) using REJECT
```

- 看到 `using REJECT` → ✅ 成功
- 看到 `using 你的节点名` → ⚠️ 规则未命中
- 看到目标域名**完全没有出现** → 可能命中了前面更宽的规则

---

## 六、排查

| 现象 | 原因 | 解决 |
|---|---|---|
| 日志里搜不到任何广告域名 | Clash 没接管 App 流量 | **必须开 TUN/VPN 模式**;检查应用过滤是否排除了该 App |
| 有域名但没被 REJECT | 规则没加载或顺序不对 | 确认规则在 `rules:` 最前面,且模式是「规则」不是「全局」 |
| 全局模式下无效 | 全局模式会忽略所有规则 | 切回「规则」模式 |
| 应用某些功能坏了 | 可能误屏蔽了业务域 | 见下方「已剔除清单」 |
| 代理节点不能用了 | `prepend` 里误加了 `MATCH` | 删掉 `MATCH` 那一行 |

---

## 七、已刻意剔除的域名(如果你要自己扩展,请避开这些)

### 该 App 的业务域 — 绝对不能屏蔽

```
*.iydsj.com           业务主域(登录/跑步/发现/H5/上传)
*.amap.com            高德地图(轨迹/定位/瓦片)
*.autonavi.com        高德地图瓦片
*.gepush.com / *.getui.com / *.igexin.com    个推推送
*.cmpassport.com      中国移动号码认证(登录)
*.geetest.com         极验验证码(登录)
*.tbs.org / *.tbs.imtt.qq.com                腾讯浏览内核(H5 依赖)
```

### 共享基础设施域 — 屏蔽可能连累系统服务

```
tracking.miui.com
sdkconfig.ad.*.xiaomi.com         ← 已屏蔽(纯广告)
metrics*.data.hicloud.com
metrics1-drcn.dt.dbankcloud.cn
metrics-ad-drcn.dt.hihonorcloud.com
```

### 裸父域 — 只保留具体广告子域

```
baidu.com / qq.com / sogou.com / xiaomi.com / taobao.com
```

> **一个真实教训**:本项目开发过程中曾考虑屏蔽 `g.alicdn.com`,
> 因为域名里含 "alicdn"。但它实际是**淘宝/天猫的图片 CDN**,
> 屏蔽会导致大量 App 的图片加载失败。
> **不要仅凭域名关键词判断是否为广告。**
