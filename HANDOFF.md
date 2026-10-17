# Handoff — P012 互動攀岩牆（模擬器 v3.0）

- 更新時間：2026-10-18T03:25+08:00
- 更新者：Claude（逐檔讀原始碼，撰寫專案交接）
- 倉庫：https://github.com/jackeytw886/Fcli
- 分支：main
- 本次基準 SHA：`08b3846bcaf8616a5a7b7778d1fb6d52601ed074`（Codex 管理整理提交）
- 核對層級：已讀全部原始碼；已執行語法檢查與無頭瀏覽器測試（見 docs/VERIFICATION.md）；**未接實體攀岩牆**。

## 1. 這是什麼

互動攀岩牆的**網頁模擬器**（單檔 HTML，v3.0「Route Voice & Guess Song Edition」）。牆上 50 個岩點，每個岩點可感應觸摸並亮燈；網頁用滑鼠點岩點代替實際觸摸，用來展示玩法、調整語音與規則。每觸發一個正確岩點就彈出一個音，爬完路線＝演奏一首藏起來的兒歌，結束時揭曉歌名。右側有「感測器紀錄」面板，模擬實體感測器的觸發訊息。

- 線上網址：https://jackeytw886.github.io/Fcli/ （GitHub Pages）
- 實體版的岩點感測器、燈光與控制板資料**不在此倉庫**。

## 2. 檔案

只有 `index.html`（約 77 KB、2,479 行），無外部程式庫、可離線。行號索引見 docs/FILE-MAP.md。

## 3. 程式怎麼運作

| 部分 | 位置／名稱 | 說明 |
| --- | --- | --- |
| 設定 | `TOTAL_HOLDS=50`、`TOTAL_TIME=90`、`DEBOUNCE_MS=400` | 岩點數、限時、同一岩點 400 ms 內重複觸發忽略（模擬感測器去彈跳） |
| 模式 | `MODES` | `melody` 音樂路線、`random` 隨機亮點、`speed` 速度挑戰、`memory` 記憶、`rhythm` 節奏、`chase` 追逐；各有 `pathLen`（easy／normal／hard）與是否顯示路線 |
| 歌曲 | `SONG_LIBRARY`、`NOTE_FREQ`、`NOTE_LABEL` | 6 首：兩隻老虎、小蜜蜂、倫敦鐵橋、瑪莉有隻小綿羊、小星星、划船歌；`notes` 是音名陣列 |
| 路線主題 | `ROUTE_THEMES` | 路線名稱與開場說明（夢幻星河、超級挑戰、節奏王者…） |
| 語音 | `VOICE_PACK`（opening、guess、reveal、ready、start、correct、wrong、combo、attract…）、`speakRandomTemplate()` | 每類多句隨機挑，`{routeName}` 等樣板會被替換 |
| 岩點 | `generateHoldPositions()`、`buildWall()` | 7 欄 × 9 列網格加亂數偏移，每次載入岩點位置不同 |
| 狀態 | `GAME`（phase、mode、difficulty、path、steppedSet、score、noteIndex、musicLayers…） | `phase`：`idle → countdown → playing → result` |
| 觸發 | `onHoldClick()` → `handleCorrect()`／`handleWrong()` | 必須依路線**順序**觸發；正確 +（100 − 錯誤數×5）分 |
| 伴奏 | `updateMusicLayers()` | 進度 >0 旋律、≥25% 鼓、≥50% 貝斯、≥75% 和弦 |
| 紀錄 | `log(msg, type)` | 右側面板，模擬 `[SENSOR #n]`、`[HIT ✓]`、`[ROUTE]` 等訊息 |

開始流程：按開始 → 抽路線與歌曲 → 約 4 秒開場語音（記憶模式另外閃一次路線順序）→ 倒數 → 開始計時。

## 4. 已知問題與待辦

- ⚠️ **隨機亮點模式邏輯錯誤（已用無頭瀏覽器重現）**：`startRandomMode()` 會定時把亮燈移到路線的下一個岩點，但 `onHoldClick()` 仍要求觸發「路線上第一個還沒踩的岩點」。只要錯過一次亮燈，之後玩家點亮著的岩點都會被判「順序錯誤」。重現：選隨機模式、開始後等 4 秒、點亮著的岩點 → 錯誤數 +1。修正方向：隨機模式改用「目前亮著的岩點」判定，或錯過時把該岩點也標記為略過。
- 追逐模式在遊戲進行中會定時改寫 `GAME.path` 的下一個目標（`startChaseMode()`），邏輯與一般模式不同，修改判定時要一起測。
- 實體感測器的通訊格式、岩點編號與燈光控制都還沒有規格文件。

## 5. 測試方式

直接開 `index.html`。自動化測試可讀 `GAME.phase`、`GAME.path`、`GAME.steppedSet`、`GAME.score`，點 `.hold`（`data-id`）或呼叫 `GAME.holds[id].el.click()` 模擬觸發；模式按鈕是 `.mode-btn[data-mode=…]`，難度按鈕帶 `data-diff`。

## 6. 修改時注意

- 新增模式要在 `MODES` 加設定，並在 `startGame()` 處理顯示方式，在 `onHoldClick()` 確認判定規則是否適用。
- 每個 `await` 後檢查 `myFlow !== GAME.flowId`，避免重設後舊流程繼續跑。

## 7. 版本沿革

- 2026-05-17：經 EZPage 部署 v3.0。
- 2026-10-17：Codex 管理整理（代號 P012、AI 入口文件）。
- 2026-10-18：Claude 補寫本交接，記錄隨機模式錯誤。

## 版本定位

基準 SHA 定位本次文件修改前的程式；本次文件提交由此檔所在的 Git 提交與倉庫歷史定位，不預填同一提交自己的 SHA。
