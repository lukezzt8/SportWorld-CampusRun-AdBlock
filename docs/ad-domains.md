# 运动世界校园 7.3.70 — 广告域名 / 广告追踪端点 分析报告

包名 `com.zjwh.android_wh_physicalfitness` · versionName 7.3.70 · versionCode 401  
分析日期 2026-10-15  
分析方式: 静态分析(apktool 反编译产物 + `base.apk` 内 `classes.dex` 明文字符串 + 广告 SDK jar + `lib/*.so`)

> **最重要的结论**: 本应用**没有自建广告域名**。广告全部来自 12 个第三方 SDK,每个 SDK 都有各自独立的、与应用业务无关的域名。因此按域名屏蔽广告**不会影响**登录、跑步轨迹、地图、蓝牙与上传 —— 只要你不误伤下面 DO-NOT-BLOCK 的域名。

---

# 🚫 DO NOT BLOCK — 绝对不能屏蔽 (会影响真实功能)

## 本应用自有域名 (浙江万航 / gxapp / iydsj) — 登录·跑步·地图·上传·H5 全靠它

| 域名 | 用途 |
|---|---|
| `iydsj.com` | 根域; 所有业务域名都在其下 |
| `gxapp.iydsj.com` | 业务主域 |
| `h5.gxapp.iydsj.com` | 全部 H5 页面: 隐私政策/跑步规则/成绩/赛事/教程 |
| `control.gxapp.iydsj.com` | 升级/公告接口 /getMessage |
| `datapoint.gxapp.iydsj.com` | 埋点与上报: /api/v76/startup/pv, /datacollection/pv, /share, /run/pv |
| `discovery.gxapp.iydsj.com` | 发现页接口 |
| `run.gxapp.iydsj.com` | 跑步业务接口 |
| `partner.iydsj.com` | 合作方接口 /api/v620/datacollection/geetest/deviceVerify |
| `swapp-image.gxapp.iydsj.com` | 图片资源 |
| `xmly.gxapp.iydsj.com` | 喜马拉雅第三方 token 校验 |
| `gxapp-images.iydsj.com` | 图片 CDN (默认头像等) |
| `gxapp-vedio.iydsj.com` | 视频 CDN (跑步视频) |
| `api.physictool.iydsj.com` | 体测工具 API |
| `gxapp-images.obs.cn-east-3.myhuaweicloud.com` | 图片对象存储 (华为云) |
| `gxapp-vedio.obs.cn-east-3.myhuaweicloud.com` | 视频对象存储 (华为云) |
| `gxapp-images.oss-cn-qingdao.aliyuncs.com` | 图片对象存储 (阿里云) |
| `gxapp-vedio.oss-cn-qingdao.aliyuncs.com` | 视频对象存储 (阿里云) |
| `iydsj-hbase-hot.obs.cn-east-3.myhuaweicloud.com` | HBase 热数据存储 |
| `api2.lptiyu.com` | 合作方(乐跑体育)登录须知/隐私接口 |
| `m.fxzb.vip` | 应用内商城 commerce_url |
| `www.boxkj.com` | 用户协议/服务协议页面 |
| `mh5.huachenjie.com` | 电子合同/投保页面 (跑步保险) |

## 高德地图 AMap / AutoNavi — 地图·定位·轨迹·导航

| 域名 | 用途 |
|---|---|
| `amap.com` | 高德根域 (com.amap.api.v2.apikey 已配置) |
| `restapi.amap.com` | 高德 Web 服务 API (geohub 3D tiles, aaid) |
| `restsdk.amap.com` | 高德定位/合规/v3·v4 接口 |
| `apilocate.amap.com` | 高德定位 (基站/WiFi) |
| `abroad.apilocate.amap.com` | 高德海外定位 |
| `dualstack-a.apilocate.amap.com` | 高德定位双栈 |
| `dualstack-arestapi.amap.com` | 高德 arestapi 双栈 (iasdkauth) |
| `httpdns.apilocate.amap.com` | 高德 HTTPDNS |
| `adiu.amap.com` | 高德设备标识 adius |
| `cgicol.amap.com` | 高德定位数据采集 |
| `autoapi.amap.com` | 高德汽车/导航 API |
| `lbs.amap.com` | 高德 LBS |
| `lbs-3dtiles-service.amap.com` | 高德 3D 瓦片 |
| `m5.amap.com` | 高德 H5 |
| `mps.amap.com` | 高德 MPS |
| `mpsapi.amap.com` | 高德 MPS API (图层 UGC) |
| `ts.amap.com` | 高德 TS |
| `tsapi.amap.com` | 高德 TS API |
| `wap.amap.com` | 高德 WAP |
| `cache.amap.com` | 高德缓存 |
| `asset.amap.com` | 高德静态资源 |
| `aos-cdn-image.amap.com` | 高德图片 CDN |
| `faas-image.amap.com` | 高德 FaaS 图片 |
| `pushimg.amap.com` | 高德推送图片 |
| `amap-activity.oss-cn-beijing.aliyuncs.com` | 高德活动 OSS |
| `is.autonavi.com` | 高德地图瓦片域 |
| `d.is.autonavi.com` | 高德瓦片 (d) |
| `mst01.is.autonavi.com` | 高德地图瓦片 appmaptile (轨迹底图) |
| `store.is.autonavi.com` | 高德 POI 瓦片 |
| `travel.is.autonavi.com` | 高德出行瓦片 |
| `autonavi.com` | AutoNavi 根域 |

## 推送 (个推/华为/荣耀/小米/魅族) — 保留才能收通知

| 域名 | 用途 |
|---|---|
| `getui.com` | 个推 (com.igexin.sdk / com.getui) |
| `getui.net` | 个推备用域 |
| `gepush.com` | 个推 gepush |
| `igexin.com` | 个推 igexin |
| `gtc.getui.net` | 个推 GTC |
| `gtc.gepush.com` | 个推 GTC |
| `c-gtc.getui.net` | 个推 GTC (C 节点) |
| `c-gtc.gepush.com` | 个推 GTC (C 节点) |
| `b-gtc.getui.net` | 个推 GTC (B 节点) |
| `b-gtc.gepush.com` | 个推 GTC (B 节点) |
| `ido.getui.net` | 个推 IDO |
| `ido.gepush.com` | 个推 IDO |
| `c-ido.getui.net` | 个推 IDO (C 节点) |
| `c-ido.gepush.com` | 个推 IDO (C 节点) |
| `c-hzgt2.getui.com` | 个推接入 /api.php |
| `d-gt.getui.com` | 个推接入 /api.htm |
| `b-qj.gepush.com` | 个推场景日志 |
| `sdk-open-phone.getui.com` | 个推 SDK 接入 |
| `sdk.open.talk.getui.net` | 个推长连接 |
| `sdk.open.talk.gepush.com` | 个推长连接 |
| `sdk.open.talk.igexin.com` | 个推长连接 |
| `sdk.gtc.dim.al.tech` | 个推 GTC (阿里云) |
| `sdk.ido.type8.s1.info` | 个推 IDO |
| `sdk.ido.type11.s1.info` | 个推 IDO |
| `dbankcloud.com` | 华为 HMS 根域 (Push/AGC) |
| `dbankcloud.cn` | 华为 HMS 根域 |
| `dbankcloud.ru` | 华为 HMS 根域 (俄) |
| `hicloud.com` | 华为云根域 |
| `huawei.com` | 华为域 (HMS 账号/推送) |
| `data-drcn.push.dbankcloud.com` | 华为推送中国区 |
| `data-dra.push.dbankcloud.com` | 华为推送亚非拉 |
| `data-dre.push.dbankcloud.com` | 华为推送欧洲 |
| `data-drru.push.dbankcloud.com` | 华为推送俄罗斯 |
| `grs.dbankcloud.com` | 华为 GRS 路由 |
| `grs.dbankcloud.cn` | 华为 GRS 路由 |
| `grs.platform.dbankcloud.ru` | 华为 GRS 路由 (俄) |
| `hwcloudtest.cn` | 华为 HMS 测试域 |
| `xiaomi.com` | 小米域 (MiPush) |
| `cn.register.xmpush.xiaomi.com` | 小米推送注册 |
| `sandbox.xmpush.xiaomi.net` | 小米推送沙箱 |
| `resolver.msg.xiaomi.net` | 小米推送长连接解析 |
| `app.chat.xiaomi.net` | 小米推送通道 |
| `cn.app.chat.xiaomi.net` | 小米推送通道 (国内) |
| `api-push.meizu.com` | 魅族推送 |

