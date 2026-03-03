# Taiwan Shelter Location Data Validation Project

> **專案名稱：** 台灣本島避難收容處所資料勘誤與核實  
> **Project Name:** Taiwan Mainland Shelter Location Data Validation  
> **建立日期：** 2026-03-03

---

## 📋 專案概述 | Project Overview

本專案針對台灣避難收容處所點位資料進行系統性的資料品質驗證，包含：
- ✅ 經緯度座標驗證（WGS84）
- ✅ 電話號碼格式驗證
- ✅ 台灣本島範圍篩選
- ✅ 離島資料分離

This project performs systematic data quality validation on Taiwan's emergency shelter location dataset, including:
- ✅ Coordinate validation (WGS84)
- ✅ Phone number format validation
- ✅ Taiwan mainland boundary filtering
- ✅ Offshore island data separation

---

## 📊 資料統計 | Data Statistics

| 項目 | 數量 | 百分比 |
|------|------|--------|
| 總資料筆數 | 5,973 | 100% |
| ✓ 有效資料 | 4,640 | 77.7% |
| ✗ 剔除資料 | 1,333 | 22.3% |

### 剔除資料細項 | Exclusion Breakdown
- 經緯度不符（離島/異常）：148 筆
- 電話格式不符：1,185 筆

---

## 📁 專案結構 | Project Structure

```
my_spatial_analysis_w1/
├── data/                                    # 原始資料目錄
│   └── 避難收容處所點位檔案v9.csv           # 來源資料
├── outputs/                                 # 分析結果目錄
│   ├── 避難收容處所_台灣本島有效資料.csv    # 有效資料（4,640筆）
│   ├── 避難收容處所_剔除資料.csv            # 剔除資料（1,333筆）
│   └── 資料驗證摘要報告.txt                 # 中文摘要報告
├── scripts/                                 # 腳本目錄
│   └── validate_shelter_coordinates.py      # 資料驗證主程式
├── AUDIT_REPORT.md                          # 完整審計報告（英文）
└── README.md                                # 本文件
```

---

## 🔍 驗證標準 | Validation Criteria

### 1️⃣ 經緯度範圍（WGS84）| Geographic Boundaries

```
台灣本島範圍：
  經度 Longitude: 120.0° < lon < 122.0°
  緯度 Latitude:  21.8° < lat < 25.5°
```

**排除區域：**
- 金門縣（Kinmen）：經度 ~118°
- 連江縣/馬祖（Matsu）：緯度 ~26°, 經度 ~119-120°
- 澎湖縣（Penghu）：經度 ~119°

### 2️⃣ 電話號碼格式 | Phone Number Format

**有效區碼 | Valid Area Codes:**
```
02 (台北/基隆)    03 (桃園/新竹/宜蘭)
04 (台中)         05 (嘉義/雲林)
06 (台南)         07 (高雄)
08 (屏東/台東)    09 (手機 Mobile)
```

**排除區碼 | Excluded Area Codes:**
```
082  - 金門 Kinmen
0826 - 金門 Kinmen
0836 - 馬祖 Matsu
```

**格式要求 | Format Requirements:**
- 手機 Mobile: `09XX-XXX-XXX` (10碼)
- 市話 Landline: `0X-XXXX-XXXX` (至少8碼含區碼)

---

## 🚀 使用方式 | Usage

### 環境需求 | Requirements

```bash
Python 3.7+
pandas
```

### 安裝依賴 | Install Dependencies

```bash
pip install pandas
```

### 執行驗證 | Run Validation

```bash
python scripts/validate_shelter_coordinates.py
```

### 輸出結果 | Output

執行後將在 `outputs/` 目錄生成：
1. `避難收容處所_台灣本島有效資料.csv` - 通過驗證的資料
2. `避難收容處所_剔除資料.csv` - 未通過驗證的資料
3. 終端機顯示詳細驗證報告

---

## 📈 主要發現 | Key Findings

### ✅ 資料品質優勢
- 經緯度完整性：99.8%（本島範圍內）
- 成功分離離島資料：148筆
- 清楚的座標邊界定義

### ⚠️ 需改進項目
- **電話號碼問題：** 19.8% 的資料缺少區碼或格式不符
- **座標異常：** 10+ 筆資料經度為 0.0（資料缺失）
- **緯度異常：** 部分桃園新屋區資料超出範圍

---

## 📝 審計報告 | Audit Report

完整的資料品質審計報告請參閱：
- **英文版：** [AUDIT_REPORT.md](AUDIT_REPORT.md)
- **中文版：** [outputs/資料驗證摘要報告.txt](outputs/資料驗證摘要報告.txt)

報告內容包含：
- ✓ 詳細驗證方法論
- ✓ 問題分類統計
- ✓ 範例資料展示
- ✓ 改進建議
- ✓ 稽核軌跡

---

## 🔧 驗證邏輯 | Validation Logic

### 兩階段驗證流程 | Two-Stage Process

```
階段 1：經緯度驗證
  輸入：5,973 筆
  ├─ 有效（本島範圍）：5,825 筆 → 進入階段2
  └─ 無效（離島/異常）：148 筆 → 剔除

階段 2：電話號碼驗證（僅本島資料）
  輸入：5,825 筆
  ├─ 有效（格式正確）：4,640 筆 → 最終有效資料
  └─ 無效（格式問題）：1,185 筆 → 剔除

最終輸出：
  ✓ 有效資料集：4,640 筆
  ✗ 剔除資料集：1,333 筆 (148 + 1,185)
```

---

## 💡 建議後續處理 | Recommendations

### 立即處理 | Immediate Actions
1. **電話號碼補正：** 聯繫相關單位補充缺少的區碼
2. **座標核實：** 重新地理編碼經度為 0.0 的資料
3. **格式標準化：** 建立統一的電話號碼輸入規範

### 長期改進 | Long-term Improvements
1. **建立離島資料集：** 為金門、馬祖、澎湖建立獨立資料庫
2. **定期驗證機制：** 每季執行資料品質稽核
3. **GIS 整合驗證：** 視覺化驗證座標準確性

---

## 📜 授權 | License

本專案僅用於教學與資料品質改善目的。

---

## 👤 作者 | Author

**建立者：** GIS 空間分析專案  
**課程：** 114-2 地理資訊系統  
**週次：** Week 2

---

## 📞 聯絡資訊 | Contact

如有資料品質問題或建議，請透過 GitHub Issues 回報。

---

**最後更新：** 2026-03-03  
**版本：** v1.0
