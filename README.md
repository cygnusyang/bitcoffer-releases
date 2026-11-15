# Bitcoffer Releases

[Read in Simplified Chinese](README.zh-CN.md)

This is the **release artifact repository** for Bitcoffer. It contains signed and notarized installers, checksums, and the OTA update manifest.

[Latest release](https://github.com/cygnusyang/bitcoffer-releases/releases/latest) · [Website](https://cygnusyang.github.io/bitcoffer-releases/en.html)

## Why a separate repository?

The source repository is private. Hosting the release assets here makes them available to the in-app updater and Homebrew without requiring a GitHub login.

[Privacy Policy](PRIVACY.md#english) · [Security Policy](SECURITY.md#english)

## Downloads

| Purpose | File |
|---|---|
| Manual installation | `Bitcoffer-v<version>.dmg` |
| In-app updates | `Bitcoffer-v<version>.zip` |
| Update manifest (Ed25519-signed) | `update-manifest.json` |

Download the installer from the [latest release](https://github.com/cygnusyang/bitcoffer-releases/releases/latest) or install with Homebrew.

```sh
brew install --cask cygnusyang/bitcoffer/bitcoffer
```

## Verification

Download `SHA256SUMS` alongside the installer. Run the check in that directory and confirm the downloaded file reports `OK`.

```sh
shasum -a 256 -c --ignore-missing SHA256SUMS
```

The app verifies the Ed25519 signature of `update-manifest.json` against its embedded public key before accepting an update.

## Build provenance

Bitcoffer’s GitLab CI builds the installers, signs them with Developer ID and Hardened Runtime, submits them to Apple for notarization, and staples the notarization ticket. CI uploads only the release artifacts to this repository.

## Bilingual release notes

Each GitHub Release presents English first, followed by Simplified Chinese. Prepare both languages under the matching version in the source repository’s `CHANGELOG.md` using level-three or deeper headings. The publishing pipeline extracts that version section and replaces the GitHub Release body on reruns.
