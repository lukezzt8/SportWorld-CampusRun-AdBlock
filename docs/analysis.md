# APK 初步分析报告

## 1. 样本信息

| 项目 | 值 |
|---|---|
| 原始路径 | `D:\base.apk.1.apk.1_tmp.apk.1_tmp` |
| 工作副本 | `00_original\base.apk` |
| 大小 | 317.4 MB (332,825,000 字节) |
| SHA256 | `5D4EA7152C77BF1C088C719FD94736C8096BAF1C0985489BFAD24140D46DB365` |
| 文件类型 | 有效 ZIP / Android APK(v1 签名 `META-INF/SPORTSWO.RSAPK`) |
| 包名 | `com.zjwh.android_wh_physicalfitness` |
| 应用名 | **运动世界校园** |
| 版本 | **7.3.70** |
| 磁盘时间 | 2026-10-15 01:54 |

条目总数 6405。注意:原文件扩展名是 `.apk.1_tmp.apk.1_tmp`,是下载中断残留的命名,
但 ZIP 尾部 EOCD 完整、`MANIFEST.MF` 齐全,**文件本身是完整的**,可以直接分析。

## 2. 应用构成(体积)

| 分区 | 文件数 | 大小 |
|---|---|---|
| lib/arm64-v8a | 59 | 106.83 MB |
| lib/armeabi-v7a | 54 | 78.22 MB |
| classes.dex | 1 | **73.81 MB** |
| assets/flutter_assets | 126 | 15.20 MB |
| resources.arsc | 1 | 4.74 MB |
| AndroidManifest.xml | 1 | 0.17 MB |

技术栈:Flutter(主 UI)+ 原生 Android(广告/地图/推送)+ 高德地图 SDK
(`build.properties` iid=6363 version=12.10.0.10010731)+ H5 Hybrid 容器。

## 3. 广告 SDK 清单(证据充分)

来自 AndroidManifest 组件声明 + 原生库 + assets,可确认存在 **11 个广告变现 SDK**:

| SDK | 证据 |
|---|---|
| 腾讯优量汇 GDT | `assets/gdt_plugin/gdtadv2.jar`、`*.gdt.fileprovider`、`libyf_ads.so` |
| 快手磁力引擎 KSAd | `com.kwad.sdk.api.proxy.app.*`(70+ 个 Activity)、`assets/ksad_*`、`libsgcore.so` |
| 阿里天目 Tianmu | `com.tianmu.biz.activity.*`、`libsecsdk_tianmu.so`、`libnative-tianmu-lib.so` |
| 京东 JAD | `com.jd.ad.sdk.*` |
| 华为广告 PPS | `com.huawei.openalliance.ad.*` |
| 荣耀广告 | `honor_ad_sdk_version = 1.0.24.300` |
| OPPO mobad | `com.opos.mobad.*`、`com.heytap.msp.mobad.*` |
| 小米 mimo/zeus | `com.miui.zeus.mimo.sdk.ad.*` |
| 百度 bdxadsdk | `assets/bdxadsdk.jar` |
| YFAN AdX | `com.yfanads.android.adx.*` |
| AdGain SDK | `com.adgain.sdk.base.activity.AdActivity` |

另外还有 **`com.acse.ottn.*`(OTS/线下推广 SDK)** —— 这是国内有名的激进推广/拉活库,
带 `AccessibiltyTsnPermissionGreenActivity`、`MyDeviceAdminReceiver`、`StartProcreeService` 等,
属于比普通广告更难受的部分(弹窗、诱导、进程保活)。

## 3.5 加固结构:dex 被完整加密(已用 dex 头字段证实)

解析 `classes.dex` 头部(112 字节):

| 字段 | 值 | 含义 |
|---|---|---|
| `file_size` | 77,391,616 | 文件 73.8 MB |
| `class_defs_size` | **30** | 里面只有 30 个类 |
| `method_ids_size` | 660 | 660 个方法 |
| `data_size` | **50,336** | 真实数据只有 50 KB |

apktool 解包后只产出 **30 个 smali 文件 / 0.34 MB**,全部位于 `com/netease/nis/wrapper/`:

```
com/netease/nis/wrapper/MyApplication.smali             51.3 KB  ← 应用入口被替换
com/netease/nis/wrapper/MyJni.smali                     32.4 KB  ← 调用 libnesec.so
com/netease/nis/wrapper/NEDialog.smali                  17.2 KB  ← 加固检测失败弹窗
com/netease/nis/wrapper/plugin/InstrumentationProxy.smali 3.8 KB ← Hook Activity 生命周期
com/netease/nis/wrapper/plugin/{a..g}.smali                    ← 动态加载真 dex
```

**结论:这个 dex 是一个 73.7 MB 的"壳",里面只有 50 KB 引导代码,广告代码和业务代码全部
加密在同一文件里,由易盾壳在运行时解密后动态加载。** 无 Root 无法脱壳,
`InstrumentationProxy` 会拦截生命周期、`NEDialog` 负责在检测到篡改时弹窗。

## 3.6 ★ 突破口:官方自己给广告域名开了"证书豁免"

`res/xml/network_security_config.xml` 原文(含开发者自己的中文注释):

