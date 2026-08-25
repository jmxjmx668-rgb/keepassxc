# 密码管理器离线归档

> 个人用归档仓库。目的只有一个：**万一上游项目哪天消失了，我手上还有能跑的程序和完整源码。**

这个仓库是 [keepassxreboot/keepassxc](https://github.com/keepassxreboot/keepassxc) 的 fork，除此之外还归档了安卓端客户端 [Kunzisoft/KeePassDX](https://github.com/Kunzisoft/KeePassDX)。

所有二进制都是 **2026-08-25 从各自官方渠道原样下载，未做任何修改**，全部附了 SHA256 和验证记录。

---

## 我要用密码库，从哪下东西

### Windows 桌面端 → [Release `2.7.12`](../../releases/tag/2.7.12)

| 文件 | 用途 |
|---|---|
| `KeePassXC-2.7.12-Win64.msi` | 装到电脑上。想指定目录：`winget install KeePassXCTeam.KeePassXC --location "D:\Program Files\KeePassXC"` |
| `KeePassXC-2.7.12-Win64.zip` | 便携版，解压即用，不写注册表。**放 U 盘里，插到任何电脑上双击就能开库** |
| `*.DIGEST` | 官方哈希文件，可直接比对 |

### 安卓端 → [Release `keepassdx-4.4.5`](../../releases/tag/keepassdx-4.4.5)

| 文件 | 用途 |
|---|---|
| `KeePassDX-4.4.5-libre.apk` | **推荐**。完全自由软件构建（F-Droid 同款） |
| `KeePassDX-4.4.5-free.apk` | Google Play 同款构建 |

安装：传到手机 → 文件管理器点开 → 允许「安装未知来源应用」。

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
