# 🌙 Deep-Sky Imaging Schedule & Dynamic Moon/Twilight Calculator - User Manual
# 深空拍攝排程與動態月光/曙暮光計算器 - 使用說明書

[English Version](#-english-user-manual) | [繁體中文版本](#-繁體中文使用說明書)

---

## 🇬🇧 English User Manual

This manual provides a detailed guide on the core calculation methodologies, interface components, button functions, data backup/exchange (JSON / CSV), and the imaging sequence scoring and sorting mechanisms.

---

### 1. Core Concepts & Calculation Methodology

1. **Dynamic Moon Phase & Illumination Fraction ($P$)**
   * The tool uses the high-precision **Astronomy Engine** to compute the exact lunar phase angle ($\theta$).
   * Illumination percentage formula:
     $$P = \frac{1 - \cos(\theta \times \frac{\pi}{180})}{2} \times 100$$

2. **Dynamic Altitude Threshold ($\text{Threshold}_{\text{alt}}$)**
   * Traditional tools rely on fixed time buffers. This tool dynamically scales the altitude threshold linearly based on the moon illumination $P$ and the user-defined full moon base altitude threshold ($\text{Threshold}_{\text{base}}$, default $12^\circ$):
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
   * **Sorting Options**: Users can switch between sorting modes (Altitude Priority, Moon Separation Priority, and Telescope Fit Priority) via the dropdown menu.

---

### 2. Interface Components & Button Operation Guide

#### 1. Observatory & Location Settings
* **Select Observatory**: Built-in presets (Hebei Wangxiang Observatory, Hong Kong Tai Mo Shan, Taiwan Guanshan Township). Click to instantly load coordinates and elevation.
* **Observatory Name / Lat / Lon / Elevation**: Manual input fields. 
* **🕒 Site UTC Offset**: Manually configure the fixed UTC time offset to ensure all display times match the observation site's local time accurately.
* **Full Moon Altitude Threshold (°)**: Slider for base threshold ($6^\circ \sim 18^\circ$).
* **⛰️ Custom Horizon Profile Sliders**: 8 directional sliders (North, Northeast, East, Southeast, South, Southwest, West, Northwest, $\pm 22.5^\circ$ each) to fine-tune minimum horizon altitude limits.
  * **Layout Alignment**: Primary cardinal directions (N, E, S, W) display their numerical values on the **left**, while compound directions (NE, SE, SW, NW) display their values on the **right** for clear symmetrical alignment.
* **💾 Save Observatory**: Saves current input parameters into preset locations.
* **🔄 Reset All**: Clears all local storage (`LocalStorage`) and resets default parameters.

#### 2. Target Management Library (📂 Toggle Target List)
Click to expand the advanced management panel:
* **Add / Update Target**: Enter Target Name, RA ($0 \sim 24$ hours), Dec ($-90^\circ \sim +90^\circ$), toggle narrowband filter compatibility, and click **➕ Add / Update Target** (supports up to 500 custom targets).
* **Target Deletion**: Both preset and custom target lists feature a **[Delete]** button to remove unwanted objects.
* **💾 Data Backup & Exchange**:
  * **📥 Export JSON Backup**: Packages custom targets, deleted defaults, and custom locations into a `.json` file.
  * **📤 Import JSON Backup**: Restores settings by uploading a `.json` backup file.
  * **📊 Export CSV Targets**: Exports all targets into a spreadsheet-compatible `.csv` format.
  * **📂 Import CSV Targets**: Batch imports target lists from a `.csv` file.

#### 3. Query & Calculation Trigger
* **Observation Date & Time**: Select the target night's timestamp.
* **Calculate Imaging Window**: Triggers the astronomical engine evaluation and displays the **📊 Assessment Report**.

#### 4. Results & Target Filtering
* **Split-Timeline Visualization**: First Half (Sunset to Midnight 00:00) and Second Half (Midnight 00:00 to Sunrise). Color-coded blocks distinguish safe windows, moonlight interference, and twilight, topped with an advanced multi-lane collision-free label layout.
* **Target Filtering Dropdowns**:
  * **Filter Targets by Optical Type**: Choose between "Narrowband & Broadband", "Narrowband Only", or "Broadband Only".
  * **🔠 Filter by Prefix (Dynamic)**: Automatically detects all target initials (supports English prefixes like M, NGC, IC and Chinese first characters). Select "All" or a specific prefix for instant filtering.
* **📊 Target Sorting Mode**:
  1. Target Name (A to Z)
  2. Moon Separation (Descending)
  3. Rise Time (Earliest First)
  4. Telescope Fit Priority [Default]

---
---

## 🇹🇼 繁體中文使用說明書

本說明書詳細解釋本工具的核心計算方法、各介面元件與按鈕的操作方式、資料備份與交換（JSON / CSV）的完整流程，以及智慧排程的評分機制。

---

### 1. 核心功能與計算方法原理

1. **動態月相與照明比例 ($P$)**
   * 工具透過高精度 **Astronomy Engine** 計算當下月相相位角 ($\theta$)。
   * 照亮百分比公式：
     $$P = \frac{1 - \cos(\theta \times \frac{\pi}{180})}{2} \times 100$$

2. **動態高度閾值計算 ($\text{Threshold}_{\text{alt}}$)**
   * 傳統軟體採用固定時間緩衝，本工具改為根據月相 $P$ 與自訂滿月基準高度角（$\text{Threshold}_{\text{base}}$，預設 $12^\circ$）進行線性縮放：
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

---

### 2. 介面元件與按鈕操作指南

#### 1. 地點與時差設定區
* **選擇常用天文台地點**：內建河北萬象天文台、香港大帽山、台灣台東關山鎮等預設地點，點擊即可一鍵載入經緯度與海拔。
* **地點名稱 / 緯度 / 經度 / 海拔**：可手動輸入。
* **🕒 天文台當地時差 (UTC offset)**：手動設定觀測地固定的時差，確保所有顯示時間與事件精準對應當地時間。
* **滿月下方高度角 (°)**：滑桿調整基準高度（$6^\circ \sim 18^\circ$）。
* **⛰️ 自訂地平線障礙物微調**：8 個方位滑桿，正方向（北、東、南、西）數值置左，複合方向（東北、東南、西南、西北）數值置右。
* **💾 儲存當前地點**：將目前輸入的參數儲存為常用地點。
* **🔄 重設 (Reset)**：清除所有本機儲存並重設參數。

#### 2. 拍攝目標管理庫 (📂 展開/收合拍攝目標清單)
點擊按鈕可展開進階管理介面：
* **新增/更新拍攝目標**：輸入目標名稱、赤經 RA、赤緯 Dec，並可勾選窄頻濾鏡標記，點擊按鈕儲存（最多支援 500 個自訂目標）。
* **資料備份與檔案交換**：支援 JSON 完整設定備份還原，以及 CSV 目標清單的匯入匯出。

#### 3. 查詢與計算觸發
* **查詢當晚日期與時間**：選擇欲查詢的夜晚時間點。
* **計算評估時段**：點擊後即刻觸發天文引擎運算，並展開評估結果報告。

#### 4. 評估結果與目標篩選
* **上下雙條時間軸**：上半場與下半場視覺化，採用多層智慧防重疊排版引擎，精準顯示事件時刻。
* **目標篩選與排列次序**：支援窄頻/全光譜篩選、動態首字母篩選、角尺寸篩選及器材適配優先排序。
