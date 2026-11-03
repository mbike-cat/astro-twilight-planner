# 🌙 Deep-Sky Imaging Schedule & Dynamic Moon/Twilight Calculator

# 深空拍攝排程與動態月光/曙暮光計算器

[English Version](#-english-documentation) | [繁體中文版本](#-繁體中文說明文件)

---

## 🇬🇧 English Documentation

### 1. Concept & Purpose

For astrophotographers and automated telescope users (such as those using smart telescopes like the ZWO Seestar series), moonlight and solar twilight are primary sources of sky glow degradation. Traditional tools rely on fixed time-based buffers, which fail to capture the dynamic geometric nature of lunar and solar interference.

This tool shifts the paradigm from a rigid time buffer to an **altitude-based threshold calculation** and provides a **single continuous observing-night timeline** from sunset to the following sunrise, without splitting the night at midnight tailored for mobile and desktop screens. It helps users calculate precise safe imaging windows based on different locations, solar astronomical twilight ($\text{Sun at } -18^\circ$), dynamic moon phases, and moon-target separation angles, while featuring a built-in target library with smart sorting to identify the best astrophotography targets.

### 2. Calculation Methodology & Equations

The application is powered by the high-precision **Astronomy Engine** library and follows these mathematical steps:

* **Moon Phase Illumination ($P$):**
The exact phase angle ($\theta$) of the Moon is computed using `Astronomy.MoonPhase(astroTime)`. The illuminated fraction percentage ($P$) is derived as:

$$P = \frac{1 - \cos(\theta \times \frac{\pi}{180})}{2} \times 100$$

* **Dynamic Altitude Threshold ($\text{Threshold}_{\text{alt}}$):**
Instead of a fixed cutoff, the altitude threshold scales linearly based on the current illumination percentage $P$ and the user-defined full moon altitude threshold ($\text{Threshold}_{\text{base}}$):

$$\text{Threshold}_{\text{alt}} = -\left(\text{Threshold}_{\text{base}} \times \frac{P}{100.0}\right)$$

* **Equipment Fit Score (0–100):**
$$\text{Fit} = \min\left(100,\ \frac{\sqrt{\text{major} \times \text{minor}}}{0.25 \times \text{FOV}_{\text{short}}} \times 100\right)$$
$\text{major}$ and $\text{minor}$ are the target's angular dimensions (arcminutes) and $\text{FOV}_{\text{short}}$ is the short side of the selected telescope profile's field of view (Seestar S30 Pro, S50 Pro, or three user-defined profiles). Large targets are not penalized for being unable to fit in a single frame (they can be shot as mosaics); targets without size data are scored as "unknown".

* **Imaging Sequence Scoring Formulas (selectable priority):**
  * Observing conditions first (default): $\text{Score} = \text{Alt} \times 1.5 + \min(\text{Sep}_{\text{eff}}, 90) \times 0.5$
  * Lowest moonlight impact: $\text{Score} = \text{Sep}_{\text{eff}} \times 1.5 + \text{Alt} \times 0.5$
  * Equipment fit first: $\text{Score} = \text{Fit} \times 1.2 + \text{Alt} \times 0.35 + \min(\text{Sep}_{\text{eff}}, 90) \times 0.25$ (unknown size is treated as $\text{Fit} = 45$)

  $\text{Sep}_{\text{eff}}$ equals the Moon separation while the Moon is above the dynamic altitude threshold at the slot midpoint, and $180^\circ$ otherwise (separation is irrelevant while the Moon is low). A target must stay above the horizon limit of its own azimuth at the start, middle and end of the slot. A $-25$ penalty is applied to targets already scheduled in previous slots to promote target diversity.

* **Time Display Consistency:** Event times are shown to the minute (seconds truncated), and every displayed duration (e.g. sunset to astronomical dusk, moonrise to interference start) is computed from those displayed minutes, so the numbers on screen always agree.

### 3. Key Features

* **Auto-Calculate on Page Load**: Evaluates observation conditions immediately upon page load without requiring manual button triggers, complete with non-blocking error status handling and secondary library fallbacks.
* **Dynamic Moon & Sun Twilight Assessment**: Computes exact windows where the sun is below $-18^\circ$ and evaluates moonlight interference based on dynamic phase thresholds and a selectable full-moon interference altitude threshold ($-6^\circ$, $-8^\circ$, $-10^\circ$, $-12^\circ$, $-14^\circ$, $-16^\circ$, or $-18^\circ$; default $-12^\circ$).
* **Continuous Observing-Night Timeline**: A single timeline from sunset through midnight to the following sunrise, highlighting twilight, moon interference, astronomical events, and **Safe Deep-Sky Windows**.
* **Responsive Desktop & Mobile Layout**: Automatically expands up to 1480px width (1760px on wide screens) and formats into a two-column layout on screens wider than 1000px, while remaining cleanly structured for narrow mobile screens.
* **Smart Target Management & Sorting**: Includes a classic deep-sky target library, custom target adding (up to 500), optical filter tagging (narrowband vs. broadband), and angular size metrics.
* **Smart Imaging Sequence Planner**: Generates an hourly prioritized observation schedule based on target altitude, Moon separation, and target diversity. Parameter changes mark an existing schedule as needing an update; schedule generation remains a manual action.
* **Custom Horizon Profile with Asymmetric Layout**: 8 directional sliders with cardinal directions (N, E, S, W) displaying numbers on the left and compound directions (NE, SE, SW, NW) on the right.
* **Observation Site & Horizon Settings at the Top**: The collapsible **📂 Observation Site & Horizon Settings** panel sits above the saved-site dropdown, so first-time users can immediately add their own site (name, latitude, longitude, elevation, 8-direction horizon) and save up to 10 sites, without scrolling to the bottom.
* **Fixed Site UTC Offset**: All inputs and displayed times follow the offset you set for the observation site (hours and 15-minute steps), independent of the device time zone.
* **Imaging Equipment Profiles**: Seestar S30 Pro, Seestar S50 Pro, or three user-defined profiles (focal length and sensor size) with live field-of-view and a fit score; targets can also be filtered by angular size.
* **Built-in Diagnostics**: A Debug Log (copy / export TXT / clear), a manual Recalculate action and Restore Defaults live in the collapsed Data Backup & Advanced Diagnostics section.
* **Multilingual & Theme-aware**: Traditional Chinese, Simplified Chinese and English, with light and dark display modes.
* **Data Backup & Exchange**: Full JSON backup/restore and CSV target list import/export.

### 4. Changelog

* **v0.6.0 (2026-11)**: First release after publishing to GitHub. Renamed the "Observatory" wording to **Observation Site** in all three languages (繁體中文 觀察地點 / 简体中文 观察地点 / English Observation Site), since imaging is not limited to observatories; moved **📂 Observation Site & Horizon Settings** to the top of the page, above the saved-site dropdown, so first-time users can add their own site (name, coordinates, elevation, 8-direction horizon) without scrolling to the bottom; synchronized the in-app version number (page title, header, JSON backup and diagnostics), which still reported 0.5.21; removed the leftover slider value element after the move back to a dropdown; fixed double-escaped degree symbols in the v0.5.22 changelog entries.
* **v0.5.22 (2026-11)**: Replaced the full-moon interference altitude slider with a dropdown offering $-6^\circ$ to $-18^\circ$ in $2^\circ$ steps (default $-12^\circ$); changed the observing-night display to one continuous sunset-to-next-sunrise timeline; moved Recalculate into the Diagnostics & Repair section; added a clickable warning icon beside the Assessment Report heading; moved Restore Defaults to the bottom of Backup & Advanced Diagnostics with a confirmation prompt and backup reminder; clarified that large deep-sky targets may be captured as mosaics rather than being penalized for not fitting in one frame.
* **v0.5.21 (2026-11)**: Removed manual "Calculate Imaging Window" button in favor of automatic calculation on page load with background scheduling; introduced non-blocking status banner with retry button `🔄` for inputs or library errors; added CDN fallback loading (unpkg); cleaned up duplicate translation dictionary keys and restored the GitHub Issues footer link.
* **v0.5.20 (2026-11)**: Converted full moon altitude interference setting to an interactive slider ($-18^\circ \sim -6^\circ$, default $-12^\circ$); dynamically updated timeline title with current moon threshold angle; rounded moon phase ($P$) and altitude threshold displays to integers; aligned twilight time delta calculations with displayed rounded time values.
* **v0.5.18 - v0.5.19 (2026-11)**: Redesigned layout for mobile and desktop screens (two-column layout for $\ge 1000\text{px}$ viewports); updated astronomical target coordinates, catalog names, and angular dimensions (added Stephan's Quintet HCG 92 & The Box HCG 61); introduced fixed UTC offset settings; upgraded label auto-collision engines and telescope profile settings.
* **v0.5.16 - v0.5.17 (2026-11)**: Re-ordered UI components for optimal workflow; added expandable Moonlight & Solar Twilight Detailed Assessment section; updated timeline title to "🌌 Observing-Night Timeline" with simplified "Twi" / "曙" abbreviations; added manual-trigger schedule recalculation indicators ("Parameters Changed").
* **v0.4.27 (2026-10)**: Adjusted numerical alignment for 8 directional custom horizon sliders: cardinal directions (N, E, S, W) display numbers on the left, compound directions (NE, SE, SW, NW) display numbers on the right.
* **v0.4.26 (2026-10)**: Standardized horizon angle value placement to the left side of sliders.
* **v0.4.5 (2026-10)**: Dynamic target initial character filter supports Chinese characters and localized "All" selection.
* **v0.3.45 (2026-10)**: Fixed timeline tick classification logic by separating `darkDawnSecs` from `twilightSecs`, ensuring complete precision where astronomical night-end (astronomical dawn) and night-start (astronomical dusk) timestamps and vertical guide lines on timelines strictly render in vivid green (#10b981).
* **v0.3.44 (2026-10)**: Bound `kind` properties directly to astronomical events, resolving millisecond timestamp matching discrepancies.
* **v0.3.43 (2026-10)**: Introduced a 3-second tolerance window for event axis tick classification.
* **v0.3.42 (2026-10)**: Upgraded timelines with professional gradient styling, independent top interference badges, and auto-collision prevention for time axes.
* **v0.3.37 - v0.3.41 (2026-10)**: Added JSON backup/restore and CSV target exchange functionality.
* **v0.3.30 - v0.3.36 (2026-09 - 2026-10)**: Expanded classic target library to 29 items, implemented angular separation calculations, and added visibility window evaluations for target altitudes $\ge 15^\circ$.
* **v0.3.0 - v0.3.29 (2026-08 - 2026-09)**: Developed offline single-file HTML client architecture, integrated Astronomy Engine, and implemented longitude-based local timezone calculations.
* **v0.1.0 - v0.2.9 (2026-08)**: Initial prototyping, basic sunset/sunrise search, and observing-night timeline layout establishment.

---

## 🇭🇰 繁體中文說明文件

### 1. 核心概念與目的

對於深空攝影愛好者與智慧望遠鏡使用者（例如操作 ZWO Seestar 系列等設備）而言，月光與太陽曙暮光是造成背景天空（Sky Glow）惡化的主要因素。傳統工具大多依賴固定的時間緩衝，無法反應天體運行的幾何動態。

本工具將排程邏輯從「僵固的時間緩衝」轉化為「基於太陽 $-18^\circ$ 天文曙暮光、動態月相照亮比例與地平線下方高度角的物理閾值計算」，並採用**日落至翌日日出的單一連續觀測夜時間軸**，跨越午夜而不中斷，方便在手機與桌面裝置上檢視整晚的天文事件與干擾時段。使用者可藉此計算不同地點、不同月相下，月球離開地平線以下的幾何安全時間範圍，並搭配內建的深空拍攝目標庫與智慧排序，快速挑選出當晚最佳的拍攝目標。

### 2. 計算方法與數學公式

本工具基於高精度的 **Astronomy Engine** 天文計算庫，核心運算邏輯與科學解釋如下：

* **月相照明百分比 ($P$)：**
透過 `Astronomy.MoonPhase(astroTime)` 計算當下的月相相位角 ($\theta$)，進而換算精確的受光照亮比例 ($P$)：

$$P = \frac{1 - \cos(\theta \times \frac{\pi}{180})}{2} \times 100$$

* **動態高度閾值 ($\text{Threshold}_{\text{alt}}$)：**
閾值會隨著月相百分比 $P$ 與使用者自訂的滿月基準高度角（$\text{Threshold}_{\text{base}}$）進行動態線性縮放：

$$\text{Threshold}_{\text{alt}} = -\left(\text{Threshold}_{\text{base}} \times \frac{P}{100.0}\right)$$

* **器材適配分數（0–100）：**
$$\text{Fit} = \min\left(100,\ \frac{\sqrt{\text{major} \times \text{minor}}}{0.25 \times \text{FOV}_{\text{short}}} \times 100\right)$$
$\text{major}$、$\text{minor}$ 為目標角尺寸長短軸（角分），$\text{FOV}_{\text{short}}$ 為所選器材（Seestar S30 Pro、S50 Pro 或三組自訂器材）視野短邊。大型目標不會因無法單張入框而被扣分（可用拼接拍攝）；無尺寸資料的目標顯示為「未知」。

* **智慧排程評分公式（可選擇優先次序）：**
  * 目標觀測條件最佳（預設）：$\text{Score} = \text{Alt} \times 1.5 + \min(\text{Sep}_{\text{eff}}, 90) \times 0.5$
  * 月光影響最低：$\text{Score} = \text{Sep}_{\text{eff}} \times 1.5 + \text{Alt} \times 0.5$
  * 器材適配優先：$\text{Score} = \text{Fit} \times 1.2 + \text{Alt} \times 0.35 + \min(\text{Sep}_{\text{eff}}, 90) \times 0.25$（尺寸未知時以 $\text{Fit} = 45$ 計）

  $\text{Sep}_{\text{eff}}$ 在該時段中點月亮高於動態高度閾值時等於月角距，月亮低於閾值時取 $180^\circ$（月亮很低時夾角不影響品質）。目標在時段開始、中間、結束三個時刻都必須高於所屬方位的地平線限制。針對已排入過序列的目標施加 $-25$ 分重複懲罰，確保多樣性。

* **時間顯示一致性：** 事件時間顯示到「分」（秒數捨去），所有顯示的時間差（例如日落至完全入夜、月出至干擾開始）都以畫面上的分鐘相減，確保畫面數字互相吻合。

### 3. 主要功能特色

* **載入即自動計算**：開啟網頁時即自動執行背景計算；另可在「備份與進階診斷」中的「診斷與修復」區手動重新計算，並提供非阻斷式錯誤提示與備用 CDN (unpkg) 載入機制。
* **動態月光與曙暮光評估**：精準計算太陽低於 $-18^\circ$ 的全黑天文夜時段，並依據動態月相門檻及滿月干擾高度角下拉選單（$-6^\circ$、$-8^\circ$、$-10^\circ$、$-12^\circ$、$-14^\circ$、$-16^\circ$、$-18^\circ$；預設 $-12^\circ$）評估月光干擾。
* **單一連續觀測夜時間軸**：從日落連續呈現至翌日日出，跨越午夜不中斷，標示曙暮光、月光干擾與天文事件，並呈現**深空拍攝安全窗（Safe Window）**。
* **響應式跨裝置版面**：螢幕寬度達 $1000\text{px}$ 時自動切換為雙欄排版（最大寬度放寬至 $1480\text{px} \sim 1760\text{px}$），手機與窄視窗維繫單欄精簡佈局。
* **目標管理與動態排序**：內建經典深空目標庫，支援自訂目標（上限 500 個）、窄頻/全光譜標籤及角尺寸資訊。
* **智慧拍攝排程器**：依據目標高度、月角距權重與防重複機制生成每小時拍攝優先序列；參數變更時會標示舊排程「參數已變更／待更新」，需由使用者手動重新生成。
* **自訂地平線與對稱排版**：8 個方位滑桿，正方向（北、東、南、西）數值置左，複合方向（東北、東南、西南、西北）數值置右。
* **觀察地點與地平線設定置頂**：可收合的「📂 觀察地點與地平線設定」位於「選擇常用觀察地點」上方，第一次使用即可直接新增自己的地點（名稱、緯度、經度、海拔、8 個方位地平線），最多儲存 10 個地點，無須捲到頁面底部。
* **固定觀察地點 UTC 時差**：所有輸入與顯示時間均依你設定的觀察地點時差（小時及 15 分鐘級距）計算，不受裝置時區影響。
* **拍攝器材設定**：內建 Seestar S30 Pro、Seestar S50 Pro，並可自訂三組器材（焦距與感光元件尺寸），即時顯示視野與器材適配分數，亦可依目標角尺寸篩選。
* **內建診斷工具**：Debug Log（複製／匯出 TXT／清除）、手動「重新計算」及「重設（恢復預設值）」收納於可收合的「資料備份與檔案交換，進階診斷」。
* **多語言與深淺色模式**：支援繁體中文、簡體中文及英文，並自動配合淺色與深色顯示。
* **資料備份與交換**：支援完整設定的 JSON 備份還原，以及目標清單的 CSV 匯入匯出。

### 4. 版本歷史 (Changelog)

* **v0.6.0 (2026-11)**：發佈至 GitHub 後的首個版本。將「天文台地點」用語統一改為「觀察地點」（簡體「观察地点」、英文 Observation Site），因拍攝地點不一定是天文台；將「📂 觀察地點與地平線設定」移至頁面最上方（「選擇常用觀察地點」之前），第一次使用的人可直接新增自己的地點（名稱、經緯度、海拔、8 個方位地平線），無須捲到頁面底部；同步程式內版本號（網頁標題、頁首、JSON 備份及診斷紀錄），原先內部仍顯示 0.5.21；移除改回下拉選單後殘留的滑桿數字元素；修正 v0.5.22 版本歷史中重複跳脫的角度符號。
* **v0.5.22 (2026-11)**：將滿月干擾高度角由滑桿改為下拉選單（$-6^\circ$ 至 $-18^\circ$，每 $2^\circ$ 一級，預設 $-12^\circ$）；觀測夜改為日落至翌日日出的單一連續時間軸；將「重新計算」移至「診斷與修復」區；在評估報告標題旁新增可點擊的警告圖示，提供故障排除提示；將「重設（恢復預設值）」移至備份與進階診斷區最底部，加入確認提示及事前備份提醒；調整器材適配說明，大型深空目標可透過多張拼接拍攝，不會僅因無法單張入框而受到不合理限制。
* **v0.5.21 (2026-11)**：移除舊有手動「計算評估時段」按鈕，改為頁面載入後自動背景計算；新增非阻斷式狀態欄與「🔄 重新計算」重試按鈕；加入天文函式庫備用載入點 (unpkg)；清理三語字典重複鍵，修復頁尾 GitHub Issues 訊息與連結。
* **v0.5.20 (2026-11)**：將滿月干擾角度改為帶有即時數字顯示的滑桿介面（範圍 $-18^\circ \sim -6^\circ$，預設 $-12^\circ$）；觀測夜時間軸標題即時同步滿月干擾設定值；統一月相與高度閾值為整數顯示；修正時間差計算方式，直接採用畫面上顯示的時分相減，確保時間差與畫面完全吻合。
* **v0.5.18 - v0.5.19 (2026-11)**：大幅重排手機與電腦的響應式網頁版面（支援大螢幕雙欄自動對齊與視窗縮放自動重排 Timeline）；優化多個深空目標的名稱、角尺寸與赤經赤緯座標（如新增斯蒂芬五重星系 HCG 92 與 The Box HCG 61 等）；引入固定 UTC offset 時差機制；升級標籤自動排版引擎與器材適配排序。
* **v0.5.16 - v0.5.17 (2026-11)**：重新重排介面順序；新增可收合的「月光與太陽曙暮光評估」詳細報告；時間軸標題更名為「🌌 觀測夜時間軸」，簡化縮寫為「曙 / Twi」；新增排程過期「參數已變更」提醒。
* **v0.4.27 (2026-10)**：調整 8 個自訂地平線方位的數值排版：正方向（北 N、東 E、南 S、西 W）的角度數目字置於滑桿左側；複合方向（東北 NE、東南 SE、西南 SW、西北 NW）的角度數目字置於滑桿右側，呈現左右交錯對稱。
* **v0.4.26 (2026-10)**：將 8 個方位的地平線角度數值統一移至滑桿左側並調整對齊方式。
* **v0.4.5 (2026-10)**：動態首字母篩選支援中文開頭目標（自動抓取中文首字作為分類），並將『所有』選項本地化。
* **v0.3.45 (2026-10)**：徹底修復時間軸刻度分類邏輯，將完全入夜時刻與天光初現時刻自 `twilightSecs` 中獨立，確保上下半場時間軸上的完全入夜與天光初現時間文字及垂直指引線 100% 精準顯示為醒目綠色（#10b981）。
* **v0.3.44 (2026-10)**：改用 `kind` 屬性直接綁定天文事件類型，解決毫秒級時間戳記比對誤差。
* **v0.3.43 (2026-10)**：引入 3 秒內誤差容許區間，優化事件軸刻度分類。
* **v0.3.42 (2026-10)**：升級時間軸排版引擎，引入高質感漸層色塊與上方獨立「月光干擾」標籤盒，時間刻度具備防重疊自動錯開機制。
* **v0.3.37 - v0.3.41 (2026-10)**：新增拍攝目標 JSON 備份、CSV 匯入/匯出功能。
* **v0.3.30 - v0.3.36 (2026-09 - 2026-10)**：擴充經典深空目標庫，實現目標-月亮夾角計算與高於地平線 15° 的可見窗口評估。
* **v0.3.0 - v0.3.29 (2026-08 - 2026-09)**：建立離線單一檔案 HTML 架構，整合 Astronomy Engine 計算核心，實作經度推算當地時區。
* **v0.1.0 - v0.2.9 (2026-08)**：專案初始化雛形，建立觀測夜時間軸排版與基礎日落/日出事件搜尋。