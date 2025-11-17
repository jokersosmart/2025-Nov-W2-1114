# HARA (Hazard Analysis and Risk Assessment) Document Schema

## 1. Purpose

定義 HARA (Hazard Analysis and Risk Assessment) 文件的介面規範,確保符合 ISO-26262-3:2018 §7 (Item Definition) & §8 (HARA) 要求。

## 2. ISO-26262 References

- **ISO-26262-3:2018 §7**: Initiation of the safety lifecycle (Item Definition)
- **ISO-26262-3:2018 §8**: Hazard analysis and risk assessment (HARA)
- **ISO-26262-3:2018 Table 4**: ASIL determination matrix
- **ISO-26262-2:2018 §6.4.3**: Work Product Requirements

## 3. Document Structure

### 3.1 Metadata Section

```json
{
  "documentId": "HARA-v{Major}.{Minor}-{Status}-{YYYYMMDD}",
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

### 3.2 Item Definition Section

```json
{
  "itemDefinition": {
    "itemName": "PCIE Gen5 Controller (SEooC)",
    "itemDescription": "High-speed PCIe Gen5 controller supporting data transfer up to 32 GT/s, designed for SEooC mode",
    "intendedFunction": "Provide reliable high-speed data communication between host and peripheral devices with safety mechanisms",
    "safetyRelevance": "Data integrity and availability critical for ASIL B applications (e.g., ADAS sensors, ECU communication)",
    "asil": "ASIL_B",
    "boundaries": {
      "functionalBoundary": "Data transmission and reception via PCIe Gen5 protocol",
      "physicalBoundary": "PCIe controller IC + PHY + power management unit",
      "interfaceBoundary": "PCIe Gen5 x4 lanes, I2C management interface, power pins"
    },
    "operatingModes": [
      "Normal Operation (data transfer)",
      "Low Power Mode (idle)",
      "Error Recovery Mode (fault detected)",
      "Safe State (shutdown)"
    ]
  }
}
```

**Required Fields**: itemName, itemDescription, intendedFunction, asil, boundaries, operatingModes

---

### 3.3 Operational Situations Section

```json
{
  "operationalSituations": [
    {
      "situationId": "OS-{nnn}",
      "description": "Operational situation description",
      "operatingMode": "Normal Operation | Low Power Mode | Error Recovery Mode | Safe State",
      "environmentalConditions": "Temperature, vibration, EMI levels",
      "driverBehavior": "Expected user actions",
      "vehicleState": "Vehicle speed, powertrain state"
    }
  ]
}
```

**範例**:
```json
{
  "situationId": "OS-001",
  "description": "Highway driving at high speed with continuous sensor data streaming",
  "operatingMode": "Normal Operation",
  "environmentalConditions": "Temperature 25°C, vibration < 5g, EMI < level 3",
  "driverBehavior": "Hands on wheel, eyes on road",
  "vehicleState": "Speed 100 km/h, powertrain active"
}
```

**Required Fields**: situationId, description, operatingMode

---

### 3.4 Hazards Section

```json
{
  "hazards": [
    {
      "hazardId": "HAZ-{nnn}",
      "description": "Hazard event description (what goes wrong)",
      "operationalSituation": "OS-{nnn}",
      "malfunction": "Malfunctioning behavior description",
      "severity": "S0 | S1 | S2 | S3",
      "severityRationale": "Justification for severity classification",
      "exposure": "E0 | E1 | E2 | E3 | E4",
      "exposureRationale": "Justification for exposure classification",
      "controllability": "C0 | C1 | C2 | C3",
      "controllabilityRationale": "Justification for controllability classification",
      "asil": "QM | ASIL_A | ASIL_B | ASIL_C | ASIL_D",
      "derivedSafetyGoals": ["SG-{nnn}", "SG-{nnn}"]
    }
  ]
}
```

**S/E/C 分類定義** (來自 ISO-26262-3:2018 Table 1-3):

#### Severity (S):
- **S0**: No injuries
- **S1**: Light and moderate injuries
- **S2**: Severe and life-threatening injuries (survival probable)
- **S3**: Life-threatening injuries (survival uncertain), fatal injuries

#### Exposure (E):
- **E0**: Incredibly unlikely (< 0.001% operating time)
- **E1**: Very low probability (0.001% - 0.1% operating time)
- **E2**: Low probability (0.1% - 1% operating time)
- **E3**: Medium probability (1% - 10% operating time)
- **E4**: High probability (> 10% operating time)

#### Controllability (C):
- **C0**: Controllable in general
- **C1**: Simply controllable (> 99% drivers/users can avoid harm)
- **C2**: Normally controllable (≥ 90% drivers/users can avoid harm)
- **C3**: Difficult to control or uncontrollable (< 90% drivers/users can avoid harm)

**ASIL Determination** (來自 ISO-26262-3:2018 Table 4):

| S  | E  | C  | ASIL |
|----|----|----|------|
| S1 | E1 | C1 | QM   |
| S1 | E2 | C2 | A    |
| S2 | E2 | C2 | B    |
| S2 | E3 | C2 | C    |
| S3 | E3 | C2 | C    |
| S3 | E4 | C3 | D    |

**Required Fields per Hazard**:
- hazardId, description, operationalSituation, malfunction
- severity, severityRationale
- exposure, exposureRationale
- controllability, controllabilityRationale
- asil (自動計算或手動填入,須符合 Table 4)
- derivedSafetyGoals (≥ 1)

---

### 3.5 Safety Goals Section

```json
{
  "safetyGoals": [
    {
      "safetyGoalId": "SG-{nnn}",
      "description": "Safety goal description (what shall be prevented)",
      "derivedFromHazard": "HAZ-{nnn}",
      "asil": "ASIL_B",
      "safeState": "Safe state description (e.g., shutdown, degraded mode)",
      "faultTolerantTimeInterval": "Time interval in milliseconds",
      "warningAndDegradationConcept": "Description of warning to user and degradation strategy",
      "traceToTSR": ["TSR-{nnn}", "TSR-{nnn}"],
      "verificationCriteria": "How to verify this goal is achieved"
    }
  ]
}
```

**Required Fields per Safety Goal**:
- safetyGoalId, description, derivedFromHazard
- asil (須 ≥ Hazard.asil, 依 ASIL 繼承規則)
- safeState
- faultTolerantTimeInterval (for ASIL B, typically 10-1000 ms)
- traceToTSR (≥ 1, 追溯至 Technical Safety Requirements)

**ASIL 繼承規則** (來自 FR-002):
```
SafetyGoal.asil ≥ Hazard.asil
範例: 若 HAZ-001.asil = ASIL_B, 則 SG-001.asil 須為 ASIL_B, ASIL_C, 或 ASIL_D
```

---

## 4. Validation Rules Summary

### 4.1 Naming Convention (FR-006)
```
Pattern: HARA_v{Major}.{Minor}_{Status}_{YYYYMMDD}.docx
Example: HARA_v1.0_Draft_20250115.docx
```

### 4.2 Completeness Checks

| Field | Requirement |
|-------|-------------|
| Metadata | documentId, version, status, owner 必填 |
| Item Definition | itemName, asil, boundaries, operatingModes 必填 |
| Operational Situations | ≥ 3 situations (covering different operating modes) |
| Hazards | ≥ 5 hazards (覆蓋主要功能失效模式) |
| Safety Goals | ≥ 5 safety goals (每個 hazard 至少 1 個) |

### 4.3 ASIL Calculation Validation

**自動驗證邏輯**:
```python
def validate_asil_calculation(hazard):
    # ISO-26262-3:2018 Table 4 lookup table
    asil_table = {
        ("S1", "E1", "C1"): "QM",
        ("S1", "E2", "C1"): "QM",
        ("S1", "E2", "C2"): "A",
        ("S2", "E2", "C1"): "A",
        ("S2", "E2", "C2"): "B",
        ("S2", "E3", "C2"): "B",
        ("S2", "E4", "C2"): "C",
        ("S3", "E2", "C2"): "B",
        ("S3", "E3", "C2"): "C",
        ("S3", "E4", "C3"): "D",
        # ... (完整 table)
    }
    
    key = (hazard['severity'], hazard['exposure'], hazard['controllability'])
    expected_asil = asil_table.get(key, "Invalid S/E/C combination")
    
    if hazard['asil'] != expected_asil:
        return {
            "valid": False,
            "error": f"ASIL mismatch: S={hazard['severity']}, E={hazard['exposure']}, C={hazard['controllability']} should yield {expected_asil}, but got {hazard['asil']}"
        }
    
    return {"valid": True}
