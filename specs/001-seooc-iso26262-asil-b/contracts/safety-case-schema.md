# Safety Case Document Schema

## 1. Purpose

定義 Safety Case 文件的介面規範,使用 GSN (Goal Structuring Notation) 建構安全論證,確保符合 ISO-26262-2:2018 §6.4.11 要求。

## 2. ISO-26262 References

- **ISO-26262-2:2018 §6.4.11**: Safety case
- **ISO-26262-2:2018 Annex B**: Example of a safety case structure
- **GSN Community Standard Version 3**: Goal Structuring Notation

## 3. Document Structure

### 3.1 Metadata Section

```json
{
  "documentId": "SAFETY-CASE-v{Major}.{Minor}-{Status}-{YYYYMMDD}",
  "version": "v{Major}.{Minor}",
  "status": "Draft | InReview | Reviewed | Approved",
  "creationDate": "YYYY-MM-DD",
  "lastModifiedDate": "YYYY-MM-DD",
  "owner": {
    "name": "Functional Safety Manager Name",
    "email": "email@company.com",
    "role": "Functional Safety Manager"
  }
}
```

---

### 3.2 GSN Elements Section

**GSN Element Types**:

1. **Goal (G)**: 安全目標或子目標
2. **Strategy (S)**: 達成目標的策略或方法
3. **Evidence (E)**: 支持目標的證據 (文件、測試結果)
4. **Context (C)**: 目標的情境或背景
5. **Assumption (A)**: 論證中的假設

```json
{
  "gsnElements": [
    {
      "elementId": "G-{nnn}" | "S-{nnn}" | "E-{nnn}" | "C-{nnn}" | "A-{nnn}",
      "type": "Goal | Strategy | Evidence | Context | Assumption",
      "description": "Element description",
      "parentElement": "Parent element ID (null for top goal)",
      "supportingElements": ["Child element ID", "Child element ID"],
      "traceToDocuments": ["HARA", "TSR", "VV-PLAN", "Test Results"],
      "status": "InProgress | Completed | Blocked"
    }
  ]
}
```

**Required Fields per GSN Element**:
- elementId, type, description
- parentElement (null for top goal, otherwise parent ID)
- supportingElements (for Goal/Strategy, list of supporting elements)
- traceToDocuments (for Evidence, list of supporting documents)

---

### 3.3 GSN Hierarchy Structure

**Top-Level Goal** (mandatory):
```
G-001: "PCIE Gen5 Controller is acceptably safe for ASIL B applications"
```

**Standard GSN Hierarchy** (來自 FR-005):

```
G-001 (Top Goal: System acceptably safe)
  ├─ S-001 (Strategy: Developed per ISO-26262:2018)
  │   ├─ G-002 (Sub-goal: All hazards identified and assessed)
  │   │   ├─ S-002 (Strategy: HARA performed per ISO-26262-3 §8)
  │   │   └─ E-001 (Evidence: HARA Report v1.0)
  │   ├─ G-003 (Sub-goal: All safety goals achieved)
  │   │   ├─ S-003 (Strategy: TSR defined and verified)
  │   │   └─ E-002 (Evidence: TSR v1.0 + V&V Results)
  │   └─ G-004 (Sub-goal: All SEooC assumptions documented and verified)
  │       ├─ S-004 (Strategy: SEooC assumptions defined per ISO-26262-10 §8)
  │       └─ E-003 (Evidence: SEooC Assumption Document v1.0)
  ├─ C-001 (Context: ASIL B application, SEooC mode)
  ├─ C-002 (Context: ISO-26262:2018 standard version)
  └─ A-001 (Assumption: Integrator follows SEooC assumptions)
```

**Validation Rules** (來自 FR-005):
- 頂層 Goal 須為 "System is acceptably safe" 或類似描述
- 每個 Goal 須有至少 1 個 Strategy 或 Evidence 支持
- 每個 Evidence 須可追溯至具體文件 (HARA, TSR, V&V Plan, Test Results)
- 所有 Assumptions 須在 SEooC Assumption Document 中記錄

---

## 4. Example GSN Structure (Complete)

