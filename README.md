# 泡泡屿 · 隐私政策

《泡泡屿》（Android 包名 `com.doyar.bubbleisles`）上架 TapTap 所需的隐私政策页。单页纯静态 HTML + CSS，无脚本、无外部依赖、无 CDN，境内直连可访问。

章节骨架参照用户提供的《精品离线游戏合集》隐私协议，并结合参考稿压缩为九节：1 收集的信息、2 使用目的、3 系统权限、4 共享与披露、5 存储与保护、6 您的权利、7 未成年人保护、8 协议变更、9 联系我们。内容只写本应用实际存在的东西：游戏在 TapTap 平台内启动，登录、实名认证与防沉迷由平台完成、游戏不接触实名信息；游戏本体只在本机保存进度、设置与登录状态标识；无尽海域排行榜通过 TapTap 提交得分与坚持时间，并展示玩家的昵称与头像。

正文面向玩家，不出现包名、权限英文名、存档文件名、引擎名等技术标识；设备信息以泛称披露（设备型号、系统版本、网络类型、IP 地址、设备标识）。

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

当前事实（1.1.0 / versionCode 2，Gradle 构建开启后）：APK 申请 `ACCESS_NETWORK_STATE`、`INTERNET`、`VIBRATE`、`WRITE_EXTERNAL_STORAGE`（maxSdk 29）、`READ_MEDIA_IMAGES`、`READ_MEDIA_VIDEO`、`REQUEST_INSTALL_PACKAGES`、`REORDER_TASKS`、`READ_EXTERNAL_STORAGE`（maxSdk 32）、`CAMERA`、自定义 `DYNAMIC_RECEIVER_NOT_EXPORTED_PERMISSION`。网络、网络状态由 TapSDK 在 TapTap 平台内使用；相机、相册、存储、安装未知应用等由 TapSDK 的分享/更新组件声明，游戏未调用，运行时不会主动申请。

APK 内含 `GodotTapTapSDK` 的 aar 与 `com.taptap.sdk` 类（4456 个），以及 `libtapsdkcore.so`、`libthemis.so`、`libbifrost_sdk.so`、`libcloudsave_sdk.so`；`Engine.has_singleton("GodotTapTapSDK")` 在 Android 包内为 true。存档为应用私有目录下 `progress.cfg`（含 `.bak`、`.tmp`），新增 `privacy_agreed` 与 `tap_open_id`；设置项为背景音乐、游戏音效、轻触振动（手机）、瞄准辅助线。

TapSDK 官方披露的处理范围为：系统版本、设备型号、设备 CPU 信息、网络类型、Android ID、设备内存信息；提供方为易玩（上海）网络科技有限公司，隐私政策 `https://developer.taptap.cn/docs/v3/sdk/start/agreement/`。游戏在 SDK 初始化完成后自动调用登录接口并调用排行榜接口，不调用实名认证或防沉迷接口；从 TapTap 客户端（TapPlay）启动时由平台侧处理。排行榜提交 `com.taptap.sdk` 数值型榜单的整数分数（游戏内按「得分 × 1,000,000 + 坚持秒数」打包），读取的回包包含昵称、头像、得分与提交时间；昵称头像只在界面内存中显示，不写入 `progress.cfg`。

新增权限、新增联网用途或接入任何第三方 SDK 时，必须同步更新正文第 1、3、4、5、8 各节，并修改顶部的更新日期与生效日期。

## 联系方式

对外公示渠道写在正文第 9 节：邮箱 `2980275041@qq.com`、电话 `15531279585`，承诺 15 天内回复。
