# ISO-26262 合規性檢查清單：SEooC 開發計畫 (ASIL B)

**Purpose**: 驗證 SEooC 開發計畫規格的需求品質，確保符合 ISO-26262 Part 4、ASPICE SWE 與 ASIL B 等級的文件完整性、追溯性與合規性要求。此檢查清單用於多階段品質保證：作者自檢 → 內部審查 → 稽核準備。

**Created**: 2025-11-14  
**Feature**: [spec.md](../spec.md)  
**Depth Level**: 正式級（稽核準備）  
**Focus Areas**: ISO-26262 合規性、安全分析品質、追溯性與文件品質  
**Target Audience**: 作者（自檢）、品質工程師（內部審查）、稽核人員（外部審查準備）

---

## 需求完整性（Requirement Completeness）

### ISO-26262 Part 4 工作產品覆蓋

- [ ] CHK001 - 規格是否明確定義所有 ISO-26262 Part 4 必要工作產品的需求？[Completeness, Spec §FR-002]
- [ ] CHK002 - SEooC 假設文件的必要內容（功能假設、安全假設、環境假設、整合假設）是否完整定義？[Completeness, Spec §FR-001, Key Entities]
- [ ] CHK003 - 安全計畫的必要元素（角色與責任、時程與里程碑、安全管理活動）是否在需求中明確？[Completeness, Key Entities]
- [ ] CHK004 - HARA 流程需求是否涵蓋危害識別、風險評估（嚴重度/曝露度/可控性）與安全目標確定？[Completeness, Spec §FR-004, User Story 3]
- [ ] CHK005 - 安全需求衍生流程是否定義從安全目標到安全需求的追溯機制？[Completeness, Spec §FR-004]
- [ ] CHK006 - 測試檢查計畫（V&V）需求是否涵蓋單元測試、整合測試、系統測試與安全驗證測試？[Completeness, Spec §FR-005, User Story 4]
- [ ] CHK007 - 配置管理需求是否包含配置項目定義、版本控制與變更控制流程？[Completeness, Spec §FR-006, User Story 5]
- [ ] CHK008 - 安全案例（Safety Case）架構需求是否明確？[Completeness, Spec §FR-009]

### ASPICE 流程對齊

- [ ] CHK009 - 需求管理流程（SWE.1）的所有活動（擷取、分析、驗證、追溯、變更管理）是否在需求中定義？[Completeness, Spec §FR-003]
- [ ] CHK010 - 規格是否明確要求流程對齊 ASPICE SWE.1 至 SWE.6？[Completeness, Spec §FR-008]
- [ ] CHK011 - 流程可評估性（Capability Level 2 要求）是否在成功標準中定義？[Completeness, Spec §SC-006]

### ASIL B 技術需求

- [ ] CHK012 - ASIL B 覆蓋率要求（語句 100%、分支 100%）是否明確定義？[Completeness, Spec §FR-007, SC-004]
- [ ] CHK013 - 診斷覆蓋率目標（90%）是否在需求與成功標準中明確？[Completeness, Spec §FR-012, SC-004, Clarifications]
- [ ] CHK014 - PCIE Gen5 核心安全機制（CRC/ECC/鏈路訓練錯誤偵測）需求是否完整定義？[Completeness, Spec §FR-012, Scope, Clarifications]
- [ ] CHK015 - 安全機制的驗證準則是否在測試檢查計畫需求中明確？[Completeness, Spec §User Story 3, Scenario 3]

---

## 需求清晰度（Requirement Clarity）

### 量化與可衡量性

- [ ] CHK016 - "文件完整性達 100%" 的衡量標準是否明確定義？[Clarity, Spec §SC-001]
- [ ] CHK017 - "2 週內完成初稿" 與 "3 個月內通過階段性審查" 的時程定義是否清晰？[Clarity, Spec §SC-002]
- [ ] CHK018 - "追溯矩陣完整性達 100%" 是否有明確的驗證方法？[Clarity, Spec §SC-003]
- [ ] CHK019 - "變更控制審查率 95% 以上" 的計算基準是否定義？[Clarity, Spec §SC-005]
- [ ] CHK020 - "假設文件清晰度滿意度達 90%" 的衡量方法是否明確？[Clarity, Spec §SC-008]

