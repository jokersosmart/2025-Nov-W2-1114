# Implementation Plan: SEooC 開發計畫 (ISO-26262 ASIL B + ASPICE)

**Branch**: `001-seooc-iso26262-asil-b` | **Date**: 2025-11-17 | **Spec**: [spec.md](./spec.md)
**Input**: Feature specification from `/specs/001-seooc-iso26262-asil-b/spec.md`

**Note**: This is a documentation/process implementation plan for ISO-26262 ASIL B compliance project targeting PCIE Gen5 Controller development.

## Summary

建立符合 ISO-26262 ASIL B 等級與 ASPICE 相容的 SEooC (Safety Element out of Context) 開發計畫文件系統,針對 PCIE Gen5 Controller 晶片。核心目標:

1. **文件模板系統**: 提供所有 ISO-26262 Part 4 必要工作產品的 Word 模板 (SEooC 假設、安全計畫、HARA、TSR、V&V 計畫、Safety Case、追溯矩陣、配置管理計畫)
2. **流程定義**: 定義需求管理、安全分析、測試驗證、配置管理與審查流程,確保符合 ISO-26262 與 ASPICE SWE.1-6 要求
3. **追溯機制**: 建立需求↔安全目標↔設計↔測試的雙向追溯系統 (初期手動,後續 Codebeamer 自動化)
4. **分級培訓計畫**: 三層培訓架構 (基礎 16hr + 進階 24hr + 專家持續學習) 確保團隊能力
5. **風險應變策略**: 涵蓋工具失效、流程瓶頸、技術挑戰、標準衝突等情境的應對措施

**Technical Approach**: 採用 Microsoft Word + Git + Excel 作為初期工具鏈,建立標準化四段式工作產品命名規範,使用簡化 Git Flow 分支策略,第 4-6 個月基於里程碑觸發 Codebeamer 遷移。文件計畫採分階段交付 (Month 1/2/3 審查),外部評估採 2 階段準備 (3 個月文件審查 + 6 個月完整評估)。

## Technical Context

