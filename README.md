# 親子活動搜尋器

## 專案目的
幫助安排與 12 歲男孩的週末與連假活動，透過 AI 搜尋台北及近郊
適合孩子體驗、培養品味與視野的活動資訊。

## 使用方式
1. 開啟 https://0xlmwhatim.github.io/family-activity-finder/
2. 填入 Gemini API Key（存於 Mac Keychain，名稱：Gemini API Key）
3. 選擇地區與興趣方向，按「搜尋並篩選活動」
4. 結果可複製清單，或點「存入 Notion」存進 Events_DB

## 技術架構
- 前端：單一 HTML 檔案，無後端
- AI：Gemini 2.5 Flash API（含 Google Search 即時搜尋）
- 部署：GitHub Pages
- 網址：https://0xlmwhatim.github.io/family-activity-finder/
- API Key：使用者手動輸入，不寫入程式碼

## Notion 整合
- 串接資料庫：Events_DB（活動收藏）
- 存入欄位：活動名稱、開始時間、地點、超連結、備註、狀態、來源、類型
- 狀態預設：收集
- 來源預設：其它
- 類型預設：親子
- 備註內容：地區 + AI 推薦理由
- 使用前需在 Events_DB 頁面連結 Notion Integration

## 搜尋條件預設值
- 孩子年齡：12 歲
- 地區：台北市區、新北／基隆、宜蘭／桃園
- 搜尋週數：未來 4 週

## 已知限制
- Gemini 回傳的活動連結需人工確認是否正確
- 日期區間（如 4/18–5/9）自動取起始日存入 Notion

## 待辦與未來規劃
- [ ] 測試 Notion 存入功能是否正常運作
- [ ] 考慮串接 Make.com 每週自動通知
- [ ] 未來可直接串接 Accupass、KKtix API 提升資料準確度

## 版本紀錄
- v1.0｜2026-04 — 初版，使用 Anthropic API
- v1.1｜2026-04 — 改為 Gemini API
- v1.2｜2026-04 — 修正模型為 gemini-2.5-flash
- v1.3｜2026-04 — 修正 JSON 解析：清除特殊字元與引用標記
- v1.4｜2026-04 — 更新 Notion 存入欄位對應 Events_DB