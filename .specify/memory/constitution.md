<!--
Sync Impact Report

Version change: 1.0.0 -> 1.1.0

Modified principles:
- [PRINCIPLE_1_NAME] -> 函式庫優先 (Library-First)
- [PRINCIPLE_2_NAME] -> CLI 與文本協定 (CLI & Text Protocol)
- [PRINCIPLE_3_NAME] -> 測試優先（不可協商）(Test-First, Non-negotiable)
- [PRINCIPLE_4_NAME] -> 整合測試覆蓋 (Integration Testing)
- [PRINCIPLE_5_NAME] -> 可觀察性、版本管理與簡潔性 (Observability, Versioning, Simplicity)

Added sections:
- 技術限制與合規要求 (Technical Constraints & Compliance)
- 開發流程與品質閘 (Development Workflow & Quality Gates)
- Prompt 記錄與變更追蹤 (Prompt Recording & Change Tracking) - v1.1.0

Removed sections: none

Templates requiring updates:
- .specify/templates/plan-template.md : ✅ updated (Constitution Check 已明確化五大原則檢查項)
- .specify/templates/spec-template.md : ✅ updated (User Scenarios 註解中已強化 TDD 與驗收測試要求)
- .specify/templates/tasks-template.md : ✅ updated (Polish 階段已補充可觀察性與版本管理任務範例)
- .specify/templates/agent-file-template.md : ✅ updated (前言已加入憲法原則引用說明)
- .specify/templates/checklist-template.md : ✅ updated (新增憲法合規檢查項範例)

Follow-up TODOs:
- TODO(RATIFICATION_DATE): 原始批准日期未知，請提供 YYYY-MM-DD 或維持 TODO
-->

# Spec Constitution

## Core Principles

### 函式庫優先 (Library-First)
所有新功能須首先以獨立、可重用的函式庫形式設計並實作。
- MUST: 每個函式庫需具備獨立測試、清楚的介面與文件。
- MUST: 函式庫不得僅為組織內部用途而存在；若無明確外部價值，須重審設計。
理由：採用函式庫優先可提高模組化與可測試性，降低系統複雜度。

### CLI 與文本協定 (CLI & Text Protocol)
所有主要工具與函式庫應至少提供命令列或文本介面以利自動化與 CI 整合。
- SHOULD: 支援 stdin/args → stdout，錯誤輸出至 stderr；並提供 JSON 與人類可讀輸出格式。
- MUST: 所有 CLI 行為應在文件與測試中明確記錄，包含主要輸入/輸出範例。
理由：文本介面提高可自動化程度並促進觀察與偵錯。

### 測試優先（不可協商）(Test-First, Non-negotiable)
測試驅動開發為本專案基本要求，所有新功能與修正皆應遵循 TDD 流程。
- MUST: 撰寫測試 → 測試失敗 → 實作 → 測試通過 → 重構。
- MUST: 每個功能的使用者情境（user scenario）需至少有一個可自動化的驗收測試。
- **例外**: 對於**文件開發專案**（如流程文件、稽核文件、合規模板等），測試優先原則應調整為**驗證優先（Verification-First）**（詳見「專案類型定義與測試優先原則適用性」章節）。
理由：保證品質、可回溯性與安全的變更流程。

### 整合測試覆蓋 (Integration Testing)
針對跨模組契約、外部整合與關鍵流程必須建立整合測試。
- MUST: 任何改動會影響外部介面或契約時，需新增或更新相對的整合測試。
- SHOULD: 使用契約測試（contract tests）來捕捉介面不一致或向後相容性問題。
理由：降低部署風險並確保跨元件正確運作。

### 可觀察性、版本管理與簡潔性 (Observability, Versioning, Simplicity)
系統與工具應易於觀察、版本管理清晰且設計追求簡潔。
- MUST: 重要操作與錯誤須有結構化日誌與足夠的追蹤資料（trace/context）。
- MUST: 版本策略採 Semantic Versioning（MAJOR.MINOR.PATCH）；breaking changes 將升級 MAJOR。
- SHOULD: 優先簡潔設計，避免過早優化或引入不必要的複雜性（YAGNI 原則）。
理由：可觀察性與明確版本管理降低維運成本；簡潔性提升開發速度與可理解度。

## 技術限制與合規要求
本專案偏好跨平台、低鎖定（lock-in）的解決方案，並採取基本安全與合規準則：
- SHOULD: 優先使用跨平台工具與標準（例如 HTTP/JSON、CLI）。
- MUST: 涉及密鑰或敏感資料時，遵循組織安全標準（不可把敏感資料寫入 repo）。
- TODO: 若專案需遵守特定合規（例如 GDPR、SOC2），請在此明確列出合規條款與責任人。

## 專案類型定義與測試優先原則適用性

### 專案類型分類
本組織承認不同性質專案需要不同的品質保證策略。專案類型分為以下兩大類：

#### 1. 應用程式開發專案 (Application Development Projects)
定義：開發可執行軟體、函式庫、API、工具或服務的專案。
- 範例：Web 應用程式、CLI 工具、SDK、微服務、自動化腳本
- 特徵：產出為**程式碼**，可透過自動化測試驗證行為
- **測試優先原則適用性**：✅ **完全適用**
  * MUST: 遵循 TDD 流程（寫測試 → 失敗 → 實作 → 通過 → 重構）
  * MUST: 每個 User Story 至少有一個自動化驗收測試
  * MUST: 整合測試覆蓋跨模組契約與外部整合

