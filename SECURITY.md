# Security Policy / 安全政策

[English](#english) · [简体中文](#简体中文)

## English

Bitcoffer is a local password manager. A security defect in this product category can
expose every credential a user owns, irreversibly — reports are handled as such.

This repository holds signed installers and the OTA update manifest. The product's full
security policy, scope definitions and threat model live with the source, which is
private; what matters for a reporter is below.

### Reporting a vulnerability

**Preferred:** use the **Security** tab of this repository → *Report a vulnerability*.
The report and its discussion stay private, visible only to you and the maintainer.

**Fallback:** `ruohuyang@163.com` (the address in the product's `LICENSE`).

Do **not** open a public issue, discussion or pull request that describes a defect.
If both channels are unavailable, open an issue that says only that you have a security
matter to report privately, with no technical detail — a private channel will follow.

Please include the affected version (or a build date), the class of issue, minimal
reproduction steps, what an attacker gains and under which preconditions, and whether
the matter has already been disclosed elsewhere.

### Response targets

| Stage | Target |
| --- | --- |
| Acknowledgement | Within 3 business days |
| Initial assessment | Within 7 business days |
| Fix or mitigation | Depends on severity; an estimate follows the assessment |
| Public disclosure | After the fix ships, at a time agreed with the reporter |

The project is early-stage; if a matter is urgent, say so in the report.

### In scope

Cryptographic implementation (Argon2id parameters, HKDF subkeys, AEAD AAD, nonces), key
management (envelope wrap/unwrap, verifier design, biometric wrapping, incomplete memory
zeroisation), container format (version-detection bypass, integrity-check evasion,
migrations that corrupt data), importers (malformed 1PUX / CSV / opvault causing crashes,
memory exhaustion or out-of-bounds access), authorisation bypass (reading data while
locked, cross-vault access), information disclosure (secrets in logs, crash reports or
backups), **any network capability at all** (the product's central promise is that there
is none besides the update check), and platform integration (misconfigured macOS
Keychain / sandbox entitlements weakening key protection).

### Out of scope

Attacks requiring physical access plus root, full-disk encryption disabled by the user,
brute force against a user-chosen weak master password (but silently *accepting* a weak
password without warning is a defect), and vulnerabilities in third-party dependencies
(report those upstream; we follow up if they affect us).

---

## 简体中文

Bitcoffer 是一款本地密码管理器。此类产品的安全缺陷可能导致用户的全部凭据泄露，且后果不可逆。我们会按这一风险处理报告。

本仓库提供签名安装包及 OTA 更新清单。完整的安全政策、范围定义和威胁模型随私有源码保存。以下是报告问题所需的信息。

### 如何报告漏洞

**首选方式：**打开本仓库的 **Security（安全）** 标签页，选择 **Report a vulnerability（报告漏洞）**。报告和讨论仅报告者与维护者可见。

**备用方式：**发邮件至 `ruohuyang@163.com`（产品 `LICENSE` 中列出的地址）。

请勿在公开 Issue、Discussion 或 Pull Request 中描述漏洞。如果上述两种渠道都不可用，可以开一个 Issue，说明你需要私下报告安全问题，请勿附上技术细节。维护者会提供私下沟通的渠道。

请提供受影响的版本或构建日期、问题类型、最少复现步骤、攻击者能获得什么及其前置条件，以及此问题是否已在其他地方披露。

### 响应目标

| 阶段 | 目标 |
| --- | --- |
| 确认收到 | 3 个工作日内 |
| 初步评估 | 7 个工作日内 |
| 修复或缓解 | 视严重程度而定，评估后给出预计时间 |
| 公开披露 | 修复发布后，与报告者商定时间 |

项目仍处于早期阶段。如果问题紧急，请在报告中说明。

### 受理范围

包括密码学实现（Argon2id 参数、HKDF 子密钥、AEAD 附加认证数据和 nonce）、密钥管理（信封加解封、验证器设计、生物识别密钥封装、内存清零不完整）、容器格式（绕过版本检测或完整性校验、迁移损坏数据）、导入器（畸形 1PUX、CSV 或 opvault 导致崩溃、内存耗尽或越界访问）、绕过授权（锁定时读取数据、跨库访问）、信息泄露（日志、崩溃报告或备份中含凭据）、**任何联网能力**（产品承诺仅为检查更新而联网），以及平台集成问题（例如 macOS 钥匙串或沙盒权限配置不当而削弱密钥保护）。

### 不在受理范围内

需要物理访问并拥有 root 权限的攻击、用户禁用全盘加密、对用户所选弱主密码的暴力破解（但应用接受弱密码时不提示仍算缺陷），以及第三方依赖中的漏洞（请向上游报告；影响本项目时我们会跟进）。
