# Bitcoffer Releases / Bitcoffer 下载与发布

This is the **release artifact repository** for Bitcoffer. It contains signed and
notarized installers, checksums, and the OTA update manifest. There is **no source code here**.

Bitcoffer 的**发布产物仓**。这里提供签名并公证过的安装包、校验文件和 OTA 更新清单，**没有源码**。

[Latest release / 最新版](https://github.com/cygnusyang/bitcoffer-releases/releases/latest) · [Website in English](https://cygnusyang.github.io/bitcoffer-releases/en.html) · [中文网站](https://cygnusyang.github.io/bitcoffer-releases/)

## Why a separate repository? / 为什么单独设一个仓

The source repository is private, and GitHub does not serve release assets of private
repositories to anonymous requests — both in-app OTA checks and the Homebrew cask would
get a 404. Artifacts therefore live here, decoupled from source.

源码仓是私有的，而 GitHub 私有仓的 Release 资产**不支持匿名下载**——应用内 OTA 检查
和 Homebrew cask 都会拿到 404。所以把产物单独放到这个公开仓，源码与分发解耦。

[Privacy Policy / 隐私政策](PRIVACY.md) · [Security Policy / 安全政策](SECURITY.md)

## Downloads / 下载

| Purpose / 用途 | File / 文件 |
|---|---|
| Manual installation / 手动安装 | `Bitcoffer-v<version>.dmg` |
| In-app updates / 应用内 OTA | `Bitcoffer-v<version>.zip` |
| Update manifest (Ed25519-signed) / 更新清单（Ed25519 签名） | `update-manifest.json` |

Download the installer from the [latest release](https://github.com/cygnusyang/bitcoffer-releases/releases/latest) or install with Homebrew.

从 [最新版本 / Latest Release](https://github.com/cygnusyang/bitcoffer-releases/releases/latest) 下载安装包，或用 Homebrew 安装。

```sh
brew install --cask cygnusyang/bitcoffer/bitcoffer
```

## Verification / 校验

Download `SHA256SUMS` alongside the installer. Run the check in that directory and confirm the downloaded file reports `OK`.

随安装包下载 `SHA256SUMS`，将文件放在同一目录后核对相应校验值。

```sh
shasum -a 256 -c --ignore-missing SHA256SUMS
```

The app verifies the Ed25519 signature of `update-manifest.json` against its embedded public key before accepting an update.

`update-manifest.json` 由 Ed25519 私钥签名，应用侧用内嵌公钥验签后才接受更新。

## Build provenance / 安装包来源

Bitcoffer’s GitLab CI builds the installers, signs them with Developer ID and Hardened Runtime, submits them to Apple for notarization, and staples the notarization ticket. CI uploads only the release artifacts to this repository.

构建由 Bitcoffer 的 GitLab CI 流水线产出（Developer ID 签名 + Hardened Runtime +
Apple 公证 + stapler 装订），CI 只把产物推送到这里，不在这里提交任何代码。

## Bilingual release notes / 双语发布说明

Every GitHub Release should include English first, followed by Simplified Chinese, in two sections that cover the same changes. Prepare both languages under the matching version in the source repository’s `CHANGELOG.md` using level-three or deeper headings. The publishing pipeline extracts that version section and replaces the GitHub Release body on reruns.

每个 GitHub Release 都应先显示「English」，再显示「简体中文」，两种语言逐项说明相同的变化。发版前在源码仓的 `CHANGELOG.md` 对应版本段中准备两种语言，使用三级或更深标题；发布流水线会提取该版本段，重跑时也会用它覆盖 GitHub Release 正文。
