# Quick Start Guide - SEooC ISO-26262 Documentation System

## 1. Overview

本指南協助團隊成員快速上手 SEooC ISO-26262 ASIL B 文件系統,涵蓋文件建立、追溯管理、審查流程等核心工作流程。

### 1.1 Target Audience
- **Functional Safety Engineers**: HARA, TSR, SEooC Assumption 文件撰寫
- **Quality Engineers**: V&V Plan, Traceability Matrix, 測試執行
- **Technical Leads**: Safety Case 建構, 審查協調
- **Project Managers**: 專案監控, 稽核準備

### 1.2 Prerequisites
- 完成基礎訓練 (Basic Training 16 hours)
- 熟悉 ISO-26262:2018 基本概念
- 安裝 Microsoft Word 2016+ (支援 .docx format)
- 安裝 Git + Git LFS (版本控制)
- 安裝 Excel 2016+ (初期追溯矩陣管理)
- 存取權限: GitHub 專案 repository

---

## 2. Getting Started

### 2.1 Clone Repository

```powershell
# Clone 專案 repository
git clone https://github.com/your-org/seooc-iso26262-asil-b.git
cd seooc-iso26262-asil-b

# 安裝 Git LFS (binary 文件管理)
git lfs install
git lfs pull
```

### 2.2 Directory Structure

```
seooc-iso26262-asil-b/
├─ docs/
│  ├─ templates/          # 9 core document templates
│  │  ├─ SEooC-ASM_Template.docx
│  │  ├─ HARA_Template.docx
│  │  ├─ TSR_Template.docx
│  │  ├─ VV-PLAN_Template.docx
│  │  ├─ SAFETY-CASE_Template.docx
│  │  ├─ TRACE-MATRIX_Template.xlsx
│  │  ├─ CM-PLAN_Template.docx
│  │  ├─ REVIEW-RECORD_Template.docx
│  │  └─ TRAIN-MAT_Template.docx
│  ├─ processes/          # 6 process definitions
│  │  ├─ HARA-Process.md
│  │  ├─ TSR-Development-Process.md
│  │  ├─ Testing-Process.md
│  │  ├─ Review-Process.md
│  │  ├─ Change-Control-Process.md
│  │  └─ Configuration-Management-Process.md
│  ├─ guidelines/         # Best practices & guidelines
│  │  ├─ Naming-Convention-Guide.md
│  │  ├─ Traceability-Guide.md
│  │  ├─ ASIL-Inheritance-Guide.md
│  │  └─ GSN-Construction-Guide.md
│  ├─ training/           # Training materials
│  │  ├─ Basic-16hr/
│  │  ├─ Advanced-24hr/
│  │  └─ Expert-Continuous/
│  └─ examples/           # Example documents
│     ├─ SEooC-ASM_Example_v1.0.docx
│     ├─ HARA_Example_v1.0.docx
│     └─ TSR_Example_v1.0.docx
├─ scripts/               # Automation & validation scripts
│  ├─ validate-naming.ps1
│  ├─ validate-traceability.ps1
│  ├─ check-coverage.ps1
│  └─ export-to-codebeamer.ps1
├─ .gitattributes         # Git LFS configuration
└─ README.md              # Project overview
```

### 2.3 Naming Convention

所有文件須遵循 **4-segment 命名規範** (FR-006):

**格式**: `{DocType}_v{Major}.{Minor}_{Status}_{YYYYMMDD}.docx`

**範例**:
- `SEooC-ASM_v1.0_Draft_20250115.docx`
- `HARA_v2.1_Approved_20250120.docx`
- `TSR_v1.3_InReview_20250118.docx`

**Segment 定義**:
1. **DocType**: 文件類型 (SEooC-ASM, HARA, TSR, VV-PLAN, SAFETY-CASE, etc.)
2. **Version**: 版本號 v{Major}.{Minor} (Major: 重大變更, Minor: 輕微修正)
3. **Status**: 狀態 (Draft | InReview | Reviewed | Approved)
4. **Date**: 建立日期 YYYYMMDD (e.g., 20250115 = 2025-01-15)

**驗證指令**:
```powershell
# 驗證文件命名是否符合規範
.\scripts\validate-naming.ps1 -FilePath ".\docs\SEooC-ASM_v1.0_Draft_20250115.docx"
```

---

## 3. Template Usage

### 3.1 Create New SEooC Assumption Document