## 登录 / 号码认证 / 人机校验 — 阻断会导致无法登录

| 域名 | 用途 |
|---|---|
| `geetest.com` | 极验验证 (登录/风控) |
| `gcaptcha4.geetest.com` | 极验 v4 验证 |
| `gcaptcha4.geevisit.com` | 极验 v4 备用 |
| `gcaptcha4.gsensebot.com` | 极验 v4 备用 |
| `static.geetest.com` | 极验静态资源 |
| `static.geevisit.com` | 极验静态资源 |
| `monitor.geetest.com` | 极验监控 |
| `riskct.geetest.com` | 极验风控上报 |
| `dkapi.geetest.com` | 极验 deepknow 设备指纹 |
| `cmpassport.com` | 中国移动统一认证 (一键登录) |
| `config2.cmpassport.com` | 移动认证配置 |
| `rcs.cmpassport.com` | 移动认证 token |
| `msg.cmpassport.com` | 移动认证取号 |
| `verify.cmpassport.com` | 移动认证校验 |
| `jzts.cmpassport.com` | 移动认证 |
| `log1.cmpassport.com` | 移动认证日志 |
| `log2.cmpassport.com` | 移动认证日志 |
| `wap.cmpassport.com` | 移动认证书面协议 |
| `id6.me` | 中国移动号码认证 (libzxprotect.so) |
| `crbt.i139.cn` | 中国移动彩铃/取号预令牌 |
| `card.e.189.cn` | 中国电信号码认证 |
| `e.189.cn` | 中国电信 SDK 协议 |
| `nisportal.10010.com` | 中国联通号码认证 (libzxprotect.so) |
| `auth.wosms.cn` | 联通/短信认证 |
| `msv6.wosms.cn` | 联通/短信认证 |
| `zxid.mobileservice.cn` | 中移互联网 ZXID 认证 |
| `zxid-m.mobileservice.cn` | 中移互联网 ZXID 认证 API |
| `api.ccidcall.cn` | UAID 认证接口 |
| `dashboard.ccidcall.cn` | UAID 隐私政策页 |
| `aip.baidubce.com` | 百度人脸识别 (跑步人脸核验) |

## 支付 / 社交登录 / 分享 — 阻断会导致无法支付或第三方登录

| 域名 | 用途 |
|---|---|
| `alipay.com` | 支付宝 SDK |
| `mclient.alipay.com` | 支付宝收银台 |
| `mobilegw.alipay.com` | 支付宝网关 |
| `wappaygw.alipay.com` | 支付宝 WAP 网关 |
| `mcgw.alipay.com` | 支付宝日志网关 |
| `loggw-exsdk.alipay.com` | 支付宝 SDK 日志 |
| `m.alipay.com` | 支付宝 H5 |
| `alipaydev.com` | 支付宝 SDK 预发 |
| `mobileclientgw.alipaydev.com` | 支付宝 SDK 预发网关 |
| `alipay.net` | 支付宝内部域 (SDK 常量, 可保留) |
| `log.wechatpay.cn` | 微信支付日志 |
| `weixin.qq.com` | 微信 OpenSDK |
| `api.weixin.qq.com` | 微信登录/用户信息 |
| `open.weixin.qq.com` | 微信登录二维码 |
| `long.open.weixin.qq.com` | 微信扫码登录 |
| `mp.weixin.qq.com` | 微信分享配置 |
| `api.weibo.com` | 微博登录 |
| `open.weibo.cn` | 微博授权 |
| `service.weibo.com` | 微博分享 |
| `openmobile.qq.com` | QQ 互联登录/分享 |
| `graph.qq.com` | QQ 互联 OpenAPI |
| `cgi.connect.qq.com` | QQ 互联策略配置 |
| `tauth.qq.com` | QQ 互联授权 |
| `imgcache.qq.com` | QQ 分享静态资源 |
| `a.app.qq.com` | 应用宝跳转页 |
| `sj.qq.com` | 应用宝应用详情 (本应用) |
| `appsupport.qq.com` | 腾讯开放平台统计 |
| `m.taobao.com` | 淘宝 deeplink |
| `h5.m.taobao.com` | 淘宝 H5 (第三方登录/跳转) |
| `alicdn.com` | 阿里 CDN (高德导航内引用) |
| `img.alicdn.com` | 阿里图片 CDN (高德导航资源) |

## WebView 内核 TBS / 系统组件

| 域名 | 用途 |
|---|---|
| `tbs.org` | 腾讯浏览服务 TBS |
| `x5.tbs.org` | TBS 内核下载 |
| `x5.oversea.tbs.org` | TBS 海外 |
| `x5.tencent.com` | TBS X5 |
| `soft.tbs.imtt.qq.com` | TBS 内核包 |
| `cfg.imtt.qq.com` | TBS 配置 |
| `log.tbs.qq.com` | TBS 日志 |
| `tbsrecovery.imtt.qq.com` | TBS 恢复 |
| `debugtbs.qq.com` | TBS 调试 |
| `debugx5.qq.com` | X5 调试 |
| `mdc.html5.qq.com` | TBS 内核直下 |
| `time.android.com` | Android NTP |
| `ntp2.aliyun.com` | 阿里 NTP (阿里云智能语音 SDK) |
| `whoami.akamai.net` | 网络诊断 (Akamai whoami) |
| `type.googleapis.com` | protobuf 类型标识 (非网络请求) |
| `dns.alidns.com` | 阿里公共 DNS (SDK 内 DoH 解析器) |
| `doh.pub` | 腾讯公共 DNS (SDK 内 DoH 解析器) |
| `nls-wave.aliyuncs.com` | 阿里云智能语音 NLS (TTS/语音) |
| `nls-router-one.cn-shanghai.aliyuncs.com` | 阿里云 NLS 路由 |
| `nls-log-gather.aliyuncs.com` | 阿里云 NLS 日志 |
| `schemas.alibaba-inc.com` | 阿里云 TTS schema (非网络请求) |

## 崩溃/统计遥测 (非广告; 若想彻底静默可自行取舍)

