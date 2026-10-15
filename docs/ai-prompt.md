# AI 提示词 — 让 AI 帮你合并这些规则

# AI 提示词 — 让别人的 AI 帮忙合并或改造规则

> 用法:把**本文件全文**加上 `02_叠加到你自己的配置.yaml`(或 `03_域名清单.txt`)一起发给对方的 AI,
> 对方 AI 就能按需帮他改配置。下面【】内的内容按他实际情况替换。

---

## 提示词正文(从这里开始复制)

```
你是一个 Clash / mihomo 配置专家。我给你一份「运动世界校园」App 的广告拦截规则,
请你帮我把它整合进我自己的 Clash 配置里。

【规则来源】
这些域名是从该 App 的 APK 里逆向提取的,并且用手机实测日志验证过:
静态来源: res/xml/network_security_config.xml、res/values/arrays.xml 的 adBlockUrl 数组、
          assets/gdt_plugin/gdtadv2.jar、assets/bdxadsdk.jar、assets/ksad_idc.json、
          classes.dex 明文字符串表
实测来源: Clash 日志(单次会话 459 次广告连接被拦截, 85% 拦截率)
覆盖的广告平台: 腾讯优量汇、字节穿山甲/GroMore、快手磁力、百度 Mobads、阿里天目、
                京东广告、YFAN AdX、AdGain、友盟、OTS 线下推广、以及 OPPO/小米/荣耀的广告 SDK

【我的环境】
- 客户端: 【例如 Clash Verge Rev / Clash Meta for Android / mihomo / OpenClash / FlClash】
- 客户端版本: 【例如 Clash Meta for Android 2.11.6】
- 内核: 【mihomo / clash.meta,如果不是请说明】
- 我现在的配置来源: 【机场订阅链接 / 本地文件】,文件在【路径】
- 我是否用 TUN 模式: 【是 / 否】

【规则文件内容】
(在此粘贴 02_叠加到你自己的配置.yaml 的全部内容,或 03_域名清单.txt 的全部内容)

【我的需求】
1. 把这些广告域名规则合并进我的配置,并且保证**每条广告规则都排在我原有规则的最前面**
   (规则按顺序匹配,排在后面就不会生效)
2. **不要改动**我的 proxies(节点)、proxy-groups(代理组)、dns、以及任何端口配置
3. **不要**加 MATCH 之类的兜底规则 —— 要保留我自己配置里的兜底规则
   (通常是最后那条 `MATCH,某个代理组`),否则我的流量会全部直连、无法科学上网
4. 输出**完整的、可以直接保存使用的配置文件**,不要只给片段
5. 合并后请逐项核对并告诉我:
   - 我原有规则有多少条、是否一条都没丢
   - 新增了多少条广告规则
   - 最后一条兜底规则是什么(应该是我的代理组,不是 DIRECT)
   - proxies 和 proxy-groups 是否和原来完全一致

【重要约束】
- 这些规则只包含广告域名,已刻意剔除了 baidu.com / qq.com / sogou.com / xiaomi.com
  这类裸父域,只保留具体广告子域。**请不要"帮我优化"成更宽泛的规则**,
  那样会误伤正常服务(例如 g.alicdn.com 是淘宝的图片 CDN,
  屏蔽它会让很多 App 的图片加载失败)。
- 同理,请不要把 DOMAIN 规则改成 DOMAIN-KEYWORD。关键词匹配会大面积误伤。
- 如果我的客户端不支持 prepend 语法,请告诉我正确的手动合并位置(通常是 rules: 下方第一行)。

【输出格式】
先给完整配置文件,再给一份改动说明(改了哪里、为什么)。
```

(提示词正文结束)

---

## 使用说明

### 本仓库提供的文件与适用场景

| 文件 | 适用场景 |
|---|---|
| `configs/standalone.yaml` | 只想屏蔽广告、不需要科学上网 → 直接导入 |
| `configs/prepend-rules.yaml` | 已有代理配置 → 配合上面的 AI 提示词合并 |
| `rules/domains.txt` | 使用 AdGuard / 路由器 / AdAway 等工具 → 纯域名 |
| `rules/hosts.txt` | 需要 hosts 格式的工具 |
| `rules/campusrun.yaml` | 使用 Clash 的 rule-provider 功能 |

### 合并到现有配置时的注意事项

1. **规则必须放在 `rules:` 的最前面**。Clash 的规则是按顺序匹配的,
   排在后面就不会生效。

2. **不要添加 `MATCH` 兜底规则**。如果加了,会覆盖你自己配置里的兜底规则,
   导致所有流量直连、无法代理。本仓库提供的
   `configs/prepend-rules.yaml` 已刻意不含兜底规则。

3. **必须开启 TUN / VPN 模式**。只开「系统代理」时,App 的流量不经过 Clash,
   规则不会生效 —— 这是最常见的失败原因。

### 其他格式

- `rules/domains.txt` —— 纯域名清单,适合 AdGuard / AdGuard Home / Pi-hole / 路由器
- `rules/hosts.txt` —— hosts 格式,适合 AdAway / NetGuard / RethinkDNS

---

## 配合 GKD 使用效果更好

这套规则工作在**网络层**。如果目标 App 的广告是**服务端下发**的
(广告数据混在正常接口响应里,与业务数据走同一个 HTTPS 连接),
那么域名规则在原理上拦不到 —— 因为它只能判断「连不连这个域名」,
无法判断「这个连接返回的是广告还是正常数据」。

这类广告需要使用**界面层**工具,推荐 [GKD](https://github.com/gkd-kit/gkd):

- 它基于无障碍服务,在界面层面自动点击跳过
- 不关心广告来自哪个域名、是否被缓存
- 可以直接处理开屏广告、弹窗、信息流广告位

**网络层(本规则集)+ 界面层(GKD)的组合,是免 Root、不修改 APK 前提下的最优解。**
