# Bitcoffer Releases

Bitcoffer 的**发布产物仓**。这里只有签名并公证过的安装包和 OTA 更新清单，**没有源码**。

This is the **release artifact repository** for Bitcoffer. It contains only signed and
notarized installers plus the OTA update manifest. There is **no source code here**.

## 为什么单独设一个仓

源码仓是私有的，而 GitHub 私有仓的 Release 资产**不支持匿名下载**——应用内 OTA 检查
和 Homebrew cask 都会拿到 404。所以把产物单独放到这个公开仓，源码与分发解耦。

The source repository is private, and GitHub does not serve release assets of private
repositories to anonymous requests — both in-app OTA checks and the Homebrew cask would
get a 404. Artifacts therefore live here, decoupled from source.

## 下载

| 用途 | 文件 |
|---|---|
| 手动安装 | `Bitcoffer-v<version>.dmg` |
| 应用内 OTA | `Bitcoffer-v<version>.zip` |
| 更新清单（Ed25519 签名） | `update-manifest.json` |

推荐用 Homebrew 安装：

```sh
brew install --cask cygnusyang/bitcoffer/bitcoffer
```

## 校验

每个 Release 附 `SHA256SUMS`。下载后核对：

```sh
shasum -a 256 -c SHA256SUMS
```

`update-manifest.json` 由 Ed25519 私钥签名，应用侧用内嵌公钥验签后才接受更新。

## 安装包来源

构建由 Bitcoffer 的 GitLab CI 流水线产出（Developer ID 签名 + Hardened Runtime +
Apple 公证 + stapler 装订），CI 只把产物推送到这里，不在这里提交任何代码。
