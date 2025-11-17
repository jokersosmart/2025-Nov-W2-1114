# Data Model - SEooC ISO-26262 Documentation System

## 1. Overview

此資料模型定義 SEooC (Safety Element out of Context) ISO-26262 文件系統中所有文件實體、關係、狀態轉換與驗證規則。

### 1.1 Model Purpose
- 定義文件實體結構與屬性
- 建立追溯關係 (Traceability Relationships)
- 規範狀態轉換規則 (State Transitions)
- 提供驗證規則參考 (Validation Rules)

### 1.2 Scope
- 涵蓋 9 類核心文件實體
- 定義 4 類追溯關係類型
- 規範 4 階段狀態轉換
- 整合 FR-001 至 FR-014 驗證規則

---

## 2. Document Entities

### 2.1 SEooC Assumption Document

**描述**: SEooC 開發假設定義文件，定義元件在目標系統中預期使用條件與限制。

**ISO-26262 參考**: ISO-26262-10:2018 §8 (SEooC)

**屬性**:
- `documentId`: 文件唯一識別碼 (格式: SEooC-ASM-v{X}.{Y}-{Status}-{YYYYMMDD})
- `version`: 版本號 (格式: v{Major}.{Minor})
- `status`: 狀態 (Draft | InReview | Reviewed | Approved)
- `creationDate`: 建立日期 (ISO-8601 格式 YYYY-MM-DD)
- `lastModifiedDate`: 最後修改日期
- `owner`: 文件負責人 (Functional Safety Engineer)
- `reviewers`: 審查人員列表 (依 FR-014 審查矩陣)
- `assumptions`: 假設列表

**Assumption 結構**:
```
{
  assumptionId: "ASM-{nnn}",
  category: "Functional" | "Environmental" | "Interface" | "Operational",
  description: "假設描述",
  rationale: "合理性說明",
  impact: "對安全的影響評估",
  verification: "驗證方法",
  traceToTSR: ["TSR-{nnn}", ...],
  traceToIntegratorChecklist: ["CHK-{nnn}", ...]
}
```

**驗證規則** (來自 FR-001):
- 至少涵蓋 4 類假設類型 (Functional, Environmental, Interface, Operational)
- 每個假設須包含合理性說明與影響評估
- 每個假設須可追溯至 TSR 或 Integrator Checklist
- 文件須經過審查矩陣指定審查人員批准

---

### 2.2 HARA (Hazard Analysis and Risk Assessment) Document

**描述**: 危害分析與風險評估報告，識別系統危害並評估風險。

**ISO-26262 參考**: ISO-26262-3:2018 §7 (Item Definition) & §8 (HARA)

**屬性**:
- `documentId`: 文件唯一識別碼 (格式: HARA-v{X}.{Y}-{Status}-{YYYYMMDD})
- `version`: 版本號
- `status`: 狀態
- `creationDate`: 建立日期
- `itemDefinition`: Item Definition (PCIE Gen5 Controller SEooC)
- `operationalSituations`: 操作情境列表
- `hazards`: 危害列表
- `safetyGoals`: 安全目標列表

**Hazard 結構**:
```
{
  hazardId: "HAZ-{nnn}",
  description: "危害描述",
  operationalSituation: "操作情境",
  severity: "S0" | "S1" | "S2" | "S3",
  exposure: "E0" | "E1" | "E2" | "E3" | "E4",
  controllability: "C0" | "C1" | "C2" | "C3",
  asil: "QM" | "ASIL_A" | "ASIL_B" | "ASIL_C" | "ASIL_D",
  derivedSafetyGoals: ["SG-{nnn}", ...]
}
```

**Safety Goal 結構**:
```
{
  safetyGoalId: "SG-{nnn}",
  description: "安全目標描述",
  derivedFromHazard: "HAZ-{nnn}",
  asil: "ASIL_B",
  safeState: "安全狀態描述",
  faultTolerantTimeInterval: "容錯時間區間 (ms)",
  traceToTSR: ["TSR-{nnn}", ...]
}
```

**驗證規則** (來自 FR-002):
- 每個 Hazard 須評估 Severity (S), Exposure (E), Controllability (C)
- ASIL 須依 ISO-26262-3 Table 4 正確計算
- 每個 Safety Goal 須可追溯至至少一個 Hazard
- ASIL B 目標須確保 Safety Goal ASIL ≥ B

