# SEooC Assumption Document Schema

## 1. Purpose

定義 SEooC (Safety Element out of Context) Assumption Document 的介面規範,確保文件結構符合 ISO-26262-10:2018 §8 要求。

## 2. ISO-26262 References

- **ISO-26262-10:2018 §8**: Guideline on ISO 26262 - Safety Element out of Context (SEooC)
- **ISO-26262-10:2018 §8.4.2**: SEooC Development Assumptions
- **ISO-26262-2:2018 §6.4.3**: Work Product Requirements

## 3. Document Structure

### 3.1 Metadata Section

```json
{
  "documentId": "SEooC-ASM-v{Major}.{Minor}-{Status}-{YYYYMMDD}",
  "version": "v{Major}.{Minor}",
  "status": "Draft | InReview | Reviewed | Approved",
  "creationDate": "YYYY-MM-DD",
  "lastModifiedDate": "YYYY-MM-DD",
  "owner": {
    "name": "Functional Safety Engineer Name",
    "email": "email@company.com",
    "role": "Functional Safety Engineer"
  },
  "reviewers": [
    {
      "name": "Reviewer Name",
      "email": "reviewer@company.com",
      "role": "Independent Safety Reviewer | FS Manager",
      "reviewDate": "YYYY-MM-DD"
    }
  ]
}
```

**Required Fields**:
- `documentId` (格式須符合 FR-006 命名規範)
- `version` (格式: v{Major}.{Minor})
- `status` (必須為 4 種狀態之一)
- `owner.name`, `owner.role` (負責人資訊)

**Optional Fields**:
- `reviewers` (審查階段後填入)

---

### 3.2 Item Definition Section

```json
{
  "itemDefinition": {
    "itemName": "PCIE Gen5 Controller (SEooC)",
    "itemDescription": "High-speed PCIe Gen5 controller supporting data transfer up to 32 GT/s",
    "intendedFunction": "Provide reliable high-speed data communication between host and peripheral devices",
    "safetyRelevance": "Data integrity and availability critical for ASIL B applications",
    "asil": "ASIL_B",
    "operatingEnvironment": {
      "temperature": "-40°C to 125°C",
      "voltage": "0.9V ± 5%",
      "humidity": "5% to 95% non-condensing"
    }
  }
}
```

**Required Fields**:
- `itemName`, `itemDescription`, `intendedFunction` (Item 基本資訊)
- `asil` (ASIL B for this project)
- `operatingEnvironment` (操作環境參數)

---

### 3.3 Assumptions Section

**結構**:
```json
{
  "assumptions": [
    {
      "assumptionId": "ASM-{nnn}",
      "category": "Functional | Environmental | Interface | Operational",
      "description": "Detailed assumption description",
      "rationale": "Why this assumption is necessary",
      "impact": "Impact on safety if assumption violated",
      "verification": "How integrator should verify this assumption",
      "traceToTSR": ["TSR-{nnn}", "TSR-{nnn}"],
      "traceToChecklist": ["CHK-{nnn}"],
      "status": "Proposed | Accepted | UnderReview"
    }
  ]
}
```

**Category Definitions** (來自 FR-001):

1. **Functional Assumptions**:
   - 範例: "The SEooC assumes that the host system will not request data transfers exceeding 32 GT/s"
   - 影響: 超速請求可能導致資料損壞

2. **Environmental Assumptions**:
   - 範例: "The SEooC assumes operating temperature remains within -40°C to 125°C"
   - 影響: 超溫可能導致硬體失效

3. **Interface Assumptions**:
   - 範例: "The SEooC assumes integrator provides stable 0.9V ± 5% power supply"
   - 影響: 電壓不穩可能導致功能異常

4. **Operational Assumptions**:
   - 範例: "The SEooC assumes periodic system resets every 24 hours to clear error states"
   - 影響: 未重置可能累積錯誤

**Required Fields per Assumption**:
- `assumptionId` (格式: ASM-{nnn}, nnn = 001-999)
- `category` (必須為 4 種類別之一)
- `description` (詳細描述,≥ 50 字)
- `rationale` (合理性說明,≥ 30 字)
- `impact` (安全影響評估,≥ 30 字)
- `verification` (驗證方法,≥ 20 字)
- `traceToTSR` or `traceToChecklist` (至少其一不為空)

