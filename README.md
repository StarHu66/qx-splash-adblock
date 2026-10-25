# SplashAdBlock for Quantumult X

自用开屏广告 SDK 拦截订阅。规则托管在 GitHub，QX 远程引用，手机上一键更新。

## 文件说明

| 文件 | 内容 | 是否必选 |
|---|---|---|
| `SplashAdBlock.list` | 主规则：穿山甲 + 广点通 + 快手 + 百度联盟（13 条） | 推荐 |
| `SplashAdBlock-Aggregator.list` | 聚合平台扩展：Mintegral / SigMob / TopOn / AdMob（默认全注释） | 可选 |
| `SplashAdBlock-Allow.list` | 误杀放行白名单 | 强烈建议 |

## 订阅链接

订阅链接（仓库用户名 `StarHu66`，分支 `main`）：

```
https://raw.githubusercontent.com/StarHu66/qx-splash-adblock/main/SplashAdBlock.list
https://raw.githubusercontent.com/StarHu66/qx-splash-adblock/main/SplashAdBlock-Aggregator.list
https://raw.githubusercontent.com/StarHu66/qx-splash-adblock/main/SplashAdBlock-Allow.list
```

国内访问 raw 不稳时的备用镜像（jsDelivr，有约数小时缓存）：

```
https://cdn.jsdelivr.net/gh/StarHu66/qx-splash-adblock@main/SplashAdBlock.list
```

## 在 QX 配置里引用

粘到配置文件的 `[filter_remote]` 段落，**顺序必须是白名单在最前**：

```
https://raw.githubusercontent.com/StarHu66/qx-splash-adblock/main/SplashAdBlock-Allow.list, tag=开屏误杀放行, force-policy=direct, update-interval=86400, opt-parser=false, inserted-resource=true, enabled=true
https://raw.githubusercontent.com/StarHu66/qx-splash-adblock/main/SplashAdBlock.list, tag=开屏广告拦截, force-policy=reject, update-interval=86400, opt-parser=false, inserted-resource=true, enabled=true
```

聚合扩展包按需添加（同上，务必带 `inserted-resource=true`）：

```
https://raw.githubusercontent.com/StarHu66/qx-splash-adblock/main/SplashAdBlock-Aggregator.list, tag=开屏广告拦截-聚合, force-policy=reject, update-interval=86400, opt-parser=false, inserted-resource=true, enabled=true
```

## 三个关键点

1. **`inserted-resource=true` 不能省。** QX 规则匹配顺序是「先本地后远程」，`filter_local` 里的 `geoip, cn, direct` 会把解析到国内 IP 的域名先判为直连。这些广告域名绝大多数解析到国内 IP，不加这个参数规则等于白写。
2. **`force-policy` 建议带上。** 这样以后在文件里只写域名、不写策略也能生效，改起来更省事。
3. **`opt-parser=false`**。文件本身就是 QX 原生格式，不需要走资源解析器，解析更快也更不容易出错。

## 更新规则

1. 在 GitHub 上改 `*.list` 文件（网页直接编辑就行，改完 Commit changes）
2. 手机 QX：右下角小风车 → 分流 → 规则资源 → 长按这条订阅 → 更新
3. `update-interval=86400` 表示最多缓存 24 小时，手动更新可立即生效

## 排查误杀

某网站打不开时：QX「网络活动」 → 找到该站点 → 看被标记 REJECT 的域名 → 加进 `SplashAdBlock-Allow.list` → 推送 → 手机更新订阅。
