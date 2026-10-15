# 使用指南(小白版)

> 这份文档假设你**完全不懂技术**。照着做就行。

---

# 🎯 最简单的方法:复制一段话给 AI

**不需要看懂任何东西,不需要装任何软件。** 你只要做一件事:

## 打开任意一个 AI,把下面这段话发给它

```
我要给手机上的「运动世界校园」App 去掉广告。

我完全不懂技术,请你手把手教我,具体这样要求:

1. 规则已经有人做好了,地址是:
   https://github.com/lukezzt8/SportWorld-CampusRun-AdBlock

2. 每次只让我做一件事,我做完回复你「好了」,你再讲下一步

3. 告诉我每一步在手机上点哪里(要具体到"打开设置 → 找到应用 → ...")

4. 不要说专业名词。必须说的,先用大白话解释一遍

5. 我如果找不到或者不会,你要换个方式教我

6. 我发截图给你,你要能看懂

最后告诉我:怎么确认成功了?万一坏了怎么恢复原样?
```

## 然后按它的指示做就行

AI 会先问你几个问题(手机什么系统、有没有装过 Clash、要不要翻墙),
然后一步一步带你做完。

**卡住了就直接截图发给它。** 所有主流 AI 都支持发图。

## 用哪个 AI

随便哪个都行:

- **豆包 / DeepSeek / 通义千问** —— 国内的,不用梯子,推荐
- **ChatGPT / Claude** —— 需要梯子

---

# 如果它让你装 Clash,照下面做

AI 大概率会让你装一个叫 **Clash Meta for Android** 的 App。
它是免费的,看起来像个"翻墙软件",但你不用它翻墙,**只用它的广告过滤功能**。

## 装它

1. 打开手机浏览器,访问:
   ```
   https://github.com/MetaCubeX/ClashMetaForAndroid/releases
   ```
2. 找到最新版本,下载名字里带 **`arm64-v8a`** 的那个 `.apk` 文件
   (如果手机很老,就下 `armeabi-v7a` 的)
3. 点开下载好的文件安装。如果提示"未知来源",选「**允许**」

## 拿到规则文件

1. 手机浏览器打开:
   ```
   https://cdn.jsdelivr.net/gh/lukezzt8/SportWorld-CampusRun-AdBlock@main/configs/standalone.yaml
   ```
2. 页面会显示一堆文字,**长按 → 全选 → 复制**
3. 打开手机自带的「**记事本**」或「**备忘录**」
4. **粘贴**,然后**保存**,文件名改成 `ad.yaml`

## 导进 Clash

1. 打开 **Clash Meta for Android**
2. 找到「**配置**」(有的版本叫 Profiles)
3. 点右下角「**+**」
4. 选「**从文件导入**」
5. 选中刚才那个 `ad.yaml`
6. 回到主页,**点一下这个新配置让它变选中**

## 打开开关

1. 回到主页,**打开那个大开关**
2. 弹出「VPN 连接请求」→ 点「**允许**」
3. 如果看到「模式」选项,选「**规则**」

**好了。** 现在打开「运动世界校园」,广告应该就没了。

---

# ✅ 怎么确认成功

打开 Clash → 找「**连接**」或「**日志**」页面 → 打开「运动世界校园」翻几页 →
搜索 `snssdk`、`gdt`、`kuaishou` 这几个词。

**看到后面写着 `REJECT` 就成功了。**

---

# 🔄 万一出问题

**关掉 Clash 那个大开关,一切立刻恢复正常。** 不会留下任何痕迹。

---

# ❓ 常见问题

**Q:会不会影响手机其他功能?**
A:不会。规则只拦广告服务器,而且是精确到具体域名的。登录、地图、推送、支付
这些必须的域名都刻意避开了。

**Q:App 会不会发现我屏蔽了广告?**
A:不会。这个方法**完全不修改 App**,只是不让它连上广告服务器。

**Q:App 更新后还有效吗?**
A:有效。规则针对的是广告服务器域名,和 App 版本无关。

**Q:会不会封号?**
A:不会。你没改 App、没破解任何功能,只是不连广告服务器。

