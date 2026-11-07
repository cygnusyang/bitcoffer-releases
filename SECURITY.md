# Security policy

Bitcoffer is a local password manager. A security defect in this product category can
expose every credential a user owns, irreversibly — reports are handled as such.

This repository holds signed installers and the OTA update manifest. The product's full
security policy, scope definitions and threat model live with the source, which is
private; what matters for a reporter is below.

## Reporting a vulnerability

**Preferred:** use the **Security** tab of this repository → *Report a vulnerability*.
The report and its discussion stay private, visible only to you and the maintainer.

**Fallback:** `ruohuyang@163.com` (the address in the product's `LICENSE`).

Do **not** open a public issue, discussion or pull request that describes a defect.
If both channels are unavailable, open an issue that says only that you have a security
matter to report privately, with no technical detail — a private channel will follow.

Please include the affected version (or a build date), the class of issue, minimal
reproduction steps, what an attacker gains and under which preconditions, and whether
the matter has already been disclosed elsewhere.

## Response targets

| Stage | Target |
| --- | --- |
| Acknowledgement | Within 3 business days |
| Initial assessment | Within 7 business days |
| Fix or mitigation | Depends on severity; an estimate follows the assessment |
| Public disclosure | After the fix ships, at a time agreed with the reporter |

The project is early-stage; if a matter is urgent, say so in the report.

## In scope

Cryptographic implementation (Argon2id parameters, HKDF subkeys, AEAD AAD, nonces), key
management (envelope wrap/unwrap, verifier design, biometric wrapping, incomplete memory
zeroisation), container format (version-detection bypass, integrity-check evasion,
migrations that corrupt data), importers (malformed 1PUX / CSV / opvault causing crashes,
memory exhaustion or out-of-bounds access), authorisation bypass (reading data while
locked, cross-vault access), information disclosure (secrets in logs, crash reports or
backups), **any network capability at all** (the product's central promise is that there
is none besides the update check), and platform integration (misconfigured macOS
Keychain / sandbox entitlements weakening key protection).

## Out of scope

Attacks requiring physical access plus root, full-disk encryption disabled by the user,
brute force against a user-chosen weak master password (but silently *accepting* a weak
password without warning is a defect), and vulnerabilities in third-party dependencies
(report those upstream; we follow up if they affect us).
