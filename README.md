# 密码管理器离线归档

> 个人用归档仓库。目的只有一个：**万一上游项目哪天消失了，我手上还有能跑的程序和完整源码。**

这个仓库是 [keepassxreboot/keepassxc](https://github.com/keepassxreboot/keepassxc) 的 fork，除此之外还归档了安卓端客户端 [Kunzisoft/KeePassDX](https://github.com/Kunzisoft/KeePassDX)。

所有二进制都是 **2026-08-25 从各自官方渠道原样下载，未做任何修改**，全部附了 SHA256 和验证记录。

---

## 📖 使用教程

不知道怎么用？看 **[使用教程.md](使用教程.md)** —— 从建库、日常存取、同站多账号、备份，到浏览器插件、桌面软件自动输入、手机端同步，外加快捷键速查和常见坑。

## 我要用密码库，从哪下东西

### Windows 桌面端 → [Release `2.7.12`](../../releases/tag/2.7.12)

| 文件 | 用途 |
|---|---|
| `KeePassXC-2.7.12-Win64.msi` | 装到电脑上。想指定目录：`winget install KeePassXCTeam.KeePassXC --location "D:\Program Files\KeePassXC"` |
| `KeePassXC-2.7.12-Win64.zip` | 便携版，解压即用，不写注册表。**放 U 盘里，插到任何电脑上双击就能开库** |
| `*.DIGEST` | 官方哈希文件，可直接比对 |

### 安卓端 → [Release `keepassdx-4.4.5`](../../releases/tag/keepassdx-4.4.5)

上游用**同一份代码**编译出两个版本（Gradle product flavor），对应两个不同的分发渠道。**装哪个要在安装前决定**：

| 文件 | 对应官方渠道 | 包名 (applicationId) | 建议 |
|---|---|---|---|
| `KeePassDX-4.4.5-libre.apk` | **F-Droid** | `com.kunzisoft.keepass.libre` | ✅ **用这个** |
| `KeePassDX-4.4.5-free.apk` | **Google Play** | `com.kunzisoft.keepass.free` | 想和 Play 商店保持一致时用 |

#### 两者到底差在哪

以下差异全部核实自 4.4.5 源码的 `app/build.gradle`（源码就在本 release 的 bundle / tarball 里，可自行复核）：

| | `libre` — F-Droid | `free` — Google Play |
|---|---|---|
| 构建标记 `CLOSED_STORE` | `false` | `true` |
| Google 云备份 API key | 无 | **有**，`googleAndroidBackupAPIKey` 写进 manifest |
| Google passkey 特权应用清单 | 无 | 有，`passkeys_privileged_apps_google.json` |
| 内购 / 捐赠提示逻辑 | 走开放渠道 | 走应用商店 |
| 可选主题 | **多一个 Blue 主题** | 无 Blue 主题 |
| 应用图标 | 各自一套独立图标 | 各自一套独立图标 |
| flavor 专属第三方依赖 | 无 | 无 |

**核心功能两者完全一致，没有任何阉割**：打开 kdbx、KDBX4 + Argon2、系统级自动填充、指纹/面容解锁、passkey，都一样。

#### 为什么推荐 libre

它不含任何 Google 服务对接点——没有 Google 云备份密钥，没有 Google 的特权应用清单。密码管理器少一个对外接口就少一份风险面。功能上你不会损失任何东西，反而多一个主题。

#### ⚠️ 两个包名不同，Android 视作两个独立应用

- **可以同时安装**，互不干扰
- 但**设置和「最近打开的库」列表不共享**
- **不能从一个「升级」成另一个**，要换必须先卸载再重装
- 你的 `.kdbx` 文件是独立的普通文件，换版本**不会丢密码**，重新指向文件即可

#### ⚠️ 关于签名与后续更新

我核对过归档里这两个 APK **由同一把 Kunzisoft 密钥签名**（指纹见下方「校验方法」）。

但 Google Play 分发时可能启用 Play App Signing 重新签名——**这一点我没有验证**。所以：

- 装归档里的 `free.apk`，之后**未必**能从 Play 商店直接更新（可能报签名不匹配）。想走 Play 更新就直接从 Play 装
- 装归档里的 `libre.apk`，之后从 F-Droid 更新是正常的（F-Droid 用作者原始签名）

#### 安装方法

传到手机 → 用文件管理器点开 APK → 允许「安装未知来源应用」。

### 源码（上游真的消失时才需要）

- **KeePassXC 源码**：就在本仓库里，`develop` 分支及其全部提交历史、63 个 tag，fork 自带
- **KeePassDX 源码**：在 [Release `keepassdx-4.4.5`](../../releases/tag/keepassdx-4.4.5) 里
  - `KeePassDX-4.4.5-full-repo.bundle`（71 MB）—— 单文件装下完整仓库：10 分支、142 tag、9024 提交
  - `KeePassDX-4.4.5-source.tar.gz`（7 MB）—— 只要 4.4.5 这一版的源码快照

从 bundle 还原成正常仓库：

```bash
git clone KeePassDX-4.4.5-full-repo.bundle KeePassDX
cd KeePassDX && git checkout 4.4.5
```

---

## 本仓库分支说明

| 分支 | 内容 |
|---|---|
| `archive` | **当前分支**，仅在上游代码之上加了这份说明文件。上游原本的 README 保留为 `README.upstream-keepassxc.md` |
| `develop` | 上游 KeePassXC 的原始默认分支，**未做任何改动**，需要同步上游时切到它 |
| 其他 `feature/*` `fix/*` | fork 时带过来的上游分支，原样保留 |

## 为什么 KeePassDX 源码用 bundle 而不是推成分支

试过，推不进来。**GitHub 拒绝把毫不相关的历史推进一个 fork**（返回 HTTP 500）——KeePassDX 和 KeePassXC 是两个独立项目，没有共同祖先，fork 网络不接受这种对象图。

`git bundle` 是等效甚至更好的方案：单文件、含全部历史、离线可用、不依赖 GitHub。已用 `git bundle verify` 确认「records a complete history」，并实际克隆验证过 `4.4.5` tag 指向 commit `d053f4c5e2be7dc3f4a2784e8d08b9ff8bb3e7af`，与上游一致。

## 校验方法

每个 release 里都带了 `*-SHA256SUMS.txt` 清单，下载后直接：

```bash
sha256sum -c KeePassXC-2.7.12-SHA256SUMS.txt
sha256sum -c KeePassDX-4.4.5-SHA256SUMS.txt
```

> **坑提醒**：KeePassXC 官方的 `*.DIGEST` 文件是 **CRLF 换行**，在 Linux / macOS / Git Bash 下直接
> `sha256sum -c xxx.DIGEST` 会报 `No such file or directory`——因为 `\r` 被当成了文件名的一部分。
> 要么用上面那份我已经转成 LF 的 `KeePassXC-2.7.12-SHA256SUMS.txt`，要么临时去掉 CR：
>
> ```bash
> tr -d '\r' < KeePassXC-2.7.12-Win64.msi.DIGEST | sha256sum -c -
> ```

APK 另有更可靠的身份凭证——签名证书。两个 APK 均由同一把密钥签名：

```
subject: C=FR, ST=FRANCE, L=Rennes, O=Kunzisoft, OU=Kunzisoft, CN=Jeremy JAMET
SHA256:  7D:55:B8:AF:21:03:81:AA:BF:96:0F:07:E1:7C:F7:85:7B:6D:2A:64:2C:A2:DA:6B:F0:BD:F1:B2:00:36:2F:04
```

Android 靠签名密钥认身份。将来任何号称是 KeePassDX 的 APK，指纹对不上就不是同一作者构建的。

⚠️ KeePassDX 上游**不发布官方 DIGEST**，其哈希是下载当时记录的，无法与官方公布值比对。补充验证：文件字节数与 GitHub API 记录完全一致。

## 许可

| 项目 | 许可 | 说明 |
|---|---|---|
| KeePassXC | GPL-2.0 / GPL-3.0 | 本仓库即含完整源码，满足 GPL 的源码提供要求 |
| KeePassDX | GPL-3.0 | 对应完整源码在 release 内的 bundle / tarball 中 |

两者均允许再分发二进制。

---

## 最后一句提醒

**这个仓库仍然活在 GitHub 上。** 账号被封、GitHub 出事、或上游因 DMCA 被下架（这种情况 GitHub 会连 fork 一起删），它就没了。

真正的保险是**把这些文件另存一份到本地硬盘或 U 盘**，和密码库文件放在一起。到那时手上是完整一套：Windows 程序 + 安卓程序 + 源码 + 自己的密码库，换任何设备都能立刻恢复。

另外，密码本身的长期可用性其实不依赖任何一个项目——**kdbx 是公开的标准格式，有几十个独立实现**（KeePass、KeePassDX、Strongbox、KeeWeb、pykeepass……）。就算这里所有东西都没了，那些客户端照样能打开你的库。