---

### 2.3 TSR (Technical Safety Requirement) Document

**描述**: 技術安全需求規格，定義系統層級安全需求。

**ISO-26262 參考**: ISO-26262-4:2018 §6 (Technical Safety Requirements)

**屬性**:
- `documentId`: 文件唯一識別碼 (格式: TSR-v{X}.{Y}-{Status}-{YYYYMMDD})
- `version`: 版本號
- `status`: 狀態
- `requirements`: 技術安全需求列表

**Requirement 結構**:
```
{
  requirementId: "TSR-{nnn}",
  type: "Functional" | "Non-Functional",
  description: "需求描述",
  asil: "ASIL_B",
  derivedFromSafetyGoal: "SG-{nnn}",
  verificationCriteria: "驗證標準",
  traceToDesign: ["DSN-{nnn}", ...],
  traceToTest: ["TST-{nnn}", ...]
}
```

**驗證規則** (來自 FR-003):
- 每個 TSR 須可追溯至至少一個 Safety Goal
- 每個 TSR 須有明確驗證標準
- ASIL 繼承規則: TSR.asil ≥ SafetyGoal.asil
- 雙向追溯: TSR ↔ Design, TSR ↔ Test

---

### 2.4 V&V Plan (Verification and Validation Plan)

**描述**: 驗證與確認計畫，定義測試策略與驗證方法。

**ISO-26262 參考**: ISO-26262-8:2018 §9-§13 (V&V)

**屬性**:
- `documentId`: 文件唯一識別碼 (格式: VV-PLAN-v{X}.{Y}-{Status}-{YYYYMMDD})
- `version`: 版本號
- `status`: 狀態
- `testStrategy`: 測試策略 (4-stage testing)
- `verificationMethods`: 驗證方法列表
- `testCases`: 測試案例列表

**Test Strategy 結構** (來自 FR-004):
```
{
  stage1: {
    name: "Unit Testing",
    scope: "Individual safety mechanisms",
    coverage: "Statement coverage ≥ 95%, Branch coverage ≥ 90%",
    asilRequirement: "ASIL B"
  },
  stage2: {
    name: "Integration Testing",
    scope: "Safety function integration",
    coverage: "Interface coverage 100%",
    asilRequirement: "ASIL B"
  },
  stage3: {
    name: "System Testing",
    scope: "Full system safety validation",
    coverage: "Safety goal coverage 100%",
    asilRequirement: "ASIL B"
  },
  stage4: {
    name: "Regression Testing",
    scope: "Change impact verification",
    coverage: "Modified code coverage ≥ 95%",
    asilRequirement: "ASIL B"
  }
}
```

**Test Case 結構**:
```
{
  testCaseId: "TC-{nnn}",
  description: "測試案例描述",
  type: "Unit" | "Integration" | "System" | "Regression",
  traceToConcept: ["TSR-{nnn}", "DSN-{nnn}", ...],
  verificationMethod: "Test" | "Review" | "Analysis" | "Simulation",
  expectedResult: "預期結果",
  actualResult: "實際結果 (測試後填入)",
  status: "NotRun" | "Passed" | "Failed"
}
```

**驗證規則** (來自 FR-004):
- 測試計畫須包含 4 階段測試策略
- 每個 TSR 須至少被一個 Test Case 驗證
- Coverage 須符合 ASIL B 要求 (Statement ≥ 95%, Branch ≥ 90%)
- 100% Safety Goal coverage 必須達成

---

### 2.5 Safety Case Document

**描述**: 安全論證案例，使用 GSN (Goal Structuring Notation) 建構安全論證。

**ISO-26262 參考**: ISO-26262-2:2018 §6.4.11 (Safety Case)

**屬性**:
- `documentId`: 文件唯一識別碼 (格式: SAFETY-CASE-v{X}.{Y}-{Status}-{YYYYMMDD})
- `version`: 版本號
- `status`: 狀態
- `gsnElements`: GSN 元素列表 (Goals, Strategies, Evidence, Context, Assumptions)

