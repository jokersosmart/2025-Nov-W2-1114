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

**Version**: 1.1.0 | **Ratified**: TODO(RATIFICATION_DATE): 原始批准日期未知 | **Last Amended**: 2025-11-17