| 域名 | 用途 |
|---|---|
| `bugly.qq.com` | 腾讯 Bugly 崩溃上报 |
| `android.bugly.qq.com` | 腾讯 Bugly |
| `astat.bugly.qcloud.com` | Bugly 统计 |
| `astat.bugly.cros.wr.pvp.net` | Bugly 统计 |
| `h.trace.qq.com` | 腾讯灯塔埋点 |
| `log.wechatpay.cn` | 微信支付日志 |

### 关于应用自身 API 的重要提醒

应用的**业务 API 基址**(登录/上传/跑步数据)位于被网易易盾加固的 `classes.dex` 方法体内,**未以明文出现在任何可读产物中**。所有能恢复出来的应用自有域名都在 `*.iydsj.com` / `*.gxapp.iydsj.com` 之下。  
**建议**: 屏蔽时把 `*.iydsj.com` 整域加入白名单, 而不是只白名单上面列出的几个子域。

另外 `libapp.so` 是 Flutter AOT 快照, 其中只出现 `h5.gxapp.iydsj.com`, 未出现其它业务 API 域名。

---

# 📊 广告域名总览

共 **176** 个唯一广告/广告追踪主机名, 分属 14 个 SDK/广告网络。

| 广告 SDK | 域名数 | 证据强度 |
|---|---|---|
| 腾讯优量汇 GDT / 广点通 (com.qq.e / gdtadv2.jar) | 23 | 18 HIGH / 5 MEDIUM |
| 快手磁力引擎 (com.kwad / ksad) | 13 | 13 HIGH / 0 MEDIUM |
| 穿山甲 / Pangle / 字节跳动 (com.bytedance.sdk.openadsdk, com.bykv) | 39 | 35 HIGH / 4 MEDIUM |
| 阿里天目 (com.tianmu / adsuyi adapter) | 1 | 1 HIGH / 0 MEDIUM |
| 京东广告 (com.jd.ad.sdk) | 0 | 见「局限性」 |
| 华为广告 PPS / HMS 分析 (com.huawei.openalliance.ad, com.huawei.cloud.opensdkhianalytics) | 5 | 0 HIGH / 5 MEDIUM |
| 荣耀广告 (com.hihonor.adsdk) | 2 | 2 HIGH / 0 MEDIUM |
| OPPO / 欢太 广告 (com.opos.mobad, com.heytap.msp.mobad) | 7 | 6 HIGH / 1 MEDIUM |
| 小米广告 MiMo / zeus (com.miui.zeus.mimo) | 9 | 8 HIGH / 1 MEDIUM |
| 百度广告 (bdxadsdk.jar / com.baidu.mobads) | 27 | 23 HIGH / 4 MEDIUM |
| YFAN AdX (com.yfanads) | 6 | 5 HIGH / 1 MEDIUM |
| AdGain / 广推科技 (com.adgain.sdk) | 2 | 2 HIGH / 0 MEDIUM |
| 友盟+ 统计/追踪 (com.umeng) | 21 | 15 HIGH / 6 MEDIUM |
| OTS / 线下推广 (com.acse.ottn) + 关联统计域 | 17 | 11 HIGH / 6 MEDIUM |
| 其它广告网络 (应用自带 adBlockUrl 列表 / 资源表) | 4 | 2 HIGH / 2 MEDIUM |

---

# 🔍 按 SDK 分组明细

## 腾讯优量汇 GDT / 广点通 (com.qq.e / gdtadv2.jar)