```xml
<base-config cleartextTrafficPermitted="true">
    <trust-anchors><certificates src="system" /></trust-anchors>   ← 只信系统证书
</base-config>
<domain-config cleartextTrafficPermitted="true">
    <domain includeSubdomains="true">i.snssdk.com</domain>
    <domain includeSubdomains="true">is.snssdk.com</domain>
    <domain includeSubdomains="true">pangolin.snssdk.com</domain>
    <domain includeSubdomains="true">extlog.snssdk.com</domain>
    <domain includeSubdomains="true">sf3-ttcdn-tos.pstatp.com</domain>
    <domain includeSubdomains="true">bds.snssdk.com</domain>
    <domain includeSubdomains="true">dig.bdurl.net</domain>
    <domain includeSubdomains="true">mi.gdt.qq.com</domain>
    <domain includeSubdomains="true">v2mi.gdt.qq.com</domain>
    <domain includeSubdomains="true">v3mi.gdt.qq.com</domain>
    <trust-anchors>
        <certificates src="user" /> //信任用户自己安装的证书
        <certificates src="system" />
    </trust-anchors>
</domain-config>
```

**这 10 个域名属于穿山甲(6 个)、腾讯优量汇(3 个)、百度(1 个)三家广告平盟,而 App 主业务
域名走 `base-config` 只信系统证书。** 后果:
- 在**无 Root** 手机上装一张用户 CA 证书,即可合法解密这 10 个域名的 HTTPS 流量;
- 主业务流量由于只信系统证书,**在技术上无法被这张 CA 解密**,因此不存在误伤风险。

这等于开发者亲手在加固堡垒上留了一个只能进广告、进不了业务的观察窗。

## 4. 加固与风控(最大障碍)

| 加固/风控 | 证据 |
|---|---|
| **网易易盾** | `com.netease.nis.wrapper.MyApplication`(应用入口被替换)、`libnesec.so`、`libnetseckit-4.2.4.so` |
| **爱加密** | `libijiami_1011.so`(文件名 `ijiami_1011` = 爱加密特征) |
| 腾讯天御 | `libsecsdk_tianmu.so` |
| 其他 | `libzxprotect.so`(libapp.so 是 Flutter AOT 产物,非加固) |

**这意味着:**
1. 静态改 smali 后重新打包必须重新签名,而 APK 签名一旦变化,易盾的校验会触发 ——
   结果是闪退或拒绝启动,而不是"广告没了"。且真代码根本不在 dex 里,无 Root 脱不了壳。
2. 用户条件是"**无 Root 手机**",因此任何需要 Root 的方案(改 hosts、LSPosed/Frida、装重签名包)
   在前提上就不成立。
3. 该应用是校园强制跑步打卡类,风控严格,网易易盾带反调试、反注入、完整性校验。

## 5. 路线评估(已按"无 Root + 功能必须正常"收敛)

| 路线 | 可行性 | 说明 |
|---|---|---|
| A. 动态 Hook(LSPosed/Frida) | ❌ 排除 | 硬性要求 Root |
| B. 静态脱壳改包重签名 | ❌ 排除 | 无 Root 无法脱壳;且重签名必触发易盾校验 |
| C. 改 hosts | ❌ 排除 | 改 hosts 需 Root |
| **D. 本地 VPN 应用做 DNS 屏蔽** | ✅ **可行** | Android 免 Root 官方机制(VpnService),不改 APK、不碰签名、易盾察觉不到 |
| **E. 本地 VPN + 定向 HTTPS 解密改写** | ✅ **可行(进阶)** | 利用 3.6 节的 10 个域名证书豁免,只解密广告流量改写响应 |

**D 和 E 是这个约束组合下仅有的两条路。** 两者都基于 Android 的 `VpnService` ——
用户级 App 即可创建本地 VPN,无需 Root、无需解锁、不修改目标 APK。

## 6. 建议执行顺序

1. **先做 D(零风险)**:装一个支持 DNS 拦截的本地 VPN App(如 RethinkDNS / NetGuard / AdGuard),
   导入广告域名黑名单。特点是立即见效、绝不误伤业务、易盾完全无法察觉(它连 APK 都没被改)。
   代价:广告位可能显示空白,少数 SDK 在拿不到广告时会重试。
2. **若 D 不够(广告仍出现),再做 E**:利用官方放行的 10 个广告域名安装用户 CA,
   在本地 VPN 里对这 10 个域名做中间人解密,直接把广告响应改写成"无广告"的空结果。
   这一步在技术上等价于"在传输层把广告抹掉",业务流量完全不受影响,因为
   `base-config` 只信系统证书,主业务流量在密码学上无法被这张 CA 解密。

## 7. 已确立的事实清单(供下一步直接使用)

- 免 Root 可用性:**成立**。`VpnService` 是 Android 官方免 Root 机制。
- 广告域名白名单(可直接作为第一批屏蔽目标,置信度最高):
  `i.snssdk.com`、`is.snssdk.com`、`pangolin.snssdk.com`、`extlog.snssdk.com`、
  `sf3-ttcdn-tos.pstatp.com`、`bds.snssdk.com`(穿山甲);
  `mi.gdt.qq.com`、`v2mi.gdt.qq.com`、`v3mi.gdt.qq.com`(腾讯优量汇);
  `dig.bdurl.net`(百度)。
- 业务域名:`physicalfitness.net`、`physicalfitness.com`、`physicalfitness.mvi.me`、
  `physicalfitness.utils.net`、`physicalfitness.httpmanager.net` —— **绝对不能屏蔽**。
- 注意:上述域名只覆盖了 3 家广告平台的"配置/监控"接口,完整的广告物料域名清单
  需要从 `assets/`(已解密)与原生库中进一步提取,或通过手机抓包实测补齐。
