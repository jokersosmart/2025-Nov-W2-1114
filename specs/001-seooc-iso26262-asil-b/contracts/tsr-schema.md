# TSR (Technical Safety Requirement) Document Schema

## 1. Purpose

定義 TSR (Technical Safety Requirement) 文件的介面規範,確保符合 ISO-26262-4:2018 §6 (Technical Safety Requirements) 要求。

## 2. ISO-26262 References

- **ISO-26262-4:2018 §6**: Initiation of product development at the system level (Technical Safety Concept & Requirements)
- **ISO-26262-4:2018 §7**: Product development at the system level (Technical Safety Concept & Requirements specification)
- **ISO-26262-4:2018 §7.4.3.8**: Traceability
- **ISO-26262-9:2018 §5**: ASIL-oriented and safety-oriented analyses (ASIL Decomposition)

## 3. Document Structure

### 3.1 Metadata Section

```json
{
  "documentId": "TSR-v{Major}.{Minor}-{Status}-{YYYYMMDD}",
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

**Required Fields**: documentId, version, status, owner

---

### 3.2 Requirements Section

```json
{
  "requirements": [
    {
      "requirementId": "TSR-{nnn}",
      "type": "Functional | Non-Functional",
      "category": "Detection | Mitigation | Control | Warning | Diagnostics",
      "description": "Detailed requirement description using 'shall' language",
      "asil": "ASIL_B",
      "derivedFromSafetyGoal": "SG-{nnn}",
      "verificationCriteria": "具體可測量的驗證標準",
      "verificationMethod": "Test | Review | Analysis | Simulation",
      "traceToDesign": ["DSN-{nnn}", "DSN-{nnn}"],
      "traceToTest": ["TC-{nnn}", "TC-{nnn}"],
      "priority": "Critical | High | Medium | Low",
      "status": "Proposed | Approved | Implemented | Verified"
    }
  ]
}
```

**Requirement Types**:

1. **Functional Requirements**:
   - 定義系統"做什麼" (what the system shall do)
   - 範例: "The controller shall detect CRC errors in received PCIe packets"

2. **Non-Functional Requirements**:
   - 定義系統"如何做" (how well the system shall perform)
   - 範例: "The controller shall detect CRC errors within 10ms of packet reception"

**Requirement Categories** (來自 FR-003):

- **Detection**: 偵測故障機制 (e.g., CRC check, parity check, watchdog)
- **Mitigation**: 減緩故障影響 (e.g., ECC correction, redundancy)
- **Control**: 控制系統狀態 (e.g., safe state transition, degradation)
- **Warning**: 警告使用者 (e.g., LED indicator, diagnostic message)
- **Diagnostics**: 診斷與記錄 (e.g., error logging, fault coverage)

**Required Fields per Requirement**:
- requirementId (格式: TSR-{nnn}, nnn = 001-999)
- type, category, description
- asil (須 ≥ derivedFromSafetyGoal.asil)
- derivedFromSafetyGoal (至少 1 個)
- verificationCriteria (具體可測量)
- verificationMethod
- traceToDesign or traceToTest (至少其一不為空)

---

### 3.3 ASIL Inheritance Rules

**規則** (來自 ISO-26262-9:2018 §5):
```
TSR.asil ≥ SafetyGoal.asil

範例:
SafetyGoal SG-001 (ASIL B) → TSR-001 (ASIL B), TSR-002 (ASIL B)
```

**ASIL Decomposition** (允許情況):
```
SafetyGoal SG-001 (ASIL B) 可分解為:
  TSR-001 (ASIL A) + TSR-002 (ASIL A) with independence requirement

條件:
- TSR-001 與 TSR-002 須獨立開發 (independent development)
- 兩者同時失效機率須 < 10^-7 (per hour)
```

**本專案不使用 ASIL Decomposition** (來自 Assumption 2):
- 簡化考量: 保持所有 TSR.asil = ASIL_B
- 避免獨立性驗證成本

---

### 3.4 Verification Criteria Definition

**良好的 Verification Criteria 範例**:

✅ **Good** (具體可測量):
```
"CRC error detection rate ≥ 99.99% in fault injection test with 10,000 trials"
```

❌ **Bad** (模糊不可測):
```
"CRC error detection should be reliable"
```

**驗證標準模板**:
```
"{Metric} {Operator} {Value} in {Test Context}"

