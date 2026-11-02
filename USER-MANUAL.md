# 🌙 Deep-Sky Imaging Schedule & Dynamic Moon/Twilight Calculator - User Manual
# 深空拍攝排程與動態月光/曙暮光計算器 - 使用說明書

[English Version](#-english-user-manual) | [繁體中文版本](#-繁體中文使用說明書)

---

## 🇬🇧 English User Manual

This manual provides a detailed guide on the core calculation methodologies, interface components, operational workflows, data backup/exchange (JSON / CSV), and the imaging sequence scoring and sorting mechanisms.

---

### 1. Core Concepts & Calculation Methodology

1. **Dynamic Moon Phase & Illumination Fraction ($P$)**
   * The tool uses the high-precision **Astronomy Engine** to compute the exact lunar phase angle ($\theta$).
   * Illumination percentage formula:
     $$P = \frac{1 - \cos(\theta \times \frac{\pi}{180})}{2} \times 100$$

2. **Dynamic Altitude Threshold ($\text{Threshold}_{\text{alt}}$)**
   * Traditional tools rely on fixed time buffers. This tool dynamically scales the altitude threshold linearly based on moon illumination $P$ and the user-defined full moon base altitude threshold ($\text{Threshold}_{\text{base}}$, default $12^\circ$, configurable from $-18^\circ$ to $-6^\circ$ via slider):
     $$\text{Threshold}_{\text{alt}} = -\left(\text{Threshold}_{\text{base}} \times \frac{P}{100.0}\right)$$
   * Smaller moon phases (e.g., crescent moons) result in lower interference thresholds, extending usable imaging windows.

3. **Solar Astronomical Twilight (-18°)**
   * Accurately computes the time delta between sunset and astronomical night ($\text{Sun at } -18^\circ$, Astro-Dark), as well as astronomical dawn ($\text{Sun at } -18^\circ$, Astro-Dawn) to sunrise, avoiding civil and nautical twilight sky glow.

4. **Imaging Sequence Priority & Scoring Mechanism**
   * The **Imaging Sequence Planner** generates an optimal hourly shooting schedule during astronomical night.
   * **Default Scoring Formula (Altitude Priority)**:
     $$\text{Score} = \text{Alt} \times 1.5 + \min(\text{Sep}, 90) \times 0.5$$
     Altitude is weighted higher ($1.5\times$) than moon separation ($0.5\times$). Therefore, targets with exceptionally high altitude may occasionally precede targets with larger moon separation angles.
   * **Duplicate Target Penalty**: To prevent the same target from dominating consecutive slots, previously scheduled targets receive a $-25$ score penalty in subsequent slots.
   * **Outdated Schedule Detection**: Modifying key parameters (date, timezone, telescope gear, targets, location, or full moon angle) marks existing schedules with a warning banner ("Parameters Changed") until manually regenerated.

---

### 2. Interface Components & Operation Guide

#### 1. Primary Controls & Location Settings
* **Select Observatory**: Built-in presets (Hebei Wangxiang Observatory, Hong Kong Tai Mo Shan, Taiwan Guanshan Township). Click to instantly load coordinates and elevation.
* **Observatory Name / Lat / Lon / Elevation**: Manual input fields. 
* **🕒 Site UTC Offset**: Manually configure the fixed UTC time offset to ensure all display times match the observation site's local time accurately.
* **Full Moon Below-Horizon Altitude Threshold (°)**: Interactive slider ($ -18^\circ \sim -6^\circ $, $1^\circ$ steps, default $-12^\circ$). Updates numbers and timeline titles in real time.
* **Observation Date & Time**: Select the target night timestamp (defaults to current date at observation location). Automatically triggers calculation on load or parameter changes.

#### 2. Assessment Report & Timeline
* **Auto-Calculation Flow**: Automatically executes calculations on page startup with a non-blocking UI state. Displays a status banner with a `🔄 Re-calculate` button if parameter errors or library loading delays occur.
* **🌌 Observing-Night Timeline**: Split-timeline layout showing First Half (Sunset to Midnight 00:00) and Second Half (Midnight 00:00 to Sunrise). Features auto-collision prevention time ticks and color-coded blocks for twilight ("Twi" / "曙") and moonlight interference.
* **📂 Collapsible Moonlight & Solar Twilight Detailed Assessment**: Expandable section detailing dynamic phase angles, altitude thresholds, and precise moonrise/moonset offsets.

#### 3. Equipment & Target Management Library
* **📂 Imaging Equipment (Collapsible)**: Configure telescope optics, focal length, sensor dimensions, FOV calculations, and target framing compatibility.
* **🎯 Target Filtering & Result List (Always Visible)**:
  * Filter by Optical Filter (Narrowband / Broadband).
  * Filter by Dynamic Initial Character (English or Chinese).
  * Filter by Angular Size ($^\prime \times ^\prime$).
  * Sort by Name, Moon Separation, Rise Time, or Telescope Fit Priority.
* **📂 Target Management List (Collapsible)**:
  * **Add / Update Target**: Enter Target Name, RA ($0 \sim 24\text{h}$), Dec ($-90^\circ \sim +90^\circ$), toggle narrowband filter compatibility, and manage up to 500 targets.
  * **💾 Data Backup & Exchange**: Export/Import JSON backup files or CSV target lists.

#### 4. Smart Imaging Sequence & Observatory Horizon
* **🌟 Smart Imaging Sequence & Recommendations**: Generates hourly prioritized target recommendations. Displays dynamic "Parameters Changed" notices when underlying settings are modified.
* **📂 Observatory Horizon Profile (Collapsible)**: 8 directional sliders (N, NE, E, SE, S, SW, W, NW) to fine-tune local horizon obstruction profiles.
* **📂 Data Backup, Exchange & Advanced Diagnostics (Collapsible)**: Provides JSON/CSV actions alongside second-level collapsed debug logs (`AstroLog`).

---
---

## 🇭🇰 繁體中文使用說明書

本說明書詳細解釋本工具的核心計算方法、各介面元件與按鈕的操作方式、資料備份與交換（JSON / CSV）的完整流程，以及智慧排程的評分機制。

---

### 1. 核心功能與計算方法原理

1. **動態月相與照明比例 ($P$)**
   * 工具透過高精度 **Astronomy Engine** 計算當下月相相位角 ($\theta$)。
   * 照亮百分比公式：
     $$P = \frac{1 - \cos(\theta \times \frac{\pi}{180})}{2} \times 100$$

2. **動態高度閾值計算 ($\text{Threshold}_{\text{alt}}$)**
   * 傳統軟體採用固定時間緩衝，本工具改為根據月相 $P$ 與自訂滿月基準高度角（$\text{Threshold}_{\text{base}}$，滑桿設定 $-18^\circ \sim -6^\circ$，預設 $-12^\circ$）進行線性縮放：
     $$\text{Threshold}_{\text{alt}} = -\left(\text{Threshold}_{\text{base}} \times \frac{P}{100.0}\right)$$
   * 當月相越小（如娥眉月），月光干擾閾值越低，可拉長有效拍攝時間。

3. **太陽天文曙暮光 (-18°)**
   * 精確計算日落至太陽低於 $-18^\circ$（完全入夜，Astro-Dark）的時間差，以及天光初現（Astro-Dawn）至日出的時間，避開民用與航海曙暮光的強烈散射光。

4. **智慧拍攝排程與評分機制**
   * **智慧拍攝排程器（Imaging Sequence Planner）** 會在完全入夜期間自動為每個小時區塊推薦最佳拍攝序列。
   * **預設評分公式（最高高度優先 Alt Priority）**：
     $$\text{Score} = \text{Alt} \times 1.5 + \min(\text{Sep}, 90) \times 0.5$$
     系統賦予目標高度（Alt）較高權重（$1.5\times$），月亮夾角（Sep）佔 $0.5\times$。
   * **防重複扣分機制**：若同一個目標在先前時段已被排入序列，後續時段若再次評估，其總分會被扣減 $25$ 分，以鼓勵系統輪流推薦不同天體。
   * **排程過期提示**：更改日期、地點、時差、滿月角度、目標或器材時，舊排程會保留並顯示「參數已變更」提示，按「生成」後更新。

---

### 2. 介面元件與操作指南

#### 1. 主要操作與地點設定區
* **選擇常用天文台地點**：內建河北萬象天文台、香港大帽山、台灣台東關山鎮等預設地點，點擊即可一鍵載入經緯度與海拔。
* **地點名稱 / 緯度 / 經度 / 海拔**：可手動輸入。
* **🕒 天文台當地時差 (UTC offset)**：手動設定觀測地固定的時差，確保所有顯示時間與事件精準對應當地時間。
* **滿月干擾於地平線下的高度角 (°)**：滑桿設定（$-18^\circ \sim -6^\circ$，步階 $1^\circ$，預設 $-12^\circ$），即時更新標題與計算。
* **查詢當晚日期與時間**：選擇欲查詢的夜晚時間點（預設為觀測地「現在」）。

#### 2. 評估結果報告與時間軸
* **載入即自動計算**：頁面開啟時自動執行計算，若參數錯誤或函式庫載入延遲，報告上方會顯示紅色狀態列與「🔄 重新計算」按鈕。
* **🌌 觀測夜時間軸**：採用上下半場雙時間軸（日落至 00:00 / 00:00 至日出），具備防重疊刻度排版與「曙 / Twi」縮寫標籤。
* **📂 展開/收合 月光與太陽曙暮光評估**：可收合區塊，展開後可查閱詳細月相照亮比例、動態高度閾值與月出月落時差。

#### 3. 拍攝器材與目標區
* **📂 拍攝器材 (預設收合)**：可設定焦距、感光元件尺寸、視野範圍資訊與器材適配判斷。
* **🎯 目標篩選與結果清單 (常駐不收合)**：
  * 支援全光譜/窄頻篩選、動態首字母篩選、角尺寸篩選及 4 種排序方式。
  * 顯示格式包含目標名稱、窄頻標籤、月角距（四捨五入至整數，如 `月角距 34°`）及角尺寸（如 `60′ × 40′`）。
* **📂 拍攝目標管理清單 (預設收合)**：
  * **新增/更新拍攝目標**：輸入名稱、赤經 RA、赤緯 Dec 與窄頻標籤（上限 500 個）。
  * **💾 資料備份與檔案交換**：支援 JSON 完整設定備份還原與 CSV 目標清單匯入匯出。

#### 4. 排程、地平線與診斷
* **🌟 智慧拍攝排程與建議序列**：獨立卡片，點擊手動生成每小時拍攝序列，參數修改後會顯示「參數已變更」提醒。
* **📂 天文台地點與地平線設定 (預設收合)**：8 個方位滑桿微調障礙物高度角。
* **📂 資料備份與檔案交換,進階診斷 (預設收合)**：備份工具位居上方，第二層收合提供 Debug Log 診斷紀錄。