```

### 4.4 Traceability Checks
- 每個 Hazard 須有至少 1 個 Safety Goal (derivedSafetyGoals 不為空)
- 每個 Safety Goal 須可追溯至至少 1 個 TSR (traceToTSR 不為空)
- 反向檢查: Safety Goal 引用的 Hazard 須存在於 hazards[] 中

### 4.5 Review Requirements (FR-014)
- Document Type: **Safety Document** → 3-Party Review
- Participants: Author + Independent Reviewer + FS Manager
- Independence: Reviewer 須非 Author 本人

---

## 5. Example Document (Minimal Valid)

```json
{
  "documentId": "HARA-v1.0-Draft-20250115",
  "version": "v1.0",
  "status": "Draft",
  "creationDate": "2025-01-15",
  "owner": {
    "name": "Bob Chen",
    "email": "bob.chen@company.com",
    "role": "Functional Safety Engineer"
  },
  "itemDefinition": {
    "itemName": "PCIE Gen5 Controller (SEooC)",
    "itemDescription": "High-speed PCIe Gen5 controller for ASIL B applications",
    "intendedFunction": "Provide reliable data communication",
    "asil": "ASIL_B",
    "boundaries": {
      "functionalBoundary": "Data transmission/reception via PCIe Gen5",
      "physicalBoundary": "Controller IC + PHY",
      "interfaceBoundary": "PCIe Gen5 x4 lanes, I2C, power pins"
    },
    "operatingModes": ["Normal Operation", "Low Power Mode", "Error Recovery Mode", "Safe State"]
  },
  "operationalSituations": [
    {
      "situationId": "OS-001",
      "description": "Highway driving at high speed",
      "operatingMode": "Normal Operation",
      "environmentalConditions": "Temperature 25°C, vibration < 5g",
      "vehicleState": "Speed 100 km/h, powertrain active"
    },
    {
      "situationId": "OS-002",
      "description": "Urban driving with frequent stops",
      "operatingMode": "Normal Operation",
      "vehicleState": "Speed 30-50 km/h, frequent braking"
    },
    {
      "situationId": "OS-003",
      "description": "Parking mode",
      "operatingMode": "Low Power Mode",
      "vehicleState": "Speed 0 km/h, powertrain off"
    }
  ],
  "hazards": [
    {
      "hazardId": "HAZ-001",
      "description": "Corrupted sensor data transmitted to ECU causing incorrect perception",
      "operationalSituation": "OS-001",
      "malfunction": "PCIe controller fails to detect CRC errors, allowing corrupted data to pass through",
      "severity": "S2",
      "severityRationale": "Incorrect perception may lead to severe injuries (e.g., collision)",
      "exposure": "E2",
      "exposureRationale": "Data corruption occurs < 1% of operating time due to transient faults",
      "controllability": "C2",
      "controllabilityRationale": "Driver can normally react if warned within 2 seconds",
      "asil": "ASIL_B",
      "derivedSafetyGoals": ["SG-001"]
    },
    {
      "hazardId": "HAZ-002",
      "description": "Loss of sensor data availability causing perception blind spot",
      "operationalSituation": "OS-001",
      "malfunction": "PCIe link down due to PHY failure, no data transmitted",
      "severity": "S2",
      "severityRationale": "Blind spot may lead to collision (severe injury)",
      "exposure": "E1",
      "exposureRationale": "PHY failure occurs < 0.1% of operating time (rare)",
      "controllability": "C2",
      "controllabilityRationale": "Driver can react if warned and fallback sensor available",
      "asil": "ASIL_A",
      "derivedSafetyGoals": ["SG-002"]
    },
    {
      "hazardId": "HAZ-003",
      "description": "Delayed sensor data causing stale perception",
      "operationalSituation": "OS-001",
      "malfunction": "PCIe retransmission mechanism causes > 100ms latency",
      "severity": "S1",
      "severityRationale": "Stale data may cause light/moderate injury (minor collision)",
      "exposure": "E2",
      "exposureRationale": "Retransmission occurs < 1% of operating time",
      "controllability": "C1",
      "controllabilityRationale": "Driver can easily react if latency warning provided",
      "asil": "QM",
      "derivedSafetyGoals": ["SG-003"]
    },
    {
      "hazardId": "HAZ-004",
      "description": "Power supply failure causing sudden shutdown",
      "operationalSituation": "OS-001",
      "malfunction": "Power management unit fails, controller shuts down",
      "severity": "S2",
      "severityRationale": "Sudden shutdown may lead to severe injury (perception lost)",
      "exposure": "E1",
      "exposureRationale": "PMU failure < 0.1% of operating time",
      "controllability": "C2",
      "controllabilityRationale": "Driver can react if redundant sensor available",
      "asil": "ASIL_A",
      "derivedSafetyGoals": ["SG-004"]
    },
    {
      "hazardId": "HAZ-005",
      "description": "Electromagnetic interference causing transient errors",
      "operationalSituation": "OS-001",
      "malfunction": "EMI causes bit flips in transmitted data",
      "severity": "S2",
      "severityRationale": "Bit flips may corrupt critical safety data (severe injury)",
      "exposure": "E2",
      "exposureRationale": "EMI events < 1% of operating time",
      "controllability": "C2",
      "controllabilityRationale": "Driver can react if error detection and warning provided",
      "asil": "ASIL_B",
      "derivedSafetyGoals": ["SG-005"]
    }
  ],
  "safetyGoals": [
    {
      "safetyGoalId": "SG-001",
      "description": "Prevent transmission of corrupted sensor data to ECU",
      "derivedFromHazard": "HAZ-001",
      "asil": "ASIL_B",
      "safeState": "Detect CRC error and discard corrupted packet; notify ECU of data loss",
      "faultTolerantTimeInterval": "10 ms",
      "warningAndDegradationConcept": "Warning LED + degraded mode (reduce data rate)",
      "traceToTSR": ["TSR-001", "TSR-002"],
      "verificationCriteria": "CRC error detection rate ≥ 99.99% in fault injection test"
    },
    {
      "safetyGoalId": "SG-002",
      "description": "Detect PCIe link down and notify ECU within 50ms",
      "derivedFromHazard": "HAZ-002",
      "asil": "ASIL_B",
      "safeState": "Switch to Safe State (shutdown); activate fallback sensor",
      "faultTolerantTimeInterval": "50 ms",
      "warningAndDegradationConcept": "Warning signal to ECU + fallback sensor activation",
      "traceToTSR": ["TSR-003"],
      "verificationCriteria": "Link down detection time ≤ 50ms in 1000 trials"
    },
    {
      "safetyGoalId": "SG-003",
      "description": "Limit PCIe retransmission latency to < 100ms",
      "derivedFromHazard": "HAZ-003",
      "asil": "QM",
      "safeState": "Drop packet if retransmission latency > 100ms",
      "faultTolerantTimeInterval": "100 ms",
      "warningAndDegradationConcept": "Warning to ECU if latency > 80ms",
      "traceToTSR": ["TSR-004"],
      "verificationCriteria": "Latency < 100ms in 99.9% of retransmission events"
    },
    {
      "safetyGoalId": "SG-004",
      "description": "Detect power supply failure and shutdown gracefully within 20ms",
      "derivedFromHazard": "HAZ-004",
      "asil": "ASIL_B",
      "safeState": "Graceful shutdown (save state, notify ECU, power down)",
      "faultTolerantTimeInterval": "20 ms",
      "warningAndDegradationConcept": "Pre-shutdown warning to ECU + state save",
      "traceToTSR": ["TSR-005"],
      "verificationCriteria": "Graceful shutdown success rate ≥ 99.9% in power-off tests"
    },
    {
      "safetyGoalId": "SG-005",
      "description": "Detect and correct EMI-induced bit flips via ECC",
      "derivedFromHazard": "HAZ-005",
      "asil": "ASIL_B",
      "safeState": "Correct single-bit errors via ECC; detect double-bit errors and discard",
      "faultTolerantTimeInterval": "1 ms",
      "warningAndDegradationConcept": "Warning if double-bit error detected",
      "traceToTSR": ["TSR-006"],
      "verificationCriteria": "ECC correction rate ≥ 99.999% for single-bit errors"
    }
  ]
}
```

---

## 6. Validation Script Example

```python
def validate_hara_document(doc):
    errors = []
    
    # Check naming convention
    if not doc['documentId'].startswith('HARA-v'):
        errors.append("documentId naming pattern incorrect")
    
    # Check completeness
    if len(doc.get('operationalSituations', [])) < 3:
        errors.append("Must have ≥ 3 operational situations")
    
    if len(doc.get('hazards', [])) < 5:
        errors.append("Must have ≥ 5 hazards")
    
    if len(doc.get('safetyGoals', [])) < 5:
        errors.append("Must have ≥ 5 safety goals")
    
    # Check ASIL calculation for each hazard
    for hazard in doc.get('hazards', []):
        result = validate_asil_calculation(hazard)
        if not result['valid']:
            errors.append(f"{hazard['hazardId']}: {result['error']}")
    
    # Check traceability
    for hazard in doc.get('hazards', []):
        if not hazard.get('derivedSafetyGoals') or len(hazard['derivedSafetyGoals']) == 0:
            errors.append(f"{hazard['hazardId']} has no derived safety goals")
    
    for sg in doc.get('safetyGoals', []):
        if not sg.get('traceToTSR') or len(sg['traceToTSR']) == 0:
            errors.append(f"{sg['safetyGoalId']} has no traceability to TSR")
    
    # Check ASIL inheritance (SafetyGoal.asil ≥ Hazard.asil)
    hazard_asil_map = {h['hazardId']: h['asil'] for h in doc.get('hazards', [])}
    asil_order = {"QM": 0, "ASIL_A": 1, "ASIL_B": 2, "ASIL_C": 3, "ASIL_D": 4}
    
    for sg in doc.get('safetyGoals', []):
        hazard_id = sg.get('derivedFromHazard')
        if hazard_id in hazard_asil_map:
            hazard_asil = hazard_asil_map[hazard_id]
            sg_asil = sg.get('asil')
            if asil_order[sg_asil] < asil_order[hazard_asil]:
                errors.append(f"{sg['safetyGoalId']} ASIL ({sg_asil}) < {hazard_id} ASIL ({hazard_asil})")
    
    return {"valid": len(errors) == 0, "errors": errors}
```

---

**Last Updated**: 2025-01-18  
**Version**: v1.0  
**Schema Type**: HARA Document
