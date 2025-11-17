# Traceability Matrix Document Schema

## 1. Purpose

定義 Traceability Matrix 文件的介面規範,建立文件間追溯關係,確保符合 ISO-26262-8:2018 §6 與 ISO-26262-4:2018 §7.4.3.8 要求。

## 2. ISO-26262 References

- **ISO-26262-8:2018 §6**: Supporting processes - Specification and management of requirements
- **ISO-26262-4:2018 §7.4.3.8**: Traceability between safety requirements and design
- **ISO-26262-8:2018 §6.4.3**: Bidirectional traceability

## 3. Document Structure

### 3.1 Metadata Section

```json
{
  "documentId": "TRACE-MATRIX-v{Major}.{Minor}-{Status}-{YYYYMMDD}",
  "version": "v{Major}.{Minor}",
  "status": "Draft | InReview | Reviewed | Approved",
  "creationDate": "YYYY-MM-DD",
  "lastModifiedDate": "YYYY-MM-DD",
  "owner": {
    "name": "Quality Engineer Name",
    "email": "email@company.com",
    "role": "Quality Engineer"
  }
}
```

---

### 3.2 Trace Links Section

```json
{
  "traceLinks": [
    {
      "linkId": "TL-{nnn}",
      "sourceType": "Hazard | SafetyGoal | TSR | Design | Test | Assumption",
      "sourceId": "源 ID (e.g., HAZ-001)",
      "targetType": "SafetyGoal | TSR | Design | Test | Assumption | Checklist",
      "targetId": "目標 ID (e.g., SG-001)",
      "traceType": "Upward | Downward | Horizontal | Verification",
      "rationale": "追溯原因說明",
      "status": "Valid | Invalid | UnderReview"
    }
  ]
}
```

**Required Fields per Trace Link**:
- linkId, sourceType, sourceId, targetType, targetId, traceType, rationale

---

### 3.3 Trace Types Definition

#### 3.3.1 Upward Traceability
從實作追溯至需求,確保所有實作皆有需求依據。

**範例路徑**:
```
Test Case (TC-001)
  → Design (DSN-001)
    → TSR (TSR-001)
      → Safety Goal (SG-001)
        → Hazard (HAZ-001)
```

#### 3.3.2 Downward Traceability
從需求追溯至實作,確保所有需求皆有實作覆蓋。

**範例路徑**:
```
Hazard (HAZ-001)
  → Safety Goal (SG-001)
    → TSR (TSR-001, TSR-002)
      → Design (DSN-001, DSN-002, DSN-003)
        → Test Case (TC-001, TC-002, TC-003, TC-004)
```

#### 3.3.3 Horizontal Traceability
同層級間追溯,確保相關元素一致性。

**範例關係**:
```
TSR-001 ↔ Design-001 (Requirements ↔ Design)
TSR-002 ↔ TSR-005 (Decomposed requirements)
Assumption-001 ↔ TSR-003 (Assumption → Requirement)
```

#### 3.3.4 Verification Traceability
驗證方法追溯,確保所有需求皆有驗證方法。

**範例關係**:
```
TSR-001 → TC-001 (Test)
TSR-002 → Review-001 (Review)
TSR-003 → Analysis-001 (Analysis)
Design-001 → Simulation-001 (Simulation)
```

---

## 4. Validation Rules

### 4.1 Naming Convention (FR-006)
```
Pattern: TRACE-MATRIX_v{Major}.{Minor}_{Status}_{YYYYMMDD}.xlsx (Excel format)
Example: TRACE-MATRIX_v1.0_Draft_20250115.xlsx
```

### 4.2 Coverage Requirements (100% Coverage)

**Downward Coverage Checks**:
- 每個 Hazard → 至少 1 個 Safety Goal
- 每個 Safety Goal → 至少 1 個 TSR
- 每個 TSR → 至少 1 個 Design
- 每個 TSR → 至少 1 個 Test Case

**Upward Coverage Checks**:
- 每個 Test Case → 至少 1 個 TSR
- 每個 Design → 至少 1 個 TSR
- 每個 TSR → 至少 1 個 Safety Goal
- 每個 Safety Goal → 至少 1 個 Hazard

### 4.3 Orphan & Dangling Detection

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

## 5. Example Document (Minimal Valid)

**Excel Format** (initial 3 months):

