# Overture哥のAndroid Macro

**Sol's RNG Android 天氣偵測器** — 透過讀取 Roblox logcat 的 [BloxstrapRPC](https://github.com/pizzaboxer/bloxstrap/wiki/Integrating-Bloxstrap-functionality-into-your-game) 資料，在天氣變更時發送 Discord Webhook 通知。100% 準確，不使用 OCR，不截取螢幕。

---

## 功能

- **Logcat 偵測** — 讀取 Roblox logcat 中的 `[BloxstrapRPC]` 資料，100% 準確辨識天氣
- **Discord Webhook 通知** — 天氣變更時發送精美 Embed（天氣開始 / 天氣結束），包含天氣顏色、Roblox 縮圖、伺服器連結與運行時間
- **Roblox 帳號偵測** — 自動從 logcat 擷取執行中的 Roblox 帳號，並在 Embed 顯示帳號（顯示名稱與 @使用者名稱）
- **稀有天氣 @everyone** — 偵測到 Glitched、Dreamspace 或 Cyberspace 時自動 @everyone
- **天氣個別設定** — 可單獨開關每個天氣的通知；稀有天氣始終開啟，無法關閉
- **自訂縮圖上傳** — 為任何天氣上傳自訂圖片，取代 Discord Embed 中的預設 Roblox 縮圖
- **自動解析天氣縮圖** — 未設定自訂縮圖時，透過 Roblox Asset API 自動取得天氣圖片
- **Webhook 測試按鈕** — 一鍵測試 Discord Webhook 是否正常運作
- - **Eden 偵測（畫面辨識）** — 透過螢幕擷取 + 文字辨識（ML Kit OCR）偵測 Eden 過場動畫，偵測到時發送含截圖的 Discord 通知（Eden 不會寫入 logcat，僅能用畫面偵測）
- **偵測紀錄** — 主頁即時顯示偵測到的天氣名稱與次數
- **前景服務** — Roblox 開啟時持續在背景運行
- **自動更新** — 檢查新版本並在應用內安裝

## 支援天氣

| 一般天氣 | 稀有天氣 |
|---------|---------|
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

## 系統需求

- Android 10+（API 29）
- [Shizuku](https://shizuku.rikka.app/) — 僅需使用**一次**來授予 `READ_LOGS` 權限
- 已啟用 [BloxstrapRPC](https://github.com/pizzaboxer/bloxstrap/wiki/Integrating-Bloxstrap-functionality-into-your-game) 的 Roblox（Sol's RNG 原生支援）
- 一個 Discord Webhook URL

## 安裝

1. 前往 [Releases 頁面](https://github.com/herrylai246-bit/Overture-Android-Macro-Public_Releases/releases)
2. 下載最新的 `Overture-Android-Macro-release.apk`
3. 在 Android 裝置上安裝 APK（可能需要允許安裝不明來源的應用程式）

## 設定指南

### 1. 安裝並啟動 Shizuku

1. 從 Play Store 安裝 [Shizuku](https://play.google.com/store/apps/details?id=moe.shizuku.privileged.api)
2. 開啟 Shizuku 並啟動服務（無線偵錯或 ADB 方式）
3. 讓 Shizuku 在背景持續運行

### 2. 設定 Macro

1. 開啟 **Overture哥のAndroid Macro**
2. 輸入你的 **Discord Webhook URL**
3. 點擊 **測試 Webhook** 確認 Webhook 是否正常運作
4. 輸入你的 **Roblox 伺服器連結**（會顯示在 Webhook 訊息中，方便其他人加入）
5. 點擊 **天氣設定** 選擇要偵測的天氣，並為每個天氣上傳自訂縮圖
6. 點擊 **設定 Shizuku 權限** — 僅需授權一次
7. 點擊 **開始偵測**

### 3. 完成！

Macro 現在會：
- 發送 **🟢 已開始** 狀態到你的 Discord Webhook
- 即時監控 Roblox logcat 的天氣變更
- 發送 **天氣開始** 和 **天氣結束** Embed，包含顏色、縮圖、伺服器連結與運行時間
- 在 Embed 顯示目前執行中的 **Roblox 帳號**（顯示名稱與 @使用者名稱）
- 偵測到稀有天氣（Glitched、Dreamspace、Cyberspace）時自動 @everyone
- 在主頁即時顯示偵測紀錄
- 停止時發送 **🔴 已停止** 狀態

---

## 運作原理

```
Roblox → logcat [BloxstrapRPC] → LogcatBiomeReader → DetectionService → Discord Webhook
```


1. Sol's RNG 將 RPC 資料寫入 Android logcat，標籤為 `[BloxstrapRPC]`
2. 應用程式透過 `READ_LOGS` 權限（經由 Shizuku 授予）即時讀取 logcat
3. 解析 JSON 資料，擷取天氣名稱和 Roblox Asset ID
4. 從 Roblox 的遊戲加入紀錄擷取帳號 userId，並透過 Roblox API 解析為帳號名稱（顯示於 Embed）
5. 檢查天氣個別設定 — 跳過已關閉的天氣，使用自訂縮圖（如有上傳）
6. 未設定自訂縮圖時，透過 Roblox API 解析 Asset ID 為縮圖 URL
7. 透過 Webhook 發送 Discord Embed