**Validation Rules**:
- 至少包含 4 類 category 各 1 個假設 (total ≥ 4 assumptions)
- 每個假設須可追溯至 TSR 或 Integrator Checklist
- description, rationale, impact, verification 不可為空

---

### 3.4 Integrator Checklist Section

```json
{
  "integratorChecklist": [
    {
      "checklistId": "CHK-{nnn}",
      "checkItem": "Verification item description",
      "verificationMethod": "Inspection | Test | Analysis | Review",
      "acceptanceCriteria": "具體驗收標準",
      "relatedAssumptions": ["ASM-{nnn}", "ASM-{nnn}"],
      "priority": "Critical | High | Medium | Low",
      "status": "NotVerified | Verified | NotApplicable"
    }
  ]
}
```

**Required Fields per Checklist Item**:
- `checklistId` (格式: CHK-{nnn})
- `checkItem` (檢查項目,≥ 20 字)
- `verificationMethod` (驗證方法,必須為 4 種之一)
- `acceptanceCriteria` (驗收標準,≥ 20 字)
- `relatedAssumptions` (關聯假設,≥ 1 個)
- `priority` (優先級)

**Validation Rules**:
- 至少包含 15-20 個 checklist items (來自 FR-001)
- 每個 assumption 須被至少 1 個 checklist item 覆蓋
- Critical + High priority items ≥ 50% of total

---

## 4. Validation Rules Summary

### 4.1 Naming Convention (FR-006)
```
Pattern: SEooC-ASM_v{Major}.{Minor}_{Status}_{YYYYMMDD}.docx
Example: SEooC-ASM_v1.0_Draft_20250115.docx
```

### 4.2 Completeness Checks

| Field | Requirement |
|-------|-------------|
| Metadata | documentId, version, status, owner.name, owner.role 必填 |
| Item Definition | itemName, asil, operatingEnvironment 必填 |
| Assumptions | ≥ 4 assumptions, 每類 category 至少 1 個 |
| Integrator Checklist | ≥ 15 items, 100% assumption coverage |

### 4.3 Traceability Checks
- 每個 assumption 須有 traceToTSR 或 traceToChecklist (至少其一)
- 每個 checklist item 須有 relatedAssumptions (≥ 1)
- 反向檢查: 每個 TSR 被追溯須在 TSR 文件中存在

### 4.4 Review Requirements (FR-014)
- Document Type: **Safety Document** → 3-Party Review
- Participants: Author + Independent Reviewer + FS Manager
- Independence: Reviewer 須非 Author 本人

---

## 5. Example Document (Minimal Valid)

