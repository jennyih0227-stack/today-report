# 今日報報 ｜ 自動圖卡產生器

每日自動彙整匯率、黃金、台股加權指數，一鍵下載成可傳 LINE 的圖卡。
品牌：熊誠毅｜誠毅傳承（躍勇業發組）。

## 線上網址（部署後）

```
https://jennyih0227-stack.github.io/today-report/
```

## 資料來源

| 項目 | 來源 | 說明 |
|------|------|------|
| 美金/日元/歐元/澳幣/人民幣 | open.er-api.com | 免金鑰、市場中價，與台銀牌告略有差距 |
| 黃金價 | api.gold-api.com | USD/oz 換算成台幣/台兩(999) |
| 台股加權指數 | TWSE 公開 JSON | 由 GitHub Actions 每日寫入 `data/twii.json`，避開瀏覽器 CORS |

> 匯率為市場中價，下載前可用「✏️ 手動微調」對齊台灣銀行牌告。

## 重要：請用瀏覽器開啟

`index.html` 已將 html2canvas 函式庫**內嵌**，不依賴任何外部 CDN，
因此下載 PNG 在大多數環境都能運作。

但仍建議用 **Safari 或 Chrome** 開網址，下載最順暢。
若在某些 App 的內建 HTML 檢視器開啟，可能出現
「Block external links in HTML viewer」阻擋（那是檢視器的安全設定，
擋的是抓取匯率/黃金/台股的 API 連線）。遇到時：
1. 改用真正的瀏覽器開啟，或
2. 數據抓不到時按「✏️」手動填，再「⬇️ 下載」，或長按圖卡「儲存影像」。

## 一次性部署步驟

1. 在 GitHub 用帳號 `jennyih0227-stack` 新建一個 repo，名稱 **`today-report`**（設為 Public）。
2. 把本資料夾所有檔案上傳（含 `.github/workflows/fetch-twii.yml`）。
3. 進 repo → **Settings → Pages**：
   - Source 選 **Deploy from a branch**
   - Branch 選 **main** / 資料夾 **/(root)** → Save。
4. 進 repo → **Settings → Actions → General**：
   - 拉到「Workflow permissions」選 **Read and write permissions** → Save。
   （這樣 Actions 才能把 twii.json 推回 repo）
5. 進 **Actions** 分頁 → 左側選「抓取台股加權指數」→ 右上 **Run workflow** 手動跑一次，確認 `data/twii.json` 有更新。

完成後，每個工作日台灣時間 **07:45** 與 **14:00** 會自動更新台股數字。

## 每日使用

1. 開啟網址（手機或電腦皆可）。
2. 自動抓取最新數據；若某項失敗，依提示在「手動微調」補上。
3. 按「⬇️ 下載 PNG」存圖，傳 LINE 群組。

## 自訂

- 落款印章文字「躍勇」：改 `index.html` 中 `.foot .seal span` 內容。
- 底部說明文字：改 `.foot .note`。
- 排程時間：改 `.github/workflows/fetch-twii.yml` 的 cron（注意是 UTC，台灣 -8 小時）。