| 主机名 | 置信度 | 依据 | 出处文件 |
|---|---|---|---|
| `c.gdt.qq.com` | **HIGH** | GDT 点击/曝光上报 (gdt_trace_a.fcg, yaq3_0.sec) | `assets/gdt_plugin/gdtadv2.jar!assets/yaq3_0.sec` |
| `c2.gdt.qq.com` | **HIGH** | GDT 广告请求域名, gdtadv2.jar | `assets/gdt_plugin/gdtadv2.jar!classes.dex` |
| `c3.gdt.qq.com` | **HIGH** | GDT 广告请求域名, gdtadv2.jar | `assets/gdt_plugin/gdtadv2.jar!classes.dex` |
| `mi.gdt.qq.com` | **HIGH** | GDT 曝光监测 gdt_mview.fcg + AndroidManifest 明文白名单 | `apk!res/8GD.xml`, `assets/gdt_plugin/gdtadv2.jar!assets/yaq3_0.sec`, `assets/gdt_plugin/gdtadv2.jar!classes.dex` (+1) |
| `v2mi.gdt.qq.com` | **HIGH** | GDT 曝光监测 v2, 明文白名单 network_security_config.xml | `apk!res/8GD.xml`, `assets/gdt_plugin/gdtadv2.jar!classes.dex`, `res/xml/network_security_config.xml` |
| `v3mi.gdt.qq.com` | **HIGH** | GDT 曝光监测 v3, 明文白名单 | `apk!res/8GD.xml`, `assets/gdt_plugin/gdtadv2.jar!classes.dex`, `res/xml/network_security_config.xml` |
| `v.gdt.qq.com` | **HIGH** | GDT 统计 gdt_stats.fcg | `assets/gdt_plugin/gdtadv2.jar!assets/yaq3_0.sec` |
| `v2.gdt.qq.com` | **HIGH** | GDT 广告请求域名 | `assets/gdt_plugin/gdtadv2.jar!classes.dex` |
| `v3.gdt.qq.com` | **HIGH** | GDT 广告请求域名 | `assets/gdt_plugin/gdtadv2.jar!classes.dex` |
| `t.gdt.qq.com` | **HIGH** | GDT 转化归因 conv/sdk/spa/conv | `assets/gdt_plugin/gdtadv2.jar!assets/yaq3_0.sec` |
| `win.gdt.qq.com` | **HIGH** | GDT 竞胜通知 win_notice.fcg | `assets/gdt_plugin/gdtadv2.jar!classes.dex` |
| `e.qq.com` | **HIGH** | GDT 主 API 域名 (e.qq.com/dev) | `assets/gdt_plugin/gdtadv2.jar!classes.dex` |
| `sdk.e.qq.com` | **HIGH** | GDT SDK API: /open/v1.1/ad/get_list, /report, /activate, /event | `assets/gdt_plugin/gdtadv2.jar!assets/yaq3_0.sec`, `assets/gdt_plugin/gdtadv2.jar!classes.dex` |
| `sdkquic.e.qq.com` | **HIGH** | GDT QUIC 接入点 | `assets/gdt_plugin/gdtadv2.jar!classes.dex` |
| `union.eff.qq.com` | **HIGH** | GDT 归因/日志/崩溃上报 pantheon/c/v1/log/* | `assets/gdt_plugin/gdtadv2.jar!assets/yaq3_0.sec`, `assets/gdt_plugin/gdtadv2.jar!classes.dex` |
| `qzs.gdtimg.com` | **HIGH** | GDT 联盟素材 CDN (union_cdn/page/temp_v2/... 明文 URL) | `assets/gdt_plugin/gdtadv2.jar!classes.dex` |
| `pgdt.ugdtimg.com` | **HIGH** | GDT 图片 CDN (https://pgdt.ugdtimg.com 明文 URL) | `assets/gdt_plugin/gdtadv2.jar!classes.dex` |
| `adsmind.ugdtimg.com` | **HIGH** | GDT 广告素材 CDN | `assets/gdt_plugin/gdtadv2.jar!classes.dex` |
| `qzs.qq.com` | **MEDIUM** | 腾讯联盟素材 CDN (/union/res/union_cdn/...) | `assets/gdt_plugin/gdtadv2.jar!classes.dex` |
| `i.gtimg.cn` | **MEDIUM** | GDT AMS 落地页性能脚本 CDN | `assets/gdt_plugin/gdtadv2.jar!classes.dex` |
| `qzonestyle.gtimg.cn` | **MEDIUM** | GDT 移动 SDK 下载/样式 CDN | `assets/gdt_plugin/gdtadv2.jar!classes.dex` |
| `mqqad.html5.qq.com` | **MEDIUM** | 腾讯 H5 广告脚本 (/adjs), 优量汇 WebView 广告 | `apk!classes.dex` |
| `tdid.m.qq.com` | **MEDIUM** | 腾讯 tdid 设备标识 (GDT 归因用) | `assets/gdt_plugin/gdtadv2.jar!classes.dex` |

## 快手磁力引擎 (com.kwad / ksad)

| 主机名 | 置信度 | 依据 | 出处文件 |
|---|---|---|---|
| `open.e.kuaishou.com` | **HIGH** | 快手广告 SDK API /rest/e/v3/open/sdk2 (classes.dex 明文) | `apk!classes.dex`, `assets/ksad_idc.json` |
| `open.e.kuaishou.cn` | **HIGH** | 快手广告 SDK API 备用域 (ksad_idc.json) | `assets/ksad_idc.json` |
| `api.e.kuaishou.com` | **HIGH** | 快手广告 eapi (ksad_idc.json) | `assets/ksad_idc.json` |
| `api.e.kuaishou.cn` | **HIGH** | 快手广告 eapi 备用 (ksad_idc.json) | `assets/ksad_idc.json` |
| `ulog-sdk.gifshow.com` | **HIGH** | 快手 SDK 日志上报 (ksad_idc.json ulog) | `assets/ksad_idc.json` |
| `ulog-sdk.ksapisrv.com` | **HIGH** | 快手 SDK 日志上报 (ksad_idc.json ulog) | `assets/ksad_idc.json` |
| `zt.gifshow.com` | **HIGH** | 快手 zt 广告域名 (ksad_idc.json) | `assets/ksad_idc.json` |
| `api.kuaishouzt.com` | **HIGH** | 快手 zt 广告域名 (ksad_idc.json) | `assets/ksad_idc.json` |
| `api.kwaizt.com` | **HIGH** | 快手 zt 广告域名 (ksad_idc.json) | `assets/ksad_idc.json` |
| `w1.kskwai.com` | **HIGH** | 快手广告素材 CDN (ksad_idc.json cdn) | `assets/ksad_idc.json` |
| `w2.kskwai.com` | **HIGH** | 快手广告素材 CDN (ksad_idc.json cdn) | `assets/ksad_idc.json` |
| `w1.gskwai.com` | **HIGH** | 快手广告素材 CDN (ksad_idc.json cdn) | `assets/ksad_idc.json` |
| `w2.gskwai.com` | **HIGH** | 快手广告素材 CDN (ksad_idc.json cdn) | `assets/ksad_idc.json` |

## 穿山甲 / Pangle / 字节跳动 (com.bytedance.sdk.openadsdk, com.bykv)

| 主机名 | 置信度 | 依据 | 出处文件 |
|---|---|---|---|
| `api-access.pangolin-sdk-toutiao.com` | **HIGH** | 穿山甲广告配置/请求 API | `apk!classes.dex` |
| `api-access.pangolin-sdk-toutiao1.com` | **HIGH** | 穿山甲 API 备用域 | `apk!classes.dex` |
| `api-access.pangolin-sdk-toutiao-b.com` | **HIGH** | 穿山甲 API 备用域 B | `apk!classes.dex` |
| `log-api.pangolin-sdk-toutiao.com` | **HIGH** | 穿山甲日志上报 /service/2/app_log/ | — |
| `log-api.pangolin-sdk-toutiao-b.com` | **HIGH** | 穿山甲日志上报备用 /service/2/app_log/ | — |
| `gromore.pangolin-sdk-toutiao.com` | **HIGH** | GroMore 聚合 SDK 域名 | `apk!classes.dex` |
| `i.snssdk.com` | **HIGH** | 穿山甲/字节 SDK 接口域 (network_security_config 明文白名单) | `apk!classes.dex`, `apk!res/8GD.xml`, `res/xml/network_security_config.xml` |
| `is.snssdk.com` | **HIGH** | 穿山甲 SDK 接口域 (明文白名单) | `apk!res/8GD.xml`, `res/xml/network_security_config.xml` |
| `pangolin.snssdk.com` | **HIGH** | 穿山甲专用域 (明文白名单) | `apk!res/8GD.xml`, `res/xml/network_security_config.xml` |
| `extlog.snssdk.com` | **HIGH** | 穿山甲扩展日志 (明文白名单) | `apk!res/8GD.xml`, `res/xml/network_security_config.xml` |
| `bds.snssdk.com` | **HIGH** | 穿山甲 bds 接口 (明文白名单) | `apk!res/8GD.xml`, `res/xml/network_security_config.xml` |
| `applog.snssdk.com` | **HIGH** | 字节 SDK 日志 /service/2/app_log/ | `apk!classes.dex` |
| `log.snssdk.com` | **HIGH** | 字节 SDK 日志/设备注册 abtest | `apk!classes.dex` |
| `rtlog.snssdk.com` | **HIGH** | 字节 SDK 实时日志 | `apk!classes.dex` |
| `rtapplog.snssdk.com` | **HIGH** | 字节 SDK 实时日志 | `apk!classes.dex` |
| `sf3-ttcdn-tos.pstatp.com` | **HIGH** | 穿山甲素材/CDN (明文白名单) | `apk!res/8GD.xml`, `res/xml/network_security_config.xml` |
| `sf3-fe-tos.pglstatp-toutiao.com` | **HIGH** | 穿山甲 csj-sdk-static 素材与落地页 CDN | `apk!classes.dex`, `assets/lottie_json/shake_phone.json`, `assets/lottie_json/swipe_right.json` |
| `sf1-amtos-cdn.bytesmanager.com` | **HIGH** | 穿山甲广告落地页/隐私页 CDN | `apk!classes.dex` |
| `dig.bdurl.net` | **HIGH** | 穿山甲深链 (明文白名单) | `apk!res/8GD.xml`, `res/xml/network_security_config.xml` |
| `toblog.ctobsnssdk.com` | **HIGH** | 穿山甲 toB 日志/ABTest/设备注册 | `apk!classes.dex` |
| `tobapplog.ctobsnssdk.com` | **HIGH** | 穿山甲 toB 日志上报 | `apk!classes.dex` |
| `success.ctobsnssdk.com` | **HIGH** | 穿山甲日志上报 | `apk!classes.dex` |
| `mon.zijieapi.com` | **HIGH** | 字节监控/埋点 | `apk!classes.dex` |
| `tnc3-aliec2.zijieapi.com` | **HIGH** | 字节 SDK 配置 CDN (阿里云) | `apk!classes.dex` |
| `tnc3-alisc1.zijieapi.com` | **HIGH** | 字节 SDK 配置 CDN (阿里云) | `apk!classes.dex` |
| `tnc3-bjlgy.zijieapi.com` | **HIGH** | 字节 SDK 配置 CDN (北京) | `apk!classes.dex` |
| `apmplus.volces.com` | **HIGH** | 字节 APMPlus 崩溃/会话上报 | `apk!classes.dex` |
| `cdn-tos-cn.bytedance.net` | **HIGH** | 穿山甲试玩广告组件 CDN | `apk!classes.dex` |
| `applog.bytedance.net` | **HIGH** | 字节 SDK 日志测试域 | `apk!classes.dex` |
| `scc.bytedance.com` | **MEDIUM** | 字节广告落地页 URL 安全扫描 (广告点击链路) | `apk!classes.dex` |
| `apps.bytesfield.com` | **HIGH** | 穿山甲应用信息接口 (广告归因) | `apk!classes.dex` |
| `apps.bytesfield-b.com` | **HIGH** | 穿山甲应用信息接口备用 | `apk!classes.dex` |
| `apps.oceanengine.com` | **HIGH** | 巨量引擎 (穿山甲后台) 应用信息接口 | `apk!classes.dex` |
| `phoniex.toutiao.com` | **MEDIUM** | 字节 Phoenix 埋点 | `apk!classes.dex` |
| `ad.toutiao.com` | **HIGH** | 头条广告落地页 tetris/page | `apk!classes.dex` |
| `www.toutiaopage.com` | **HIGH** | 头条广告落地页 (SDK 正则中与 ad.toutiao.com 并列) | `apk!classes.dex` |
| `www.chengzijianzhan.com` | **HIGH** | 橙子建站广告落地页 (SDK 正则中并列) | `apk!classes.dex` |
| `www.pangle.cn` | **MEDIUM** | 穿山甲官网 (合规页, 非流量) | `apk!classes.dex` |
| `www.csjplatform.com` | **MEDIUM** | 穿山甲平台官网 (合规页, 非流量) | `apk!classes.dex` |

> 说明: `res/xml/network_security_config.xml` 里被显式允许明文 HTTP 的 10 个域名中, 7 个属于穿山甲, 3 个属于 GDT —— 这是打包方**自己承认**的广告流量白名单, 证据等级最高。

## 阿里天目 (com.tianmu / adsuyi adapter)

| 主机名 | 置信度 | 依据 | 出处文件 |
|---|---|---|---|
| `sdk.tianmu.mobi` | **HIGH** | 天目广告 SDK 配置/API 主机 (classes.dex) | `apk!classes.dex` |

## 京东广告 (com.jd.ad.sdk)

_未在可读产物中恢复出任何明文主机名 —— 见文末「局限性」章节。_

## 华为广告 PPS / HMS 分析 (com.huawei.openalliance.ad, com.huawei.cloud.opensdkhianalytics)

| 主机名 | 置信度 | 依据 | 出处文件 |
|---|---|---|---|
| `metrics1-drcn.dt.dbankcloud.cn` | **MEDIUM** | HMS Ads/OpenSDK HiAnalytics 中国区上报 (grs_sdk_global_route_config_opensdkService.json) | `assets/grs_sdk_global_route_config_opensdkService.json` |
| `metrics-dra.dt.hicloud.com` | **MEDIUM** | HMS Ads/OpenSDK HiAnalytics 亚非拉上报 | `assets/grs_sdk_global_route_config_opensdkService.json` |
| `metrics2.data.hicloud.com` | **MEDIUM** | HMS Ads/OpenSDK HiAnalytics 欧洲上报 | `assets/grs_sdk_global_route_config_opensdkService.json` |
| `metrics5.data.hicloud.com` | **MEDIUM** | HMS Ads/OpenSDK HiAnalytics 上报 | `assets/grs_sdk_global_route_config_opensdkService.json` |
| `metrics5.dt.dbankcloud.ru` | **MEDIUM** | HMS Ads/OpenSDK HiAnalytics 俄罗斯上报 | `assets/grs_sdk_global_route_config_opensdkService.json` |

## 荣耀广告 (com.hihonor.adsdk)

| 主机名 | 置信度 | 依据 | 出处文件 |
|---|---|---|---|
| `ads-adnet-drcn.cloud.honor.com` | **HIGH** | 荣耀广告 SDK ads_base_url / env_prod / env_union (res/values/strings.xml) | `apk!resources.arsc`, `res/values/strings.xml` |
| `metrics-ad-drcn.dt.hihonorcloud.com` | **HIGH** | 荣耀广告 SDK ads_ha_url (广告埋点上报) | `apk!resources.arsc`, `res/values/strings.xml` |

## OPPO / 欢太 广告 (com.opos.mobad, com.heytap.msp.mobad)

| 主机名 | 置信度 | 依据 | 出处文件 |
|---|---|---|---|
| `uapi.ads.heytapmobi.com` | **HIGH** | OPPO 广告 union API: /union/ads/v2/show, /union/strategy/v3/select | `apk!classes.dex` |
| `ssp-adx.ads.heytapmobi.com` | **HIGH** | OPPO SSP/ADX 聚合: /v1/mixapi/ad_adapter/dsp/getAdInfo | `apk!classes.dex` |
| `adx.ads.heytapmobi.com` | **HIGH** | OPPO 广告频控: /show/frequency/req/check | `apk!classes.dex` |
| `stg-data.ads.heytapmobi.com` | **HIGH** | OPPO 广告策略/错误监控: /proxy/strategy/, /monitor/stat/err | `apk!classes.dex` |
| `adsfs.heytapimage.com` | **HIGH** | OPPO 广告素材/互动落地页 CDN | `apk!classes.dex` |
| `api-audit.heytapmobi.com` | **HIGH** | OPPO 广告内容审核上报 (source=advertise) | `apk!classes.dex` |
| `mdp-usertrace-cn.heytapmobi.com` | **MEDIUM** | OPPO MDP 用户行为追踪 (广告 SDK 依赖) | `apk!classes.dex` |

## 小米广告 MiMo / zeus (com.miui.zeus.mimo)

| 主机名 | 置信度 | 依据 | 出处文件 |
|---|---|---|---|
| `sdkconfig.ad.xiaomi.com` | **HIGH** | 小米广告 SDK 配置 (国内) | `apk!classes.dex` |
| `sdkconfig.ad.intl.xiaomi.com` | **HIGH** | 小米广告 SDK 配置 (国际) | `apk!classes.dex` |
| `sdkconfig.ad.india.xiaomi.com` | **HIGH** | 小米广告 SDK 配置 (印度) | `apk!classes.dex` |
| `sdkconfig.ad.rus.xiaomi.com` | **HIGH** | 小米广告 SDK 配置 (俄罗斯) | `apk!classes.dex` |
| `tracking.miui.com` | **HIGH** | MIUI 广告/推广追踪上报 | `apk!classes.dex` |
| `tracking.intl.miui.com` | **HIGH** | MIUI 广告追踪 (国际) | `apk!classes.dex` |
| `tracking.india.miui.com` | **HIGH** | MIUI 广告追踪 (印度) | `apk!classes.dex` |
| `tracking.rus.miui.com` | **HIGH** | MIUI 广告追踪 (俄罗斯) | `apk!classes.dex` |
| `s.mi1.cc` | **MEDIUM** | 小米/米1 广告追踪短域 | `apk!classes.dex` |

## 百度广告 (bdxadsdk.jar / com.baidu.mobads)

| 主机名 | 置信度 | 依据 | 出处文件 |
|---|---|---|---|
| `mobads.baidu.com` | **HIGH** | 百度 Mobads 广告主 API: /cpro/ui/mads.php, /ads/index.htm, /query_ad | `assets/bdxadsdk.jar!classes.dex` |
| `mobads-logs.baidu.com` | **HIGH** | 百度广告日志: /dz.zb, /brwhis.log | `assets/bdxadsdk.jar!classes.dex` |
| `aden.baidu.com` | **HIGH** | 百度广告 ADEN 接口 | `assets/bdxadsdk.jar!classes.dex` |
| `union.baidu.com` | **HIGH** | 百度联盟广告 | `apk!classes.dex`, `assets/bdxadsdk.jar!classes.dex` |
| `cpu.baidu.com` | **HIGH** | 百度 CPU 内容联盟广告 | `assets/bdxadsdk.jar!classes.dex` |
| `cpu-openapi.baidu.com` | **HIGH** | 百度 CPU 广告开放 API: /api/v2/data/ads, /aggregation/channels | `assets/bdxadsdk.jar!classes.dex` |
| `dxp.baidu.com` | **HIGH** | 百度 DXP 广告追踪: /autoTracker, /vizParser, /circleConfig | `assets/bdxadsdk.jar!classes.dex` |
| `hmma.baidu.com` | **HIGH** | 百度移动统计/广告归因 /app.gif | `assets/bdxadsdk.jar!classes.dex` |
| `bes-mtj.baidu.com` | **HIGH** | 百度移动统计 /auto.gif | `assets/bdxadsdk.jar!classes.dex` |
| `wn.pos.baidu.com` | **HIGH** | 百度联盟广告计费: /b.php, /en.php | `assets/bdxadsdk.jar!classes.dex` |
| `pos.baidu.com` | **HIGH** | 百度广告位统计 (应用自带 adBlockUrl 列表) | `apk!resources.arsc`, `res/values/arrays.xml` |
| `cpro.baidu.com` | **HIGH** | 百度联盟广告 (应用自带 adBlockUrl 列表) | `apk!resources.arsc`, `assets/bdxadsdk.jar!classes.dex`, `res/values/arrays.xml` |
| `cpro.baidustatic.com` | **HIGH** | 百度广告素材 (应用自带 adBlockUrl 列表) | `apk!classes.dex`, `apk!resources.arsc`, `assets/bdxadsdk.jar!classes.dex` (+1) |
| `cpro2.baidustatic.com` | **HIGH** | 百度广告素材 (应用自带 adBlockUrl 列表) | `apk!resources.arsc`, `res/values/arrays.xml` |
| `ubmcmm.baidustatic.com` | **HIGH** | 百度广告素材 (应用自带 adBlockUrl 列表) | `apk!resources.arsc`, `res/values/arrays.xml` |
| `cbjs.baidu.com` | **HIGH** | 百度广告点击脚本 (应用自带 adBlockUrl 列表) | `apk!resources.arsc`, `res/values/arrays.xml` |
| `nsclick.baidu.com` | **HIGH** | 百度广告反作弊点击 (应用自带 adBlockUrl 列表) | `apk!resources.arsc`, `res/values/arrays.xml` |
| `a.baidu.com` | **HIGH** | 百度广告域 (应用自带 adBlockUrl 列表) | `apk!resources.arsc`, `res/values/arrays.xml` |
| `c.baidu.com` | **HIGH** | 百度广告域 (应用自带 adBlockUrl 列表) | `apk!resources.arsc`, `res/values/arrays.xml` |
| `b.bdstatic.com` | **HIGH** | 百度广告静态资源 (bdxadsdk: /searchbox/androidvideo) | `assets/bdxadsdk.jar!classes.dex` |
| `pms-zeus-xcdn.bdstatic.com` | **HIGH** | 百度广告/搜索静态资源 CDN | `assets/bdxadsdk.jar!classes.dex` |
| `mobads-pre-config.bj.bcebos.com` | **HIGH** | 百度广告 SDK 预置素材/配置 CDN | `assets/bdxadsdk.jar!classes.dex` |
| `mobads-pre-config.cdn.bcebos.com` | **HIGH** | 百度广告 SDK 预置素材/配置 CDN | `assets/bdxadsdk.jar!classes.dex` |
| `render-server.cdn.bcebos.com` | **MEDIUM** | 百度广告 SDK 渲染素材 CDN | `assets/bdxadsdk.jar!classes.dex` |
| `browserkernel.baidu.com` | **MEDIUM** | 百度浏览器内核 (广告落地页内核) | `assets/bdxadsdk.jar!classes.dex` |
| `media-interact.baidu.com` | **MEDIUM** | 百度广告媒体互动 token 接口 | `assets/bdxadsdk.jar!classes.dex` |
| `haokan.baidu.com` | **MEDIUM** | 百度好看视频广告活动落地页 (SDK 硬编码) | `assets/bdxadsdk.jar!classes.dex` |

> 说明: `res/values/arrays.xml` 中存在名为 `adBlockUrl` 的字符串数组 (应用自带广告拦截名单), `mobads*` / `cpro*` / `pos.baidu.com` / `cbjs.baidu.com` / `nsclick.baidu.com` / `a.baidu.com` / `c.baidu.com` / `s.lianmeng.360.cn` / `union.sogou.com` 都直接来自该数组。

## YFAN AdX (com.yfanads)

| 主机名 | 置信度 | 依据 | 出处文件 |
|---|---|---|---|
| `api.yfanads.com` | **HIGH** | YFAN AdX 广告请求 API | `apk!classes.dex` |
| `adx-data.yfanads.com` | **HIGH** | YFAN AdX 广告数据 /v1/ad | `apk!classes.dex` |
| `log.yfanads.com` | **HIGH** | YFAN AdX 日志上报 /ads/v1/upload/log | `apk!classes.dex` |
| `tracker.yfanads.com` | **HIGH** | YFAN AdX 点击/曝光追踪 | `apk!classes.dex` |
| `reward.yfanads.cn` | **HIGH** | YFAN AdX 激励视频域名 | `apk!classes.dex` |
| `yumao.puata.info` | **MEDIUM** | YFAN SDK 反作弊/点击归因 (/anti_logs, /cc_info); 字符串与 yf_* 资源同区 | `apk!classes.dex` |

## AdGain / 广推科技 (com.adgain.sdk)

| 主机名 | 置信度 | 依据 | 出处文件 |
|---|---|---|---|
| `xadsmartget.adgain.cn` | **HIGH** | AdGain SDK 广告请求/智能投放接口 | `apk!classes.dex` |
| `cdn2.guangtuikeji.com` | **HIGH** | AdGain(广推科技) 广告 SDK 落地页 CDN /adx/sdk/app-info.html | `apk!classes.dex` |

## 友盟+ 统计/追踪 (com.umeng)

| 主机名 | 置信度 | 依据 | 出处文件 |
|---|---|---|---|
| `ulogs.umeng.com` | **HIGH** | 友盟统计日志上报 | `apk!classes.dex` |
| `ulogs.umengcloud.com` | **HIGH** | 友盟统计日志上报 (云) | `apk!classes.dex` |
| `preulogs.umeng.com` | **HIGH** | 友盟统计预发日志上报 | `apk!classes.dex` |
| `alogus.umeng.com` | **HIGH** | 友盟统计(海外/备用)上报 | `apk!classes.dex` |
| `alogsus.umeng.com` | **HIGH** | 友盟统计(海外/备用)上报 | `apk!classes.dex` |
| `plbslog.umeng.com` | **HIGH** | 友盟 LBS 日志上报 | `apk!classes.dex` |
| `pslog.umeng.com` | **HIGH** | 友盟性能监控日志上报 | `apk!classes.dex` |
| `ccs.umeng.com` | **HIGH** | 友盟 U-APM /ra /sa 上报 | `apk!classes.dex` |
| `sss.umeng.com` | **HIGH** | 友盟 /api/v2/al 上报 | `apk!classes.dex` |
| `aspect-upush.umeng.com` | **HIGH** | 友盟 U-Push 事件上报 /occa/v1/event/report | `apk!classes.dex` |
| `pre-aaid.umeng.com` | **HIGH** | 友盟 AAID 设备标识服务 | `apk!classes.dex` |
| `errlog.umeng.com` | **HIGH** | 友盟错误日志 | `apk!classes.dex` |
| `errlogos.umeng.com` | **HIGH** | 友盟错误日志(海外) | `apk!classes.dex` |
| `errnewlog.umeng.com` | **HIGH** | 友盟崩溃 SDK 日志采集 | `apk!classes.dex` |
| `errnewlogos.umeng.com` | **HIGH** | 友盟崩溃 SDK 日志采集(海外) | `apk!classes.dex` |
| `ai.login.umeng.com` | **MEDIUM** | 友盟 U-Verify 登录事件上报 (/api/umed/event) | `apk!classes.dex` |
| `oauth2.umeng.com` | **MEDIUM** | 友盟 U-Verify OAuth token | `apk!classes.dex` |
| `pre-ucc.umeng.com` | **MEDIUM** | 友盟 U-Verify 预发配置 | `apk!classes.dex` |
| `c.umsns.com` | **MEDIUM** | 友盟 U-Link 短链服务 /ulink/getRTC | `apk!classes.dex` |
| `pre-c.umsns.com` | **MEDIUM** | 友盟 U-Link 短链服务(预发) | `apk!classes.dex` |
| `log.umsns.com` | **MEDIUM** | 友盟 U-Link 短链日志 | `apk!classes.dex` |

## OTS / 线下推广 (com.acse.ottn) + 关联统计域

| 主机名 | 置信度 | 依据 | 出处文件 |
|---|---|---|---|
| `api-auth.zztfly.com` | **HIGH** | com.acse.ottn SDK 鉴权 API (与 com.acse.ottn 类名同处 classes.dex 0x106xxxx 区段) | `apk!classes.dex` |
| `cdn-api-auth.zztfly.com` | **HIGH** | acse.ottn SDK 鉴权 CDN | `apk!classes.dex` |
| `conf-auth.zztfly.com` | **HIGH** | acse.ottn SDK 配置鉴权 | `apk!classes.dex` |
| `log-auth.zztfly.com` | **HIGH** | acse.ottn SDK 日志鉴权 | `apk!classes.dex` |
| `cfgc.zztfly.com` | **HIGH** | acse.ottn SDK 配置下发 (含 api/push_ad_ecpm, api/clear_ad_time) | `apk!classes.dex` |
| `devc.zztfly.com` | **HIGH** | acse.ottn SDK 设备注册 | `apk!classes.dex` |
| `errc.zztfly.com` | **HIGH** | acse.ottn SDK 错误上报 | `apk!classes.dex` |
| `fdl.zztfly.com` | **HIGH** | acse.ottn SDK 文件/素材下载 | `apk!classes.dex` |
| `gd.zztfly.com` | **HIGH** | acse.ottn SDK 广告/应用分发接口 (api/an/gc/get_app_list) | `apk!classes.dex` |
| `tgc.zztfly.com` | **HIGH** | acse.ottn SDK 接口 | `apk!classes.dex` |
| `upc.zztfly.com` | **HIGH** | acse.ottn SDK 上传 | `apk!classes.dex` |
| `report.idazhe.net` | **MEDIUM** | com.acse.oozk 统计上报 /ss/ (与 com.acse.ottn 同一 SDK 家族) | `apk!classes.dex` |
| `a.idazhe.net` | **MEDIUM** | com.acse.oozk 统计 /ss/tongji_oozk_cn/ | `apk!classes.dex` |
| `t.idazhe.net` | **MEDIUM** | com.acse.oozk 统计 /ss/ | `apk!classes.dex` |
| `idazhe.net` | **MEDIUM** | OTS/线下推广 SDK 根域; 该 SDK 自带的协议页域名 | — |
| `www.idazhe.net` | **MEDIUM** | com.acse.oozk 自带协议页 /course/snt-*.html (非登录必需) | `apk!classes.dex` |
| `tongji.oozk.cn` | **MEDIUM** | com.acse.oozk 统计域名 | `apk!classes.dex` |

> 说明: `zztfly.com` 的 11 个子域与 `com.acse.ottn` 类名位于 `classes.dex` 的同一字符串区段 (0x1061F13–0x10E1660), 同区段还有 `api/push_ad_ecpm`、`api/clear_ad_time`、`api/limit_tips`、`api/an/gc/get_app_list` 等广告控制接口路径。`assets/packages/packages` 中 200+ 个第三方 App 包名 正是该 SDK 用于「已安装应用探测」的广告归因名单。

## 其它广告网络 (应用自带 adBlockUrl 列表 / 资源表)

| 主机名 | 置信度 | 依据 | 出处文件 |
|---|---|---|---|
| `s.lianmeng.360.cn` | **HIGH** | 360 联盟广告 (res/values/arrays.xml adBlockUrl) | `apk!resources.arsc`, `res/values/arrays.xml` |
| `union.sogou.com` | **HIGH** | 搜狗联盟广告 (res/values/arrays.xml adBlockUrl) | `apk!resources.arsc`, `res/values/arrays.xml` |
| `sogou.com` | **MEDIUM** | 搜狗联盟广告根域 (arrays.xml adBlockUrl, 仅封锁广告子链) | `apk!resources.arsc`, `res/values/arrays.xml` |
| `images.sohu.com` | **MEDIUM** | 搜狐广告 JS /cs/jsfile/js/c.js (arrays.xml adBlockUrl) | `apk!resources.arsc`, `res/values/arrays.xml` |

---

# ⚠️ 灰色地带 (未写入屏蔽表, 请自行判断)

| 域名 | 说明 |
|---|---|
| `utoken.umeng.com` | 友盟 U-Push token — 屏蔽会断友盟推送 |
| `ucc.umeng.com` | 友盟 U-Push 配置下发 — 屏蔽会断友盟推送 |
| `pre-msg.umengcloud.com` | 友盟 U-Push 预发长连接 — 屏蔽会断友盟推送 |
| `idazhe.net / www.idazhe.net` | 归属 com.acse.oozk (线下推广 SDK), 已按广告处理; 若你发现某个「一键登录」流程异常, 优先恢复这一条 |
| `cgicol.amap.com` | 高德定位数据采集 — 屏蔽可省流量, 但可能影响定位精度/合规初始化 |
| `adiu.amap.com` | 高德设备指纹 adius — 屏蔽可能影响定位初始化 |
| `mobile.umeng.com` | 友盟官网图片 — 无功能影响, 也非广告 |
| `www.umeng.com` | 友盟官网 (分享 SDK 文档链接) |
| `developer.umeng.com` | 友盟开发者文档链接 — 非流量 |
| `sogou.com` | 搜狗根域 (来自应用自带 adBlockUrl)。hosts 式屏蔽只影响根域本身; 若用通配符 `||sogou.com^` 请留意可能影响应用内网页跳转 |

---

# 📉 非广告的遥测/统计 (不影响广告, 也不影响功能)

这些是崩溃上报、性能统计、埋点, **不是广告**。屏蔽它们只会少发遥测数据, 不会减少广告, 但也基本不会破坏功能。是否屏蔽由你决定, 因此**没有**放进屏蔽表。

| 域名 | 所属 | 说明 |
|---|---|---|
| `bugly.qq.com` | 腾讯 Bugly | 崩溃上报 `libBugly.so` / `libcrashsdk.so` |
| `android.bugly.qq.com` | 腾讯 Bugly | 崩溃上报备用 |
| `astat.bugly.qcloud.com` | 腾讯 Bugly | 统计上报 |
| `astat.bugly.cros.wr.pvp.net` | 腾讯 Bugly | 统计上报 |
| `h.trace.qq.com` | 腾讯灯塔 | 埋点上报 |
| `cgicol.amap.com` | 高德 | 定位数据采集 |
| `adiu.amap.com` | 高德 | 设备指纹 adius |
| `datapoint.gxapp.iydsj.com` | 应用自身 | **业务埋点, 不要屏蔽** |

---

# 🧱 局限性与未解项

1. **应用自身 API 基址不可见**。主 `classes.dex` 被网易易盾加固 (方法体被抽空/加密), 应用自己的业务域名可能由代码拼接或存于加密数据段, 静态分析无法恢复。因此屏蔽策略必须**以 `*.iydsj.com` 整域白名单兜底**。

2. **京东广告 SDK (`com.jd.ad.sdk`) 域名未恢复**。AndroidManifest 里确实注册了 `com.jd.ad.sdk.bl.adinteraction.deeplink.JADWebViewActivity`、`JADMultiProvider` 等组件, 但 `classes.dex` 明文字符串中 **不含任何 `jd.com` / `jad.jd.com` 主机名** (0 命中)。推测其域名在加密数据段或被混淆。**未凭猜测写入屏蔽表**。

3. **华为广告 (PPS) 域名不完整**。只恢复出 GRS 配置里的 HiAnalytics 上报域 (`metrics*.dbankcloud.cn` / `metrics*.hicloud.com`)。`hwpps://` scheme 是本地方案, 非 DNS。华为广告主 API (`*.hispace.dbankcloud.*`) 未在明文中出现。

4. **快手磁力广告的部分域名仅在 `assets/ksad_idc.json` 中**, 该文件是 SDK 自带的 IDC 配置, 可信度极高; 但 SDK 运行时还可能下发新的备用域。

5. **广告 SDK 的 native 库基本无用**(`libyf_ads.so`、`libnative-tianmu-lib.so`、`libsecsdk_tianmu.so`、`libads-ac.so`、`libPglbizssdk_ml.so` 等字符串被加密), 本报告的域名主要来自 dex 字符串、jar 内 dex 与 SDK 配置文件。

6. **只做 DNS 屏蔽 ≠ 完全去广告**。开屏/信息流广告的**请求**会被拦掉, 但部分 SDK 有本地缓存素材, 首屏可能仍显示上一次缓存的广告; 另外若 SDK 走 HTTPDNS 或 IP 直连, hosts 类屏蔽可能失效 —— 这也是为什么文末给出了通配符规则。

7. **未做动态验证**。本报告全部为静态分析结论, 没有真机抓包验证。建议先只屏蔽 「HIGH 置信度」的中段几个 SDK (穿山甲 / 优量汇 / 快手 / OPPO / 荣耀 / 百度) 观察 1–2 天。

8. **已剔除 DEX 长度字节假阳性**。DEX 字符串以 ULEB128 长度前缀开头, 当长度值恰好落在 `0x30`–`0x39`(`'0'`–`'9'`) 等可打印区间时, 会被误读成主机名首字符。本报告已逐条做字节级复核, 剔除了 2 个假主机 (`3pgdt.ugdtimg.com` → 实为 `pgdt.ugdtimg.com`, `8qzs.gdtimg.com` → 实为 `qzs.gdtimg.com`), 并确认 `log-api.pangolin-sdk-toutiao.com`、`api-access.pangolin-sdk-toutiao1.com` 等的首字符不是长度字节。复核脚本: `02_inspect/33_artifact_sweep.py`、`34_final_check.py`、`35_precise.py`。

9. **有 8 个广告域名挂在「共享父域」下**, 而父域本身在 DO-NOT-BLOCK 里 (`sdkconfig.ad.*.xiaomi.com` ⊂ `xiaomi.com`; `metrics*-drcn.dt.dbankcloud.cn` ⊂ `dbankcloud.cn`; `metrics*.data.hicloud.com` ⊂ `hicloud.com`; `metrics5.dt.dbankcloud.ru` ⊂ `dbankcloud.ru`)。**hosts 文件式屏蔽只会拦这些精确子域, 不影响小米推送 / 华为推送**; 但如果你的工具用 `*.xiaomi.com` 之类的通配白名单, 白名单会覆盖黑名单 —— 这种情况下请把上面 8 条从黑名单里删掉 (它们只是广告埋点, 拦截与否对去广告影响很小)。

---

# 🛠 非 root 手机的落地方式

| 方式 | 说明 |
|---|---|
| **AdGuard for Android** (推荐) | 支持通配符与 DNS 过滤, 无需 root; 用「DNS 过滤」+ 自定义规则, 把文末通配符块粘进去 |
| **私人 DNS (DoT/DoH)** | 系统设置 → 网络 → 私人 DNS, 填 NextDNS / AdGuard DNS 的地址; 在服务商后台导入本表 |
| **NextDNS** | 网页后台 → Denylist, 直接粘贴 `03_ad_domains_blocklist.txt` 中的主机名 (去掉 `0.0.0.0`) |
| **Pi-hole / AdGuard Home (家里路由)** | 只对同一 WiFi 生效; 把本表加入 `/etc/hosts` 或自定义过滤规则 |
| **VPN 类 (Blokada / RethinkDNS)** | 导入 hosts 格式列表, 本机生效 |

**分阶段建议**

1. 先应用「穿山甲 / 优量汇 / 快手 / 百度」四组 —— 这四家是国内开屏+信息流的主力, 通常已能去掉 80% 广告。
2. 观察登录、跑步、地图、上传均正常后, 再加「OPPO / 荣耀 / 小米 / 华为 / 天目 / YFAN / AdGain」。
3. 最后才考虑友盟统计与 OTS 线下推广组 (这两组同时承担部分统计职责)。