**Project Type**: Documentation/Process Management (ISO-26262 Compliance Project)
**Document Format**: Microsoft Word .docx (初期) → Codebeamer (第 4-6 個月遷移)
**Version Control**: Git (簡化 Git Flow: main + develop + feature/*)
**Traceability Tool**: Manual (Excel/Word 初期 3 個月) → Codebeamer automated (第 4-6 個月)
**Target Standard**: ISO-26262:2018 ASIL B + ASPICE Capability Level 2
**Target Product**: PCIE Gen5 Controller (SEooC 模式開發)

**Primary Deliverables**:
- Work Product Templates: 9 個核心模板 (SEooC Assumption, Safety Plan, HARA Report, TSR, V&V Plan, Safety Case, Traceability Matrix, CM Plan, Review Records)
- Process Definitions: 需求管理 / 安全分析 (HARA) / V&V (四階段測試) / 配置管理 (Git) / 審查流程 (分級審查矩陣)
- Checklists: 外部審查準備 25 項檢查清單 + 整合者檢查清單 15-20 項
- Training Materials: 三層培訓計畫 (基礎/進階/專家) + 能力驗證標準

**Technical Dependencies**:
- ISO-26262:2018 Standard (Parts 1-10, 特別是 Part 2/3/4/8/10)
- ASPICE PAM 3.1 (SWE.1-6 流程領域)
- Git for version control (LFS for Word files)
- Microsoft Word for work products
- Excel for manual traceability matrix (初期)
- Codebeamer (計畫第 4-6 個月遷移,觸發條件: ASPICE CL2 通過 + 3 個月手動追溯穩定運作)

**Team Composition** (依假設 4):
- 功能安全工程師 (Functional Safety Engineer): 1 人 100% (核心 3 個月) → 50% (維護階段)
- 品質工程師 (Quality Engineer): 1 人 100% (核心 3 個月,兼追溯矩陣維護) → 50% (維護階段)
- 技術負責人 (Technical Lead): 1 人 50% (全程,審查與技術決策)
- 測試工程師 (Test Engineer): 1-2 人 (V&V 階段投入)

**Performance Goals**:
- 文件計畫完成: 1 個月內完成核心模板與流程框架 (Layer 1-2 完成標準)
- 階段性審查: Month 1/2/3 三階段審查通過 (0 重大缺陷)
- 追溯性完整性: 100% 雙向追溯 (需求↔安全目標↔設計↔測試)
- 變更審查率: ≥95% 變更包含四維度影響分析
- 外部評估: 通過 ISO-26262 ASIL B 認證 (3 個月文件審查 + 6 個月完整評估)

**Constraints**:
- 文件格式: 初期限定 Microsoft Word .docx (與 Git 兼容性問題需管理)
- 人力資源: 核心團隊 3 人 (功能安全工程師 + 品質工程師 + 技術負責人)
- 時程壓力: 1 個月完成文件計畫 (ASPICE CL2 流程定義 + ISO-26262 模板)
- 工具遷移不確定性: Codebeamer 部署時程可能延遲,需後備方案 (Word/Excel 持續)
- 安全覆蓋率要求: 結構覆蓋率 100% (語句+分支) + 功能覆蓋率 100% + 診斷覆蓋率 90%

**Scale/Scope**:
- 工作產品數量: ~15-20 個核心文件 (模板 + 實例)
- 追溯關係: 預估 200-300 個追溯連結 (需求/安全目標/設計/測試)
- 流程定義: 6 個主要流程 (需求管理/HARA/V&V/CM/審查/變更控制)
- 檢查清單: 3 個主要清單 (外部審查 25 項 + 整合者檢查 15-20 項 + 培訓驗證標準)
- 培訓時數: 基礎 16hr (全員) + 進階 24hr (角色專屬) + 新人 8hr 快速通道

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

依據專案憲法 `.specify/memory/constitution.md` **v1.2.0** (2025-11-17 修訂)，此功能符合以下原則：

**專案類型聲明**: 本專案為**文件開發專案 (Documentation Development Project)**，依憲法 v1.2.0 §專案類型定義與測試優先原則適用性，採用**驗證優先 (Verification-First)** 替代傳統測試優先 (TDD)，實踐方式為**檢查表驅動開發 (Checklist-Driven Development, CDD)**。

### ✅ 函式庫優先 (Library-First)
**狀態**: 部分適用 (文件專案特性)
- **說明**: 本專案產出為文件模板與流程定義,非傳統軟體函式庫。但**文件模板本身可視為「可重用元件」**,每個模板 (如 SEooC Assumption Template, HARA Template) 具備獨立性、清楚結構與使用說明
- **符合性**: 
  - ✅ 每個模板具備獨立文件 (使用指引、填寫範例、審查標準)
  - ✅ 模板設計通用化,可供其他 SEooC 專案重用 (不限於 PCIE Gen5)
  - ✅ 流程定義 (如 HARA 流程、V&V 流程) 模組化,可獨立執行與驗證
- **限制**: 文件模板無法如程式碼般自動化測試,但透過「試點驗證」與「團隊演練」確保可用性

### ✅ CLI 與文本協定 (CLI & Text Protocol)
**狀態**: 適用於工具鏈部分
- **說明**: 文件專案主體為 Word/PDF,但**配套工具與自動化腳本須遵循 CLI 原則**
- **符合性**:
  - ✅ Git 版本控制採用標準 CLI 命令 (git commit/push/tag)
  - ✅ 追溯矩陣驗證腳本 (計畫開發) 將提供 CLI 介面與 JSON 輸出
  - ✅ 文件命名規範驗證腳本 (計畫開發) 支援批次檢查與 CI 整合
  - ⚠️ Microsoft Word 本身非 CLI 工具,但重要版本同步匯出 PDF (自動化可行)
- **Codebeamer 遷移後**: API 自動化追溯矩陣生成與報告

### ✅ 測試優先（不可協商）(Test-First)
**狀態**: 調整為「驗證優先」(Verification-First) 適用於文件專案
- **說明**: 文件專案的「測試」等同於「審查驗證」,採用**「檢查清單驅動開發」**
- **符合性**:
  - ✅ **模板設計前先定義檢查清單**: 每個模板撰寫前,先建立對應的 ISO-26262 合規檢查清單 (如 SEooC Assumption 需包含 4 類假設、HARA 需包含 S/E/C 評估)
  - ✅ **試點驗證循環**: 模板初稿 → 試點填寫 → 檢查清單驗證 → 發現缺失 → 模板修訂 → 重新驗證 (TDD 精神)
  - ✅ **審查作為驗收測試**: 每個 User Story 的 Acceptance Scenarios 定義明確驗收標準 (如「三方審查無重大缺陷」)
  - ✅ **自動化驗證腳本**: 開發文件命名規範驗證、追溯完整性驗證等自動化檢查工具
- **驗證層次**:
  1. **靜態驗證**: 檢查清單逐項勾選 (如 25 項外部審查檢查清單)
  2. **試點驗證**: 團隊成員實際填寫模板並評估可用性
  3. **審查驗證**: 分級審查矩陣 (三方審查確認符合 ISO-26262)

### ✅ 整合測試覆蓋 (Integration Testing)
**狀態**: 適用於跨文件追溯性驗證
- **說明**: 文件專案的「整合測試」為**追溯矩陣完整性驗證**與**跨流程一致性檢查**
- **符合性**:
  - ✅ **追溯矩陣驗證**: 自動化檢查需求↔安全目標↔設計↔測試的雙向連結完整性 (目標 100%)
  - ✅ **跨文件一致性測試**: 驗證 HARA 報告中的危害是否完整映射至安全目標文件
  - ✅ **命名規範一致性**: 驗證所有工作產品遵循四段式命名 `<DocType>_vX.Y_<Status>_<YYYYMMDD>.docx`
  - ✅ **Git Tag 與文件版本對應**: 驗證 Git tag `v1.0-Month1-Review` 對應的所有文件均為 `v1.0_Approved` 狀態
- **整合測試範例**:
  - 契約測試: HARA 輸出 (安全目標清單) 與 TSR 輸入 (安全目標引用) 的介面一致性
  - 端到端測試: 模擬從 SEooC 假設 → HARA → TSR → V&V Plan → Safety Case 的完整文件流程

### ✅ 可觀察性、版本管理與簡潔性
**狀態**: 完全符合
- **可觀察性**:
  - ✅ **結構化文件記錄**: 每個文件包含修訂歷史表、審查記錄、變更理由
  - ✅ **Git 提交日誌**: 所有變更透過 Git commit message 記錄理由與影響範圍
  - ✅ **月度 Dashboard**: 品質工程師彙總 SC-001~008 達成率儀表板 (可視化追蹤)
  - ✅ **審查缺陷追蹤**: 所有審查發現的缺陷記錄於 Review Records 並追蹤狀態
- **版本管理**:
  - ✅ **Semantic Versioning 調整為文件版本**: vX.Y (X=主版本審查通過時遞增, Y=次版本內容修改時遞增)
  - ✅ **Git Tag 基線管理**: 每月末審查通過後打 tag (v1.0-Month1-Review, v2.0-Month2-Review)
  - ✅ **狀態標籤清晰**: Draft → InReview → Reviewed → Approved 四狀態明確
  - ✅ **Breaking Changes 管理**: 安全假設變更、ASIL 等級調整等視為 MAJOR 變更,需重新審查受影響文件
- **簡潔性**:
  - ✅ **四段式命名避免過度複雜**: `<DocType>_vX.Y_<Status>_<YYYYMMDD>.docx` 簡潔且資訊充足
  - ✅ **簡化 Git Flow**: 僅 main + develop + feature/*,避免 hotfix/release 複雜分支
  - ✅ **模板設計遵循 YAGNI**: 僅包含 ISO-26262 必要元素,不過度設計 (如避免引入不必要的表格或章節)
  - ✅ **流程定義避免過度形式主義**: 「實用性 > 形式主義」原則 (如手動追溯矩陣採批次更新而非即時更新)

---

### 總體評估: ✅ 通過憲法檢查

**調整說明**:
- 原則 1-2 針對文件專案特性調整解釋,但核心精神 (可重用性、自動化) 符合
- 原則 3-5 完全適用,透過「檢查清單驅動」、「追溯驗證」、「版本管理」體現

**無需 Complexity Tracking**: 所有原則皆符合或合理調整,無違反情況

## Project Structure

### Documentation (this feature)

```text
specs/001-seooc-iso26262-asil-b/
├── spec.md                    # ✅ Feature specification (已完成, 81.5% 完整性)
├── plan.md                    # ✅ This implementation plan (Phase 1 輸出)
├── research.md                # Phase 0 輸出 - 研究決策記錄
├── data-model.md              # Phase 1 輸出 - 文件模型與追溯關係
├── quickstart.md              # Phase 1 輸出 - 快速入門指南
├── contracts/                 # Phase 1 輸出 - 文件介面契約
│   ├── seooc-assumption-schema.md
│   ├── hara-report-schema.md
│   ├── tsr-schema.md
│   ├── vv-plan-schema.md
│   ├── safety-case-schema.md
│   └── traceability-matrix-schema.md
├── tasks.md                   # Phase 2 輸出 (/speckit.tasks command)
├── checklists/                # ✅ 品質檢查清單 (已完成 5 份)
│   ├── requirements-quality.md       # ✅ 143 items
│   ├── process-compliance.md         # ✅ 120 items
│   ├── traceability.md               # ✅ 112 items
│   ├── safety-analysis.md            # ✅ 127 items
│   └── verification-validation.md    # ✅ 128 items
└── validation-report.md       # ✅ 全面驗證報告 (51.6% 通過率)
```

### Source Code (repository root) - 文件專案調整結構

**說明**: 本專案為文件/流程管理專案,非傳統軟體開發,因此調整為文件導向結構

```text
# 文件專案結構 (Documentation Project Structure)
docs/
├── templates/                 # Phase 1-2 輸出 - 工作產品模板
│   ├── seooc-assumption-template.docx
│   ├── safety-plan-template.docx
│   ├── hara-report-template.docx
│   ├── tsr-template.docx
│   ├── vv-plan-template.docx
│   ├── safety-case-template.docx
│   ├── traceability-matrix-template.xlsx
│   ├── cm-plan-template.docx
│   └── review-record-template.docx
├── processes/                 # Phase 1-2 輸出 - 流程定義文件
│   ├── requirements-management-process.md
│   ├── hara-process.md
│   ├── vv-process.md
│   ├── configuration-management-process.md
│   ├── review-approval-process.md
│   └── change-control-process.md
├── guidelines/                # Phase 1-2 輸出 - 使用指引
│   ├── template-usage-guide.md
│   ├── naming-convention-guide.md
│   ├── git-workflow-guide.md
│   ├── review-matrix-guide.md
│   └── codebeamer-migration-guide.md
├── training/                  # Phase 2 輸出 - 培訓材料
│   ├── layer1-basic-training.md (16hr 課程大綱)
│   ├── layer2-advanced-training.md (24hr 角色專屬)
│   ├── layer3-expert-training.md (持續學習)
│   ├── new-hire-fast-track.md (8hr 快速通道)
│   └── competency-assessment-criteria.md
└── examples/                  # Phase 2 輸出 - 填寫範例
    ├── seooc-assumption-example.docx (PCIE Gen5)
    ├── hara-report-example.docx (PCIE Gen5)
    └── traceability-matrix-example.xlsx

scripts/                       # Phase 2 輸出 - 自動化腳本
├── validate-naming.ps1        # 驗證文件命名規範
├── check-traceability.ps1     # 驗證追溯矩陣完整性
├── generate-dashboard.ps1     # 生成 SC 達成率儀表板
└── git-tag-baseline.ps1       # 自動化基線 tag 管理

# Git 版本控制結構 (已存在)
.git/                          # Git 版本庫
.gitattributes                 # Git LFS 設定 (管理 Word 大檔案)
README.md                      # 專案總覽
CHANGELOG.md                   # 版本變更日誌
```

**Structure Decision**: 
- **選擇文件專案結構** (非 Option 1/2/3): 因本專案核心產出為文件模板、流程定義與培訓材料,而非軟體程式碼
- **docs/ 為主要目錄**: 包含所有工作產品模板、流程文件、使用指引、培訓材料與範例
- **scripts/ 為輔助目錄**: 提供文件驗證、追溯檢查、儀表板生成等自動化工具 (符合 CLI 原則)
- **Git 管理整體專案**: 使用 Git LFS 管理 Word/Excel 二進位檔案,避免儲存庫膨脹
- **未來擴展**: Codebeamer 遷移後,docs/ 內容將同步至 Codebeamer,scripts/ 調整為 API 整合腳本

## Complexity Tracking

> **本專案無憲法違反情況,但記錄關鍵設計權衡決策以提升透明度**

| 設計選擇 | 選擇理由 | 替代方案被拒絕原因 |
|---------|----------|-------------------|
| Microsoft Word 作為初期文件格式 | ISO-26262 稽核現實:稽核員習慣審查 Word/PDF,Markdown 可能不被接受。Word 提供審查標註、簽核欄位、追蹤修訂等稽核必要功能 | **Markdown 替代方案被拒**: 雖 Git 友善、純文本易版本控制,但:(1) 稽核員不熟悉 Markdown (2) 缺乏審查簽核欄位標準化 (3) 轉換為 PDF 排版複雜。**LaTeX 替代方案被拒**: 學習曲線陡峭,團隊不熟悉,模板維護成本高 |
| 手動追溯矩陣 (Excel) 初期 3 個月 | 快速啟動,避免工具部署延遲專案。團隊熟悉 Excel,學習曲線低。可在 3 個月實戰中驗證追溯結構,避免 Codebeamer 設定錯誤 | **直接 Codebeamer 替代方案被拒**: (1) 工具部署需 1-2 個月 (培訓/設定/驗證) (2) 團隊不熟悉可能導致初期錯誤頻繁 (3) 手動驗證追溯邏輯更符合 TDD 精神 (先手動測試再自動化) |
| 簡化 Git Flow (僅 main + develop + feature/*) | 減少分支管理複雜度,避免團隊混淆。文件專案變更頻率低於程式碼,不需 hotfix/release 分支 | **完整 Git Flow 替代方案被拒**: hotfix 與 release 分支對文件專案過度工程化,增加學習成本與操作錯誤風險。若需緊急修正,直接 feature/* → main 並打 tag 即可 |
| 四段式文件命名 (非五段式或更複雜) | 平衡資訊充足與簡潔性。`<DocType>_vX.Y_<Status>_<YYYYMMDD>` 涵蓋關鍵資訊 (類型/版本/狀態/日期) 且易記憶 | **五段式 (加作者) 替代方案被拒**: 檔名過長,Git 與 Windows 路徑長度限制問題。作者資訊應在文件內 metadata,非檔名。**三段式 (去狀態) 替代方案被拒**: 無法快速識別文件審查狀態,影響審查流程效率 |
| 分級審查矩陣 (三方 vs 同儕) | 平衡品質與效率。安全文件需嚴格三方審查 (功能安全工程師+品質工程師+技術負責人),一般文件同儕審查避免過度負擔 | **全文件三方審查替代方案被拒**: 資源浪費,技術負責人 50% 投入無法支撐所有文件三方審查。**全文件同儕審查替代方案被拒**: 安全文件風險高,僅同儕審查可能遺漏 ASIL 分配錯誤或 ISO-26262 條款違反 |
| Codebeamer 遷移基於里程碑觸發 (非固定時間) | 確保遷移時機成熟,避免過早遷移導致流程不穩定。觸發條件 (ASPICE CL2 + 3個月手動追溯穩定) 確保團隊能力與流程就緒 | **固定 Month 4 遷移替代方案被拒**: 若 ASPICE 未通過或手動追溯問題頻繁,強行遷移增加風險。**不遷移持續手動替代方案被拒**: 長期手動追溯維護負擔過重 (變更審查率可能降至 90% 以下),影響 SC-005 達成 |

---

**設計權衡總結**:
- **實用主義 > 理想主義**: 選擇 Word/Excel 而非 Markdown/純工具,因稽核現實需求
- **漸進式改善**: 手動追溯 → Codebeamer 遷移,而非一步到位 (符合敏捷精神)
- **簡潔性優先**: 簡化 Git Flow、四段式命名,避免過度複雜化
- **風險導向分級**: 審查矩陣分級 (三方 vs 同儕),避免資源浪費

所有權衡決策符合憲法原則 (特別是「簡潔性」與「實用性 > 形式主義」),無違反情況。
````