**GSN Element 結構** (來自 FR-005):
```
{
  elementId: "G-{nnn}" | "S-{nnn}" | "E-{nnn}" | "C-{nnn}" | "A-{nnn}",
  type: "Goal" | "Strategy" | "Evidence" | "Context" | "Assumption",
  description: "元素描述",
  parentElement: "parent elementId (if applicable)",
  supportingElements: ["child elementId", ...],
  traceToDocuments: ["HARA", "TSR", "VV-PLAN", ...]
}
```

**GSN 層次結構範例**:
```
G-001 (頂層目標: PCIE Gen5 Controller 安全於 ASIL B)
  ├─ S-001 (策略: 依 ISO-26262 流程開發)
  │   ├─ G-002 (子目標: 所有 Hazards 已識別並評估)
  │   │   └─ E-001 (證據: HARA Report)
  │   ├─ G-003 (子目標: 所有 Safety Goals 已達成)
  │   │   └─ E-002 (證據: TSR + V&V Results)
  │   └─ G-004 (子目標: 所有假設已記錄並驗證)
  │       └─ E-003 (證據: SEooC Assumption Document)
  ├─ C-001 (情境: ASIL B 應用)
  └─ A-001 (假設: Integrator 遵循 SEooC 假設)
```

**驗證規則** (來自 FR-005):
- 頂層 Goal 須為 "System is acceptably safe"
- 每個 Goal 須有 Strategy 或 Evidence 支持
- Evidence 須可追溯至具體文件 (HARA, TSR, V&V Plan, Test Results)
- 所有 Assumptions 須在 SEooC Assumption Document 中記錄

---

### 2.6 Traceability Matrix Document

**描述**: 追溯矩陣，建立文件間的追溯關係。

**ISO-26262 參考**: ISO-26262-8:2018 §6 (Traceability), ISO-26262-4:2018 §7.4.3.8

**屬性**:
- `documentId`: 文件唯一識別碼 (格式: TRACE-MATRIX-v{X}.{Y}-{Status}-{YYYYMMDD})
- `version`: 版本號
- `status`: 狀態
- `traceLinks`: 追溯連結列表

**Trace Link 結構** (來自 FR-006):
```
{
  linkId: "TL-{nnn}",
  sourceType: "Hazard" | "SafetyGoal" | "TSR" | "Design" | "Test" | "Assumption",
  sourceId: "源 ID (e.g., HAZ-001)",
  targetType: "SafetyGoal" | "TSR" | "Design" | "Test" | "Assumption" | "Checklist",
  targetId: "目標 ID (e.g., SG-001)",
  traceType: "Upward" | "Downward" | "Horizontal" | "Verification",
  rationale: "追溯原因說明"
}
```

**追溯類型定義**:
- **Upward Traceability**: 從實作追溯至需求 (e.g., Test → TSR → SafetyGoal → Hazard)
- **Downward Traceability**: 從需求追溯至實作 (e.g., Hazard → SafetyGoal → TSR → Test)
- **Horizontal Traceability**: 同層級間追溯 (e.g., TSR ↔ Design)
- **Verification Traceability**: 驗證方法追溯 (e.g., TSR → Test Case)

**驗證規則** (來自 FR-006):
- 100% Coverage: 每個 Hazard → SafetyGoal → TSR → Test 須完整追溯
- 雙向追溯: Upward & Downward 須一致
- 孤兒檢測: 無 source link 的元素須標記為 orphan
- 懸空檢測: 無 target link 的元素須標記為 dangling

---

### 2.7 Configuration Management Plan

**描述**: 配置管理計畫,定義版本控制與變更管理流程。

**ISO-26262 參考**: ISO-26262-8:2018 §5 (Configuration Management)

**屬性**:
- `documentId`: 文件唯一識別碼 (格式: CM-PLAN-v{X}.{Y}-{Status}-{YYYYMMDD})
- `version`: 版本號
- `status`: 狀態
- `versioningScheme`: 版本規則 (v{Major}.{Minor})
- `branchingStrategy`: 分支策略 (Simplified Git Flow)
- `changeControlProcess`: 變更控制流程

**Versioning Scheme** (來自 FR-007):
```
{
  majorVersion: "重大變更 (e.g., ASIL 變更, 架構重構)",
  minorVersion: "輕微變更 (e.g., 內容修正, 新增章節)",
  versionFormat: "v{Major}.{Minor}",
  exampleProgression: "v1.0 → v1.1 → v1.2 → v2.0"
}
```

