# Bitcoffer 下载与发布 / Bitcoffer Releases

Bitcoffer 的**发布产物仓**。这里提供签名并公证过的安装包、校验文件和 OTA 更新清单，**没有源码**。

This is the **release artifact repository** for Bitcoffer. It contains signed and
notarized installers, checksums, and the OTA update manifest. There is **no source code here**.

## 为什么单独设一个仓 / Why a separate repository?

源码仓是私有的，而 GitHub 私有仓的 Release 资产**不支持匿名下载**——应用内 OTA 检查
和 Homebrew cask 都会拿到 404。所以把产物单独放到这个公开仓，源码与分发解耦。

The source repository is private, and GitHub does not serve release assets of private
repositories to anonymous requests — both in-app OTA checks and the Homebrew cask would
get a 404. Artifacts therefore live here, decoupled from source.

[隐私政策 / Privacy Policy](PRIVACY.md) · [安全政策 / Security Policy](SECURITY.md)

## 下载 / Downloads

| 用途 / Purpose | 文件 / File |
|---|---|
| 手动安装 / Manual installation | `Bitcoffer-v<version>.dmg` |
| 应用内 OTA / In-app updates | `Bitcoffer-v<version>.zip` |
| 更新清单（Ed25519 签名） / Update manifest (Ed25519-signed) | `update-manifest.json` |

从 [最新版本 / Latest Release](https://github.com/cygnusyang/bitcoffer-releases/releases/latest) 下载安装包，或用 Homebrew 安装。

Download the installer from the latest release or install with Homebrew.

```sh
brew install --cask cygnusyang/bitcoffer/bitcoffer
```

## 校验 / Verification

随安装包下载 `SHA256SUMS`，将文件放在同一目录后核对相应校验值。

Download `SHA256SUMS` alongside the installer. Run the check in that directory and confirm the downloaded file reports `OK`.

```sh
shasum -a 256 -c --ignore-missing SHA256SUMS
```

`update-manifest.json` 由 Ed25519 私钥签名，应用侧用内嵌公钥验签后才接受更新。

The app verifies the Ed25519 signature of `update-manifest.json` against its embedded public key before accepting an update.

## 安装包来源 / Build provenance

构建由 Bitcoffer 的 GitLab CI 流水线产出（Developer ID 签名 + Hardened Runtime +
Apple 公证 + stapler 装订），CI 只把产物推送到这里，不在这里提交任何代码。

Bitcoffer’s GitLab CI builds the installers, signs them with Developer ID and Hardened Runtime, submits them to Apple for notarization, and staples the notarization ticket. CI uploads only the release artifacts to this repository.

## 双语发布说明 / Bilingual release notes

每个 GitHub Release 都应包含「简体中文」和「English」两节，两种语言逐项说明相同的变化。发版前在源码仓的 `CHANGELOG.md` 对应版本段中准备两种语言，使用三级或更深标题；发布流水线会提取该版本段，重跑时也会用它覆盖 GitHub Release 正文。

Every GitHub Release should include Simplified Chinese and English sections that cover the same changes. Prepare both languages under the matching version in the source repository’s `CHANGELOG.md` using level-three or deeper headings. The publishing pipeline extracts that version section and replaces the GitHub Release body on reruns.
