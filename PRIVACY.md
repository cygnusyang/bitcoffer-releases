# Privacy Policy / 隐私政策

[English](#english) · [简体中文](#简体中文)

## English

Published at <https://ai.cygnusyang.top/bitcoffer/privacy> (English) and <https://ai.cygnusyang.top/bitcoffer/privacy-zh> (Chinese). This file is the authoritative English text; the Chinese page follows it.


**TL;DR (English)** — Bitcoffer collects nothing: no account, no telemetry, no analytics, no cloud sync, no third-party SDKs. Your vault exists only on your Mac, encrypted with your master password. The direct-download build only connects when *you* click "Check for Updates", and it goes to four fixed GitHub hosts. The App Store build has no update check; Apple handles subscriptions through StoreKit (§3.1). §8 explains how to verify every claim below yourself.

---

### 1. What we do not do

| Not done | Detail |
| --- | --- |
| No account | There is no sign-up, no email, no user ID anywhere in the product |
| No telemetry | No analytics SDK, no crash reporter, no usage counters, no "anonymous" beacons |
| No cloud sync | The vault is a local file; Bitcoffer never uploads it |
| No ads, no tracking | Nothing to opt out of, because nothing is collected |
| No third-party network SDKs | The app links no ad/analytics/font/CDN frameworks |

### 2. What stays on your Mac

| Data | Where | Protection |
| --- | --- |
| Vault (items, TOTP secrets, passkey metadata) | `~/Library/Containers/top.cygnusyang.coffer/Data/Documents/Bitcoffer/<vault-uuid>/` | Argon2id-derived key + AEAD; master password never stored |
| Diagnostics log | `~/Library/Containers/top.cygnusyang.coffer/Data/Library/Logs/Bitcoffer-diag.log` | Local text; **never uploaded automatically** |
| Clipboard | macOS pasteboard, only when you copy a field | Optional auto-clear after a timeout you set |
| Preferences | `UserDefaults` in the app container | Language, lock timeout, etc. |

Nothing here leaves the machine unless **you** export or copy it.

### 3. The only network access: update checks

The direct-download build performs a network request **only when you explicitly ask it to check for updates**. There is no background polling.

| Item | Value |
| --- | --- |
| Hosts (allow-listed) | `api.github.com`, `github.com`, `objects.githubusercontent.com`, `release-assets.githubusercontent.com` |
| What is sent | A standard HTTPS GET for `…/releases/latest/download/update-manifest.json`, plus the default HTTP headers of the system URL session (e.g. `User-Agent`). **No vault data, no identifiers, no version fingerprint beyond the request path.** |
| What is received | Signed manifest (Ed25519) + download URLs; the manifest signature is verified against a public key embedded in the app before anything is trusted |
| Downloads | `Bitcoffer-vX.Y.Z.zip` from the same allow-list; SHA-256 is checked against the signed manifest |

If the update check fails, the app keeps working offline — it simply reports the error.

#### 3.1 Mac App Store subscriptions

Apple handles purchases and renewals for Bitcoffer Pro. Bitcoffer asks the system StoreKit framework for the current entitlement. The macOS `storekitd` daemon communicates with Apple and returns transaction metadata to Bitcoffer, such as the product ID, expiration date and revocation status. No vault content, item names or usage data are involved. The App Store build omits the updater and declares no `network.*` entitlement.

### 4. AI tools / MCP integration (opt-in)

- The MCP server speaks **stdio** or a **Unix domain socket** (`$BITCOFFER_MCP_UDS`) — never TCP, never a public port.
- Tools are **read-only by default**: an AI client can list item names/metadata and, if you approve, run a command with a secret injected into that subprocess's environment.
- Secret values are never returned to the model context; there is no tool that prints a password.
- Registration with Claude Code / CodeBuddy / Codex writes a local config file; nothing is sent to us.
- If you never enable it, no MCP process runs.

### 5. Diagnostics

- Logs are written locally and **rotated on disk**. They never leave the machine on their own.
- If you file a bug, you may attach the relevant log manually — **review it first**; logs deliberately never contain password values (design rule R2), but they do contain timestamps, error codes and file paths.
- There is no automatic crash reporting. If the app crashes, macOS may create a crash report in `~/Library/Logs/DiagnosticReports/` — that file is yours, and is only shared if you choose to.

### 6. Deleting your data

| Action | Effect |
| --- | --- |
| Delete an item | Moves to Trash inside the vault; can be restored or permanently deleted |
| Delete a vault | Removes the vault directory; **unrecoverable** without an encrypted backup |
| Delete the app | Removes the binary; the vault directory stays unless you remove it |
| Full removal | Delete the app **and** `~/Library/Containers/top.cygnusyang.coffer/` |

### 7. Children / legal

Bitcoffer collects no personal data at all, so it collects nothing from minors either. It is a local utility, not a service; there is no account relationship and no data processing agreement to enter into.

### 8. Verify these claims yourself

Trust should not require believing a markdown file:

1. **Entitlements** — the direct-download build declares App Sandbox plus exactly one network entitlement:
   ```bash
   codesign -d --entitlements - /Applications/Bitcoffer.app
   ```
   You should see `com.apple.security.app-sandbox` and `com.apple.security.network.client`, and no other `network.*` key. The App Store build has no `network.*` entitlement (§3.1).
2. **Watch the traffic** — run the app under Little Snitch / LuLu / `nettop`, or simply disconnect from the network: everything except the update check keeps working.
3. **Read the source** — network code is isolated in the updater: `macos/Bitcoffer/Updater/`. Grep for `URLSession` to enumerate every call site.
4. **Check the logs** — `~/Library/Containers/top.cygnusyang.coffer/Data/Library/Logs/Bitcoffer-diag.log` is plain text; read it.

### 9. Changes to this policy

Any change to data handling will be recorded in `CHANGELOG.md` and in this file's git history. Because Bitcoffer has no server side, there is no way for us to change what happens to your data without shipping a new app version — and every version is a signed, inspectable artifact.

### 10. Contact

- Security issues: see [`SECURITY.md`](SECURITY.md) (private channel).
- Everything else: [GitHub Issues](https://github.com/cygnusyang/bitcoffer-releases/issues).

### 11. Website analytics

Bitcoffer’s website records page views, download clicks, referral tags, page language and country codes. The browser keeps referral tags for the current session. The server uses a daily IP hash for rate limiting and retains analytics events for 90 days. Website analytics receive no vault contents.

---

*Last updated: 2026-10-25 · applies to Bitcoffer v2.13.x*

---

## 简体中文

发布页：[简体中文](https://ai.cygnusyang.top/bitcoffer/privacy-zh) · [English](https://ai.cygnusyang.top/bitcoffer/privacy)。中文版以英文原文为准。

**摘要**：Bitcoffer 应用不收集数据，无账号、遥测、使用分析、云同步或第三方 SDK。密码库只保存在你的 Mac 上，由主密码保护。直装版仅在你点击「检查更新」时访问四个固定的 GitHub 域名。App Store 版没有更新检查，订阅由 Apple 通过 StoreKit 处理（第 3.1 节）。第 8 节说明如何自行验证。网站分析见第 11 节。

### 1. 我们不做什么

| 项目 | 说明 |
| --- | --- |
| 无账号 | 产品无需注册，不使用邮箱或用户 ID |
| 无遥测 | 不含分析 SDK、崩溃上报框架、使用计数器或「匿名」信标 |
| 无云同步 | 密码库是本地文件，Bitcoffer 从不上传它 |
| 无广告、无追踪 | 应用不收集相关数据，无需手动退出 |
| 无第三方网络 SDK | 应用不链接广告、分析、字体或 CDN 框架 |

### 2. 哪些数据保存在 Mac 上

| 数据 | 位置 | 保护方式 |
| --- | --- | --- |
| 密码库（条目、TOTP 密钥、通行密钥元数据） | `~/Library/Containers/top.cygnusyang.coffer/Data/Documents/Bitcoffer/<vault-uuid>/` | Argon2id 派生密钥和 AEAD；不保存主密码 |
| 诊断日志 | `~/Library/Containers/top.cygnusyang.coffer/Data/Library/Logs/Bitcoffer-diag.log` | 本地文本；**不会自动上传** |
| 剪贴板 | 仅在你复制字段时写入 macOS 剪贴板 | 可选：到达你设定的时限后自动清除 |
| 偏好设置 | 应用容器中的 `UserDefaults` | 保存语言、锁定超时等设置 |

除非你主动导出或复制，否则这些数据不会离开本机。

### 3. 应用唯一的联网行为：检查更新

直装版仅在你明确要求检查更新时发起网络请求，不在后台轮询。

| 项目 | 说明 |
| --- | --- |
| 允许访问的域名 | `api.github.com`、`github.com`、`objects.githubusercontent.com`、`release-assets.githubusercontent.com` |
| 发送内容 | 对 `…/releases/latest/download/update-manifest.json` 发起标准 HTTPS GET 请求，使用系统 URL 会话的默认 HTTP 请求头（例如 `User-Agent`）。**不发送密码库数据、标识符或超出请求路径所含信息的版本指纹。** |
| 接收内容 | 使用 Ed25519 签名的更新清单及下载链接；应用先用内嵌公钥验签，再信任清单内容 |
| 下载内容 | 从同一域名白名单下载 `Bitcoffer-vX.Y.Z.zip`，并按签名清单校验 SHA-256 |

检查更新失败时，应用会报告错误，其余功能继续离线工作。

#### 3.1 Mac App Store 订阅

Bitcoffer Pro 的购买和续订由 Apple 处理。Bitcoffer 通过系统 StoreKit 框架查询当前订阅权益。macOS 的 `storekitd` 进程与 Apple 通信，并向 Bitcoffer 返回产品 ID、到期日期及撤销状态等交易元数据。过程中不涉及密码库内容、条目名称或使用数据。App Store 版不含更新检查，也未声明任何 `network.*` 权限。

### 4. AI 工具与 MCP 集成（由你主动启用）

- MCP 服务通过 **stdio** 或 **Unix 域套接字**（`$BITCOFFER_MCP_UDS`）通信，不使用 TCP，也不开放公共端口。
- 工具默认只读。AI 客户端可以列出条目名称和元数据；经你批准后，可以运行命令并把凭据注入相应子进程的环境。
- 凭据值不会返回到模型上下文，没有工具会打印密码。
- 在 Claude Code、CodeBuddy 或 Codex 中注册会写入本地配置文件，不会向我们发送数据。
- 如果你没有启用此功能，MCP 进程不会运行。

### 5. 诊断

- 日志只在本机写入并轮换，不会自动发送。
- 提交问题时可以手动附上相关日志。**请先检查内容。**日志按设计规则 R2 不记录密码值，但会记录时间戳、错误码及文件路径。
- 应用不自动上报崩溃。macOS 可能在 `~/Library/Logs/DiagnosticReports/` 中生成崩溃报告；仅当你主动分享时才会交给他人。

### 6. 删除数据

| 操作 | 结果 |
| --- | --- |
| 删除条目 | 移入库内回收站，可以恢复或永久删除 |
| 删除密码库 | 删除密码库目录；没有加密备份就**无法恢复** |
| 删除应用 | 删除应用程序，密码库目录仍会保留 |
| 彻底移除 | 删除应用及 `~/Library/Containers/top.cygnusyang.coffer/` |

### 7. 未成年人及法律事项

Bitcoffer 应用不收集个人数据，包括未成年人的个人数据。它是本地工具，无需开设账号或签署数据处理协议。

### 8. 如何自行验证

1. **查看权限声明**。直装版声明 App Sandbox 和一项网络权限：
   ```bash
   codesign -d --entitlements - /Applications/Bitcoffer.app
   ```
   可看到 `com.apple.security.app-sandbox` 和 `com.apple.security.network.client`，没有其他 `network.*` 权限。App Store 版没有任何 `network.*` 权限（第 3.1 节）。
2. **观察网络流量**。用 Little Snitch、LuLu 或 `nettop` 查看应用流量。也可以直接断网，检查更新以外的功能仍可使用。
3. **阅读源码**。网络代码集中在更新模块 `macos/Bitcoffer/Updater/`。搜索 `URLSession` 可列出所有调用位置。
4. **查看日志**。`~/Library/Containers/top.cygnusyang.coffer/Data/Library/Logs/Bitcoffer-diag.log` 是可直接阅读的纯文本。

### 9. 政策变更

数据处理方式如有变更，将记录在 `CHANGELOG.md` 和本文件的 Git 历史中。Bitcoffer 应用没有服务端；如需改变应用处理数据的方式，必须发布新版本。每个版本都有签名，可自行检查。

### 10. 联系方式

- 安全问题：请通过[安全政策](SECURITY.md)中的私下渠道报告。
- 其他问题：[GitHub Issues](https://github.com/cygnusyang/bitcoffer-releases/issues)。

### 11. 网站分析

Bitcoffer 网站记录页面浏览、下载点击、来源标签、页面语言和国家或地区代码。浏览器在当前会话中保留来源标签。服务端用按日计算的 IP 哈希限制请求频率，并将分析事件保留 90 天。网站分析不会收到密码库内容。

---

*原文最后更新于 2026-10-25，适用于 Bitcoffer v2.13.x。*
