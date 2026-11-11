# 南山四合願｜智慧營運盤點

Node.js 及 SQLite 後端集中保存問卷，填答者不需登入；Dashboard 彙整各大樓、主題、子系統及物業模組狀態。填答明細與 CSV 匯出需輸入管理密碼；統計 API 僅回傳預設分類與計數所需欄位，不傳回姓名、廠商、聯絡資訊或文字答案。此版本未設定應填總數，不計算整體完成率。

## 執行

需要 Node.js 22.13 或更新版本，建議 Node.js 24 LTS，不需安裝外部套件：

```bash
export ADMIN_TOKEN='自行設定至少20字元的隨機管理密碼'
export DB_PATH='./data/inventory.sqlite'
export PORT=3000
node server.js
```

瀏覽 `http://localhost:3000`。資料庫首次執行自動建立；`DB_PATH` 應放在具持久化能力的伺服器磁碟，包含 WAL 與 SHM 等 SQLite 相關檔案。管理密碼應使用部署環境變數或秘密管理服務設定，不可上傳 GitHub。填答時的未送出草稿暫存在填答者的瀏覽器，送出後才儲存至伺服器。

## 部署及資料安全

GitHub 只保存程式；GitHub Pages 無法執行 Node.js 或儲存共用 SQLite，需另行部署至可執行 Node.js 且支援持久化磁碟的 VM、主機或容器服務，並設定 HTTPS、備份及監控。SQLite 以單機使用為前提，避免多主機共用資料庫檔案；需多機擴展時應遷移至共用資料庫。上線前應依南山要求完成資安評估、限流、權限及資料保存政策，並考慮管理者個別帳號及操作稽核。不要在沒有額外保護的公開網路正式收集敏感資料。

SQLite 與先前 JSON 版本的檔案不同，舊的 `data/responses.json` 不會自動匯入，若已有正式資料需先備份並另行遷移。先前預覽網站採用瀏覽器儲存，資料不會自動上傳至本後端。

網站的靜態 GitHub Pages 分支只供預覽，與本 Node.js 後端部署分離。IndexedDB 位於各使用者自己的瀏覽器，不會儲存在 GitHub，也無法自行彙整其他人的資料。
