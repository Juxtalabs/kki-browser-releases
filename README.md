# KKI Browser — releases

Public distribution point for **KKI Browser** installers and the update manifests
the browser reads to know when a newer version exists.

This repository holds **no source code**. It contains only:

- `standard/‹platform›.json` — small manifests the browser polls (version + download URL + sha256)
- GitHub **Releases** — the actual installer files, attached as release assets

## Channel and platform

| Channel    | Manifest                  | Build                        |
|------------|---------------------------|------------------------------|
| `standard` | `standard/win-x64.json`   | The shipping locked product, Windows |
| `standard` | `standard/mac-universal.json` | The shipping locked product, macOS (Intel and Apple silicon) |

KKI Browser ships the `standard` channel on Windows and macOS. The macOS manifest points at
the `.zip` the updater installs; the `.dmg` on the same release is for a first install.

## Manifest shape

```json
{
  "version": "0.9.18",
  "url": "https://github.com/Juxtalabs/kki-browser-releases/releases/download/v0.9.18/KKIBrowserSetup.exe",
  "sha256": "‹64-hex lowercase over the installer›",
  "notes": "- What changed."
}
```

The `url` points at an asset on this repo's own GitHub Release. The browser only
trusts downloads under `https://github.com/Juxtalabs/`, and it reads **only this
repository** — a Pluto Browser release can never reach a KKI machine.

## How an update ships

Built by hand and published by hand; there is no GitHub Action for KKI.

```powershell
scripts\deployment\windows-package.ps1 -QtDir <qt> -Channel standard -Brand kki `
    -ApiBase https://sejati.juxtalabs.io
```
```bash
scripts/deployment/publish-release.sh --brand kki --channel standard <version> <installer>
```

The publish script creates the release, attaches the installer and rewrites the
manifest. Release notes come from `CHANGELOG-kki.md` in the source repo and are
deliberately generic.