```json
{
  "documentId": "SAFETY-CASE-v1.0-Draft-20250115",
  "version": "v1.0",
  "status": "Draft",
  "creationDate": "2025-01-15",
  "owner": {
    "name": "Eve Wu",
    "email": "eve.wu@company.com",
    "role": "Functional Safety Manager"
  },
  "gsnElements": [
    {
      "elementId": "G-001",
      "type": "Goal",
      "description": "PCIE Gen5 Controller is acceptably safe for ASIL B applications",
      "parentElement": null,
      "supportingElements": ["S-001", "C-001", "C-002", "A-001"],
      "status": "InProgress"
    },
    {
      "elementId": "S-001",
      "type": "Strategy",
      "description": "Argue safety by demonstrating compliance with ISO-26262:2018 development process",
      "parentElement": "G-001",
      "supportingElements": ["G-002", "G-003", "G-004"],
      "status": "InProgress"
    },
    {
      "elementId": "C-001",
      "type": "Context",
      "description": "Target ASIL: ASIL B; Development mode: SEooC (Safety Element out of Context)",
      "parentElement": "G-001",
      "supportingElements": [],
      "status": "Completed"
    },
    {
      "elementId": "C-002",
      "type": "Context",
      "description": "Applicable standard: ISO-26262:2018 (Road vehicles - Functional safety)",
      "parentElement": "G-001",
      "supportingElements": [],
      "status": "Completed"
    },
    {
      "elementId": "A-001",
      "type": "Assumption",
      "description": "Integrator follows all SEooC assumptions documented in SEooC Assumption Document v1.0",
      "parentElement": "G-001",
      "supportingElements": [],
      "traceToDocuments": ["SEooC-ASM-v1.0"],
      "status": "InProgress"
    },
    {
      "elementId": "G-002",
      "type": "Goal",
      "description": "All hazards have been identified and risk assessed",
      "parentElement": "S-001",
      "supportingElements": ["S-002"],
      "status": "InProgress"
    },
    {
      "elementId": "S-002",
      "type": "Strategy",
      "description": "Perform HARA per ISO-26262-3:2018 §8 (Hazard Analysis and Risk Assessment)",
      "parentElement": "G-002",
      "supportingElements": ["E-001"],
      "status": "InProgress"
    },
    {
      "elementId": "E-001",
      "type": "Evidence",
      "description": "HARA Report v1.0: 5 hazards identified, ASIL determined, 5 safety goals derived",
      "parentElement": "S-002",
      "supportingElements": [],
      "traceToDocuments": ["HARA-v1.0"],
      "status": "Completed"
    },
    {
      "elementId": "G-003",
      "type": "Goal",
      "description": "All safety goals have been achieved",
      "parentElement": "S-001",
      "supportingElements": ["S-003"],
      "status": "InProgress"
    },
    {
      "elementId": "S-003",
      "type": "Strategy",
      "description": "Define Technical Safety Requirements (TSR) per ISO-26262-4:2018 §6 and verify via V&V",
      "parentElement": "G-003",
      "supportingElements": ["E-002", "E-003"],
      "status": "InProgress"
    },
    {
      "elementId": "E-002",
      "type": "Evidence",
      "description": "TSR Document v1.0: 10 technical safety requirements defined with ASIL B integrity",
      "parentElement": "S-003",
      "supportingElements": [],
      "traceToDocuments": ["TSR-v1.0"],
      "status": "Completed"
    },
    {
      "elementId": "E-003",
      "type": "Evidence",
      "description": "V&V Results: 100% TSR coverage, 95% statement coverage, 90% branch coverage achieved",
      "parentElement": "S-003",
      "supportingElements": [],
      "traceToDocuments": ["VV-PLAN-v1.0", "Test-Results-v1.0"],
      "status": "InProgress"
    },
    {
      "elementId": "G-004",
      "type": "Goal",
      "description": "All SEooC assumptions have been documented and verified",
      "parentElement": "S-001",
      "supportingElements": ["S-004"],
      "status": "InProgress"
    },
    {
      "elementId": "S-004",
      "type": "Strategy",
      "description": "Define SEooC assumptions per ISO-26262-10:2018 §8 and provide integrator checklist",
      "parentElement": "G-004",
      "supportingElements": ["E-004"],
      "status": "InProgress"
    },
    {
      "elementId": "E-004",
      "type": "Evidence",
      "description": "SEooC Assumption Document v1.0: 4 assumption categories, 15-item integrator checklist",
      "parentElement": "S-004",
      "supportingElements": [],
      "traceToDocuments": ["SEooC-ASM-v1.0"],
      "status": "Completed"
    }
  ]
}
```

---

## 5. Validation Rules

### 5.1 Naming Convention (FR-006)
```
Pattern: SAFETY-CASE_v{Major}.{Minor}_{Status}_{YYYYMMDD}.docx
Example: SAFETY-CASE_v1.0_Draft_20250115.docx
```

### 5.2 GSN Structure Validation

```python
def validate_gsn_structure(safety_case):
    errors = []
    
    # Check top-level goal exists
    top_goals = [e for e in safety_case['gsnElements'] if e['type'] == 'Goal' and e['parentElement'] is None]
    if len(top_goals) != 1:
        errors.append("Must have exactly 1 top-level goal")
    
    # Check all goals have supporting elements
    for element in safety_case['gsnElements']:
        if element['type'] == 'Goal':
            if not element.get('supportingElements') or len(element['supportingElements']) == 0:
                errors.append(f"{element['elementId']} (Goal) has no supporting elements")
    
    # Check all evidence has document traceability
    for element in safety_case['gsnElements']:
        if element['type'] == 'Evidence':
            if not element.get('traceToDocuments') or len(element['traceToDocuments']) == 0:
                errors.append(f"{element['elementId']} (Evidence) has no traceability to documents")
    
    return {"valid": len(errors) == 0, "errors": errors}
```

### 5.3 Completeness Checks

| Field | Requirement |
|-------|-------------|
| Top Goal | Exactly 1 top-level goal (parentElement = null) |
| Goals | ≥ 4 goals (covering hazards, safety goals, assumptions, verification) |
| Strategies | ≥ 3 strategies (one per major goal) |
| Evidence | ≥ 4 evidence elements (tracing to HARA, TSR, V&V, SEooC Assumptions) |
| Context | ≥ 2 context elements (ASIL, standard version) |
| Assumptions | ≥ 1 assumption (SEooC integrator assumptions) |

---

**Last Updated**: 2025-01-18  
**Version**: v1.0  
**Schema Type**: Safety Case Document