### 模糊術語澄清

- [ ] CHK021 - "符合 ISO-26262 Part 4 所有必要工作產品" 是否有明確清單或參考？[Clarity, Spec §FR-002]
- [ ] CHK022 - "適當審查" 的審查標準與角色是否定義？[Clarity, Spec §FR-010]
- [ ] CHK023 - V&V（驗證與確認）術語是否已明確定義或白話化？[Clarity, Spec §User Story 4, 名詞解釋]
- [ ] CHK024 - SEooC（Safety Element out of Context）概念是否在規格中充分解釋？[Clarity, Spec §User Story 1]

### 工具與格式明確性

- [ ] CHK025 - 文件格式要求（Word .docx → Codebeamer 遷移路徑）是否清晰定義？[Clarity, Spec §FR-002, Assumptions, Clarifications]
- [ ] CHK026 - 追溯矩陣管理責任（初期手動 vs 未來自動化）是否明確？[Clarity, Spec §FR-011, Key Entities, Clarifications]
- [ ] CHK027 - Codebeamer 遷移時機與觸發條件是否定義？[Ambiguity, Dependencies]

---

## 需求一致性（Requirement Consistency）

### 內部一致性

- [ ] CHK028 - User Story 優先級（P1/P2/P3）是否與成功標準的時程要求一致？[Consistency, User Stories vs SC-002]
- [ ] CHK029 - FR-002 的文件格式要求是否與 Scope (In Scope) 中的描述一致？[Consistency, FR-002 vs Scope]
- [ ] CHK030 - FR-012 的安全機制範圍是否與 Scope (In Scope) 與 (Out of Scope) 定義一致？[Consistency, FR-012 vs Scope]
- [ ] CHK031 - 追溯矩陣管理責任在 FR-011、Key Entities、Clarifications 中的描述是否一致？[Consistency]
- [ ] CHK032 - 診斷覆蓋率目標在 FR-012、SC-004、User Story 3、Clarifications 中是否一致（90%）？[Consistency]

### 外部標準對齊

- [ ] CHK033 - ASIL B 覆蓋率要求是否與 ISO-26262-4:2018 Table 9 一致？[Consistency, Spec §FR-007]
- [ ] CHK034 - ASPICE SWE.1-SWE.6 流程領域需求是否與 ASPICE PAM 3.1 定義對齊？[Consistency, Spec §FR-008]
- [ ] CHK035 - 診斷覆蓋率 90% 目標是否符合 ISO-26262 Part 5 對 ASIL B 的推薦值？[Consistency, Spec §FR-012, Clarifications]

---

## 驗收標準品質（Acceptance Criteria Quality）

### 可測試性

- [ ] CHK036 - User Story 1 的驗收場景是否可客觀驗證（文件包含必要元素）？[Measurability, User Story 1, Scenarios]
- [ ] CHK037 - User Story 2 的驗收場景是否可客觀驗證（流程符合標準）？[Measurability, User Story 2, Scenarios]
- [ ] CHK038 - User Story 3 的驗收場景是否包含可衡量的驗證準則（診斷覆蓋率 90%）？[Measurability, User Story 3, Scenario 3]
- [ ] CHK039 - 所有成功標準（SC-001 至 SC-008）是否都包含可客觀衡量的指標？[Measurability, Success Criteria]

### Given-When-Then 結構完整性

- [ ] CHK040 - 所有驗收場景是否都遵循完整的 Given-When-Then 格式？[Completeness, User Stories]
- [ ] CHK041 - 驗收場景的 "Then" 條件是否明確定義預期結果？[Clarity, User Stories]
- [ ] CHK042 - 驗收場景是否涵蓋正常流程與異常流程（如 User Story 1, Scenario 3 的修訂循環）？[Coverage, User Stories]

---

## 情境覆蓋（Scenario Coverage）

### 主要流程