**Git Branching Strategy** (來自 research.md Decision 3):
```
{
  mainBranch: "main (production-ready documents)",
  developBranch: "develop (integration branch)",
  featureBranches: "feature/{document-type}-{description}",
  releaseProcess: "develop → main via Pull Request + approval"
}
```

**驗證規則** (來自 FR-007):
- 每次變更須更新版本號 (major or minor)
- 版本號遵循 Semantic Versioning 原則
- Git commits 須包含有意義的 commit message
- 重大變更 (major version) 須經過正式審查

---

### 2.8 Review Record Document

**描述**: 審查記錄文件，記錄文件審查過程與結果。

**ISO-26262 參考**: ISO-26262-8:2018 §7 (Reviews)

**屬性**:
- `documentId`: 文件唯一識別碼 (格式: REVIEW-{DocType}-v{X}.{Y}-{YYYYMMDD})
- `version`: 版本號
- `reviewDate`: 審查日期
- `reviewedDocument`: 被審查文件 ID
- `reviewType`: 審查類型 (Peer Review | 3-Party Review)
- `reviewers`: 審查人員列表
- `findings`: 審查發現列表
- `decision`: 審查決定 (Approved | Rejected | ConditionalApproval)

**Finding 結構** (來自 FR-014):
```
{
  findingId: "FND-{nnn}",
  severity: "Major" | "Minor" | "Observation",
  description: "發現描述",
  location: "文件章節位置",
  recommendation: "建議處理方式",
  status: "Open" | "Resolved" | "Deferred",
  resolution: "解決方案 (if resolved)"
}
```

**Review Matrix** (來自 FR-014):
```
{
  "SafetyDocuments": {
    reviewType: "3-Party Review",
    participants: ["Author", "Independent Reviewer", "FS Manager"],
    criteria: "ISO-26262-8:2018 §7.4.4 (Independence requirement)"
  },
  "GeneralDocuments": {
    reviewType: "Peer Review",
    participants: ["Author", "Peer Reviewer"],
    criteria: "Technical correctness + completeness"
  }
}
```

**驗證規則** (來自 FR-014):
- 每個文件須經過對應類型審查 (依 Review Matrix)
- 所有 Major findings 須在 Approval 前 Resolved
- 審查人員須符合獨立性要求 (3-Party Review)
- 審查記錄須保存至專案結束

---

### 2.9 Training Material Documents

**描述**: 培訓教材文件，包含基礎訓練、進階訓練與專家訓練教材。

**ISO-26262 參考**: ISO-26262-2:2018 §5.4.4 (Competence Management)

**屬性**:
- `documentId`: 文件唯一識別碼 (格式: TRAIN-{Level}-{Module}-v{X}.{Y})
- `version`: 版本號
- `trainingLevel`: 培訓級別 (Basic | Advanced | Expert)
- `modules`: 培訓模組列表
- `duration`: 培訓時長 (hours)
- `assessmentCriteria`: 評估標準

**Training Levels** (來自 Assumption 1):
```
{
  "Basic": {
    target: "新進工程師 (0-1 年經驗)",
    duration: "16 hours",
    modules: [
      "ISO-26262 基礎概念",
      "HARA 執行方法",
      "TSR 撰寫規範",
      "Git 版本控制",
      "文件範本使用"
    ]
  },
  "Advanced": {
    target: "資深工程師 (1-3 年經驗)",
    duration: "24 hours",
    modules: [
      "Safety Case 建構 (GSN)",
      "Traceability Matrix 管理",
      "ASPICE 流程整合",
      "Codebeamer 工具使用",
      "審查技巧"
    ]
  },
  "Expert": {
    target: "FS Manager / Lead Engineer",
    duration: "Continuous",
    modules: [
      "ISO-26262 最新標準更新",
      "複雜系統 HARA 實務",
      "ASIL 分解策略",
      "稽核準備與應對"
    ]
  }
}
```

**驗證規則** (來自 Assumption 1):
- 所有團隊成員須完成對應級別培訓
- 培訓須包含實作練習 (hands-on)
- 培訓後須進行評估 (assessment)
- 培訓記錄須保存至專案結束

---

## 3. Traceability Relationships

### 3.1 Relationship Types

依據 ISO-26262-4:2018 §7.4.3.8,定義以下 4 類追溯關係:

#### 3.1.1 Upward Traceability (向上追溯)
從實作追溯至需求,確保所有實作皆有需求依據。

**範例路徑**:
```
Test Case (TC-001)
  → Design (DSN-001)
    → TSR (TSR-001)
      → Safety Goal (SG-001)
        → Hazard (HAZ-001)
```

**驗證規則**:
- 每個 Test Case 須可追溯至至少一個 TSR
- 每個 Design 須可追溯至至少一個 TSR
- 每個 TSR 須可追溯至至少一個 Safety Goal
- 每個 Safety Goal 須可追溯至至少一個 Hazard

#### 3.1.2 Downward Traceability (向下追溯)
從需求追溯至實作,確保所有需求皆有實作覆蓋。

**範例路徑**:
```
Hazard (HAZ-001)
  → Safety Goal (SG-001)
    → TSR (TSR-001, TSR-002)
      → Design (DSN-001, DSN-002, DSN-003)
        → Test Case (TC-001, TC-002, TC-003, TC-004)
```

**驗證規則**:
- 每個 Hazard 須有至少一個 Safety Goal
- 每個 Safety Goal 須有至少一個 TSR
- 每個 TSR 須有至少一個 Design
- 每個 TSR 須有至少一個 Test Case

#### 3.1.3 Horizontal Traceability (水平追溯)
同層級間追溯,確保相關元素一致性。

**範例關係**:
```
TSR-001 ↔ Design-001 (Requirements ↔ Design)
TSR-002 ↔ TSR-005 (Decomposed requirements)
Assumption-001 ↔ TSR-003 (Assumption → Requirement)
```

**驗證規則**:
- TSR 與 Design 須一致 (no conflicts)
- 分解的 TSR 須完整覆蓋母需求
- Assumption 須可追溯至相關 TSR 或 Checklist

#### 3.1.4 Verification Traceability (驗證追溯)
驗證方法追溯,確保所有需求皆有驗證方法。

**範例關係**:
```
TSR-001 → TC-001 (Test)
TSR-002 → Review-001 (Review)
TSR-003 → Analysis-001 (Analysis)
Design-001 → Simulation-001 (Simulation)
```

**驗證規則** (來自 FR-004):
- 每個 TSR 須有至少一個驗證方法
- 驗證方法須符合 ASIL B 要求
- 100% TSR coverage by verification methods

---

### 3.2 Traceability Matrix Structure

**Excel 格式** (初期 3 個月,來自 research.md Decision 2):

| Source Type | Source ID | Target Type | Target ID | Trace Type | Rationale | Status |
|-------------|-----------|-------------|-----------|------------|-----------|--------|
| Hazard | HAZ-001 | SafetyGoal | SG-001 | Downward | Data corruption hazard → Data integrity goal | Valid |
| SafetyGoal | SG-001 | TSR | TSR-001, TSR-002 | Downward | Decomposed into 2 requirements | Valid |
| TSR | TSR-001 | Design | DSN-001 | Downward | Implemented by CRC check mechanism | Valid |
| Test | TC-001 | TSR | TSR-001 | Upward | Verifies CRC check requirement | Valid |
| ... | ... | ... | ... | ... | ... | ... |

**Codebeamer 格式** (3 個月後,來自 research.md Decision 6):
- 使用 Codebeamer Trace Links 功能
- 自動化追溯驗證 (100% coverage check)
- 視覺化追溯圖 (Traceability Graph)

---

## 4. State Transitions

### 4.1 Document States

所有文件遵循以下狀態轉換規則:

```
Draft → InReview → Reviewed → Approved
  ↑                   ↓
  └──────────────────┘
      (Revision needed)
```

#### 4.1.1 Draft State
- **定義**: 文件初稿,作者編輯中
- **允許操作**: 作者可任意修改
- **轉換條件**: 作者認為文件完成 → 送審 → InReview

#### 4.1.2 InReview State
- **定義**: 文件審查中
- **允許操作**: 審查人員提供意見,作者回應 findings
- **轉換條件**:
  - 審查通過 → Reviewed
  - 需要重大修改 → Draft (Revision)

#### 4.1.3 Reviewed State
- **定義**: 審查完成,等待最終批准
- **允許操作**: 只能進行輕微修正 (typos, formatting)
- **轉換條件**: FS Manager 批准 → Approved