| Link ID | Source Type | Source ID | Target Type | Target ID | Trace Type | Rationale | Status |
|---------|-------------|-----------|-------------|-----------|------------|-----------|--------|
| TL-001 | Hazard | HAZ-001 | SafetyGoal | SG-001 | Downward | Data corruption hazard → Data integrity goal | Valid |
| TL-002 | SafetyGoal | SG-001 | TSR | TSR-001 | Downward | Decomposed into CRC detection requirement | Valid |
| TL-003 | SafetyGoal | SG-001 | TSR | TSR-002 | Downward | Decomposed into detection time requirement | Valid |
| TL-004 | TSR | TSR-001 | Design | DSN-001 | Downward | Implemented by CRC check module | Valid |
| TL-005 | TSR | TSR-001 | Test | TC-001 | Verification | Verified by single-bit corruption test | Valid |
| TL-006 | TSR | TSR-001 | Test | TC-002 | Verification | Verified by multi-bit corruption test | Valid |
| TL-007 | Test | TC-001 | TSR | TSR-001 | Upward | Verifies CRC check requirement | Valid |
| TL-008 | Design | DSN-001 | TSR | TSR-001 | Upward | Implements CRC check requirement | Valid |
| TL-009 | Hazard | HAZ-002 | SafetyGoal | SG-002 | Downward | Link-down hazard → Availability goal | Valid |
| TL-010 | SafetyGoal | SG-002 | TSR | TSR-003 | Downward | Decomposed into link-down detection requirement | Valid |

**JSON Format** (Codebeamer migration after 3 months):

```json
{
  "documentId": "TRACE-MATRIX-v1.0-Draft-20250115",
  "version": "v1.0",
  "status": "Draft",
  "creationDate": "2025-01-15",
  "owner": {
    "name": "Frank Lee",
    "email": "frank.lee@company.com",
    "role": "Quality Engineer"
  },
  "traceLinks": [
    {
      "linkId": "TL-001",
      "sourceType": "Hazard",
      "sourceId": "HAZ-001",
      "targetType": "SafetyGoal",
      "targetId": "SG-001",
      "traceType": "Downward",
      "rationale": "Data corruption hazard → Data integrity goal",
      "status": "Valid"
    },
    {
      "linkId": "TL-002",
      "sourceType": "SafetyGoal",
      "sourceId": "SG-001",
      "targetType": "TSR",
      "targetId": "TSR-001",
      "traceType": "Downward",
      "rationale": "Decomposed into CRC detection requirement",
      "status": "Valid"
    },
    {
      "linkId": "TL-003",
      "sourceType": "SafetyGoal",
      "sourceId": "SG-001",
      "targetType": "TSR",
      "targetId": "TSR-002",
      "traceType": "Downward",
      "rationale": "Decomposed into detection time requirement",
      "status": "Valid"
    },
    {
      "linkId": "TL-004",
      "sourceType": "TSR",
      "sourceId": "TSR-001",
      "targetType": "Design",
      "targetId": "DSN-001",
      "traceType": "Downward",
      "rationale": "Implemented by CRC check module",
      "status": "Valid"
    },
    {
      "linkId": "TL-005",
      "sourceType": "TSR",
      "sourceId": "TSR-001",
      "targetType": "Test",
      "targetId": "TC-001",
      "traceType": "Verification",
      "rationale": "Verified by single-bit corruption test",
      "status": "Valid"
    },
    {
      "linkId": "TL-006",
      "sourceType": "TSR",
      "sourceId": "TSR-001",
      "targetType": "Test",
      "targetId": "TC-002",
      "traceType": "Verification",
      "rationale": "Verified by multi-bit corruption test",
      "status": "Valid"
    },
    {
      "linkId": "TL-007",
      "sourceType": "Test",
      "sourceId": "TC-001",
      "targetType": "TSR",
      "targetId": "TSR-001",
      "traceType": "Upward",
      "rationale": "Verifies CRC check requirement",
      "status": "Valid"
    },
    {
      "linkId": "TL-008",
      "sourceType": "Design",
      "sourceId": "DSN-001",
      "targetType": "TSR",
      "targetId": "TSR-001",
      "traceType": "Upward",
      "rationale": "Implements CRC check requirement",
      "status": "Valid"
    },
    {
      "linkId": "TL-009",
      "sourceType": "Hazard",
      "sourceId": "HAZ-002",
      "targetType": "SafetyGoal",
      "targetId": "SG-002",
      "traceType": "Downward",
      "rationale": "Link-down hazard → Availability goal",
      "status": "Valid"
    },
    {
      "linkId": "TL-010",
      "sourceType": "SafetyGoal",
      "sourceId": "SG-002",
      "targetType": "TSR",
      "targetId": "TSR-003",
      "traceType": "Downward",
      "rationale": "Decomposed into link-down detection requirement",
      "status": "Valid"
    }
  ]
}
```

---

## 6. Migration to Codebeamer

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

**Last Updated**: 2025-01-18  
**Version**: v1.0  
**Schema Type**: Traceability Matrix Document