```json
{
  "documentId": "SEooC-ASM-v1.0-Draft-20250115",
  "version": "v1.0",
  "status": "Draft",
  "creationDate": "2025-01-15",
  "owner": {
    "name": "Alice Wang",
    "email": "alice.wang@company.com",
    "role": "Functional Safety Engineer"
  },
  "itemDefinition": {
    "itemName": "PCIE Gen5 Controller (SEooC)",
    "itemDescription": "High-speed PCIe Gen5 controller supporting data transfer up to 32 GT/s",
    "asil": "ASIL_B",
    "operatingEnvironment": {
      "temperature": "-40°C to 125°C",
      "voltage": "0.9V ± 5%"
    }
  },
  "assumptions": [
    {
      "assumptionId": "ASM-001",
      "category": "Functional",
      "description": "The SEooC assumes that the host system will not request data transfers exceeding 32 GT/s",
      "rationale": "PCIE Gen5 spec maximum data rate is 32 GT/s; exceeding this may cause data corruption",
      "impact": "Over-speed requests may lead to CRC errors and data loss",
      "verification": "Integrator shall validate that host driver limits data rate to ≤ 32 GT/s",
      "traceToChecklist": ["CHK-001"],
      "status": "Proposed"
    },
    {
      "assumptionId": "ASM-002",
      "category": "Environmental",
      "description": "The SEooC assumes operating temperature remains within -40°C to 125°C",
      "rationale": "Hardware components rated for industrial temperature range",
      "impact": "Out-of-range temperature may cause timing violations and hardware failure",
      "verification": "Integrator shall monitor temperature via thermal sensors",
      "traceToChecklist": ["CHK-002"],
      "status": "Proposed"
    },
    {
      "assumptionId": "ASM-003",
      "category": "Interface",
      "description": "The SEooC assumes integrator provides stable 0.9V ± 5% power supply",
      "rationale": "Core logic requires 0.9V nominal voltage per chip specification",
      "impact": "Voltage deviation > 5% may cause functional errors or latch-up",
      "verification": "Integrator shall use voltage regulator with ≤ 2% ripple and monitor via ADC",
      "traceToTSR": ["TSR-015"],
      "traceToChecklist": ["CHK-003"],
      "status": "Proposed"
    },
    {
      "assumptionId": "ASM-004",
      "category": "Operational",
      "description": "The SEooC assumes periodic system resets every 24 hours to clear error states",
      "rationale": "Long-running operation may accumulate transient errors in error counters",
      "impact": "Unreset error states may trigger false alarms or mask real faults",
      "verification": "Integrator shall implement watchdog timer with 24-hour reset cycle",
      "traceToChecklist": ["CHK-004"],
      "status": "Proposed"
    }
  ],
  "integratorChecklist": [
    {
      "checklistId": "CHK-001",
      "checkItem": "Verify host driver limits data rate to ≤ 32 GT/s",
      "verificationMethod": "Test",
      "acceptanceCriteria": "Data rate monitoring shows max 32 GT/s over 1000 transactions",
      "relatedAssumptions": ["ASM-001"],
      "priority": "Critical",
      "status": "NotVerified"
    },
    {
      "checklistId": "CHK-002",
      "checkItem": "Verify operating temperature within -40°C to 125°C",
      "verificationMethod": "Inspection",
      "acceptanceCriteria": "Thermal sensor readings remain in range during 8-hour soak test",
      "relatedAssumptions": ["ASM-002"],
      "priority": "High",
      "status": "NotVerified"
    },
    {
      "checklistId": "CHK-003",
      "checkItem": "Verify power supply stability 0.9V ± 5%",
      "verificationMethod": "Test",
      "acceptanceCriteria": "ADC readings show voltage 0.855V - 0.945V with ≤ 2% ripple",
      "relatedAssumptions": ["ASM-003"],
      "priority": "Critical",
      "status": "NotVerified"
    },
    {
      "checklistId": "CHK-004",
      "checkItem": "Verify 24-hour periodic reset implemented",
      "verificationMethod": "Review",
      "acceptanceCriteria": "Watchdog timer configuration shows 24-hour period",
      "relatedAssumptions": ["ASM-004"],
      "priority": "Medium",
      "status": "NotVerified"
    }
  ]
}
```

---

## 6. Validation Script Example

```python
def validate_seooc_assumption_doc(doc):
    errors = []
    
    # Check naming convention
    if not doc['documentId'].startswith('SEooC-ASM-v'):
        errors.append("documentId naming pattern incorrect")
    
    # Check completeness
    required_fields = ['documentId', 'version', 'status', 'owner', 'itemDefinition', 'assumptions', 'integratorChecklist']
    for field in required_fields:
        if field not in doc or not doc[field]:
            errors.append(f"Missing required field: {field}")
    
    # Check assumptions
    if len(doc.get('assumptions', [])) < 4:
        errors.append("Must have ≥ 4 assumptions")
    
    categories = set([a['category'] for a in doc.get('assumptions', [])])
    required_categories = {'Functional', 'Environmental', 'Interface', 'Operational'}
    if not required_categories.issubset(categories):
        errors.append(f"Missing assumption categories: {required_categories - categories}")
    
    # Check traceability
    for assumption in doc.get('assumptions', []):
        if not assumption.get('traceToTSR') and not assumption.get('traceToChecklist'):
            errors.append(f"{assumption['assumptionId']} has no traceability")
    
    # Check integrator checklist
    if len(doc.get('integratorChecklist', [])) < 15:
        errors.append("Integrator checklist must have ≥ 15 items")
    
    # Check assumption coverage by checklist
    assumption_ids = set([a['assumptionId'] for a in doc.get('assumptions', [])])
    covered_assumptions = set()
    for item in doc.get('integratorChecklist', []):
        covered_assumptions.update(item.get('relatedAssumptions', []))
    
    uncovered = assumption_ids - covered_assumptions
    if uncovered:
        errors.append(f"Assumptions not covered by checklist: {uncovered}")
    
    return {"valid": len(errors) == 0, "errors": errors}
```

---

**Last Updated**: 2025-01-18  
**Version**: v1.0  
**Schema Type**: SEooC Assumption Document