1. **複製範本**:
   ```powershell
   Copy-Item ".\docs\templates\SEooC-ASM_Template.docx" `
             ".\docs\SEooC-ASM_v1.0_Draft_20250115.docx"
   ```

2. **填寫 Metadata**:
   - Document ID: `SEooC-ASM-v1.0-Draft-20250115`
   - Version: `v1.0`
   - Status: `Draft`
   - Owner: Your name + role (Functional Safety Engineer)

3. **定義 Item Definition**:
   - Item Name: PCIE Gen5 Controller (SEooC)
   - ASIL: ASIL_B
   - Operating Environment: Temperature, Voltage, Humidity

4. **填寫 Assumptions** (至少 4 個, 涵蓋 4 類):
   - **Functional Assumption** (ASM-001): 範例見 `contracts/seooc-assumption-schema.md`
   - **Environmental Assumption** (ASM-002)
   - **Interface Assumption** (ASM-003)
   - **Operational Assumption** (ASM-004)

5. **建立 Integrator Checklist** (15-20 items):
   - 每個 assumption 須對應至少 1 個 checklist item
   - 範例: CHK-001 → ASM-001 (Verify data rate ≤ 32 GT/s)

6. **驗證完整性**:
   ```powershell
   # 驗證文件結構是否完整
   .\scripts\validate-completeness.ps1 -DocType "SEooC-ASM" `
                                        -FilePath ".\docs\SEooC-ASM_v1.0_Draft_20250115.docx"
   ```

7. **提交 Git**:
   ```powershell
   git add .\docs\SEooC-ASM_v1.0_Draft_20250115.docx
   git commit -m "docs(seooc-asm): Add SEooC Assumption Document v1.0 draft

   - Defined 4 assumption categories (Functional, Environmental, Interface, Operational)
   - Created integrator checklist with 15 items
   - Status: Draft (ready for review)"
   
   git push origin feature/add-seooc-assumptions
   ```

---

### 3.2 Create HARA Document

1. **複製範本** + 填寫 Metadata (同上)

2. **定義 Item Definition**:
   - Item Name, Intended Function, ASIL, Boundaries, Operating Modes

3. **定義 Operational Situations** (≥ 3):
   - OS-001: Highway driving at high speed
   - OS-002: Urban driving with frequent stops
   - OS-003: Parking mode

4. **識別 Hazards** (≥ 5):
   - HAZ-001: Corrupted sensor data → Severity (S2), Exposure (E2), Controllability (C2) → ASIL B
   - 範例見 `contracts/hara-report-schema.md`

5. **定義 Safety Goals** (≥ 5):
   - SG-001: Prevent transmission of corrupted data (derived from HAZ-001)
   - 須包含: ASIL, Safe State, Fault Tolerant Time Interval, Trace to TSR

6. **驗證 ASIL 計算**:
   ```powershell
   # 自動驗證 S/E/C → ASIL 計算是否符合 ISO-26262-3 Table 4
   .\scripts\validate-asil-calculation.ps1 -FilePath ".\docs\HARA_v1.0_Draft_20250115.docx"
   ```

7. **驗證追溯關係**:
   - 每個 Hazard → 至少 1 個 Safety Goal
   - 每個 Safety Goal → 至少 1 個 TSR (稍後在 TSR 文件中建立)

---

### 3.3 Create TSR Document

1. **複製範本** + 填寫 Metadata

2. **定義 Requirements** (≥ 10):
   - TSR-001: CRC error detection (Functional, Detection category)
   - TSR-002: Detection time ≤ 10ms (Non-Functional, Detection category)
   - 涵蓋 5 類: Detection, Mitigation, Control, Warning, Diagnostics

3. **填寫每個 Requirement**:
   - **derivedFromSafetyGoal**: SG-001 (來自 HARA)
   - **ASIL**: ASIL_B (須 ≥ Safety Goal ASIL)
   - **Verification Criteria**: "CRC error detection rate ≥ 99.99% in 10,000 trials" (具體可測量)
   - **Trace to Design**: DSN-001
   - **Trace to Test**: TC-001, TC-002

