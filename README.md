# 親子活動搜尋器

## 專案目的
幫助安排與 12 歲男孩的週末與連假活動，透過 AI 即時搜尋台北及近郊
適合孩子體驗、培養品味與視野的活動資訊，並可將值得收藏的活動存入 Notion。

## 使用方式
1. 開啟 https://0xlmwhatim.github.io/family-activity-finder/
2. 填入 Gemini API Key
3. 設定孩子年齡、搜尋週數、活動地區與興趣方向
4. 按「搜尋並篩選活動」
5. 在結果中勾選想收藏的活動，或使用「全選／取消」快速調整
6. 可按「複製清單」複製全部搜尋結果，或按「存入已選活動」只將勾選項目存進 Notion Events_DB

## 技術架構
- 前端：單一 HTML 檔案，無後端
- AI：Gemini 2.5 Flash API（含 Google Search 即時搜尋）
- 部署：GitHub Pages
- 網址：https://0xlmwhatim.github.io/family-activity-finder/
- 金鑰安全：Gemini API Key 僅存在瀏覽器畫面中；Notion 存取則改由 Make.com Webhook 中繼處理，前端不需填寫 Token。

## 主要功能
- 依孩子年齡、未來週數、地區與興趣方向搜尋親子活動
- 預設搜尋台北市區、新北／基隆、宜蘭／桃園，也可加入外縣市旅行
- AI 回傳 3–5 個推薦活動，包含日期、地點、地區、連結與推薦理由
- 搜尋結果可一鍵複製成文字清單
- 每個活動卡片都有「加入收藏」勾選框，預設全選
- 可使用「全選／取消」批次切換收藏狀態
- 存入 Notion 時只會存入目前已勾選的活動

## Notion 整合 (透過 Make.com Webhook)
- 串接資料庫：Events_DB（活動收藏）
- 存入方式：網頁前端透過 Webhook 將資料打包送出，再由 Make.com 代理寫入 Notion
- 存入欄位：活動名稱、開始時間、地點、超連結、備註、狀態、來源、類型
- 狀態預設：收集
- 來源預設：其它
- 類型預設：親子
- 備註內容：地區 + AI 推薦理由
- 設定方式：需在 Make.com 新增情境「Custom Webhook ➔ Notion (Create a Database Item)」並準備好對應欄位

## 搜尋條件預設值
- 孩子年齡：12 歲
- 搜尋週數：未來 4 週
- 預選地區：台北市區、新北／基隆、宜蘭／桃園
- 可選地區：外縣市旅行
- 興趣方向：預設不選，不選則全搜

## 已知限制
- Gemini 回傳的活動連結需人工確認是否正確
- 日期區間（如 4/18–5/9）自動取起始日存入 Notion
- 「複製清單」會複製全部搜尋結果，不受活動勾選狀態影響

## 待辦與未來規劃
- [x] 解決直連 Notion API 的 CORS 錯誤 (改用 Make Webhook)
- [x] 移除每次使用需重複輸入 Notion Token 的麻煩與資安風險
- [x] 新增 `externalId` 以支援 Make.com 阻擋記錄重複寫入 Notion
- [x] 考慮讓「複製清單」也支援只複製已選活動
- [ ] 考慮串接 Make.com 每週自動搜尋並通知

## 迭代優化建議 (Future Iterations)
1. **防止重複存檔 (Deduplication)**：配合網頁傳送的 `externalId` (活動名稱＋日期)，可在 Make 自動化流程中加入「Search Objects」判斷式，實現「無則新增，有則略過」。
2. **自動觸發與定時報表**：善用 Make 內建的排程功能 (Schedule)，可讓中繼站每週五固定抓取週末活動，並自動透過 LINE 或 Email 發送通知。
3. **引入官方售票 API**：為進階提升活動豐富性與避免 AI 幻覺，未來可逐漸取代純 LLM 搜尋，改串接 Accupass、KKtix 或各縣市文化局官方 API。
4. **地圖定位整合**：目前已在 Notion 中設定純文字地址紀錄，未來若串接資料庫能取得精準地址，可進一步嵌入 Google Maps 連結，方便出遊導航。

## 版本紀錄
- v1.0｜2026-04 — 初版，使用 Anthropic API
- v1.1｜2026-04 — 改為 Gemini API
- v1.2｜2026-04 — 修正模型為 gemini-2.5-flash
- v1.3｜2026-04 — 修正 JSON 解析：清除特殊字元與引用標記
- v1.4｜2026-04 — 更新 Notion 存入欄位對應 Events_DB
- v1.5｜2026-04 — 新增活動勾選收藏、全選／取消，Notion 改為只存入已選活動
- v1.6｜2026-04 — 架構大翻新：解決 Notion CORS 報錯，改用 Make.com Webhook 中繼，並移除前端輸入 Token 欄位
- v1.7｜2026-04 — 資料結構優化：新增送出 `externalId` 特徵碼，支援後續防重覆機制
- v1.8｜2026-04 — UX 優化：新增 LocalStorage 記憶 Gemini API Key (保留 BYOK 模式)