範例:
- "Latency < 10ms in 99.9% of test cases under normal load"
- "Coverage ≥ 95% statement coverage and ≥ 90% branch coverage"
- "Fault detection time ≤ 50ms in link-down scenario test"
```

---

## 4. Validation Rules Summary

### 4.1 Naming Convention (FR-006)
```
Pattern: TSR_v{Major}.{Minor}_{Status}_{YYYYMMDD}.docx
Example: TSR_v1.0_Draft_20250115.docx
```

### 4.2 Completeness Checks

| Field | Requirement |
|-------|-------------|
| Metadata | documentId, version, status, owner 必填 |
| Requirements | ≥ 10 requirements (covering 5 categories) |
| Verification Criteria | 100% of requirements have verificationCriteria |
| Traceability | 100% of requirements trace to SafetyGoal & (Design or Test) |

### 4.3 ASIL Inheritance Validation

```python
def validate_asil_inheritance(tsr_doc, hara_doc):
    errors = []
    asil_order = {"QM": 0, "ASIL_A": 1, "ASIL_B": 2, "ASIL_C": 3, "ASIL_D": 4}
    
    # Build SafetyGoal ASIL map
    sg_asil_map = {sg['safetyGoalId']: sg['asil'] for sg in hara_doc['safetyGoals']}
    
    for req in tsr_doc.get('requirements', []):
        sg_id = req.get('derivedFromSafetyGoal')
        if sg_id in sg_asil_map:
            sg_asil = sg_asil_map[sg_id]
            tsr_asil = req.get('asil')
            
            if asil_order[tsr_asil] < asil_order[sg_asil]:
                errors.append(
                    f"{req['requirementId']} ASIL ({tsr_asil}) < {sg_id} ASIL ({sg_asil})"
                )
    
    return {"valid": len(errors) == 0, "errors": errors}
```

### 4.4 Traceability Validation

**Downward Traceability** (SafetyGoal → TSR):
```python
def validate_downward_trace(tsr_doc, hara_doc):
    errors = []
    sg_ids = set([sg['safetyGoalId'] for sg in hara_doc['safetyGoals']])
    sg_covered = set()
    
    for req in tsr_doc.get('requirements', []):
        sg_id = req.get('derivedFromSafetyGoal')
        if sg_id:
            sg_covered.add(sg_id)
    
    uncovered = sg_ids - sg_covered
    if uncovered:
        errors.append(f"Safety Goals not covered by TSR: {uncovered}")
    
    return {"valid": len(errors) == 0, "errors": errors}
```

**Upward Traceability** (TSR → SafetyGoal):
```python
def validate_upward_trace(tsr_doc, hara_doc):
    errors = []
    sg_ids = set([sg['safetyGoalId'] for sg in hara_doc['safetyGoals']])
    
    for req in tsr_doc.get('requirements', []):
        sg_id = req.get('derivedFromSafetyGoal')
        if sg_id and sg_id not in sg_ids:
            errors.append(f"{req['requirementId']} traces to non-existent {sg_id}")
    
    return {"valid": len(errors) == 0, "errors": errors}