4. **驗證 ASIL 繼承**:
   ```powershell
   # 驗證 TSR ASIL ≥ Safety Goal ASIL
   .\scripts\validate-asil-inheritance.ps1 -TSRPath ".\docs\TSR_v1.0_Draft_20250115.docx" `
                                           -HARAPath ".\docs\HARA_v1.0_Approved_20250115.docx"
   ```

5. **驗證 Verification Criteria**:
   - 確保每個 TSR 有明確可測量的驗證標準
   - 格式: "{Metric} {Operator} {Value} in {Test Context}"

---

### 3.4 Create V&V Plan

1. **定義 4-Stage Test Strategy**:
   - Stage 1: Unit Testing (Statement ≥ 95%, Branch ≥ 90%)
   - Stage 2: Integration Testing (Interface 100%)
   - Stage 3: System Testing (Safety Goal 100%)
   - Stage 4: Regression Testing (Modified code ≥ 95%)

2. **定義 Verification Methods** (≥ 4):
   - Test, Review, Analysis, Simulation

3. **建立 Test Cases** (≥ 20):
   - TC-001: Verify CRC error detection (trace to TSR-001)
   - 每個 TSR 須至少被 1 個 Test Case 覆蓋

4. **驗證 TSR Coverage**:
   ```powershell
   # 驗證所有 TSR 皆有 Test Case 覆蓋
   .\scripts\check-coverage.ps1 -VVPlanPath ".\docs\VV-PLAN_v1.0_Draft_20250115.docx" `
                                 -TSRPath ".\docs\TSR_v1.0_Approved_20250115.docx"
   ```

---

## 4. Process Workflows

### 4.1 HARA → TSR → V&V Workflow

```
Step 1: HARA (Hazard Analysis)
  ↓
  Identify Hazards (HAZ-001, HAZ-002, ...)
  ↓
  Evaluate S/E/C → Determine ASIL
  ↓
  Define Safety Goals (SG-001, SG-002, ...)

Step 2: TSR (Technical Safety Requirements)
  ↓
  Decompose Safety Goals into TSR (TSR-001, TSR-002, ...)
  ↓
  Define Verification Criteria for each TSR

Step 3: V&V Plan
  ↓
  Create Test Cases (TC-001, TC-002, ...) to verify TSR
  ↓
  Execute Tests → Collect Results

Step 4: Traceability Matrix
  ↓
  Establish trace links: HAZ → SG → TSR → Test
  ↓
  Validate 100% coverage (Downward + Upward)
```

---

### 4.2 Review & Approval Process

#### 4.2.1 Peer Review (General Documents)

**適用文件**: CM Plan, Training Materials

**流程**:
1. **作者**: 文件完成 → 狀態改為 `InReview` → 提交 Pull Request
2. **Peer Reviewer**: 審查文件 → 提供 comments → 記錄 findings
3. **作者**: 回應 findings → 修正文件
4. **Peer Reviewer**: 確認修正 → Approve PR
5. **狀態更新**: `InReview` → `Reviewed` → `Approved`

#### 4.2.2 3-Party Review (Safety Documents)

**適用文件**: SEooC Assumption, HARA, TSR, V&V Plan, Safety Case

**流程**:
1. **作者**: 文件完成 → 狀態改為 `InReview`
2. **Independent Reviewer**: 審查文件 (須非作者本人) → 記錄 findings
3. **FS Manager**: 第二輪審查 → 驗證獨立性 → 記錄 findings
4. **作者**: 回應所有 Major findings → 修正文件
5. **FS Manager**: 最終批准 → 狀態改為 `Approved`

**Review Record 範例**:
```
Document: HARA_v1.0_InReview_20250115.docx
Reviewers:
  - Independent Reviewer: Alice Wang
  - FS Manager: Bob Chen
Findings:
  - FND-001 (Major): HAZ-003 ASIL calculation incorrect (S2+E2+C2 should be ASIL B, not QM)
  - FND-002 (Minor): Typo in SG-002 description
Decision: Conditional Approval (resolve FND-001 before final approval)
```

---

### 4.3 Change Control Process

**觸發情境**: Approved 文件需要變更

**流程**:
1. **提出 Change Request (CR)**:
   - 填寫 CR 表單: 變更原因, 影響範圍, 風險評估
   - 範例: "CR-001: Update TSR-003 verification criteria (coverage 90% → 95%)"

2. **Change Review**:
   - FS Manager 審查 CR → 評估影響 (Major or Minor)
   - Major: 影響 ASIL, 架構, 安全論證 → 需完整重審
   - Minor: 內容修正, 新增章節 → 輕度審查

3. **執行變更**:
   - 建立新版本 (Major: v2.0, Minor: v1.1)
   - 更新文件 → 狀態 `Draft` → `InReview` → ... → `Approved`

4. **Impact Analysis**:
   - 更新 Traceability Matrix (if 影響追溯關係)
   - 執行 Regression Testing (if 影響測試)
   - 更新 Safety Case (if 影響安全論證)

