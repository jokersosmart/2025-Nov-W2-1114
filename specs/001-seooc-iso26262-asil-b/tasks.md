---
description: "任務分解 - SEooC 開發計畫 (ISO-26262 ASIL B + ASPICE)"
---

# Tasks: SEooC 開發計畫 (ISO-26262 ASIL B + ASPICE)

**Branch**: `001-seooc-iso26262-asil-b`  
**Input**: spec.md (9 User Stories, P1/P2/P3), plan.md (技術堆疊與專案結構), data-model.md (9 文件實體), contracts/ (6 schema), research.md (7 技術決策)

**Prerequisites**: 
- ✅ plan.md (實作計畫, 包含技術堆疊 Word/Excel/Git/PowerShell, 專案結構 docs/ + scripts/)
- ✅ spec.md (9 User Stories: US-001 至 US-009, 優先級 P1/P2/P3)
- ✅ research.md (7 技術決策: Word 格式, 手動 Excel→Codebeamer, 簡化 Git Flow, 4 段命名, 分級審查, 里程碑觸發遷移, 3 層培訓)
- ✅ data-model.md (9 文件實體: SEooC Assumption, HARA, TSR, V&V Plan, Safety Case, Traceability Matrix, CM Plan, Review Record, Training Materials)
- ✅ contracts/ (6 schema 檔案: seooc-assumption, hara-report, tsr, vv-plan, safety-case, traceability-matrix)
- ✅ quickstart.md (快速入門指南, 工作流程, 常見任務)

**專案特性**:
- **類型**: 文件/流程管理專案 (非軟體開發)
- **專案類型聲明**: 文件開發專案 (Documentation Development Project，依憲法 v1.2.0)
- **測試策略**: Verification-First (Checklist-Driven Development, CDD)
- **格式**: Microsoft Word (.docx), Excel (.xlsx)
- **版本控制**: Git + Git LFS (管理 Word 大檔案)
- **自動化**: PowerShell 驗證腳本
- **目標**: ISO-26262:2018 ASIL B + ASPICE Capability Level 2

**CDD 流程說明** (依憲法 v1.2.0 §專案類型定義與測試優先原則適用性):
1. **撰寫合規檢查表** → 定義驗證標準 (等同於 TDD 的「寫測試」)
2. **建立文件模板** → 實作內容結構 (等同於 TDD 的「實作」)
3. **試點驗證** → 以實際案例測試模板 (等同於 TDD 的「執行測試」)
4. **檢查表驗證** → 依檢查表審查試點輸出 (等同於 TDD 的「測試通過」)
5. **模板修訂** → 根據驗證結果改進 (等同於 TDD 的「重構」)

---

## 任務格式: `[ID] [P?] [Story?] 描述 (檔案路徑)`

- **[P]**: 可平行執行 (不同檔案, 無依賴關係)
- **[Story]**: User Story 對應 (US1, US2, ..., US9)
- **檔案路徑**: 包含完整相對路徑 (從專案根目錄)

---

## Phase 1: Setup (專案初始化與基礎建設)

**目的**: 建立專案結構、Git 設定、命名規範驗證腳本

**⚠️ 重要**: 此階段完成後才能開始文件撰寫

### Git 與版本控制設定

- [ ] T001 建立專案根目錄結構 (docs/, scripts/, README.md, CHANGELOG.md)
- [ ] T002 設定 .gitattributes 檔案,啟用 Git LFS 管理 Word/Excel 二進位檔案 (.docx, .xlsx) (檔案路徑: `.gitattributes`)
- [ ] T003 [P] 建立 docs/ 子目錄結構 (templates/, processes/, guidelines/, training/, examples/) (檔案路徑: `docs/templates/`, `docs/processes/`, `docs/guidelines/`, `docs/training/`, `docs/examples/`)
- [ ] T004 [P] 建立 scripts/ 目錄,準備存放驗證腳本 (檔案路徑: `scripts/`)

### 命名規範與驗證工具

- [ ] T005 實作文件命名驗證腳本 `validate-naming.ps1`,驗證 4 段命名規範 `{DocType}_v{X}.{Y}_{Status}_{YYYYMMDD}.docx` (檔案路徑: `scripts/validate-naming.ps1`)
  - 驗證 DocType 合法性 (SEooC-ASM, HARA, TSR, VV-PLAN, SAFETY-CASE, TRACE-MATRIX, CM-PLAN, REVIEW-RECORD, TRAIN-MAT)
  - 驗證版本號格式 v{Major}.{Minor}
  - 驗證狀態 (Draft | InReview | Reviewed | Approved)
  - 驗證日期格式 YYYYMMDD
- [ ] T006 [P] 建立 README.md,說明專案目的、結構、命名規範與快速入門連結 (檔案路徑: `README.md`)
- [ ] T007 [P] 建立 CHANGELOG.md,記錄版本變更歷史 (採 Semantic Versioning v{X}.{Y}) (檔案路徑: `CHANGELOG.md`)

**Checkpoint**: 專案基礎建設完成,可開始建立文件模板

---

## Phase 2: Foundational (共享範本與流程框架)

**目的**: 建立可重用的核心流程定義與指引,作為所有 User Story 的基礎

**⚠️ 關鍵**: 此階段提供共享元件,所有 User Story 任務均依賴這些流程框架

### 流程定義文件 (Process Framework)