- [ ] CHK043 - 是否涵蓋 SEooC 假設定義的完整流程（撰寫 → 審查 → 修訂 → 通過）？[Coverage, User Story 1]
- [ ] CHK044 - 是否涵蓋流程文件建立與執行的完整生命週期？[Coverage, User Story 2]
- [ ] CHK045 - 是否涵蓋 HARA 到安全需求衍生的完整流程？[Coverage, User Story 3]
- [ ] CHK046 - 是否涵蓋測試計畫建立、審查與執行的完整流程？[Coverage, User Story 4]

### 異常與恢復流程

- [ ] CHK047 - 是否定義假設文件審查不通過時的修訂流程？[Coverage, Exception Flow, User Story 1, Scenario 3]
- [ ] CHK048 - 是否定義測試覆蓋率無法達標時的處理策略？[Coverage, Exception Flow, Edge Cases]
- [ ] CHK049 - 是否定義需求變更時的影響分析與審查流程？[Coverage, Exception Flow, User Story 5, Scenario 3]
- [ ] CHK050 - 是否定義 ASIL B 與 ASPICE 要求衝突時的取捨原則？[Coverage, Exception Flow, Edge Cases]

### 邊界條件

- [ ] CHK051 - 是否定義假設使用情境與實際應用不符的處理機制？[Coverage, Edge Case, Edge Cases]
- [ ] CHK052 - 是否定義 Codebeamer 遷移失敗或延遲的應對措施？[Gap, Recovery Flow]
- [ ] CHK053 - 是否定義稽核時程延遲或提前的調整機制？[Gap, Exception Flow]

---

## 追溯性需求（Traceability Requirements）

### 需求 ID 與引用

- [ ] CHK054 - 所有功能需求（FR-001 至 FR-012）是否都有唯一 ID？[Traceability, Functional Requirements]
- [ ] CHK055 - 所有成功標準（SC-001 至 SC-008）是否都有唯一 ID？[Traceability, Success Criteria]
- [ ] CHK056 - User Stories 是否有明確的優先級標記（P1/P2/P3）？[Traceability, User Stories]
- [ ] CHK057 - Key Entities 是否都在功能需求或 User Stories 中被引用？[Traceability]

### 雙向追溯能力

- [ ] CHK058 - 規格是否明確要求建立需求、設計、測試之間的雙向追溯？[Completeness, Spec §FR-011]
- [ ] CHK059 - 追溯矩陣模板需求是否定義追溯的粒度與格式？[Clarity, Spec §FR-011, Key Entities]
- [ ] CHK060 - 是否定義追溯矩陣完整性的驗證方法（SC-003 的 100% 如何衡量）？[Measurability, Spec §SC-003]

### ISO-26262 參考文件追溯

- [ ] CHK061 - 規格是否明確引用 ISO-26262 參考文件（`.specify/memory/ISO-26262-*.md`）？[Traceability, Dependencies]
- [ ] CHK062 - FR-007 是否明確引用 ISO-26262-4:2018 Table 9？[Traceability, Spec §FR-007]
- [ ] CHK063 - 診斷覆蓋率要求是否明確引用 ISO-26262 Part 5 推薦值？[Traceability, Spec §FR-012, Clarifications]

---

## 非功能需求（Non-Functional Requirements）

### 時程與里程碑

- [ ] CHK064 - 是否定義文件計畫完成時限（專案啟動 1 個月內）？[Completeness, Assumptions]
- [ ] CHK065 - 是否定義階段性稽核時程（3 個月文件審查、6 個月完整稽核）？[Completeness, Assumptions, Clarifications, Dependencies]
- [ ] CHK066 - 是否定義 SEooC 假設文件初稿完成時限（2 週）？[Completeness, Spec §SC-002]
- [ ] CHK067 - 時程需求是否與成功標準中的時程指標一致？[Consistency, SC-002 vs Assumptions]

### 人力資源與角色

- [ ] CHK068 - 是否明確定義功能安全工程師的職責範圍？[Completeness, Assumptions]
- [ ] CHK069 - 是否明確定義品質工程師的職責範圍（包含初期追溯矩陣維護）？[Completeness, Assumptions, Clarifications]
- [ ] CHK070 - 是否定義配置管理員的角色與責任？[Completeness, User Story 5]
- [ ] CHK071 - 是否定義審查與核准流程中的角色（安全工程師、技術負責人、品質工程師）？[Completeness, User Stories]