5. **版本控制**:
   ```powershell
   # 提交變更
   git add .\docs\TSR_v1.1_Draft_20250120.docx
   git commit -m "docs(tsr): Update TSR v1.1 - CR-001 (verification criteria)"
   
   # 標記版本 (Approved 後)
   git tag -a v1.1-approved-20250125 -m "TSR v1.1 approved (CR-001 implemented)"
   git push origin v1.1-approved-20250125
   ```

---

## 5. Common Tasks

### 5.1 Update Traceability Matrix

**初期 3 個月 (Excel format)**:

1. **開啟 Excel 檔案**:
   ```
   .\docs\TRACE-MATRIX_v1.0_Draft_20250115.xlsx
   ```

2. **新增 Trace Link**:
   | Link ID | Source Type | Source ID | Target Type | Target ID | Trace Type | Rationale | Status |
   |---------|-------------|-----------|-------------|-----------|------------|-----------|--------|
   | TL-011 | TSR | TSR-004 | Design | DSN-004 | Downward | Implemented by power monitoring module | Valid |

3. **驗證追溯完整性**:
   ```powershell
   # 檢查 100% coverage (Downward + Upward)
   .\scripts\validate-traceability.ps1 -MatrixPath ".\docs\TRACE-MATRIX_v1.0_Draft_20250115.xlsx"
   ```

4. **檢查 Orphan & Dangling**:
   - **Orphan**: 無 incoming link 的元素 (除了 Hazard)
   - **Dangling**: 無 outgoing link 的元素 (除了 Test Case)

**3 個月後 (Codebeamer migration)**:

1. **Export Excel to CSV**:
   ```powershell
   # 匯出 Excel 為 CSV 格式
   .\scripts\export-to-csv.ps1 -ExcelPath ".\docs\TRACE-MATRIX_v1.0_Approved_20250220.xlsx"
   ```

2. **Import to Codebeamer via API**:
   ```powershell
   # 透過 Codebeamer API 匯入追溯資料
   .\scripts\export-to-codebeamer.ps1 -CSVPath ".\exports\trace-matrix-20250220.csv"
   ```

3. **驗證匯入結果**:
   - 登入 Codebeamer → Traceability View
   - 驗證 100% coverage → 視覺化追溯圖

---

### 5.2 Conduct Document Review

**3-Party Review Example** (HARA Document):

1. **準備審查**:
   - 作者: 提交 HARA v1.0 Draft
   - 審查人員: 指派 Independent Reviewer (Alice) + FS Manager (Bob)

2. **執行審查**:
   - Independent Reviewer (Alice): 檢查 HARA 完整性, ASIL 計算, 追溯關係
   - 記錄 Findings:
     ```
     FND-001 (Major): HAZ-003 Controllability should be C3, not C2
     FND-002 (Minor): Typo in SG-002 description ("shoudl" → "should")
     ```

3. **作者回應**:
   - 作者: 修正 FND-001 (HAZ-003 Controllability C2 → C3 → ASIL recalculated)
   - 作者: 修正 FND-002 (Typo fixed)
   - 更新文件: HARA_v1.0_InReview_20250116.docx

4. **FS Manager 批准**:
   - FS Manager (Bob): 二次審查 → 確認 FND-001 已解決
   - Decision: Approved
   - 狀態更新: `InReview` → `Reviewed` → `Approved`

5. **記錄審查結果**:
   - 建立 Review Record: `REVIEW-HARA-v1.0-20250116.docx`
   - 儲存至 `docs/review-records/`

---

### 5.3 Prepare for Audit

**稽核前準備 Checklist**:

1. **文件完整性**:
   - ☐ 所有 9 類文件皆已建立且 Approved
   - ☐ 命名符合 4-segment 規範
   - ☐ 版本號一致 (Traceability Matrix 引用的版本須存在)

2. **追溯完整性**:
   - ☐ 100% Downward Traceability (Hazard → SafetyGoal → TSR → Test)
   - ☐ 100% Upward Traceability (Test → TSR → SafetyGoal → Hazard)
   - ☐ 無 Orphan or Dangling elements

3. **審查記錄**:
   - ☐ 所有 Safety Documents 皆有 3-Party Review Record
   - ☐ 所有 Major Findings 皆已 Resolved
   - ☐ Review Records 包含 Reviewer 簽名 + 日期

4. **測試結果**:
   - ☐ 100% TSR coverage by Test Cases
   - ☐ Statement Coverage ≥ 95%, Branch Coverage ≥ 90%
   - ☐ 100% Safety Goal Coverage
   - ☐ 所有 Test Cases status = Passed