- [ ] T008 [P] 建立需求管理流程文件 (Requirements Management Process),定義需求擷取、分析、驗證、追溯與變更管理流程 (符合 FR-003) (檔案路徑: `docs/processes/requirements-management-process.md`)
- [ ] T009 [P] 建立 HARA 流程文件 (HARA Process),定義危害分析與風險評估流程 (符合 FR-004, ISO-26262-3 §7) (檔案路徑: `docs/processes/hara-process.md`)
- [ ] T010 [P] 建立測試驗證流程文件 (V&V Process),定義四階段測試策略 (Unit/Integration/System/Safety Validation) (符合 FR-005) (檔案路徑: `docs/processes/vv-process.md`)
- [ ] T011 [P] 建立配置管理流程文件 (Configuration Management Process),定義 Git 分支策略 (簡化 Git Flow: main + develop + feature/*) 與變更控制流程 (符合 FR-006) (檔案路徑: `docs/processes/configuration-management-process.md`)
- [ ] T012 [P] 建立審查與核准流程文件 (Review & Approval Process),定義分級審查矩陣 (安全文件三方審查 vs 一般文件同儕審查) (符合 FR-010) (檔案路徑: `docs/processes/review-approval-process.md`)
- [ ] T013 [P] 建立變更控制流程文件 (Change Control Process),定義四維度影響分析 (安全影響/追溯影響/測試影響/時程影響) (符合 FR-006) (檔案路徑: `docs/processes/change-control-process.md`)

### 使用指引文件 (Guidelines)

- [ ] T014 [P] 建立命名規範指引 (Naming Convention Guide),詳細說明 4 段命名規則、範例與常見錯誤 (檔案路徑: `docs/guidelines/naming-convention-guide.md`)
- [ ] T015 [P] 建立追溯性指引 (Traceability Guide),說明 4 類追溯關係 (Upward/Downward/Horizontal/Verification) 與追溯矩陣維護方法 (符合 FR-011) (檔案路徑: `docs/guidelines/traceability-guide.md`)
- [ ] T016 [P] 建立 ASIL 繼承指引 (ASIL Inheritance Guide),說明 ASIL 等級正確繼承規則 (TSR.asil ≥ SafetyGoal.asil) (符合 FR-004) (檔案路徑: `docs/guidelines/asil-inheritance-guide.md`)
- [ ] T017 [P] 建立 GSN 建構指引 (GSN Construction Guide),說明 Goal Structuring Notation 標準方法 (符合 FR-009) (檔案路徑: `docs/guidelines/gsn-construction-guide.md`)
- [ ] T018 [P] 建立 Git 工作流程指引 (Git Workflow Guide),詳細說明簡化 Git Flow 分支策略、Pull Request 流程、基線管理與 tag 規範 (檔案路徑: `docs/guidelines/git-workflow-guide.md`)

### 自動化驗證腳本 (初步框架)

- [ ] T019 [P] 建立追溯矩陣完整性驗證腳本框架 `validate-traceability.ps1`,支援雙向追溯檢查 (向上追溯/向下追溯) (檔案路徑: `scripts/validate-traceability.ps1`)
- [ ] T020 [P] 建立測試覆蓋率檢查腳本框架 `check-coverage.ps1`,驗證功能覆蓋率與結構覆蓋率 (符合 FR-007) (檔案路徑: `scripts/check-coverage.ps1`)
- [ ] T021 [P] 建立 SC 達成率儀表板生成腳本框架 `generate-dashboard.ps1`,彙總 SC-001~008 達成率 (檔案路徑: `scripts/generate-dashboard.ps1`)

**Checkpoint**: 流程框架與指引完成,User Story 實作可開始

---

## Phase 3: US-001 - 建立 SEooC 假設與邊界定義 (Priority: P1) 🎯 MVP

**目標**: 產出 SEooC Assumption Document 範本、範例與整合者檢查清單

**獨立測試**: 檔案符合 4 段命名規範、模板包含 4 類假設 (Functional/Envir![alt text](image-1.png)onmental/Interface/Operational)、三方審查通過
![alt text](image.png)
### 文件模板與範例

- [ ] T022 [P] [US1] 建立 SEooC Assumption Document 範本 (Template),包含文件結構、假設分類 (4 類)、假設記錄表格、審查簽核欄位 (符合 data-model.md §2.1, contracts/seooc-assumption-schema.md) (檔案路徑: `docs/templates/SEooC-ASM_Template.docx`)
- [ ] T023 [P] [US1] 建立 SEooC Assumption 填寫範例文件 (Example),基於 PCIE Gen5 Controller 實際假設 (環境溫度 -40°C~125°C, PCIE Gen5 鏈路誤碼率 < 10⁻¹², 等) (檔案路徑: `docs/examples/SEooC-ASM_Example_v1.0_Approved_20250117.docx`)
- [ ] T024 [US1] 建立整合者檢查清單 (Integrator Checklist),包含 3 大類驗證項目 (環境參數 5-7 項, 介面規格 6-8 項, 安全條件 4-5 項) (符合 spec.md Edge Cases, contracts/seooc-assumption-schema.md Integrator Checklist) (檔案路徑: `docs/templates/Integrator-Checklist_Template.xlsx`)

### 流程文件補充

- [ ] T025 [US1] 撰寫 SEooC Assumption 撰寫流程指引,說明如何使用範本、假設合理性評估、整合者責任定義 (符合 ISO-26262-10 §8) (檔案路徑: `docs/processes/seooc-assumption-development-process.md`)
- [ ] T026 [US1] 更新 Review Process,新增 SEooC Assumption 審查檢查清單 (確認 4 類假設完整性、合理性說明、影響評估) (檔案路徑: `docs/processes/review-approval-process.md` 附錄)

### 驗證與測試

- [ ] T027 [US1] 使用 validate-naming.ps1 驗證 SEooC-ASM_Example 檔名符合 4 段命名規範
- [ ] T028 [US1] 執行試點驗證: Functional Safety Engineer 填寫範本,品質工程師檢查完整性,技術負責人審查 (三方審查)

**Checkpoint**: SEooC Assumption 文件系統完整,可獨立產出符合 ISO-26262 要求的假設文件

---

## Phase 4: US-002 - 建立安全分析與風險評估文件 (Priority: P1) 🎯 MVP

**目標**: 產出 HARA (危害分析與風險評估) 報告範本、範例與 ASIL 判定矩陣

**獨立測試**: HARA 報告包含 S/E/C 評估、ASIL 判定依 ISO-26262-3 Table 4 正確、所有危害有對應安全目標

### 文件模板與範例

- [ ] T029 [P] [US2] 建立 HARA Report 範本 (Template),包含 Item Definition、操作情境、危害清單、S/E/C 評估表格、ASIL 判定矩陣、安全目標清單 (符合 data-model.md §2.2, contracts/hara-report-schema.md) (檔案路徑: `docs/templates/HARA_Template.docx`)
- [ ] T030 [P] [US2] 建立 HARA Report 填寫範例,基於 PCIE Gen5 Controller 危害場景 (CRC 錯誤導致錯誤資料傳輸、鏈路訓練失敗導致通訊中斷, ASIL B 分析) (檔案路徑: `docs/examples/HARA_Example_v1.0_Approved_20250120.docx`)
- [ ] T031 [US2] 建立 ASIL 判定矩陣工具 (Excel),自動化計算 ASIL 等級 (依 S/E/C 輸入查表 ISO-26262-3 Table 4) (檔案路徑: `docs/templates/ASIL-Matrix_Calculator.xlsx`)

### 流程文件補充

- [ ] T032 [US2] 更新 HARA Process,補充詳細 HARA 執行步驟 (Item Definition → 操作情境識別 → 危害分析 → S/E/C 評估 → ASIL 判定 → 安全目標衍生) (檔案路徑: `docs/processes/hara-process.md`)
- [ ] T033 [US2] 建立 Safety Goal 衍生指引,說明如何從危害推導安全目標、Safe State 定義、容錯時間區間 (FTTI) 設定 (檔案路徑: `docs/guidelines/safety-goal-derivation-guide.md`)

### 驗證與測試

- [ ] T034 [US2] 驗證 HARA_Example 中所有危害的 ASIL 計算正確性 (人工檢查 S/E/C → ASIL 對應 ISO-26262-3 Table 4)
- [ ] T035 [US2] 執行試點驗證: Functional Safety Engineer 主導 HARA 執行,技術負責人參與,品質工程師驗證追溯完整性

**Checkpoint**: HARA 文件系統完整,可獨立產出符合 ISO-26262-3 §7-§8 要求的風險評估報告

---

## Phase 5: US-003 - 建立技術安全需求文件 (Priority: P1) 🎯 MVP

**目標**: 產出 TSR (Technical Safety Requirement) 文件範本、範例與 ASIL 繼承驗證工具

**獨立測試**: TSR 可追溯至 Safety Goal、ASIL 等級正確繼承 (TSR.asil ≥ SG.asil)、驗證標準明確

### 文件模板與範例

- [ ] T036 [P] [US3] 建立 TSR 文件範本 (Template),包含需求清單、ASIL 等級、可追溯性欄位 (追溯至 Safety Goal)、驗證標準欄位 (符合 data-model.md §2.3, contracts/tsr-schema.md) (檔案路徑: `docs/templates/TSR_Template.docx`)
- [ ] T037 [P] [US3] 建立 TSR 填寫範例,基於 HARA_Example 的安全目標衍生 TSR (如「CRC 錯誤偵測機制須在 10ms 內觸發診斷」, ASIL B) (檔案路徑: `docs/examples/TSR_Example_v1.0_Approved_20250125.docx`)
- [ ] T038 [US3] 建立 ASIL 繼承驗證腳本 `validate-asil-inheritance.ps1`,自動檢查 TSR.asil ≥ SafetyGoal.asil (符合 FR-004, ISO-26262-4 §6.4.1.2) (檔案路徑: `scripts/validate-asil-inheritance.ps1`)

### 流程文件補充

- [ ] T039 [US3] 撰寫 TSR 開發流程指引 (TSR Development Process),說明如何從 Safety Goal 衍生 TSR、如何撰寫可測量驗證標準、ASIL 繼承原則 (檔案路徑: `docs/processes/tsr-development-process.md`)
- [ ] T040 [US3] 更新 Traceability Guide,補充 Safety Goal → TSR → Design → Test 追溯鏈建立方法 (檔案路徑: `docs/guidelines/traceability-guide.md` 附錄)

### 驗證與測試

- [ ] T041 [US3] 執行 validate-asil-inheritance.ps1,驗證 TSR_Example 所有 TSR 的 ASIL 繼承正確
- [ ] T042 [US3] 執行試點驗證: Functional Safety Engineer 撰寫 TSR,技術負責人審查技術可行性,品質工程師驗證追溯完整性

**Checkpoint**: TSR 文件系統完整,可獨立產出符合 ISO-26262-4 §6 要求的技術安全需求

---

## Phase 6: US-004 - 建立測試檢查計畫 (Priority: P2)

**目標**: 產出 V&V (驗證與確認) 計畫範本、範例與測試覆蓋率驗證工具

**獨立測試**: V&V Plan 涵蓋四階段測試策略、覆蓋率目標明確 (功能 100%, 結構 100%, 診斷 90%)

### 文件模板與範例

- [ ] T043 [P] [US4] 建立 V&V Plan 範本 (Template),包含測試策略 (四階段: Unit/Integration/System/Safety Validation)、覆蓋率要求 (ASIL B: 100%/100%/90%)、測試工具驗證 (TI/TCL 分類) (符合 data-model.md §2.4, contracts/vv-plan-schema.md, FR-005) (檔案路徑: `docs/templates/VV-PLAN_Template.docx`)
- [ ] T044 [P] [US4] 建立 V&V Plan 填寫範例,基於 TSR_Example 設計測試案例 (CRC 錯誤注入測試、鏈路訓練失敗測試) (檔案路徑: `docs/examples/VV-PLAN_Example_v1.0_Approved_20250130.docx`)
- [ ] T045 [US4] 建立測試工具驗證檢查清單 (Tool Qualification Checklist),依 ISO-26262-8 §11 分類工具 (TI1/TI2/TI3, TCL1/TCL2/TCL3) (檔案路徑: `docs/templates/Tool-Qualification-Checklist_Template.xlsx`)

### 流程文件補充

- [ ] T046 [US4] 更新 V&V Process,補充四階段測試策略執行細節、靜態驗證方法 (Code Review, MISRA-C 分級合規), 測試工具驗證流程 (檔案路徑: `docs/processes/vv-process.md`)
- [ ] T047 [US4] 建立 MISRA-C 合規指引 (MISRA-C Compliance Guide),定義 ASIL B 合規等級 (Mandatory 100%, Required ≥98%, Advisory ≥80%) 與偏差管理流程 (檔案路徑: `docs/guidelines/misra-c-compliance-guide.md`)

### 驗證與測試

- [ ] T048 [US4] 更新 check-coverage.ps1,實作功能覆蓋率驗證 (TSR → Test Case 追溯完整性 100%)
- [ ] T049 [US4] 執行試點驗證: Test Engineer 填寫 V&V Plan,品質工程師驗證覆蓋率計算,技術負責人審查測試策略

**Checkpoint**: V&V Plan 文件系統完整,可獨立產出符合 ISO-26262-8 §9-§13 要求的測試計畫

---

## Phase 7: US-005 - 建立 Safety Case 論證結構 (Priority: P2)

**目標**: 產出 Safety Case 範本、GSN (Goal Structuring Notation) 指引與論證範例

**獨立測試**: Safety Case 採用 GSN 標準方法、論證邏輯完整、證據可追溯

### 文件模板與範例

- [ ] T050 [P] [US5] 建立 Safety Case 範本 (Template),包含 GSN 架構 (Goals/Strategies/Evidence/Context/Assumptions)、論證層次結構、證據連結表格 (符合 data-model.md §2.5, contracts/safety-case-schema.md, FR-009) (檔案路徑: `docs/templates/SAFETY-CASE_Template.docx`)
- [ ] T051 [P] [US5] 建立 Safety Case 填寫範例,基於 PCIE Gen5 Controller 論證 ASIL B 合規性 (頂層目標: Controller 符合 ASIL B, 策略: 透過 HARA/TSR/V&V, 證據: HARA_Example + TSR_Example + VV-PLAN_Example) (檔案路徑: `docs/examples/SAFETY-CASE_Example_v1.0_Approved_20250205.docx`)
- [ ] T052 [US5] 更新 GSN Construction Guide,補充 GSN 符號說明、論證模式範例 (分析論證、測試論證)、常見錯誤 (檔案路徑: `docs/guidelines/gsn-construction-guide.md`)

### 流程文件補充

- [ ] T053 [US5] 建立 Safety Case 建構流程指引 (Safety Case Development Process),說明如何從安全目標建構論證樹、如何選擇論證策略、如何連結證據文件 (檔案路徑: `docs/processes/safety-case-development-process.md`)
- [ ] T054 [US5] 更新 Review Process,新增 Safety Case 審查檢查清單 (論證邏輯完整性、GSN 符號正確性、證據可追溯性) (檔案路徑: `docs/processes/review-approval-process.md` 附錄)

### 驗證與測試

- [ ] T055 [US5] 驗證 SAFETY-CASE_Example 中所有證據連結可追溯至實際文件 (HARA_Example, TSR_Example, VV-PLAN_Example)
- [ ] T056 [US5] 執行試點驗證: Technical Lead 主導 Safety Case 建構,Functional Safety Engineer 驗證論證邏輯,品質工程師檢查證據完整性

**Checkpoint**: Safety Case 文件系統完整,可獨立產出符合 ISO-26262-2 §6.4.10 要求的安全論證

---

## Phase 8: US-006 - 建立追溯矩陣管理流程 (Priority: P2)

**目標**: 產出追溯矩陣範本、手動維護流程與完整性驗證工具

**獨立測試**: 追溯矩陣支援 4 類追溯關係、100% 雙向追溯、驗證腳本可自動檢查孤立項目

### 文件模板與範例

- [ ] T057 [P] [US6] 建立追溯矩陣範本 (Template, Excel),包含 4 類追溯關係工作表 (Upward: 需求→安全目標, Downward: 安全目標→需求, Horizontal: 需求間依賴, Verification: 測試結果→測試案例→需求) (符合 data-model.md §3, contracts/traceability-matrix-schema.md, FR-011) (檔案路徑: `docs/templates/TRACE-MATRIX_Template.xlsx`)
- [ ] T058 [P] [US6] 建立追溯矩陣填寫範例,基於已完成文件建立追溯連結 (SEooC Assumption → TSR, HARA Hazard → Safety Goal → TSR → VV-PLAN Test Case) (檔案路徑: `docs/examples/TRACE-MATRIX_Example_v1.0_Approved_20250210.xlsx`)
- [ ] T059 [US6] 更新 validate-traceability.ps1,實作完整性驗證功能:
  - 雙向追溯檢查 (向上追溯: 每個需求/設計/測試追溯至源頭; 向下追溯: 每個安全目標追溯至實作與測試)
  - 孤立項目掃描 (識別無追溯關係的需求/設計/測試)
  - 覆蓋率計算 (SC-003: 完整性 = 已建立追溯關係數 / 應建立追溯關係總數 ≥ 100%)
  (檔案路徑: `scripts/validate-traceability.ps1`)

### 流程文件補充

- [ ] T060 [US6] 更新 Traceability Guide,補充手動追溯矩陣維護流程 (批次更新模式、每週固定時段集中更新、變更影響分析時更新追溯) (檔案路徑: `docs/guidelines/traceability-guide.md`)
- [ ] T061 [US6] 建立 Codebeamer 遷移指引 (Codebeamer Migration Guide),定義遷移觸發條件 (ASPICE CL2 通過 + 手動追溯 3 個月穩定 + 1 次階段審查完成)、遷移策略 (先試點後全面)、後備方案 (遷移失敗保留 Excel) (符合 research.md Decision 2) (檔案路徑: `docs/guidelines/codebeamer-migration-guide.md`)

### 驗證與測試

- [ ] T062 [US6] 執行 validate-traceability.ps1,驗證 TRACE-MATRIX_Example 完整性達 100% (無孤立項目)
- [ ] T063 [US6] 執行試點驗證: Quality Engineer 維護追溯矩陣,Functional Safety Engineer 驗證安全追溯邏輯,Technical Lead 審查追溯策略

**Checkpoint**: 追溯矩陣管理系統完整,可達成 SC-003 目標 (追溯完整性 100%)

---

## Phase 9: US-007 - 建立配置管理計畫 (Priority: P3)

**目標**: 產出 CM (Configuration Management) 計畫範本、Git 基線管理流程與自動化 tag 工具

**獨立測試**: CM Plan 定義簡化 Git Flow、基線 tag 規範、變更控制流程明確

### 文件模板與範例

- [ ] T064 [P] [US7] 建立 CM Plan 範本 (Template),包含 Git 分支策略 (簡化 Git Flow: main + develop + feature/*)、基線管理 (每月末打 tag v{X}.0-Month{N}-Review)、變更控制流程 (四維度影響分析) (符合 data-model.md §2.7, FR-006) (檔案路徑: `docs/templates/CM-PLAN_Template.docx`)
- [ ] T065 [P] [US7] 建立 CM Plan 填寫範例,定義 SEooC 專案配置項目 (9 核心文件範本 + 6 流程定義 + 追溯矩陣) 與版本編號規範 (檔案路徑: `docs/examples/CM-PLAN_Example_v1.0_Approved_20250215.docx`)
- [ ] T066 [US7] 建立 Git 基線自動化腳本 `git-tag-baseline.ps1`,自動化月末基線 tag 管理 (驗證所有文件狀態為 Approved、生成 tag v{X}.0-Month{N}-Review、記錄審查記錄) (檔案路徑: `scripts/git-tag-baseline.ps1`)

### 流程文件補充

- [ ] T067 [US7] 更新 Configuration Management Process,補充基線建立流程 (階段性審查通過 → 驗證所有文件 Approved 狀態 → 執行 git-tag-baseline.ps1 → 鎖定基線 tag) (檔案路徑: `docs/processes/configuration-management-process.md`)
- [ ] T068 [US7] 更新 Git Workflow Guide,補充 Pull Request 流程 (feature/* → develop: 同儕審查, develop → main: 三方審查 + 階段性審查) 與 tag 基線對應關係 (檔案路徑: `docs/guidelines/git-workflow-guide.md`)

### 驗證與測試

- [ ] T069 [US7] 執行 git-tag-baseline.ps1 模擬打 tag (驗證腳本可正確檢查文件狀態、生成 tag、記錄審查)
- [ ] T070 [US7] 執行試點驗證: Quality Engineer 執行基線建立流程,Technical Lead 驗證 tag 正確性,全員確認 Git 操作理解

**Checkpoint**: 配置管理系統完整,可達成 SC-005 目標 (變更審查率 ≥95%)

---

## Phase 10: US-008 - 建立審查記錄模板 (Priority: P3)

**目標**: 產出 Review Record 範本、分級審查矩陣與缺陷追蹤工具

**獨立測試**: Review Record 包含審查者簽名、缺陷清單、缺陷解決狀態追蹤

### 文件模板與範例

- [ ] T071 [P] [US8] 建立 Review Record 範本 (Template),包含審查資訊 (審查類型: 三方 vs 同儕、審查者簽名、審查日期)、缺陷清單 (缺陷 ID、嚴重度、描述、狀態)、缺陷解決追蹤 (符合 data-model.md §2.8, FR-010) (檔案路徑: `docs/templates/REVIEW-RECORD_Template.docx`)
- [ ] T072 [P] [US8] 建立 Review Record 填寫範例,記錄 SEooC-ASM_Example 三方審查結果 (3 個 Minor Defect, 1 個 Observation, 全部已解決) (檔案路徑: `docs/examples/REVIEW-RECORD_Example_v1.0_Approved_20250220.docx`)
- [ ] T073 [US8] 建立分級審查矩陣工具 (Excel),自動判定文件類型應執行哪種審查 (安全文件 → 三方審查, 一般文件 → 同儕審查) (檔案路徑: `docs/templates/Review-Matrix_Tool.xlsx`)

### 流程文件補充

- [ ] T074 [US8] 更新 Review & Approval Process,補充缺陷分類標準 (Major Defect: 違反 ISO-26262 條款、ASIL 錯誤; Minor Defect: 格式錯誤、追溯缺失; Observation: 改善建議)、缺陷解決時限 (Major 7 天、Minor 14 天) (檔案路徑: `docs/processes/review-approval-process.md`)
- [ ] T075 [US8] 建立外部審查準備指引 (External Audit Preparation Guide),定義 2 階段準備活動 (3 個月文件審查 + 6 個月完整評估)、25 項檢查清單 (文件完整性 10 項 + 流程合規性 8 項 + 技術正確性 7 項) (符合 FR-014, spec.md 開發保障 §6) (檔案路徑: `docs/guidelines/external-audit-preparation-guide.md`)

### 驗證與測試

- [ ] T076 [US8] 執行試點驗證: 使用 Review-Matrix_Tool.xlsx 判定 9 個文件範本的審查類型,驗證分級邏輯正確
- [ ] T077 [US8] 模擬審查流程: 技術負責人、功能安全工程師、品質工程師執行 TSR_Example 三方審查,填寫 Review Record 並追蹤缺陷解決

**Checkpoint**: 審查管理系統完整,可確保 SC-007 目標 (外部評估確認文件計畫符合 ISO-26262 ASIL B)

---

## Phase 11: US-009 - 建立培訓教材結構 (Priority: P3)

**目標**: 產出 3 層培訓計畫 (Basic/Advanced/Expert) 與能力驗證標準

**獨立測試**: 培訓教材涵蓋 Layer 1/2/3、能力驗證標準明確、新人快速通道 (8hr) 可執行

### 文件模板與培訓材料

- [ ] T078 [P] [US9] 建立 Layer 1 基礎培訓材料 (Basic Training 16hr),包含 ISO-26262 概述 (4hr) + ASIL/HARA 基礎 (3hr) + SEooC 流程 (2hr) + 追溯矩陣/配置管理 (2hr) + ASPICE SWE 概述 (2hr) + spec.md 導讀 (3hr) (符合 data-model.md §2.9, spec.md 假設 1) (檔案路徑: `docs/training/layer1-basic-training.md`)
- [ ] T079 [P] [US9] 建立 Layer 2 進階培訓材料 (Advanced Training 24hr),分 3 角色:
  - Functional Safety Engineer: ISO-26262 Part 3-4 深度 (8hr) + GSN 實作 (4hr) + 診斷覆蓋率 (4hr) + HARA 案例 (4hr) + 實作演練 (4hr)
  - Quality Engineer: Part 8 配置管理 (6hr) + 追溯矩陣實務 (4hr) + ASPICE 評估 (6hr) + MISRA-C (4hr) + 實作演練 (4hr)
  - Technical Lead: 技術管理 (6hr) + 分級審查 (4hr) + 變更控制 (4hr) + 外部評估 (6hr) + 模擬決策 (4hr)
  (檔案路徑: `docs/training/layer2-advanced-training.md`)
- [ ] T080 [P] [US9] 建立 Layer 3 專家培訓材料 (Expert Training, 持續學習),包含 Part 5-6 深度 (16hr) + TÜV/SGS 認證準備 (40hr, 選修) + 產業研討會 (季度 8hr) + 案例學習 (不定期) (檔案路徑: `docs/training/layer3-expert-training.md`)
- [ ] T081 [P] [US9] 建立新人快速通道培訓材料 (New Hire Fast Track 8hr),包含 spec.md 精讀 (4hr) + 角色職責/工具培訓 (4hr) + 理解測驗 (75% 通過標準) (檔案路徑: `docs/training/new-hire-fast-track.md`)
- [ ] T082 [US9] 建立能力驗證標準文件 (Competency Assessment Criteria),定義 Layer 1/2/3 能力驗證方法 (筆試門檻、實作評估、導師評估) 與能力維持機制 (季度檢核、缺口補充培訓) (檔案路徑: `docs/training/competency-assessment-criteria.md`)

### 流程文件補充

- [ ] T083 [US9] 建立培訓執行流程指引 (Training Execution Process),定義培訓時程 (Layer 1: 專案啟動 2 週內、Layer 2: 第 1 個月內、Layer 3: 第 6 個月內建議完成 ≥1 項)、能力驗證流程、培訓記錄管理 (檔案路徑: `docs/processes/training-execution-process.md`)
- [ ] T084 [US9] 建立培訓記錄範本 (Training Record Template),記錄學員培訓時數、測驗成績、能力驗證結果 (供外部評估查核) (檔案路徑: `docs/templates/Training-Record_Template.xlsx`)

### 驗證與測試

- [ ] T085 [US9] 執行試點驗證: 選擇 1-2 名團隊成員執行新人快速通道培訓,驗證 8hr 時程可行性、理解測驗難度適當
- [ ] T086 [US9] 驗證能力維持機制: 模擬季度能力檢核 (20% 抽查 mini-test),確認缺口補充培訓流程可執行

**Checkpoint**: 培訓系統完整,可確保團隊能力符合 ISO-26262-2 §5.4.2 能力管理要求

---

## Phase 12: Polish & Cross-Cutting Concerns (整合與優化)

**目的**: 跨文件整合、驗證腳本完善、稽核準備最終檢查

### 文件整合與驗證

- [ ] T087 [P] 執行全面追溯驗證: 運行 validate-traceability.ps1 驗證所有文件追溯完整性達 100% (SC-003)
- [ ] T088 [P] 執行命名規範驗證: 運行 validate-naming.ps1 批次檢查所有範例文件命名符合 4 段規範
- [ ] T089 [P] 執行 ASIL 繼承驗證: 運行 validate-asil-inheritance.ps1 檢查所有 TSR ASIL 等級正確繼承
- [ ] T090 生成 SC 達成率儀表板: 運行 generate-dashboard.ps1,彙總 SC-001~008 達成率 (目標: SC-001~004 100%, SC-005 ≥95%, SC-006~008 已準備)
- [ ] T091 [P] 更新 CHANGELOG.md,記錄 v1.0 版本所有文件完成記錄 (9 範本 + 6 流程 + 4 指引 + 3 培訓 + 4 腳本)
- [ ] T092 [P] 更新 README.md,補充快速入門連結、專案狀態 (已完成 9 User Stories)、稽核準備狀態

### 稽核準備檢查

- [ ] T093 建立外部審查證據打包結構 (Audit Evidence Package),依 External Audit Preparation Guide 組織 10 類文件夾:
  1. SEooC Assumption + Integrator Checklist
  2. HARA Report + ASIL 判定矩陣
  3. TSR + ASIL 繼承驗證報告
  4. V&V Plan + 測試覆蓋率報告
  5. Safety Case + GSN 論證樹
  6. Traceability Matrix + 完整性驗證報告
  7. CM Plan + Git 基線 tag 記錄
  8. Review Records (所有三方審查記錄)
  9. Training Records (所有能力驗證記錄)
  10. Process Definitions (6 流程文件 + 4 指引)
  (檔案路徑: `audit-evidence/` 目錄結構)
- [ ] T094 執行 25 項外部審查檢查清單自我檢查 (文件完整性 10 項 + 流程合規性 8 項 + 技術正確性 7 項),所有項目須達 100% (符合 SC-007)
- [ ] T095 模擬外部審查問題應答演練: Technical Lead 主導,Functional Safety Engineer + Quality Engineer 參與,準備常見稽核問題應答 (如「如何確保 ASIL 繼承正確?」「追溯矩陣如何維護?」)

### 可觀察性與版本管理 (憲法原則 5)

- [ ] T096 [P] 確認所有文件包含修訂歷史表、審查記錄、變更理由 (可觀察性要求)
- [ ] T097 [P] 驗證 Git 提交日誌完整性,所有變更包含理由與影響範圍 (commit message 規範)
- [ ] T098 [P] 確認版本管理策略採 Semantic Versioning (v{X}.{Y}: X=主版本審查通過時遞增, Y=次版本內容修改時遞增)
- [ ] T099 執行基線建立: 運行 git-tag-baseline.ps1,建立 v1.0-Month1-Review tag (所有文件 v1.0_Approved 狀態)

### 最終驗收

- [ ] T100 執行 quickstart.md 端到端驗證: 邀請未參與專案的工程師依 quickstart.md 操作,驗證新人可在 8hr 內上手
- [ ] T101 全員審查會議: Technical Lead 主持,Functional Safety Engineer + Quality Engineer + Project Manager 參與,確認所有 9 User Stories 驗收標準通過

**Checkpoint**: 專案完成,所有 SC-001~008 達成,可進入外部評估階段

---

## 依賴關係與執行順序

### Phase 依賴關係

1. **Setup (Phase 1)**: 無依賴,可立即開始
2. **Foundational (Phase 2)**: 依賴 Setup 完成,**阻擋所有 User Story 任務**
3. **User Stories (Phase 3-11)**: 全部依賴 Foundational 完成
   - US-001 (P1): 優先執行,無其他 User Story 依賴
   - US-002 (P1): 優先執行,無其他 User Story 依賴
   - US-003 (P1): 優先執行,依賴 US-002 (需 HARA Safety Goal)
   - US-004 (P2): 依賴 US-003 (需 TSR)
   - US-005 (P2): 依賴 US-002, US-003, US-004 (需 HARA + TSR + V&V Plan 作為證據)
   - US-006 (P2): 依賴 US-001~005 (需所有文件建立追溯)
   - US-007 (P3): 可與其他 User Story 平行,無依賴
   - US-008 (P3): 可與其他 User Story 平行,無依賴
   - US-009 (P3): 可與其他 User Story 平行,無依賴
4. **Polish (Phase 12)**: 依賴所有 User Story 完成

### 建議執行順序

#### MVP 策略 (僅 P1 User Stories)

1. **Week 1**: Phase 1 (Setup) + Phase 2 (Foundational)
2. **Week 2**: Phase 3 (US-001 SEooC Assumption) → 產出可用的 SEooC 假設範本
3. **Week 3**: Phase 4 (US-002 HARA) → 產出可用的 HARA 報告範本
4. **Week 4**: Phase 5 (US-003 TSR) → 產出可用的 TSR 文件範本
5. **驗收**: 完成 P1 User Stories,可進行第 1 個月末審查 (符合 SC-002)

#### 完整交付策略

1. **Month 1**: Phase 1-5 (Setup + Foundational + US-001~003) → 第 1 個月末審查
2. **Month 2**: Phase 6-7 (US-004~005) + 部分 Phase 8-11 (US-006~009) → 第 2 個月末審查
3. **Month 3**: 完成 Phase 8-11 (US-006~009) + Phase 12 (Polish) → 第 3 個月末最終審查

#### 平行團隊策略 (多人協作)

Foundational (Phase 2) 完成後,可平行執行:

- **Developer A**: Phase 3-5 (US-001~003, P1 優先)
- **Developer B**: Phase 6-7 (US-004~005, P2)
- **Developer C**: Phase 8-11 (US-006~009, P3)

所有 Phase 完成後,共同執行 Phase 12 (Polish)。

### User Story 內部依賴

每個 User Story 內部任務執行順序:

1. **文件模板與範例** (可平行): 先建立 Template 和 Example
2. **流程文件補充** (依賴模板): 流程指引參考模板結構
3. **驗證與測試** (依賴所有文件): 最後執行驗證

### 平行執行機會

#### Phase 1 (Setup)

- T003, T004, T006, T007 可平行 (不同目錄/檔案)

#### Phase 2 (Foundational)

- T008-T013 (流程定義) 全部可平行
- T014-T018 (指引) 全部可平行
- T019-T021 (腳本框架) 全部可平行

#### 每個 User Story 內部

- Template + Example 可平行 (不同檔案)
- 指引更新可平行 (不同檔案)

---

## 平行執行範例

### Phase 2 流程定義 (6 個流程可同時開工)

```powershell
# 同時啟動 6 個流程定義任務
Task T008: 建立 docs/processes/requirements-management-process.md
Task T009: 建立 docs/processes/hara-process.md
Task T010: 建立 docs/processes/vv-process.md
Task T011: 建立 docs/processes/configuration-management-process.md
Task T012: 建立 docs/processes/review-approval-process.md
Task T013: 建立 docs/processes/change-control-process.md
```

### Phase 3 US-001 模板與範例 (2 個文件可同時開工)

```powershell
# 同時啟動模板與範例撰寫
Task T022: 建立 docs/templates/SEooC-ASM_Template.docx
Task T023: 建立 docs/examples/SEooC-ASM_Example_v1.0_Approved_20250117.docx
```

---

## 實施策略

### 關鍵原則

1. **Foundational First**: Phase 2 完成前,不可開始任何 User Story 任務
2. **優先級驅動**: P1 User Stories (US-001~003) 優先於 P2/P3
3. **獨立驗收**: 每個 User Story 完成後獨立驗收,確認 Acceptance Scenarios 通過
4. **持續驗證**: 每個 Phase 完成後執行驗證腳本 (validate-naming, validate-traceability, validate-asil-inheritance)
5. **階段性審查**: Month 1/2/3 三階段審查,每階段通過後打 Git tag 基線

### 品質門檻

- **Phase 1 完成**: 所有驗證腳本可執行,命名規範驗證通過
- **Phase 2 完成**: 6 流程定義完成,4 指引完成,團隊理解流程框架
- **每個 User Story 完成**: Template + Example + Process 完成,試點驗證通過,三方審查無 Major Defect
- **Phase 12 完成**: SC-001~008 全部達成,外部審查檢查清單 100%,可進入外部評估

### 風險緩解

- **時程風險**: 若 1 個月內無法完成所有 P1 User Stories,優先確保 US-001 (SEooC Assumption) 完成,其餘延後至 Month 2
- **品質風險**: 若試點驗證發現重大缺陷,暫停後續任務,優先修正模板與流程
- **資源風險**: 若人力不足,採 MVP 策略僅完成 P1 User Stories,P2/P3 延後或簡化

---

## 注意事項

- **[P] 任務**: 不同檔案,無依賴關係,可平行執行
- **[Story] 標籤**: 追溯至 spec.md 對應 User Story,確保需求覆蓋
- **檔案路徑**: 所有任務包含完整檔案路徑,避免混淆
- **驗證優先**: 每個 Phase 完成後執行驗證腳本,發現問題立即修正
- **提交規範**: 每個任務或邏輯群組完成後提交 Git,commit message 包含任務 ID (如 "T022: 建立 SEooC Assumption 範本")
- **Checkpoint 驗收**: 每個 Checkpoint 停止並驗證該階段可獨立運作
- **避免**: 模糊任務描述、同檔案衝突、跨 Story 依賴破壞獨立性

---

**總任務數**: 101 個任務
**預估時程**: 3 個月 (Month 1: P1 完成, Month 2: P2 完成, Month 3: P3 + Polish 完成)
**關鍵里程碑**: 
- Month 1 末審查 (US-001~003 完成)
- Month 2 末審查 (US-004~006 完成)
- Month 3 末最終審查 (US-007~009 + Polish 完成, 外部評估準備就緒)
