# 泡泡屿 · 隐私政策

《泡泡屿》（Android 包名 `com.doyar.bubbleisles`）上架 TapTap 所需的隐私政策页。单页纯静态 HTML + CSS，无脚本、无外部依赖、无 CDN，境内直连可访问。

章节骨架参照 TapTap 官方《TapSDK 隐私政策》的一至八结构（收集与使用 / 存储 / 共享转让披露 / 玩家管理 / 儿童信息 / 安全保护 / 更新 / 联系我们），内容只写本应用实际存在的东西：不收集任何个人信息、只有振动一项权限、无第三方服务组件、数据只在设备本地。

正文面向玩家，不出现包名、权限英文名、存档文件名、引擎名等技术标识；这些事实只记录在下面的「改文案时要核对的事实」一节，更新时按它核对，不要写回正文。

## 文件

| 文件 | 说明 |
| --- | --- |
| `index.html` | 隐私政策正文，Pages 根路径即本页 |
| `styles.css` | 版式，含深浅色两套主题、打印隐藏页脚 |

TapTap 开发者后台的「隐私政策链接」填：

```
https://aura1n.github.io/bubbleisles-privacy/
```

## 本地预览

```sh
py -3 -m http.server 8014 --directory .
# 打开 http://127.0.0.1:8014/
```

## 部署

仓库 Settings → Pages → Source 选 `Deploy from a branch`，分支 `main`、目录 `/ (root)`。

## 改文案时要核对的事实

页面里每条技术事实都应来自当前发布包，不要凭印象写。核对命令（在 paopao 项目根目录）：

```sh
# 权限清单、包名、版本、ABI
"$ANDROID_SDK/build-tools/36.0.0/aapt2.exe" dump permissions builds/android/*release*.apk
"$ANDROID_SDK/build-tools/36.0.0/aapt2.exe" dump badging builds/android/*release*.apk

# 导出配置里的权限开关与包名（该文件含 keystore 口令，不要公开）
grep -E "package/unique_name|permissions/|version/name|architectures" export_presets.cfg

# 存档写了哪些本地数据
grep -n "set_value" autoload/ProgressStore.gd
```

当前事实（1.0.0 / versionCode 1）：只申请 `VIBRATE`；`permissions/internet=false`，脚本内无 HTTP/WebSocket 调用；APK 内只有 TapTap SDK 的 GDScript 包装，无 aar/so，`Engine.has_singleton("GodotTapTapSDK")` 返回 false；存档为应用私有目录下 `progress.cfg`（含 `.bak`、`.tmp`）；设置项为背景音乐、游戏音效、轻触振动（手机）、瞄准辅助线。

新增权限、开始联网或接入任何第三方 SDK 时，必须同步更新正文第 1.2、1.4、二、三、六各节，并修改顶部的更新日期与生效日期。

## 联系方式

对外公示渠道写在第八条：邮箱 `2980275041@qq.com`、电话 `15531279585`，承诺 15 天内回复。
