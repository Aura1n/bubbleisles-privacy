# 泡泡屿 · 法律与合规信息站

《泡泡屿》（Android 包名 `com.doyar.bubbleisles`）上架 TapTap 所需的静态说明站，纯 HTML + CSS，无脚本、无外部依赖、无 CDN，境内直连可访问。

## 页面

| 文件 | 用途 | TapTap 后台对应栏 |
| --- | --- | --- |
| `index.html` | 站点首页：三页入口 + 应用信息 | — |
| `privacy.html` | 隐私政策 | 「隐私政策链接」 |
| `terms.html` | 用户协议 | 「用户协议」/应用介绍区 |
| `permissions.html` | 权限与第三方 SDK 说明 | 「权限申请说明」「SDK 目录」 |

## 本地预览

```sh
python -m http.server 8000
# 打开 http://127.0.0.1:8000/
```

## 部署

仓库 Settings → Pages → Source 选 `Deploy from a branch`，分支 `main`、目录 `/ (root)`。发布后链接形如：

```
https://aura1n.github.io/bubbleisles-privacy/privacy.html
```

后台填具体页面 URL（`privacy.html`），不要只填站点根地址，避免审核人员还要二次点击。

## 改文案时要核对的事实

页面里的每一条技术事实都应来自当前发布包，不要凭印象写。核对命令（在 paopao 项目根目录）：

```sh
# 权限清单、包名、版本、ABI
"$ANDROID_SDK/build-tools/36.0.0/aapt2.exe" dump permissions builds/android/*release*.apk
"$ANDROID_SDK/build-tools/36.0.0/aapt2.exe" dump badging builds/android/*release*.apk

# 导出配置里的权限开关与包名（注意该文件含 keystore 口令，不要公开）
grep -E "package/unique_name|permissions/|version/name|architectures" export_presets.cfg

# 存档写了哪些本地数据
grep -n "set_value" autoload/ProgressStore.gd
```

当前事实（1.0.0 / versionCode 1）：只申请 `VIBRATE`；`permissions/internet=false`，脚本内无 HTTP/WebSocket 调用；APK 内只有 TapTap SDK 的 GDScript 包装，无 aar/so，`Engine.has_singleton("GodotTapTapSDK")` 返回 false；存档为应用私有目录下 `progress.cfg`（含 `.bak`、`.tmp`）；设置项为背景音乐、游戏音效、轻触振动（手机）、瞄准辅助线。

新增权限或接入任何第三方 SDK 时，必须同步更新 `privacy.html` 与 `permissions.html`，并修改页面顶部的生效日期。

## 联系信息

对外公示的联系渠道目前是电话 15531279585（三个页面底部）。若换成邮箱，直接全局替换该段落即可。