```

### 4.5 Review Requirements (FR-014)
- Document Type: **Safety Document** → 3-Party Review
- Participants: Author + Independent Reviewer + FS Manager

---

## 5. Example Document (Minimal Valid)

```json
{
  "documentId": "TSR-v1.0-Draft-20250115",
  "version": "v1.0",
  "status": "Draft",
  "creationDate": "2025-01-15",
  "owner": {
    "name": "Carol Liu",
    "email": "carol.liu@company.com",
    "role": "Functional Safety Engineer"
  },
  "requirements": [
    {
      "requirementId": "TSR-001",
      "type": "Functional",
      "category": "Detection",
      "description": "The controller shall detect CRC errors in received PCIe packets using CRC-32 algorithm",
      "asil": "ASIL_B",
      "derivedFromSafetyGoal": "SG-001",
      "verificationCriteria": "CRC error detection rate ≥ 99.99% in fault injection test with 10,000 corrupted packets",
      "verificationMethod": "Test",
      "traceToDesign": ["DSN-001"],
      "traceToTest": ["TC-001", "TC-002"],
      "priority": "Critical",
      "status": "Proposed"
    },
    {
      "requirementId": "TSR-002",
      "type": "Non-Functional",
      "category": "Detection",
      "description": "The controller shall detect CRC errors within 10ms of packet reception",
      "asil": "ASIL_B",
      "derivedFromSafetyGoal": "SG-001",
      "verificationCriteria": "CRC error detection time ≤ 10ms in 99.9% of test cases measured via timestamp comparison",
      "verificationMethod": "Test",
      "traceToDesign": ["DSN-001"],
      "traceToTest": ["TC-003"],
      "priority": "High",
      "status": "Proposed"
    },
    {
      "requirementId": "TSR-003",
      "type": "Functional",
      "category": "Detection",
      "description": "The controller shall detect PCIe link down status via PHY link status register polling every 10ms",
      "asil": "ASIL_B",
      "derivedFromSafetyGoal": "SG-002",
      "verificationCriteria": "Link down detection time ≤ 50ms in 1000 link-down simulation trials",
      "verificationMethod": "Test",
      "traceToDesign": ["DSN-002"],
      "traceToTest": ["TC-004"],
      "priority": "Critical",
      "status": "Proposed"
    },
    {
      "requirementId": "TSR-004",
      "type": "Non-Functional",
      "category": "Control",
      "description": "The controller shall limit PCIe retransmission attempts to 3 retries before dropping packet",
      "asil": "QM",
      "derivedFromSafetyGoal": "SG-003",
      "verificationCriteria": "Retransmission count ≤ 3 in packet drop test with 1000 trials; latency < 100ms verified",
      "verificationMethod": "Test",
      "traceToDesign": ["DSN-003"],
      "traceToTest": ["TC-005"],
      "priority": "Medium",
      "status": "Proposed"
    },
    {
      "requirementId": "TSR-005",
      "type": "Functional",
      "category": "Detection",
      "description": "The controller shall detect power supply voltage drop below 0.855V via ADC monitoring at 1kHz sampling rate",
      "asil": "ASIL_B",
      "derivedFromSafetyGoal": "SG-004",
      "verificationCriteria": "Voltage drop detection time ≤ 5ms in power-off simulation test with 100 trials",
      "verificationMethod": "Test",
      "traceToDesign": ["DSN-004"],
      "traceToTest": ["TC-006"],
      "priority": "Critical",
      "status": "Proposed"
    },
    {
      "requirementId": "TSR-006",
      "type": "Functional",
      "category": "Mitigation",
      "description": "The controller shall correct single-bit errors in transmitted data using Hamming ECC (7,4) encoding",
      "asil": "ASIL_B",
      "derivedFromSafetyGoal": "SG-005",
      "verificationCriteria": "Single-bit error correction rate ≥ 99.999% in fault injection test with 100,000 single-bit errors",
      "verificationMethod": "Test",
      "traceToDesign": ["DSN-005"],
      "traceToTest": ["TC-007"],
      "priority": "Critical",
      "status": "Proposed"
    },
    {
      "requirementId": "TSR-007",
      "type": "Functional",
      "category": "Detection",
      "description": "The controller shall detect double-bit errors in transmitted data via ECC parity check",
      "asil": "ASIL_B",
      "derivedFromSafetyGoal": "SG-005",
      "verificationCriteria": "Double-bit error detection rate ≥ 99.99% in fault injection test with 10,000 double-bit errors",
      "verificationMethod": "Test",
      "traceToDesign": ["DSN-005"],
      "traceToTest": ["TC-008"],
      "priority": "High",
      "status": "Proposed"
    },
    {
      "requirementId": "TSR-008",
      "type": "Functional",
      "category": "Control",
      "description": "The controller shall transition to Safe State (shutdown) within 20ms upon power supply failure detection",
      "asil": "ASIL_B",
      "derivedFromSafetyGoal": "SG-004",
      "verificationCriteria": "Safe state transition time ≤ 20ms in 99.9% of power-off tests; state saved successfully",
      "verificationMethod": "Test",
      "traceToDesign": ["DSN-004"],
      "traceToTest": ["TC-009"],
      "priority": "Critical",
      "status": "Proposed"
    },
    {
      "requirementId": "TSR-009",
      "type": "Functional",
      "category": "Warning",
      "description": "The controller shall send warning signal to ECU via I2C interface within 5ms of fault detection",
      "asil": "ASIL_B",
      "derivedFromSafetyGoal": "SG-002",
      "verificationCriteria": "Warning signal transmission time ≤ 5ms in fault scenario test with 1000 trials",
      "verificationMethod": "Test",
      "traceToDesign": ["DSN-006"],
      "traceToTest": ["TC-010"],
      "priority": "High",
      "status": "Proposed"
    },
    {
      "requirementId": "TSR-010",
      "type": "Functional",
      "category": "Diagnostics",
      "description": "The controller shall log all detected faults (CRC errors, link-down, power failure, ECC errors) with timestamp to non-volatile memory",
      "asil": "ASIL_B",
      "derivedFromSafetyGoal": "SG-001",
      "verificationCriteria": "Fault logging success rate ≥ 99.9%; logs retrievable after power cycle in 100 trials",
      "verificationMethod": "Test",
      "traceToDesign": ["DSN-007"],
      "traceToTest": ["TC-011"],
      "priority": "Medium",
      "status": "Proposed"
    }
  ]
}
```

---

## 6. Validation Script Example

```python
def validate_tsr_document(doc):
    errors = []
    
    # Check naming convention
    if not doc['documentId'].startswith('TSR-v'):
        errors.append("documentId naming pattern incorrect")
    
    # Check completeness
    if len(doc.get('requirements', [])) < 10:
        errors.append("Must have ≥ 10 requirements")
    
    # Check verification criteria
    for req in doc.get('requirements', []):
        if not req.get('verificationCriteria') or len(req['verificationCriteria']) < 20:
            errors.append(f"{req['requirementId']} missing or insufficient verificationCriteria")
    
    # Check traceability
    for req in doc.get('requirements', []):
        if not req.get('derivedFromSafetyGoal'):
            errors.append(f"{req['requirementId']} has no traceability to Safety Goal")
        
        if not req.get('traceToDesign') and not req.get('traceToTest'):
            errors.append(f"{req['requirementId']} has no traceability to Design or Test")
    
    # Check category coverage
    categories = set([req['category'] for req in doc.get('requirements', [])])
    required_categories = {'Detection', 'Mitigation', 'Control', 'Warning', 'Diagnostics'}
    if len(categories.intersection(required_categories)) < 3:
        errors.append(f"Must cover at least 3 requirement categories; only {categories} found")
    
    return {"valid": len(errors) == 0, "errors": errors}
```

---

**Last Updated**: 2025-01-18  
**Version**: v1.0  
**Schema Type**: TSR Document