#### 4.1.4 Approved State
- **定義**: 文件正式發佈,可用於稽核
- **允許操作**: 不可修改 (除非啟動 Change Control)
- **版本控制**: Git tag 標記 (e.g., `v1.0-approved-20250115`)

---

### 4.2 Change Control Process

當 Approved 文件需要變更時,遵循以下流程:

1. **提出變更請求** (Change Request):
   - 提交 CR 文件,說明變更原因與影響
   - CR 須包含影響分析 (Impact Analysis)

2. **變更評審** (Change Review):
   - FS Manager 審查 CR
   - 評估變更對安全的影響
   - 決定變更類型 (Major or Minor)

3. **執行變更**:
   - 建立新版本 (Major: v2.0, Minor: v1.1)
   - 更新文件狀態 Draft → InReview → ... → Approved
   - 更新 Traceability Matrix (如影響追溯關係)

4. **變更驗證**:
   - 執行 Regression Testing (如影響測試)
   - 更新 Safety Case (如影響安全論證)

---

## 5. Validation Rules

### 5.1 Naming Convention Validation (來自 FR-006)

**規則**: 所有文件須遵循 4-segment 命名規範

**格式**: `{DocType}_v{Major}.{Minor}_{Status}_{YYYYMMDD}.docx`

**範例**:
- `SEooC-ASM_v1.0_Draft_20250115.docx`
- `HARA_v2.1_Approved_20250120.docx`
- `TSR_v1.3_InReview_20250118.docx`

**驗證邏輯**:
```powershell
function Validate-DocumentName {
    param([string]$fileName)
    
    $pattern = "^([A-Z-]+)_v(\d+)\.(\d+)_(Draft|InReview|Reviewed|Approved)_(\d{8})\.docx$"
    
    if ($fileName -match $pattern) {
        return @{
            Valid = $true
            DocType = $Matches[1]
            MajorVersion = $Matches[2]
            MinorVersion = $Matches[3]
            Status = $Matches[4]
            Date = $Matches[5]
        }
    } else {
        return @{ Valid = $false; Error = "Naming pattern mismatch" }
    }
}
```

---

### 5.2 Completeness Validation

**必要欄位檢查** (依文件類型):

| Document Type | Required Fields |
|---------------|-----------------|
| SEooC Assumption | documentId, version, owner, assumptions[] (≥1), integratorChecklist[] (≥1) |
| HARA | documentId, version, itemDefinition, hazards[] (≥1), safetyGoals[] (≥1) |
| TSR | documentId, version, requirements[] (≥1), 每個 TSR 須有 verificationCriteria |
| V&V Plan | documentId, version, testStrategy (4 stages), testCases[] (≥1) |
| Safety Case | documentId, version, gsnElements[] (≥1 Goal, ≥1 Evidence) |
| Traceability Matrix | documentId, version, traceLinks[], 100% coverage check |

**驗證邏輯範例** (TSR):
```python
def validate_tsr_completeness(tsr_doc):
    errors = []
    
    if not tsr_doc.get('documentId'):
        errors.append("Missing documentId")
    
    if not tsr_doc.get('requirements') or len(tsr_doc['requirements']) == 0:
        errors.append("No requirements defined")
    
    for req in tsr_doc.get('requirements', []):
        if not req.get('verificationCriteria'):
            errors.append(f"TSR {req['requirementId']} missing verificationCriteria")
        if not req.get('derivedFromSafetyGoal'):
            errors.append(f"TSR {req['requirementId']} not traced to Safety Goal")
    
    return {"valid": len(errors) == 0, "errors": errors}
```

---

### 5.3 Traceability Validation (來自 FR-006)

**100% Coverage 檢查**:

1. **Downward Coverage**:
   - 每個 Hazard → 至少 1 個 Safety Goal
   - 每個 Safety Goal → 至少 1 個 TSR
   - 每個 TSR → 至少 1 個 Design
   - 每個 TSR → 至少 1 個 Test Case

2. **Upward Coverage**:
   - 每個 Test Case → 至少 1 個 TSR
   - 每個 Design → 至少 1 個 TSR
   - 每個 TSR → 至少 1 個 Safety Goal
   - 每個 Safety Goal → 至少 1 個 Hazard

