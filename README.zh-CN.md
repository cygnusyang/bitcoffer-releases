# Bitcoffer 下载与发布

[English](README.md)

这是 Bitcoffer 的**发布产物仓**，提供签名并公证过的安装包、校验文件和 OTA 更新清单。

[最新版本](https://github.com/cygnusyang/bitcoffer-releases/releases/latest) · [官方网站](https://cygnusyang.github.io/bitcoffer-releases/)

## 为什么单独设一个仓

源码仓是私有的。把发布产物放在这个公开仓，应用内更新和 Homebrew 就能在无需登录 GitHub 的情况下下载安装包。

[隐私政策](PRIVACY.md#简体中文) · [安全政策](SECURITY.md#简体中文)

## 下载

| 用途 | 文件 |
|---|---|
| 手动安装 | `Bitcoffer-v<version>.dmg` |
| 应用内更新 | `Bitcoffer-v<version>.zip` |
| 更新清单（Ed25519 签名） | `update-manifest.json` |

从[最新版本](https://github.com/cygnusyang/bitcoffer-releases/releases/latest)下载安装包，或用 Homebrew 安装。

```sh
brew install --cask cygnusyang/bitcoffer/bitcoffer
```

## 校验

随安装包下载 `SHA256SUMS`，将文件放在同一目录后运行以下命令，确认已下载文件的校验结果为 `OK`。

```sh
shasum -a 256 -c --ignore-missing SHA256SUMS
```

应用使用内嵌公钥验证 `update-manifest.json` 的 Ed25519 签名，验签通过后才接受更新。

## 安装包来源

Bitcoffer 的 GitLab CI 构建安装包，使用 Developer ID 签名并启用 Hardened Runtime，提交给 Apple 公证，然后装订公证票据。CI 只向本仓库上传发布产物。

## 双语发布说明

GitHub Release 先显示英文，再显示简体中文。发版前在源码仓的 `CHANGELOG.md` 对应版本段中准备两种语言，使用三级或更深标题。发布流水线提取该版本段，重跑时以它覆盖 GitHub Release 正文。
