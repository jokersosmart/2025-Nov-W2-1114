# Prompt 記錄

本檔案記錄所有與 AI 助理的互動 prompt 與時間點，確保每個動作都有完整的追溯紀錄。

---

## 2025-11-14 16:00 - Constitution 更新與 Templates 同步

**使用者 Prompt**:
```
Follow instructions in [speckit.constitution.prompt.md](file:///c%3A/Users/joker.kang/SDD/2025-Nov-W2-1113/.github/prompts/speckit.constitution.prompt.md).
每做一個動作都要記得 commit 確保每一個動作都有被記錄下來，而且也會需要一個 Prompt 被記錄下來~
所以我會需要另外一個檔案去記錄我在什麼時間點下什麼 Prompt 給你的，名稱就要 Prompt.md 好了一樣放在
```

**執行動作**:
1. 依照 `speckit.constitution.prompt.md` 指示更新 `.specify/memory/constitution.md`（版本 1.0.0）
2. 同步更新五個 templates 以對齊憲法原則：
   - `.specify/templates/plan-template.md`（明確化 Constitution Check）
   - `.specify/templates/spec-template.md`（強化 TDD 與測試要求）
   - `.specify/templates/tasks-template.md`（補充可觀察性與版本管理任務）
   - `.specify/templates/agent-file-template.md`（加入憲法引用說明）
   - `.specify/templates/checklist-template.md`（新增憲法合規檢查項）
3. 更新 constitution.md 的 Sync Impact Report（標記所有 templates 為 ✅ updated）
4. 建立本檔案 `Prompt.md` 記錄 prompt 歷史

**相關 Commits**:
- `8bc95b1` - docs: amend constitution to v1.0.0 (initial principles + governance)
- `e8c57d0` - docs: sync templates with constitution v1.0.0 (align all templates with governance principles)
- `7469633` - docs: add Prompt.md for tracking AI interactions
- `628c90b` - docs: move Prompt.md to .specify/memory/ (align with constitution.md location)

**結果**:
- 憲法版本：1.0.0（繁體中文）
- 所有 templates 已對齊憲法原則
- Sync Impact Report 已完成
- Prompt.md 已建立並移至 `.specify/memory/` 資料夾
- 待處理：`TODO(RATIFICATION_DATE)` 需人工提供批准日期

---

## 2025-11-14 16:05 - 移動 Prompt.md 至 memory 資料夾

**使用者 Prompt**:
```
我想要放在 memory 資料夾底下，跟 #file:constitution.md 一樣
```

**執行動作**:
1. 使用 `git mv` 將 `Prompt.md` 移至 `.specify/memory/Prompt.md`
2. Commit 變更

**相關 Commits**:
- `628c90b` - docs: move Prompt.md to .specify/memory/ (align with constitution.md location)

**結果**:
- Prompt.md 現位於 `.specify/memory/` 資料夾，與 constitution.md 同一位置

---

## 2025-11-14 16:10 - 建立 Prompt 同步更新機制

**使用者 Prompt**:
```
Follow instructions in [speckit.constitution.prompt.md](file:///c%3A/Users/joker.kang/SDD/2025-Nov-W2-1113/.github/prompts/speckit.constitution.prompt.md).
每下一次 Prompt 就必須同步更新 Prompt.md 記錄
```

**執行動作**:
1. 更新 `Prompt.md` 記錄本次互動
2. Commit 變更
3. 建立機制：未來每次收到新 prompt，必須先更新此檔案並 commit

**相關 Commits**:
- （待建立）- docs: update Prompt.md - establish prompt sync mechanism

**結果**:
- 已建立 prompt 同步更新機制
- 未來每次互動都會記錄於此檔案並 commit

---

