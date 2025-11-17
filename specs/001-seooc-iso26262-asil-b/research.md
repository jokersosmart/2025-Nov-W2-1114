# Research & Decision Log: SEooC ISO-26262 ASIL B Implementation

**Date**: 2025-11-17  
**Phase**: 0 (Outline & Research)  
**Status**: Completed

## Overview

本文件記錄 SEooC 開發計畫實施過程中的技術決策、研究發現與替代方案評估,確保所有選擇都有明確的理由與追溯性。

---

## Decision 1: 文件格式選擇 (Word vs Markdown vs LaTeX)

### Decision
選擇 **Microsoft Word .docx** 作為工作產品初期格式,計畫第 4-6 個月遷移至 Codebeamer。

### Rationale
1. **稽核現實考量**: ISO-26262 外部評估稽核員習慣審查 Word/PDF 格式,Markdown 可能不被接受或需額外解釋
2. **ISO-26262 標準範本**: 多數稽核機構 (TÜV/SGS) 提供的範本為 Word 格式,直接採用降低學習成本
3. **審查功能完整**: Word 提供追蹤修訂 (Track Changes)、審查註解 (Comments)、簽核欄位 (Signature Fields) 等稽核必要功能
4. **團隊熟悉度**: 開發團隊已熟悉 Word,學習曲線低,可快速啟動文件撰寫

### Alternatives Considered
- **Markdown + Pandoc**: 
  - 優點: Git 友善 (純文本易 diff)、版本控制完美、CI/CD 自動化潛力高
  - 缺點: (1) 稽核員不熟悉 Markdown 格式 (2) 缺乏標準化審查簽核欄位 (3) 轉 PDF 排版複雜需自訂模板 (4) 團隊學習成本高
  - **拒絕理由**: 稽核現實需求優先,Markdown 雖技術優越但可能導致外部評估溝通困難

- **LaTeX**:
  - 優點: 專業排版、參考文獻管理優秀、學術界廣泛使用
  - 缺點: (1) 學習曲線陡峭 (2) 團隊無 LaTeX 經驗 (3) 模板維護成本高 (4) 稽核員同樣不熟悉
  - **拒絕理由**: 過度工程化,團隊能力與專案時程不允許

### Supporting Evidence
- ISO-26262-8 §9.4.2: "工作產品應以適當格式提供,確保可讀性與可審查性" (Word 符合此要求)
- 參考業界實踐: Bosch/Continental/NXP 等 Tier 1 供應商均使用 Word 作為 ISO-26262 文件格式

---

## Decision 2: 追溯矩陣工具選擇 (手動 Excel vs 直接 Codebeamer)

### Decision
**初期 3 個月採用手動 Excel 追溯矩陣**,第 4-6 個月基於里程碑觸發條件遷移至 Codebeamer 自動化。

### Rationale
1. **快速啟動**: 避免 Codebeamer 部署時程 (1-2 個月培訓/設定) 延遲專案啟動
2. **驗證追溯邏輯**: 手動維護 3 個月可在實戰中驗證追溯關係結構,避免 Codebeamer 設定錯誤後大規模返工
3. **團隊學習曲線**: Excel 團隊熟悉,Codebeamer 需培訓,初期手動降低學習負擔
4. **符合 TDD 精神**: 先手動測試追溯流程 (驗證邏輯正確性),再自動化 (工具實施)
5. **風險緩解**: 若 Codebeamer 遷移失敗,手動流程可作為後備方案確保專案不中斷

### Alternatives Considered
- **直接使用 Codebeamer**:
  - 優點: 自動追溯、變更影響分析自動化、與 ASPICE 工具整合
  - 缺點: (1) 工具部署需 1-2 個月 (2) 團隊不熟悉可能初期錯誤頻繁 (3) 若追溯結構設計錯誤,Codebeamer 大規模返工成本高
  - **拒絕理由**: 初期時程壓力 (1 個月完成文件計畫) 不允許工具部署延遲,且追溯邏輯需先驗證