#### 2. 文件開發專案 (Documentation Development Projects)
定義：開發流程文件、合規文件、稽核模板、標準作業程序（SOP）、政策文件或技術文件的專案。
- 範例：ISO-26262 合規文件模板、品質管理系統文件、稽核檢查表、流程手冊、訓練教材
- 特徵：產出為**文件內容**（Word、Markdown、PDF 等），無法透過傳統單元測試或整合測試驗證
- **測試優先原則適用性**：⚠️ **調整為驗證優先（Verification-First）**
  * MUST: 遵循**檢查表驅動開發（Checklist-Driven Development, CDD）**流程：
    1. 撰寫合規檢查表（Compliance Checklist）→ 定義驗證標準
    2. 建立文件模板（Document Template）→ 實作內容結構
    3. 試點驗證（Pilot Validation）→ 以實際案例測試模板
    4. 檢查表驗證（Checklist Verification）→ 依檢查表審查試點輸出
    5. 模板修訂（Template Revision）→ 根據驗證結果改進
  * MUST: 每個文件模板至少有一個**三方審查流程**（作者 → 審查者 → 批准者）並定義明確驗收標準
  * SHOULD: 使用自動化工具驗證可驗證項目（例如：文件結構完整性、必填欄位存在性、引用連結有效性）
  * MUST: 整合測試調整為**跨文件一致性驗證**（例如：追溯矩陣完整性、版本號一致性、術語表對齊）

### 專案類型判定流程
每個新專案在立項階段（Specification Phase）必須明確聲明專案類型：

1. **在 `spec.md` 的 Project Overview 章節中聲明**：
   ```markdown
   ## Project Overview
   **專案類型**: [應用程式開發專案 / 文件開發專案]
   **測試策略**: [TDD / Verification-First (CDD)]
   ```

2. **在 `plan.md` 的 Constitution Check 章節中說明測試策略調整**：
   - 應用程式開發專案：說明如何實施 TDD
   - 文件開發專案：說明如何實施 CDD，並映射「測試優先」原則到「驗證優先」

3. **在 `tasks.md` 中反映相應任務順序**：
   - 應用程式開發專案：測試任務先於實作任務
   - 文件開發專案：檢查表建立任務先於模板建立任務

### 憲法原則對應關係（文件開發專案）

| 憲法原則 | 應用程式開發專案 | 文件開發專案 |
|---------|----------------|------------|
| **測試優先** | 單元測試 → 整合測試 → 驗收測試 | 合規檢查表 → 試點驗證 → 三方審查 |
| **函式庫優先** | 可重用程式碼模組 | 可重用文件模板與標準段落 |
| **CLI 與文本協定** | CLI 工具（stdin/stdout） | 文件生成腳本、驗證腳本（支援批次處理） |
| **整合測試** | API 契約測試、E2E 測試 | 跨文件追溯性驗證、術語一致性檢查 |
| **可觀察性** | 結構化日誌、追蹤資料 | 文件變更歷史、審查紀錄、版本追蹤 |

### 混合型專案處理
若專案同時包含應用程式開發與文件開發（例如：開發文件生成工具 + 產出合規文件模板），則：
- MUST: 在 `spec.md` 中明確劃分兩類任務範圍
- MUST: 應用程式部分遵循 TDD，文件部分遵循 CDD
- MUST: 在 `tasks.md` 中使用標籤區分任務類型（例如：`[CODE]` vs `[DOC]`）



## 開發流程與品質閘
開發流程需包含明確的評審與 CI 品質閘:
- MUST: 所有變更以 PR 方式提交,並至少通過一位審查者的核准(或維護者群組規則)。
- MUST: PR 必須通過自動測試與 linters;若有憲法違反(Constitution Check),需在 PR 描述中說明豁免理由。
- SHOULD: 週期性(建議每 6 個月)檢視憲法與治理規則。

### Prompt 記錄與變更追蹤 (Prompt Recording & Change Tracking)
所有與 AI 助理的互動都必須記錄並納入版本管理,確保完整的追溯性。
- MUST: 每次與 AI 助理互動後,必須將 Prompt 與執行動作記錄到 `.specify/memory/Prompt.md`。
- MUST: Prompt.md 的每次更新都必須獨立 commit,commit message 格式為 `docs: update Prompt.md - [簡短描述]`。
- MUST: Prompt.md 記錄格式應包含:
  * 日期時間戳記 (YYYY-MM-DD HH:MM)
  * 使用者 Prompt 原文
  * 執行動作摘要
  * 相關 Commits 列表
  * 結果與影響範圍
- SHOULD: 重大互動(例如規格制定、架構決策)應在記錄中標註 **重要** 或類似標記。
理由: 確保所有決策過程可追溯,團隊成員可理解每個變更的背景與理由,避免知識流失。

## Governance
憲法為專案治理的最高指引；任何修訂需遵循以下程序：
- 提案（PR）：提出修訂內容、變更理由與遷移計畫（若有）並標記為 `governance` 類別。
- 審核：維護者或治理委員會審查並在 PR 中討論；重大改動需至少 2 位維護者同意。
- 版本與發佈：修訂完成後，更新 `CONSTITUTION_VERSION` 與 `Last Amended` 日期並在 PR 說明中記錄變更摘要。

版本策略與小節：
- MAJOR: 當移除或不相容重新定義原則時。 (向下不相容)
- MINOR: 當新增原則或擴展既有指引時（向後相容）。
- PATCH: 文辭修正、錯字或小幅澄清。

**Version**: 1.2.0 | **Ratified**: TODO(RATIFICATION_DATE): 原始批准日期未知 | **Last Amended**: 2025-11-17
