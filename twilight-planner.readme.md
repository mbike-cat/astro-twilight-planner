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

* **Dynamic Moon & Sun Twilight Assessment**: Computes exact windows where the sun is below $-18^\circ$ and evaluates moonlight interference based on dynamic phase thresholds.
* **Split-Timeline Visualization**: Dual timeline bars (Sunset to Midnight, Midnight to Sunrise) highlighting twilight, moon interference, and pristine **Safe Deep-Sky Windows**.
* **Smart Target Management & Sorting**: Includes a classic deep-sky target library, custom target adding (up to 500), and dynamic sorting options.
* **Smart Imaging Sequence Planner**: Automatically generates an hourly prioritized observation schedule based on altitude, moon separation, and target uniqueness.
* **Custom Horizon Profile with Asymmetric Layout**: 8 directional sliders with cardinal directions (N, E, S, W) displaying numbers on the left and compound directions (NE, SE, SW, NW) on the right.
* **Data Backup & Exchange**: Full JSON backup/restore and CSV target list import/export.

### 4. Changelog

* **v0.5.19 (2026-11)**：引入固定 UTC offset 時差機制，不依賴瀏覽器或裝置時區自動推算；升級標籤自動排版引擎，支援多層交錯防重疊；增強器材適配與評分排序功能。
* **v0.4.27 (2026-10)**：調整 8 個自訂地平線方位的數值排版：正方向（北 N、東 E、南 S、西 W）的角度數目字置於滑桿左側；複合方向（東北 NE、東南 SE、西南 SW、西北 NW）的角度數目字置於滑桿右側，呈現左右交錯對稱。
* **v0.4.26 (2026-10)**：將 8 個方位的地平線角度數值統一移至滑桿左側並調整對齊方式。
* **v0.4.5 (2026-10)**：動態首字母篩選支援中文開頭目標（自動抓取中文首字作為分類），並將『所有』選項本地化。
* **v0.3.45 (2026-10)**: Fixed timeline tick classification logic by separating `darkDawnSecs` from `twilightSecs`, ensuring complete precision where astronomical night-end (astronomical dawn) and night-start (astronomical dusk) timestamps and vertical guide lines on timelines strictly render in vivid green (#10b981).
* **v0.3.44 (2026-10)**: Bound `kind` properties directly to astronomical events, resolving millisecond timestamp matching discrepancies.
* **v0.3.43 (2026-10)**: Introduced a 3-second tolerance window for event axis tick classification.
* **v0.3.42 (2026-10)**: Upgraded timelines with professional gradient styling, independent top interference badges, and auto-collision prevention for time axes.
* **v0.3.37 - v0.3.41 (2026-10)**: Added JSON backup/restore and CSV target exchange functionality.
* **v0.3.30 - v0.3.36 (2026-09 - 2026-10)**: Expanded classic target library to 29 items, implemented angular separation calculations, and added visibility window evaluations for target altitudes $\ge 15^\circ$.
* **v0.3.0 - v0.3.29 (2026-08 - 2026-09)**: Developed offline single-file HTML client architecture, integrated Astronomy Engine, and implemented longitude-based local timezone calculations.
* **v0.1.0 - v0.2.9 (2026-08)**: Initial prototyping, basic sunset/sunrise search, and split-timeline layout establishment.

---

## 🇹🇼 繁體中文說明文件

### 1. 核心概念與目的

對於深空攝影愛好者與智慧望遠鏡使用者（例如操作 ZWO Seestar 系列等設備）而言，月光與太陽曙暮光是造成背景天空（Sky Glow）惡化的主要因素。傳統工具大多依賴固定的時間緩衝，無法反應天體運行的幾何動態。

本工具將排程邏輯從「僵固的時間緩衝」轉化為「基於太陽 $-18^\circ$ 天文曙暮光、動態月相照亮比例與地平線下方高度角的物理閾值計算」**，並採用**「上下雙條時間軸」（上半場：日落至午夜 00:00；下半場：午夜 00:00 至日出），完美適應手機與桌面裝置的檢視需求。使用者可藉此計算不同地點、不同月相下，月球離開地平線以下的幾何安全時間範圍，並搭配內建的深空拍攝目標庫與智慧排序，快速挑選出當晚最佳的拍攝目標。

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

* **動態月光與曙暮光評估**：精準計算太陽低於 $-18^\circ$ 的全黑天文夜時段，並依據動態月相門檻避開月光干擾。
* **上下雙條時間軸視覺化**：直覺呈現日落、午夜、日出與各個干擾區段，以純黑色高亮顯示**深空拍攝安全窗（Safe Window）**。
* **目標管理與動態排序**：內建經典深空目標庫，支援自訂目標（上限 500 個）。
* **智慧拍攝排程器**：依據高度、月亮夾角權重與防重複機制自動生成每小時最佳拍攝優先序列。
* **自訂地平線與對稱排版**：8 個方位滑桿，正方向（北、東、南、西）數值置左，複合方向（東北、東南、西南、西北）數值置右。
* **資料備份與交換**：支援完整設定的 JSON 備份還原，以及目標清單的 CSV 匯入匯出。

### 4. 版本歷史 (Changelog)

* **v0.5.19 (2026-11)**：引入固定 UTC offset 時差機制，不依賴瀏覽器或裝置時區自動推算；升級標籤自動排版引擎，支援多層交錯防重疊；增強器材適配與評分排序功能。
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