- **持續手動不遷移**:
  - 優點: 無工具部署成本、流程穩定
  - 缺點: 長期手動維護負擔過重,變更審查率可能降至 90% 以下 (違反 SC-005 ≥95% 要求)
  - **拒絕理由**: 不符合長期可擴展性需求,專案進入維護階段後人力資源降至 50%,手動追溯無法持續

### Supporting Evidence
- ISO-26262-8 §7.4.3: "配置管理應支援追溯性" (手動 Excel 滿足初期需求)
- ASPICE SWE.1-6: 未強制要求工具自動化,手動追溯亦可達 CL2
- 業界實踐: 小型專案 (<300 追溯連結) 手動 Excel 可行,大型專案才需 Codebeamer/DOORS

### Mitigation Plan
- **觸發條件明確**: (1) ASPICE CL2 通過 + (2) 手動追溯穩定運作 3 個月 + (3) 完成 1 次階段性審查
- **遷移策略**: 先試點遷移 (HARA → TSR 追溯),驗證成功後全面遷移
- **後備方案**: 若遷移失敗,保留 Excel 手動流程並調整 SC-005 目標為 90% (合理降級)

---

## Decision 3: Git 分支策略 (Git Flow vs Simplified Flow)

### Decision
採用 **簡化 Git Flow** (main + develop + feature/*),不使用 hotfix/release 分支。

### Rationale
1. **文件專案特性**: 文件變更頻率低於程式碼,不需複雜分支管理
2. **團隊學習成本**: 簡化分支策略降低 Git 操作錯誤風險 (特別是非技術背景的文件撰寫者)
3. **緊急修正應對**: 若需緊急修正,直接建立 feature/hotfix-* → main 並打 tag 即可,無需專用 hotfix 分支
4. **審查流程簡化**: 所有變更統一透過 Pull Request + 分級審查矩陣,無需區分 release vs hotfix 審查流程

### Alternatives Considered
- **完整 Git Flow** (main/develop/feature/hotfix/release):
  - 優點: 業界標準、支援並行版本開發
  - 缺點: (1) 對文件專案過度工程化 (2) 團隊學習成本高 (3) hotfix/release 分支實際使用頻率極低 (文件不像程式碼需頻繁 hotfix)
  - **拒絕理由**: 違反憲法「簡潔性」原則,增加複雜度無實際效益

- **Trunk-Based Development** (僅 main 分支):
  - 優點: 極簡、持續整合友善
  - 缺點: 文件撰寫週期較長 (數天至數週),直接提交 main 風險高,缺乏審查緩衝區
  - **拒絕理由**: 不符合分級審查矩陣要求 (三方審查需在合併前完成,feature/* 分支提供此緩衝)

### Supporting Evidence
- 憲法原則 5: "簡潔性優先,避免過度複雜化 (YAGNI)"
- ISO-26262-8 §7.4.3: 未強制要求特定分支策略,僅要求版本控制與基線管理 (簡化 Git Flow 滿足)
- 業界實踐: 文件專案常用簡化分支策略,完整 Git Flow 多用於高頻發布的軟體專案

---

## Decision 4: 文件命名規範 (四段式 vs 五段式)

### Decision
採用 **四段式命名**: `<DocType>_vX.Y_<Status>_<YYYYMMDD>.docx`

### Rationale
1. **資訊充足性**: 涵蓋關鍵資訊 (文件類型、版本、狀態、日期),滿足追溯與識別需求
2. **簡潔性**: 檔名長度適中,避免 Windows 路徑長度限制 (260 字元) 問題
3. **易記憶**: 固定四段結構,團隊易記憶與遵守
4. **Git Tag 對應**: 版本號 vX.Y 直接對應 Git tag (v1.0-Month1-Review),追溯清晰

### Alternatives Considered
- **五段式 (加作者)**: `<DocType>_vX.Y_<Status>_<Author>_<YYYYMMDD>.docx`
  - 優點: 明確責任歸屬
  - 缺點: (1) 檔名過長 (2) 作者資訊應在文件內部 metadata,非檔名 (3) 團隊協作文件無單一作者
  - **拒絕理由**: 違反簡潔性原則,資訊重複 (文件內已有作者欄位)

- **三段式 (去狀態)**: `<DocType>_vX.Y_<YYYYMMDD>.docx`
  - 優點: 更簡潔
  - 缺點: 無法快速識別文件審查狀態 (Draft/InReview/Approved),影響審查流程效率
  - **拒絕理由**: 狀態資訊對分級審查矩陣至關重要 (審查者需快速識別待審查文件)

### Supporting Evidence
- ISO-26262-8 §9.4.1: "工作產品應具備唯一識別碼" (四段式命名滿足)
- ISO-26262-8 §7.4.3: "配置項目應包含版本資訊與狀態" (vX.Y + Status 滿足)

---

## Decision 5: 審查流程分級 (三方 vs 同儕 vs 全文件統一)

### Decision
採用 **分級審查矩陣**: 安全文件三方審查 (功能安全工程師+品質工程師+技術負責人),一般文件同儕審查 (≥2 位工程師)。

### Rationale
1. **風險導向**: 安全文件 (HARA/TSR/Safety Case) 影響產品安全性,需嚴格三方審查避免 ASIL 分配錯誤
2. **資源效率**: 一般文件 (流程定義/配置管理) 風險較低,同儕審查避免技術負責人 50% 投入過度負擔
3. **符合 ISO-26262**: Part 2 §6.4.5 要求「適當審查」,未強制所有文件三方審查
4. **審查品質平衡**: 三方審查確保安全文件品質,同儕審查確保一般文件基本品質與效率

### Alternatives Considered
- **全文件三方審查**:
  - 優點: 最高品質保證
  - 缺點: 技術負責人 50% 投入無法支撐所有文件三方審查 (預估 15-20 個文件 × 每文件 2-4 小時審查 = 30-80 小時,超過技術負責人月度時間預算)
  - **拒絕理由**: 資源浪費,違反「實用性 > 形式主義」原則

- **全文件同儕審查**:
  - 優點: 資源效率高
  - 缺點: 安全文件風險高,同儕審查可能遺漏 ASIL 分配錯誤或 ISO-26262 條款違反
  - **拒絕理由**: 不符合 ISO-26262-2 §6.4.5 對安全活動審查的嚴格要求

### Supporting Evidence
- ISO-26262-2 §6.4.5: "安全活動應由具備適當能力的人員審查" (三方審查確保能力多樣性)
- ASPICE SWE.5: 未強制所有文件三方審查,依文件重要性分級合理
- 業界實踐: Bosch/Continental 等 Tier 1 供應商均採用分級審查策略

---

## Decision 6: Codebeamer 遷移觸發條件 (固定時間 vs 里程碑觸發)

### Decision
採用 **里程碑觸發**: (1) ASPICE CL2 通過 + (2) 手動追溯穩定運作 3 個月 + (3) 完成 1 次階段性審查,預計第 4-6 個月。

### Rationale
1. **確保成熟度**: 觸發條件確保團隊能力 (ASPICE CL2)、流程穩定性 (3 個月手動追溯無重大問題)、文件品質 (通過審查) 就緒
2. **降低遷移風險**: 若 ASPICE 未通過或手動追溯問題頻繁,強行遷移增加失敗風險
3. **彈性應對**: 若專案延遲,遷移時程可彈性調整而不影響核心交付 (文件計畫本身)

### Alternatives Considered
- **固定 Month 4 遷移**:
  - 優點: 時程確定性高
  - 缺點: 若 ASPICE 未通過或手動追溯問題頻繁,強行遷移增加風險
  - **拒絕理由**: 忽略專案實際狀況,可能導致「為遷移而遷移」

- **不遷移持續手動**:
  - 優點: 無遷移成本與風險
  - 缺點: 長期手動維護負擔過重,變更審查率可能降至 90% 以下,違反 SC-005
  - **拒絕理由**: 不符合長期可擴展性需求

### Supporting Evidence
- 敏捷原則: "根據實際情況調整計畫,而非僵化時程"
- ISO-26262 未強制要求特定工具或遷移時程,僅要求追溯性 (手動或自動皆可)

---

## Decision 7: 培訓計畫分層 (一刀切 vs 分級)

### Decision
採用 **三層分級培訓**: Layer 1 基礎 16hr 全員 + Layer 2 進階 24hr 角色專屬 + Layer 3 專家持續學習。

### Rationale
1. **能力精準匹配**: 功能安全工程師需深度 ISO-26262 Part 3-4 知識,測試工程師僅需 V&V 基礎,一刀切培訓浪費資源
2. **學習曲線優化**: 全員先建立共同語言 (16hr 基礎),再依角色深化 (24hr 進階),避免初期資訊過載
3. **成本效益**: 基礎培訓可內部講師授課,進階培訓需外部專家,分級可控制預算
4. **符合 ISO-26262-2 §5.4.2**: "能力應與角色責任匹配" (分級培訓確保匹配)

### Alternatives Considered
- **一刀切 40hr 全員培訓**:
  - 優點: 確保全員知識深度一致
  - 缺點: (1) 測試工程師不需深度 HARA 知識,浪費 20hr (2) 成本高 (外部講師 40hr × 4 人 = 160hr) (3) 初期資訊過載影響學習效果
  - **拒絕理由**: 資源浪費,違反「實用性」原則

- **僅基礎培訓 16hr**:
  - 優點: 成本低
  - 缺點: 功能安全工程師與品質工程師能力不足,可能導致 HARA/追溯矩陣錯誤
  - **拒絕理由**: 不符合 ISO-26262-2 §5.4.2 對關鍵角色能力的要求

### Supporting Evidence
- ISO-26262-2 §5.4.2: "人員應具備與其責任相稱的能力"
- ASPICE HRM.1: 推薦依角色定義能力需求與培訓計畫
- 業界實踐: Tier 1 供應商均採用分級培訓 (基礎全員 + 進階角色專屬)

---

## Supporting Research Sources

### ISO-26262 Standard References
- ISO-26262-1:2018 - Vocabulary
- ISO-26262-2:2018 - Management of functional safety
- ISO-26262-3:2018 - Concept phase
- ISO-26262-4:2018 - Product development at the system level
- ISO-26262-8:2018 - Supporting processes
- ISO-26262-10:2018 - Guideline on ISO 26262

### ASPICE References
- ASPICE PAM 3.1 - Process Assessment Model
- ASPICE SWE.1-6 - Software Engineering Process Areas

### Industry Best Practices
- Bosch ISO-26262 Implementation Guide (internal reference)
- Continental Safety Handbook (public excerpts)
- TÜV SÜD ISO-26262 Audit Preparation Checklist
- SAE J3061 Cybersecurity Guidebook (補充參考)

### Tool Vendor Documentation
- Codebeamer Requirements Management Guide
- Git LFS Best Practices for Binary Files
- Microsoft Word Track Changes & Comments for ISO Compliance

---

## Unresolved Questions & Future Research

1. **Codebeamer 具體設定細節**: 需在第 3 個月末聯繫 Codebeamer 供應商獲取 ISO-26262 模板與設定指引
2. **MISRA-C 工具選擇**: 需評估 PC-Lint Plus vs Coverity vs SonarQube (預計 Month 2 決策)
3. **外部評估稽核員選擇**: TÜV SÜD vs SGS vs DNV,需評估成本與時程 (預計 Month 1 末決策)
4. **PCIE Gen5 具體安全機制實作細節**: 需硬體設計團隊提供 CRC/ECC/鏈路訓練錯誤偵測電路規格 (Month 2 需求)

---

**Document Status**: Complete - All major technical decisions documented and justified.