### 工具與基礎設施

- [ ] CHK072 - 是否明確配置管理工具需求（Git）？[Completeness, Assumptions, Dependencies]
- [ ] CHK073 - 是否定義初期文件管理工具（Microsoft Word）與未來遷移工具（Codebeamer）？[Completeness, Assumptions, Dependencies, Clarifications]
- [ ] CHK074 - 是否定義 Codebeamer 部署的相依性與訓練需求？[Completeness, Dependencies]

---

## 相依性與假設（Dependencies & Assumptions）

### 外部相依性驗證

- [ ] CHK075 - PCIE Gen5 Controller 功能規格的可用性假設是否明確？[Completeness, Assumptions, Dependencies]
- [ ] CHK076 - ISO-26262 參考文件的可用性是否確認（`.specify/memory/ISO-26262-*.md`）？[Completeness, Dependencies]
- [ ] CHK077 - 外部稽核機構時程的相依性是否明確？[Completeness, Dependencies]
- [ ] CHK078 - 人力資源配置（功能安全工程師、品質工程師）的假設是否合理？[Assumption Validation, Assumptions]

### 假設合理性

- [ ] CHK079 - 團隊已接受 ISO-26262 與 ASPICE 訓練的假設是否合理？[Assumption Validation, Assumptions]
- [ ] CHK080 - Git 與 Word 工具已就緒的假設是否驗證？[Assumption Validation, Assumptions]
- [ ] CHK081 - 硬體設計為內部開發（排除供應商管理）的假設是否明確？[Assumption Validation, Scope]
- [ ] CHK082 - 網路資訊安全僅在與功能安全交互時考慮的假設是否合理？[Assumption Validation, Scope]

---

## 模糊性與衝突（Ambiguities & Conflicts）

### 未解決的模糊性

- [ ] CHK083 - "所有重要工作產品" 的定義是否明確？[Ambiguity, User Story 5, Scenario 1]
- [ ] CHK084 - "適當管理" 的標準是否定義？[Ambiguity, User Story 5, Independent Test]
- [ ] CHK085 - "整合者" 的角色與職責是否定義？[Ambiguity, Edge Cases, SC-008]
- [ ] CHK086 - "明確的流程定義與執行證據" 的具體內容是否明確？[Ambiguity, SC-006]

### 潛在衝突

- [ ] CHK087 - Word 格式與 Git 版本控制的兼容性問題是否考慮？[Conflict, Assumptions]
- [ ] CHK088 - 手動追溯矩陣維護與 95% 變更審查率的可行性是否評估？[Conflict, SC-005 vs FR-011]
- [ ] CHK089 - 2 週完成初稿與確保文件品質的平衡是否合理？[Conflict, SC-002]
- [ ] CHK090 - ASPICE Capability Level 2 與 1 個月文件計畫完成時限的可行性是否評估？[Conflict, SC-006 vs Assumptions]

---

## 範圍界定品質（Scope Definition Quality）

### 包含範圍清晰度

- [ ] CHK091 - In Scope 項目是否都有對應的功能需求或 User Story？[Traceability, Scope vs Requirements]
- [ ] CHK092 - PCIE Gen5 核心安全機制（CRC/ECC/鏈路訓練）是否在 In Scope 與 FR-012 中一致描述？[Consistency, Scope vs FR-012]
- [ ] CHK093 - 審查與核准流程是否在 In Scope 與 FR-010 中一致描述？[Consistency, Scope vs FR-010]

### 排除範圍合理性

- [ ] CHK094 - 排除硬體設計與實作的決定是否與 SEooC 概念一致？[Consistency, Scope vs User Story 1]
- [ ] CHK095 - 排除 Part 5/Part 6 詳細流程的決定是否與 Part 4 聚焦一致？[Consistency, Scope]
- [ ] CHK096 - 排除非核心安全機制（電源/時鐘/溫度監控）的決定是否與 ASIL B 範圍一致？[Consistency, Scope vs FR-012]
- [ ] CHK097 - 排除網路資訊安全詳細分析的決定是否有風險評估？[Gap, Scope]

---