**Q:广告还有一些去不掉?**
A:有些广告是 App 自己服务器发过来的(和正常内容混在一起),域名屏蔽拿它没办法。
这种情况装一个 **GKD**(自动点击跳过广告的工具):
[github.com/gkd-kit/gkd](https://github.com/gkd-kit/gkd)

**Q:这些规则有毒吗?**
A:没有。规则就是一堆网址,不含任何程序。你可以打开文件看,里面全是这种:
```
DOMAIN-SUFFIX,snssdk.com,REJECT
```
意思是"不连 snssdk.com"。

**Q:我手机已经有梯子了,怎么办?**
A:**不要**用上面那个 `standalone.yaml`,会把你的节点弄丢。
改用 `configs/prepend-rules.yaml`,并且**这个最省事的办法仍然是问 AI** ——
把这句话告诉它:「我已经有代理订阅了,不要动我的节点」。

**Q:我用的是 AdGuard / 路由器,能行吗?**
A:能。用这个网址:
```
https://cdn.jsdelivr.net/gh/lukezzt8/SportWorld-CampusRun-AdBlock@main/rules/hosts.txt
```
**不确定怎么加?还是问 AI。**

---
---

# 以下内容给技术用户

不懂技术的话,**上面就够了,不用往下看**。

## 技术要点

| 项目 | 说明 |
|---|---|
| 规则条数 | 128 条(99 条 `DOMAIN-SUFFIX` 父域 + 29 条 `DOMAIN` 精确) |
| 适用客户端 | Clash / mihomo 系(Clash Verge Rev、Clash Meta for Android、FlClash…) |
| 覆盖平台 | 腾讯优量汇、字节穿山甲+GroMore、快手磁力、百度 Mobads、阿里天目、京东广告、YFAN AdX、AdGain、友盟、OTS 线下推广、厂商广告 SDK |
| 实测拦截率 | 85%(单次会话 459 次广告连接被拦截) |
| **关键约束 1** | 规则必须位于 `rules:` **最前面** —— Clash 按顺序匹配,放后面不生效 |
| **关键约束 2** | 叠加使用时**不可加 `MATCH` 兜底规则**,否则会覆盖你自己的兜底规则导致无法代理 |
| **关键约束 3** | 必须开 **TUN / VPN 模式**。「系统代理」模式下 App 流量不经过 Clash |

## 三种接入方式

**① 独立导入**(不需要代理)
`configs/standalone.yaml` —— 含直连占位节点,除广告外全部直连。

**② rule-provider**(推荐)
```yaml
rule-providers:
  campusrun:
    type: http
    behavior: domain
    format: yaml
    interval: 86400
    url: 'https://cdn.jsdelivr.net/gh/lukezzt8/SportWorld-CampusRun-AdBlock@main/rules/campusrun.yaml'

rules:
  - 'RULE-SET,campusrun,REJECT'    # 必须在最前面
  - 'MATCH,你的代理组'              # 你原有的兜底规则保持在最后
```

**③ 叠加合并**
`configs/prepend-rules.yaml` —— 适用于 Clash Verge Rev 的规则覆写、mihomo 的 mixin/patch。

## 其他格式

- `rules/domains.txt` —— 纯域名,给 AdGuard Home / Pi-hole / 路由器
- `rules/hosts.txt` —— hosts 格式,给 AdAway / NetGuard / RethinkDNS

## 已刻意剔除的域名(自行扩展时请避开)

**业务域(屏蔽会破坏功能)**
```
*.iydsj.com          业务主域(登录/跑步/发现/H5/上传)
*.amap.com           高德地图(轨迹/定位/瓦片)
*.autonavi.com       高德地图瓦片
*.gepush.com / *.getui.com / *.igexin.com   个推推送
*.cmpassport.com     中国移动号码认证(登录)
*.geetest.com        极验验证码(登录)
*.tbs.org / *.tbs.imtt.qq.com               腾讯浏览内核(H5 依赖)
```

**共享基础设施域(屏蔽可能连累系统服务)**
```
tracking.miui.com
metrics*.data.hicloud.com
metrics1-drcn.dt.dbankcloud.cn
metrics-ad-drcn.dt.hihonorcloud.com
```

**裸父域(只保留具体广告子域)**
```
baidu.com / qq.com / sogou.com / xiaomi.com
```

> **一个真实教训**:开发过程中曾考虑屏蔽 `g.alicdn.com`(因为域名里含 "alicdn"),
> 但它实际是**淘宝/天猫的图片 CDN**,屏蔽会导致大量 App 图片加载失败。
> **不要仅凭域名关键词判断是否为广告。**

## 想自己重新分析一遍

见仓库的 `docs/analysis.md`,以及 `docs/ad-domains.md`(596 行域名证据报告)。
