# 小鲸学习 · Android App

这是一个 Capacitor 原生壳，启动后加载线上小鲸学习：

```text
https://increased-whitefish-d5bib9cd3d51-1519026807.tcloudbaseapp.com/?app=1
```

因此：

- 不是浏览器地址栏，打开就是独立 App；
- 你以后更新网页，App 内立即生效，不用重发 APK；
- 只有改 App 图标、包名、原生权限等壳层内容时，才需要重新构建 APK；
- 第一次打开需要联网。

## 一、上传到 GitHub

把本目录（`mobile/`）里的**内容**作为一个新 GitHub 仓库的根目录，不要把 `mobile` 文件夹本身再套一层。

不要上传：

- `node_modules/`
- `signing/`
- `android/.gradle/`、`android/app/build/`

 `.gitignore` 已经排除了这些；用 GitHub Desktop 或命令行上传最稳。

## 二、添加签名 Secrets

在 GitHub 仓库：

```text
Settings → Secrets and variables → Actions → New repository secret
```

添加：

| Secret | 值 |
| --- | --- |
| `ANDROID_KEYSTORE_BASE64` | `signing/keystore.base64.txt` 的完整内容 |
| `ANDROID_KEYSTORE_PASSWORD` | `signing/SECRETS.txt` 里的密码 |
| `ANDROID_KEY_PASSWORD` | 同上 |

签名密钥必须长期保存。以后更新 APK 必须用同一个密钥，否则手机不能覆盖安装。

## 三、构建

进入仓库的 **Actions → Build Android APK → Run workflow**。

完成后在该次构建页面底部下载：

```text
xiaojing-study-apk
```

解压后得到 `xiaojing-study.apk`，发到 Android 手机/平板安装；首次需要允许“安装未知来源应用”。

## 四、更新 App

### 只改网页内容

直接部署新的完整 PWA 即可，App 打开时会加载最新网页，无需重新构建 APK。

### 改 App 壳层

修改 `android/` 里的图标、包名、权限等后，重新跑 Actions。

同时把 `android/app/build.gradle` 的：

```gradle
versionCode 1
versionName "1.0"
```

递增，例如：

```gradle
versionCode 2
versionName "1.1"
```

再构建新的 APK。用户安装新 APK 会覆盖旧版，本地数据保留。

## 注意

- 这是 Android 安装包；iPhone/iPad 需要 Mac + Xcode，Windows 无法生成。
- App 内数据仍保存在本机 WebView；当前没有跨设备云同步。
- WebView 里的通知/文件下载能力受 Android 原生权限限制，后续可按需补 Capacitor 插件。