## 合規性特定檢查（Compliance-Specific Checks）

### ISO-26262 Part 4 條款對應

- [ ] CHK098 - 需求是否涵蓋 Clause 6（產品開發：系統層級規格）？[Completeness, FR-001, FR-002]
- [ ] CHK099 - 需求是否涵蓋 Clause 7（產品開發：技術安全概念）？[Completeness, FR-012]
- [ ] CHK100 - 需求是否涵蓋 Clause 8（產品開發：系統設計）？[Gap, Scope - Out of Scope]
- [ ] CHK101 - 需求是否涵蓋 Clause 9（產品開發：驗證）？[Completeness, FR-005, User Story 4]

### ASPICE SWE 流程領域檢查

- [ ] CHK102 - SWE.1（軟體需求分析）流程需求是否定義？[Completeness, FR-003]
- [ ] CHK103 - SWE.2（軟體架構設計）流程需求是否定義？[Gap]
- [ ] CHK104 - SWE.3（軟體詳細設計與單元測試）流程需求是否定義？[Gap]
- [ ] CHK105 - SWE.4（軟體單元驗證）流程需求是否定義？[Completeness, FR-005]
- [ ] CHK106 - SWE.5（軟體整合與整合測試）流程需求是否定義？[Completeness, FR-005]
- [ ] CHK107 - SWE.6（軟體確認測試）流程需求是否定義？[Completeness, FR-005]

### ASIL B 技術措施

- [ ] CHK108 - 結構覆蓋率要求（100% 語句、100% 分支）是否符合 ASIL B 要求？[Compliance, SC-004]
- [ ] CHK109 - 診斷覆蓋率要求（90%）是否符合 ASIL B 高診斷覆蓋率推薦？[Compliance, FR-012, SC-004]
- [ ] CHK110 - 安全機制（CRC/ECC/錯誤偵測）需求是否滿足 ASIL B 最低要求？[Compliance, FR-012]

---

## 稽核準備特定項目（Audit Readiness）

### 文件追溯準備

- [ ] CHK111 - 規格是否明確定義所有需要稽核的工作產品清單？[Completeness, FR-002, SC-001]
- [ ] CHK112 - 追溯矩陣完整性驗證機制是否能支援稽核要求？[Completeness, SC-003]
- [ ] CHK113 - 是否定義稽核證據收集與管理流程？[Gap]

### 合規性聲明

- [ ] CHK114 - 規格是否明確聲明目標 ASIL 等級（ASIL B）？[Completeness, User Story 1, Scenario 1]
- [ ] CHK115 - 規格是否明確聲明符合 ISO-26262 Part 4？[Completeness, User Story 2, SC-001]
- [ ] CHK116 - 規格是否明確聲明 ASPICE 目標等級（Capability Level 2）？[Completeness, SC-006]

### 稽核時程準備

- [ ] CHK117 - 3 個月文件審查的準備需求是否定義？[Completeness, SC-002, Dependencies]
- [ ] CHK118 - 6 個月完整稽核的準備需求是否定義？[Completeness, Assumptions, Dependencies]
- [ ] CHK119 - 是否定義稽核不符合項的處理流程？[Gap]
- [ ] CHK120 - 是否定義稽核延遲或失敗的應對策略？[Gap, Risk Management]

---

## Notes

- ✅ 勾選項目表示該需求品質檢查已通過
- 📝 在項目下方添加審查發現或建議
- 🔗 連結至相關規格章節或參考文件
- 🚨 高風險項目（CHK001-020, CHK054-063, CHK098-110, CHK111-120）應優先檢查
- 📊 追溯性達標：120 項中 105 項（87.5%）包含明確的規格引用或標記
- 🎯 此檢查清單測試「需求本身的品質」，而非「實作是否符合需求」

**階段使用指引**:
- **作者自檢階段**：重點檢查 CHK001-063（需求完整性、清晰度、一致性、追溯性）
- **內部審查階段**：全面檢查 CHK001-120，特別關注 CHK064-097（非功能需求、相依性、範圍界定）
- **稽核準備階段**：強制檢查 CHK098-120（合規性特定項目、稽核準備），確保 CHK001-063 已 100% 通過
