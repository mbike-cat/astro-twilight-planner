# 🌙 Deep-Sky Imaging Schedule & Dynamic Moon/Twilight Calculator

# 深空拍攝排程與動態月光/曙暮光計算器

[English Version](#-english-documentation) | [繁體中文版本](#-繁體中文說明文件)

---

## 🇬🇧 English Documentation

### 1. Concept & Purpose

For astrophotographers and automated telescope users (such as those using smart telescopes like the ZWO Seestar series), moonlight and solar twilight are primary sources of sky glow degradation. Traditional tools rely on fixed time-based buffers, which fail to capture the dynamic geometric nature of lunar and solar interference.

This tool shifts the paradigm from a rigid time buffer to an **altitude-based threshold calculation** and provides a **split-timeline visualization** (First Half: Sunset to Midnight; Second Half: Midnight to Sunrise) tailored for mobile and desktop screens. It helps users calculate precise safe imaging windows based on different locations, solar astronomical twilight ($\text{Sun at } -18^\circ$), dynamic moon phases, and moon-target separation angles, while featuring a built-in target library with smart sorting to identify the best astrophotography targets.

### 2. Calculation Methodology & Equations

The application is powered by the high-precision **Astronomy Engine** library and follows these mathematical steps:

* **Moon Phase Illumination ($P$):**
The exact phase angle ($\theta$) of the Moon is computed using `Astronomy.MoonPhase(astroTime)`. The illuminated fraction percentage ($P$) is derived as:

$$P = \frac{1 - \cos(\theta \times \frac{\pi}{180})}{2} \times 100$$

* **Dynamic Altitude Threshold ($\text{Threshold}_{\text{alt}}$):**
Instead of a fixed cutoff, the altitude threshold scales linearly based on the current illumination percentage $P$ and the user-defined full moon altitude threshold ($\text{Threshold}_{\text{base}}$):

$$\text{Threshold}_{\text{alt}} = -\left(\text{Threshold}_{\text{base}} \times \frac{P}{100.0}\right)$$

* **Imaging Sequence Scoring Formula:**
$$\text{Score} = \text{Alt} \times 1.5 + \min(\text{Sep}, 90) \times 0.5$$
With a $-25$ penalty applied to targets already scheduled in previous hourly slots to promote target diversity.

### 3. Key Features

* **Auto-Calculate on Page Load**: Evaluates observation conditions immediately upon page load without requiring manual button triggers, complete with non-blocking error status handling and secondary library fallbacks.
* **Dynamic Moon & Sun Twilight Assessment**: Computes exact windows where the sun is below $-18^\circ$ and evaluates moonlight interference based on dynamic phase thresholds and slider adjustments ($-18^\circ \sim -6^\circ$).
* **Split-Timeline Visualization**: Dual timeline bars (Sunset to Midnight, Midnight to Sunrise) highlighting twilight, moon interference, and pristine **Safe Deep-Sky Windows**.
* **Responsive Desktop & Mobile Layout**: Automatically expands up to 1480px width (1760px on wide screens) and formats into a two-column layout on screens wider than 1000px, while remaining cleanly structured for narrow mobile screens.
* **Smart Target Management & Sorting**: Includes a classic deep-sky target library, custom target adding (up to 500), optical filter tagging (narrowband vs. broadband), and angular size metrics.
* **Smart Imaging Sequence Planner**: Automatically generates an hourly prioritized observation schedule based on altitude, moon separation, and target uniqueness with real-time parameter change notifications.
* **Custom Horizon Profile with Asymmetric Layout**: 8 directional sliders with cardinal directions (N, E, S, W) displaying numbers on the left and compound directions (NE, SE, SW, NW) on the right.
* **Data Backup & Exchange**: Full JSON backup/restore and CSV target list import/export.

### 4. Changelog

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
* **v0.1.0 - v0.2.9 (2026-08)**: Initial prototyping, basic sunset/sunrise search, and split-timeline layout establishment.

---

## 🇭🇰 繁體中文說明文件

### 1. 核心概念與目的

對於深空攝影愛好者與智慧望遠鏡使用者（例如操作 ZWO Seestar 系列等設備）而言，月光與太陽曙暮光是造成背景天空（Sky Glow）惡化的主要因素。傳統工具大多依賴固定的時間緩衝，無法反應天體運行的幾何動態。

本工具將排程邏輯從「僵固的時間緩衝」轉化為「基於太陽 $-18^\circ$ 天文曙暮光、動態月相照亮比例與地平線下方高度角的物理閾值計算」，並採用**「上下雙條時間軸」（上半場：日落至午夜 00:00；下半場：午夜 00:00 至日出），完美適應手機與桌面裝置的檢視需求。使用者可藉此計算不同地點、不同月相下，月球離開地平線以下的幾何安全時間範圍，並搭配內建的深空拍攝目標庫與智慧排序，快速挑選出當晚最佳的拍攝目標。

### 2. 計算方法與數學公式

本工具基於高精度的 **Astronomy Engine** 天文計算庫，核心運算邏輯與科學解釋如下：

* **月相照明百分比 ($P$)：**
透過 `Astronomy.MoonPhase(astroTime)` 計算當下的月相相位角 ($\theta$)，進而換算精確的受光照亮比例 ($P$)：

$$P = \frac{1 - \cos(\theta \times \frac{\pi}{180})}{2} \times 100$$

* **動態高度閾值 ($\text{Threshold}_{\text{alt}}$)：**
閾值會隨著月相百分比 $P$ 與使用者自訂的滿月基準高度角（$\text{Threshold}_{\text{base}}$）進行動態線性縮放：

$$\text{Threshold}_{\text{alt}} = -\left(\text{Threshold}_{\text{base}} \times \frac{P}{100.0}\right)$$

* **智慧排程評分公式：**
$$\text{Score} = \text{Alt} \times 1.5 + \min(\text{Sep}, 90) \times 0.5$$
針對已排入過序列的目標施加 $-25$ 分重複懲罰，確保多樣性。

### 3. 主要功能特色

* **載入即自動計算**：開啟網頁時即自動執行背景計算，免除手動點擊按鈕，並具備非阻斷式錯誤提示列與備用 CDN (unpkg) 自動載入機制。
* **動態月光與曙暮光評估**：精準計算太陽低於 $-18^\circ$ 的全黑天文夜時段，並依據動態月相門檻與滑桿（$-18^\circ \sim -6^\circ$）避開月光干擾。
* **上下雙條時間軸視覺化**：直覺呈現日落、午夜、日出與各個干擾區段，以純黑色高亮顯示**深空拍攝安全窗（Safe Window）**。
* **響應式跨裝置版面**：螢幕寬度達 $1000\text{px}$ 時自動切換為雙欄排版（最大寬度放寬至 $1480\text{px} \sim 1760\text{px}$），手機與窄視窗維繫單欄精簡佈局。
* **目標管理與動態排序**：內建經典深空目標庫，支援自訂目標（上限 500 個）、窄頻/全光譜標籤及角尺寸資訊。
* **智慧拍攝排程器**：依據高度、月亮夾角權重與防重複機制自動生成每小時最佳拍攝優先序列，並於參數變更時自動提示舊排程已過期。
* **自訂地平線與對稱排版**：8 個方位滑桿，正方向（北、東、南、西）數值置左，複合方向（東北、東南、西南、西北）數值置右。
* **資料備份與交換**：支援完整設定的 JSON 備份還原，以及目標清單的 CSV 匯入匯出。

### 4. 版本歷史 (Changelog)

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
* **v0.1.0 - v0.2.9 (2026-08)**：專案初始化雛形，建立雙時間軸排版與基礎日落/日出事件搜尋。