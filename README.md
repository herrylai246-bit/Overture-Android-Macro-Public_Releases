# Overture哥のAndroid Macro

**Sol's RNG biome detector for Android** — reads Roblox logcat via [BloxstrapRPC](https://github.com/pizzaboxer/bloxstrap/wiki/Integrating-Bloxstrap-functionality-into-your-game), sends Discord webhook notifications when biomes change. 100% accurate, no OCR, no screen capture.

> ⚠️ **Pre-release** — this is an early version. Please report bugs via Issues.

---

## Features

- **Logcat-based detection** — reads `[BloxstrapRPC]` data from Roblox logcat for 100% accurate biome identification
- **Discord webhook notifications** — sends rich embeds (Biome Started / Biome Ended) with biome color, Roblox thumbnail, server link, and uptime
- **@everyone ping for rare biomes** — automatically pings `@everyone` when Glitched, Dreamspace, or Cyberspace is detected
- **Auto-resolve biome thumbnails** — fetches the biome image directly from Roblox asset API
- **Foreground service** — runs in the background while Roblox is open
- **Auto-updater** — checks for new releases from GitHub and installs in-app
- **One-time setup** — after granting READ_LOGS via Shizuku, you can uninstall Shizuku and the macro continues to work

## Supported Biomes

| Regular | Rare |
|---------|------|
| Corruption | Cyberspace |
| Eggland | Dreamspace |
| Heaven | Glitched |
| Hell | |
| Normal | |
| Null | |
| Rainy | |
| Sand Storm | |
| Snowy | |
| Starfall | |
| Windy | |

---

## Requirements

- Android 10+ (API 29)
- [Shizuku](https://shizuku.rikka.app/) — needed **once** to grant `READ_LOGS` permission
- Roblox with [BloxstrapRPC](https://github.com/pizzaboxer/bloxstrap/wiki/Integrating-Bloxstrap-functionality-into-your-game) data enabled (Sol's RNG supports this natively)
- A Discord webhook URL

## Installation

1. Go to the [Releases page](https://github.com/herrylai246-bit/Overture-Android-Macro-Releases/releases)
2. Download the latest `app-release.apk`
3. Install the APK on your Android device (you may need to allow installs from unknown sources)

## Setup Guide

### 1. Install & Start Shizuku

1. Install [Shizuku](https://play.google.com/store/apps/details?id=moe.shizuku.privileged.api) from the Play Store
2. Open Shizuku and start the service (wireless debugging or ADB method)
3. Keep Shizuku running in the background

### 2. Configure the Macro

1. Open **Overture哥のAndroid Macro**
2. Enter your **Discord Webhook URL**
3. Enter your **Roblox server link** (displayed in webhook messages so others can join)
4. Tap **設定 Shizuku 權限** (Setup Shizuku Permission) — this grants `READ_LOGS` once
5. Tap **開始偵測** (Start Detection)

### 3. Done!

The macro will now:
- Send a **🟢 Started** status to your Discord webhook
- Monitor Roblox logcat for biome changes
- Send **Biome Started** and **Biome Ended** embeds with color, thumbnail, server link, and uptime
- Ping `@everyone` for rare biomes (Glitched, Dreamspace, Cyberspace)
- Send a **🔴 Stopped** status when you stop the macro



---

## How It Works

```
Roblox → logcat [BloxstrapRPC] → LogcatBiomeReader → DetectionService → Discord Webhook
```

1. Sol's RNG writes RPC data to Android logcat with the `[BloxstrapRPC]` tag
2. The app reads logcat in real-time via the `READ_LOGS` permission (granted through Shizuku)
3. Parses the JSON payload to extract the biome name and Roblox asset ID
4. Resolves the asset ID to a thumbnail URL via the Roblox API
5. Sends a  Discord embed via your webhook