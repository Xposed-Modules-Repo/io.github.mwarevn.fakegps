# Fake GPS: Fake GPS Navigation & Joystick

> Ultra-realistic GPS spoofing for rooted Android — powered by LSPosed / libxposed.
> _Giả lập GPS siêu thực cho Android đã root — chạy trên LSPosed / libxposed._

  [![Kotlin](https://img.shields.io/badge/Kotlin-1.9+-blue.svg?logo=kotlin)](https://kotlinlang.org)
  [![Platform](https://img.shields.io/badge/Platform-Android_8.0+-green.svg?logo=android)](https://www.android.com)
  [![LsPosed](https://img.shields.io/badge/LsPosed-Required-red.svg)](#)
  [![Mapbox](https://img.shields.io/badge/Mapbox-v11-blue.svg)](#)
  [![Maintainer](https://img.shields.io/badge/Maintained%3F-yes-brightgreen.svg)](#)
  [![Download](https://img.shields.io/badge/Download-Latest_Release-brightgreen.svg?style=flat-square)](../../releases)

---

## 🎬 Demo Videos & Screenshots (Click to view demo) / Demo (Click để xem)

<div align="center">

<table>
  <tr>
    <td align="center" width="33%">
       <a href="https://pub-a052aef593f24fe69f2aed2f3b80118f.r2.dev/demolight.mp4">
          <img src="https://pub-a052aef593f24fe69f2aed2f3b80118f.r2.dev/demolightcover.jpg" width="100%"/>
       </a>
      <br/>
      <sub><b>Demo</b> - Route Navigation</sub>
    </td>
    <td align="center" width="33%">
       <a href="https://pub-a052aef593f24fe69f2aed2f3b80118f.r2.dev/demodark.mp4">
          <img src="https://pub-a052aef593f24fe69f2aed2f3b80118f.r2.dev/demodarkcover.jpg" width="100%"/>
       </a>
      <br/>
      <sub><b>Demo</b> — 3D Style</sub>
    </td>
    <td align="center" width="33%">
      <img src="https://raw.githubusercontent.com/mwarevn/fake-gps/release/screenshot.jpg" width="100%"/>
      <br/>
      <sub><b>Screenshot</b> — Drawer</sub>
    </td>
  </tr>
</table>

</div>

---

# 🇬🇧 English

## Overview

Fake GPS hooks deep into Android's location stack so **every app on the device**
sees the fake fix instead of the real one. It intercepts the platform location
framework (`system_server`, the Google Play Services fused provider, and the two
raw message channels — NMEA and GnssStatus), and additionally denies the
side-channels (sensors, network, cell, Wi-Fi / Bluetooth scans, reverse
geocoding) that a determined app would otherwise use to re-derive your real
position.

Beyond a simple static point, it ships a full location toolkit: route navigation
simulation, an analog joystick, GPS track recording / replay, and unattended
scheduling that keeps working even while the app is killed.

## Features

### Spoofing

- **Static spoof** — teleport the fix to any point on the map.
- **Unstable indoor GPS (jitter)** — optional small autocorrelated wander within
  the reported accuracy radius, so a "held" fix wobbles like a real stationary
  receiver instead of sitting dead-still.

### Navigation

- **Route navigation simulator** — route planning via Mapbox Directions; the
  engine drives the fake fix along the polyline in a background foreground
  service.
    - Simulated **red lights** (configurable density + wait range)
    - **Auto-decelerate** into curves
    - **Ping-pong loop** mode (A → B → A …)
    - Off-road connectors, smooth interpolation
- **Analog joystick** — live-drive the fake position in real time at up to
  150 km/h with fine control near the centre.

### Recording & Replay

- **Record** the real device GPS track (+ pause / resume).
- **Replay** a track at its real recorded timing, or at a custom speed.

### Automation

- **Scheduled actions** (one-shot / daily / weekly): fire a static spoof,
  navigate, replay, or stop — executed by a native `AlarmManager`, so they run
  **even when the app process is killed**. State reconciles live when the app
  reopens.

### Stealth / Anti-detection

- **Per-app** or **system-wide** spoofing (system mode via the `system_server` hook).
- Independent toggles to deny the real signals that leak position:
    - Accuracy, motion sensors
    - Network location, cell (cell-ID) resolution
    - Wi-Fi scan, Bluetooth scan
    - Reverse geocoding
- Fake satellite **sky view** (GnssStatus) and **NMEA** streams.

### Quality of life

- Share-link **in / out**: Google Maps, Apple Maps, Waze, Grab, OSM, `geo:` URIs.
- **Favorites** for points and routes.
- **Backup & restore** (settings, favorites, schedules, recorded tracks).
- **In-app updater** — self-sideload of GitHub release APKs, with release
  metadata verified by **ECDSA P-256** (anti-MITM) and an APK-signature
  anti-tamper vault.
- Multi-language UI: **English, Tiếng Việt, 简体中文, 繁體中文**.

## Requirements

- A **rooted** device.
- **LSPosed** (the modern _Vector_ build for libxposed API 102).
- **Zygisk** (e.g. _ReZygisk_).
- Android **7.0+** (API 24).

> ⚠️ Use the latest LSPosed (Vector) and Zygisk (ReZygisk). Remove any older
> module build before installing this one.

## Installation

> 🎬 **Video guide:** https://youtu.be/26nd7CD_Erc

### 1. Prepare the device

- Make sure the device is **rooted** (Magisk / KernelSU / APatch).
- Install **Zygisk** (e.g. _ReZygisk_) and **LSPosed** (the _Vector_ build for
  libxposed API 102).
- If you previously installed an older build of this module, **uninstall it
  first**.

### 2. Install the APK

- Download the latest APK from the [Releases](https://github.com/mwarevn/fake-gps/releases) page.
- Install it on the device (you may need to allow "Install unknown apps").

### 3. Enable the module in LSPosed

1. Open the **LSPosed** manager.
2. Go to **Modules** and enable **Fake GPS**.
3. Select the **scope**:
    - `system` + `android` + `com.google.android.gms` → **system-wide** spoofing
      (affects every app).
    - Or pick individual apps for **per-app** spoofing.

### 4. Reboot

- **Reboot the device.** LSPosed hooks are applied at process fork time, so the
  module only becomes active after a fresh boot.

### 5. Grant permissions

- Open the app and grant the requested **location** permissions.
- (Optional) Grant `POST_NOTIFICATIONS` for the status notification.
- (Optional) Disable battery optimization for reliable scheduled actions.

## Usage

1. Open **Fake GPS** — the full-screen map is the home screen.
2. **Search** or **tap** a point on the map, then tap **Set** to start spoofing
   there. Open any other app to confirm it sees the fake location.
    - Tap the fake marker again and use **Replace** to move it, or **Unset** to stop.
3. **Route navigation:** mark a destination → **Directions** → adjust the draft
   → press **Start**. Use the HUD to change speed, toggle red lights /
   auto-decelerate / loop, pause, or skip a red light.
4. **Joystick:** open the joystick control and drag to live-drive the fake fix.
5. **Record / Replay:** record your real track, then replay it at real timing or
   a custom speed.
6. **Schedules:** create one-shot / daily / weekly actions (spoof, navigate,
   replay, stop) that fire even while the app is closed.
7. **Settings → Anti-detection:** fine-tune the stealth toggles per your needs.

> System-wide mode (hooking `system_server`) takes effect on the next process
> load — a **device reboot** is required after switching it on.

## Troubleshooting

| Symptom                           | Fix                                                                                                                         |
| --------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| Module not listed in LSPosed      | Reinstall the APK; confirm it is a libxposed API 102 build and that Zygisk is enabled.                                      |
| Apps still show the real location | Ensure the scope includes `system` + `com.google.android.gms`, then **reboot**. Verify the module is active inside the app. |
| Scheduled action did not fire     | Disable battery optimization for the app; allow exact alarms; check the schedule is enabled.                                |
| Map is blank / crashes on launch  | Provide a valid Mapbox **public token** (`pk.`) in the app settings.                                                        |
| Spoof stops after a while         | Some apps re-check; make sure your anti-detection toggles match the target app.                                             |

## Disclaimer

This module is intended for **testing, development, and privacy**. Spoofing your
location may violate the terms of service of some apps and may be illegal in
certain contexts. **You are responsible for how you use it.** Use it only where
you are permitted to.

---

<div align="center">

**Made with ❤️ for the rooted-Android community**

`io.github.mwarevn.fakegps`

</div>