5. **Git 版本控制**:
   - ☐ 所有 Approved 文件皆有 Git tag (e.g., `v1.0-approved-20250125`)
   - ☐ Commit messages 符合 Constitution 規範
   - ☐ 無未提交的變更 (git status clean)

6. **Safety Case**:
   - ☐ GSN 結構完整 (Top Goal + Strategies + Evidence)
   - ☐ 所有 Evidence 可追溯至具體文件
   - ☐ 所有 Assumptions 在 SEooC Assumption Document 中記錄

**執行驗證**:
```powershell
# 一鍵驗證所有文件完整性
.\scripts\audit-readiness-check.ps1 -DocsPath ".\docs" -OutputReport ".\audit-readiness-report.html"
```

---

## 6. Troubleshooting

### 6.1 Git LFS Issues

**問題**: Git LFS 未正確追蹤 .docx 文件

**解決方案**:
```powershell
# 檢查 Git LFS 追蹤狀態
git lfs ls-files

# 重新配置 .gitattributes
echo "*.docx filter=lfs diff=lfs merge=lfs -text" > .gitattributes
git add .gitattributes
git commit -m "fix: Configure Git LFS for .docx files"

# 重新追蹤現有文件
git lfs migrate import --include="*.docx"
git push --force origin feature/fix-git-lfs
```

---

### 6.2 Naming Validation Errors

**問題**: 文件命名不符合 4-segment 規範

**錯誤訊息**:
```
Error: File name 'HARA_Draft.docx' does not match pattern:
{DocType}_v{Major}.{Minor}_{Status}_{YYYYMMDD}.docx
```

**解決方案**:
```powershell
# 重新命名文件
Rename-Item ".\docs\HARA_Draft.docx" ".\docs\HARA_v1.0_Draft_20250115.docx"

# 驗證命名
.\scripts\validate-naming.ps1 -FilePath ".\docs\HARA_v1.0_Draft_20250115.docx"
# Output: ✅ Naming convention valid
```

---

### 6.3 Traceability Coverage Issues

**問題**: TSR-005 無 Test Case 覆蓋

**錯誤訊息**:
```
Warning: TSR-005 has no traceability to Test Cases
Coverage: 90% (9/10 TSRs covered)
```

**解決方案**:
1. 檢查 V&V Plan 是否遺漏 TSR-005 測試
2. 新增 Test Case TC-011 覆蓋 TSR-005
3. 更新 Traceability Matrix: TL-050 (TSR-005 → TC-011)
4. 重新驗證:
   ```powershell
   .\scripts\check-coverage.ps1 -VVPlanPath ".\docs\VV-PLAN_v1.1_Draft_20250116.docx" `
                                 -TSRPath ".\docs\TSR_v1.0_Approved_20250115.docx"
   # Output: ✅ 100% TSR coverage (10/10 TSRs covered)
   ```

---

### 6.4 Word Version Conflicts

**問題**: 多人同時編輯 Word 文件導致衝突

**避免方法**:
1. **使用 Git Branching Strategy**:
   - 每人建立獨立 feature branch
   - 範例: `feature/alice-update-hara`, `feature/bob-update-tsr`

2. **避免同時編輯相同文件**:
   - 使用 GitHub Issues 或 Jira 協調編輯順序
   - 範例: "Alice editing HARA (2025-01-15 to 2025-01-16)"

3. **衝突發生時**:
   - 無法自動 merge .docx 文件 (binary format)
   - 手動協調: 保留最新版本 + 合併修改內容

---

## 7. Next Steps

### 7.1 進階訓練

完成 Quick Start 後,建議參加:
- **Advanced Training (24 hours)**: Safety Case 建構 (GSN), Traceability Matrix 管理, ASPICE 流程整合
- **Expert Training (Continuous)**: 複雜系統 HARA 實務, ASIL 分解策略, 稽核準備

### 7.2 工具升級

**3 個月後 (Codebeamer Migration)**:
- 參加 Codebeamer 訓練 (8 hours)
- 學習 API integration for traceability automation
- 熟悉 Traceability Graph 視覺化工具

### 7.3 持續改進

- 每月 Retrospective: 檢討流程改進機會
- 每季 Metrics Review: 追蹤文件錯誤率, 審查時間, 測試覆蓋率
- 每年 Standard Update: ISO-26262 新版本更新培訓

---

**Last Updated**: 2025-01-18  
**Version**: v1.0  
**Document Type**: Quick Start Guide

**Support Contact**:
- Functional Safety Team: fs-team@company.com
- Technical Support: tech-support@company.com