3. **Orphan Detection**:
   - 找出無 source link 的元素
   - 範例: TSR 無追溯至任何 Safety Goal

4. **Dangling Detection**:
   - 找出無 target link 的元素
   - 範例: Safety Goal 無追溯至任何 TSR

**驗證邏輯範例**:
```python
def validate_traceability_coverage(trace_matrix):
    orphans = []
    danglings = []
    
    # Build graph
    sources = set([link['sourceId'] for link in trace_matrix['traceLinks']])
    targets = set([link['targetId'] for link in trace_matrix['traceLinks']])
    
    # Find orphans (elements with no incoming links)
    all_elements = sources.union(targets)
    for element in all_elements:
        incoming = [link for link in trace_matrix['traceLinks'] if link['targetId'] == element]
        if len(incoming) == 0 and not element.startswith('HAZ'):  # HAZ is root
            orphans.append(element)
    
    # Find danglings (elements with no outgoing links)
    for element in all_elements:
        outgoing = [link for link in trace_matrix['traceLinks'] if link['sourceId'] == element]
        if len(outgoing) == 0 and not element.startswith('TC'):  # TC is leaf
            danglings.append(element)
    
    return {
        "orphans": orphans,
        "danglings": danglings,
        "valid": len(orphans) == 0 and len(danglings) == 0
    }
```

---

### 5.4 ASIL Inheritance Validation

**規則** (來自 ISO-26262-9:2018 §5 ASIL Decomposition):
- 子元素 ASIL ≥ 父元素 ASIL
- 範例: 若 Safety Goal ASIL = B, 則 TSR ASIL 須 ≥ B

**驗證邏輯**:
```python
def validate_asil_inheritance(trace_matrix, documents):
    errors = []
    asil_order = {"QM": 0, "ASIL_A": 1, "ASIL_B": 2, "ASIL_C": 3, "ASIL_D": 4}
    
    for link in trace_matrix['traceLinks']:
        if link['traceType'] == 'Downward':
            source_asil = get_element_asil(link['sourceId'], documents)
            target_asil = get_element_asil(link['targetId'], documents)
            
            if asil_order[target_asil] < asil_order[source_asil]:
                errors.append(
                    f"ASIL inheritance violation: {link['sourceId']} (ASIL {source_asil}) "
                    f"→ {link['targetId']} (ASIL {target_asil})"
                )
    
    return {"valid": len(errors) == 0, "errors": errors}
```

---

## 6. Integration Points

### 6.1 Git LFS Integration

**Binary 文件管理** (來自 research.md Decision 1):
- Word .docx 文件使用 Git LFS 儲存
- 配置 `.gitattributes`:
  ```
  *.docx filter=lfs diff=lfs merge=lfs -text
  ```

**驗證規則**:
- 所有 .docx 文件須經 Git LFS 追蹤
- 檢查指令: `git lfs ls-files`

---

### 6.2 Codebeamer Migration

**觸發條件** (來自 research.md Decision 6):
- Milestone 1 完成: HARA + TSR 初版完成
- 追溯關係數量 > 200 links
- 手動維護 Excel 錯誤率 > 5%

**遷移流程**:
1. Export Excel Traceability Matrix to CSV
2. Import CSV to Codebeamer via API
3. Validate imported data (100% coverage check)
4. Establish Codebeamer as single source of truth

---

## 7. Summary

### 7.1 Entity Count
- **9 Document Types**: SEooC Assumption, HARA, TSR, V&V Plan, Safety Case, Traceability Matrix, CM Plan, Review Record, Training Material
- **4 Trace Types**: Upward, Downward, Horizontal, Verification
- **4 Document States**: Draft, InReview, Reviewed, Approved

### 7.2 Key Relationships
- Hazard → Safety Goal → TSR → Design → Test (核心追溯鏈)
- SEooC Assumption → TSR / Integrator Checklist (假設驗證)
- Safety Case (GSN) → All Documents (安全論證)

### 7.3 Validation Checkpoints
- Naming Convention: 4-segment format
- Completeness: Required fields per document type
- Traceability: 100% coverage (Downward + Upward)
- ASIL Inheritance: Child ASIL ≥ Parent ASIL
- State Transition: Draft → InReview → Reviewed → Approved

---

**Last Updated**: 2025-01-18  
**Version**: v1.0  
**Author**: SEooC Documentation Team
