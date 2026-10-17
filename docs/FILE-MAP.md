# File Map — P012 檔案地圖

核對基準：`befc918ec4bf5e772c53706716dd44821fac8749`。下表列出整理前檔案，大小來自 Git tree；新增管理文件另列於文末。

| 路徑 | 大小（bytes） | 類型 |
| --- | ---: | --- |
| [README.md](../README.md) | 55 | md |
| [index.html](../index.html) | 77225 | html |

## 管理入口

README.md、AGENTS.md、HANDOFF.md、PROJECT.json、CLAUDE.md、GEMINI.md、.github/copilot-instructions.md、docs/VERIFICATION.md。

## 檔內結構（行號索引，2026-10-18 Claude 依基準 `08b3846` 整理）

只讀需要的段落；標 ⚠️ 的是超長單行資料，不要整行載入。

| 約略行號 | 內容 |
| ---: | --- |
| 7–913 | `<style>` |
| 915–1137 | HTML 介面（攀岩牆、模式與難度按鈕、狀態列、感測器紀錄面板、結算視窗） |
| 1138–2476 | 主程式 `<script>` |
| 1144–1195 | 設定與 `MODES`（6 種模式） |
| 1196–1281 | 音名頻率、`SONG_LIBRARY`（6 首）、`ROUTE_THEMES` |
| 1282–1466 | `VOICE_PACK` 語音句庫 |
| 1467–1520 | `GAME` 狀態與 DOM 參照 |
| 1521–1697 | 紀錄 `log()`、音效、語音、歌曲與路線抽選 |
| 1698–1958 | 岩點生成、牆面、路線、特效、攀岩者、音名提示、伴奏層 |
| 1959–2011 | `setPhase()` 與按鈕事件 |
| 2012–2209 | `startGame()`、記憶／隨機／追逐模式、倒數 |
| 2210–2323 | `onHoldClick()`、`handleCorrect()`、`handleWrong()` |
| 2324–2476 | `endGame()` 結算與重設 |
