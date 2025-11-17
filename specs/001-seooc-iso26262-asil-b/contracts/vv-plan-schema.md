# V&V Plan (Verification and Validation Plan) Document Schema

## 1. Purpose

定義 V&V (Verification and Validation) Plan 文件的介面規範,確保符合 ISO-26262-8:2018 §9-§13 (V&V) 要求。

## 2. ISO-26262 References

- **ISO-26262-8:2018 §9**: Verification of the software safety requirements
- **ISO-26262-8:2018 §10**: Verification of the software architectural design
- **ISO-26262-8:2018 §11**: Verification of the software unit design and implementation
- **ISO-26262-8:2018 §13**: Verification of the software integration and testing
- **ISO-26262-8:2018 Table 12**: Verification methods for ASIL B

## 3. Document Structure

### 3.1 Metadata Section

```json
{
  "documentId": "VV-PLAN-v{Major}.{Minor}-{Status}-{YYYYMMDD}",
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

### 3.2 Test Strategy Section

**4-Stage Testing Strategy** (來自 FR-004):

```json
{
  "testStrategy": {
    "stage1_unitTesting": {
      "name": "Unit Testing",
      "scope": "Individual safety mechanisms (CRC check, ECC, link-down detection, power monitoring)",
      "coverage": {
        "statementCoverage": "≥ 95%",
        "branchCoverage": "≥ 90%",
        "mcdc": "Not required for ASIL B"
      },
      "asilRequirement": "ASIL_B",
      "tools": ["Verilog simulator", "Code coverage tool"],
      "estimatedEffort": "80 hours"
    },
    "stage2_integrationTesting": {
      "name": "Integration Testing",
      "scope": "Safety function integration (PCIe TX/RX + error detection + safe state control)",
      "coverage": {
        "interfaceCoverage": "100% (all module interfaces tested)",
        "scenarioCoverage": "≥ 90% (operational scenarios)"
      },
      "asilRequirement": "ASIL_B",
      "tools": ["System testbench", "Interface protocol analyzer"],
      "estimatedEffort": "120 hours"
    },
    "stage3_systemTesting": {
      "name": "System Testing",
      "scope": "Full system safety validation (end-to-end data integrity, fault tolerance)",
      "coverage": {
        "safetyGoalCoverage": "100% (all safety goals verified)",
        "faultInjectionCoverage": "≥ 95% (fault scenarios)"
      },
      "asilRequirement": "ASIL_B",
      "tools": ["Fault injection tool", "System-level test framework"],
      "estimatedEffort": "160 hours"
    },
    "stage4_regressionTesting": {
      "name": "Regression Testing",
      "scope": "Change impact verification (re-run affected test cases after code changes)",
      "coverage": {
        "modifiedCodeCoverage": "≥ 95%",
        "affectedTestCaseRerun": "100%"
      },
      "asilRequirement": "ASIL_B",
      "tools": ["Automated test harness", "Version control system"],
      "estimatedEffort": "40 hours (per regression cycle)"
    }
  }
}
```

**Required Fields per Stage**:
- name, scope, coverage, asilRequirement, tools, estimatedEffort

---

### 3.3 Verification Methods Section

**ISO-26262-8:2018 Table 12 Verification Methods** (for ASIL B):

```json
{
  "verificationMethods": [
    {
      "methodId": "VM-001",
      "methodName": "Test",
      "description": "Execution of test cases with actual or simulated inputs",
      "applicableStages": ["Unit Testing", "Integration Testing", "System Testing", "Regression Testing"],
      "asilRecommendation": "Highly Recommended (++)",
      "coverage": "Statement ≥ 95%, Branch ≥ 90%"
    },
    {
      "methodId": "VM-002",
      "methodName": "Review",
      "description": "Peer review or formal inspection of design and code",
      "applicableStages": ["Unit Testing", "Integration Testing"],
      "asilRecommendation": "Recommended (+)",
      "coverage": "100% safety-critical modules reviewed"
    },
    {
      "methodId": "VM-003",
      "methodName": "Analysis",
      "description": "Static analysis (e.g., MISRA-C compliance, control/data flow)",
      "applicableStages": ["Unit Testing"],
      "asilRecommendation": "Recommended (+)",
      "coverage": "100% code analyzed for MISRA-C violations"
    },
    {
      "methodId": "VM-004",
      "methodName": "Simulation",
      "description": "Model-based simulation or fault injection",
      "applicableStages": ["System Testing"],
      "asilRecommendation": "Recommended (+)",
      "coverage": "≥ 95% fault scenarios simulated"
    }
  ]
}
```

**Required Fields per Method**:
- methodId, methodName, applicableStages, asilRecommendation, coverage

---

### 3.4 Test Cases Section

```json
{
  "testCases": [
    {
      "testCaseId": "TC-{nnn}",
      "description": "Test case description",
      "type": "Unit | Integration | System | Regression",
      "traceToConcept": {
        "tsr": ["TSR-{nnn}", "TSR-{nnn}"],
        "design": ["DSN-{nnn}"],
        "safetyGoal": ["SG-{nnn}"]
      },
      "verificationMethod": "Test | Review | Analysis | Simulation",
      "testProcedure": "Step-by-step test execution procedure",
      "testInput": "Input data or stimulus description",
      "expectedResult": "Expected output or behavior",
      "acceptanceCriteria": "Specific pass/fail criteria",
      "actualResult": "Actual output (filled after execution)",
      "status": "NotRun | Passed | Failed | Blocked",
      "executionDate": "YYYY-MM-DD",
      "tester": "Tester Name"
    }
  ]
}
```

**Required Fields per Test Case**:
- testCaseId, description, type, traceToConcept (至少 TSR), verificationMethod, expectedResult, acceptanceCriteria

---

## 4. Validation Rules Summary

### 4.1 Naming Convention (FR-006)
```
Pattern: VV-PLAN_v{Major}.{Minor}_{Status}_{YYYYMMDD}.docx
Example: VV-PLAN_v1.0_Draft_20250115.docx
```

### 4.2 Completeness Checks

| Field | Requirement |
|-------|-------------|
| Metadata | documentId, version, status, owner 必填 |
| Test Strategy | 4 stages 必須全部定義 (Unit, Integration, System, Regression) |
| Verification Methods | ≥ 4 methods (Test, Review, Analysis, Simulation) |
| Test Cases | ≥ 20 test cases (covering all TSRs) |

### 4.3 Coverage Requirements (ASIL B)

**Statement Coverage**: ≥ 95%
**Branch Coverage**: ≥ 90%
**Safety Goal Coverage**: 100%

```python
def validate_coverage(vv_plan):
    errors = []
    
    # Check statement coverage
    unit_stage = vv_plan['testStrategy'].get('stage1_unitTesting', {})
    statement_cov = unit_stage.get('coverage', {}).get('statementCoverage', '')
    if not statement_cov.startswith('≥ 95%'):
        errors.append("Unit testing statement coverage must be ≥ 95%")
    
    # Check branch coverage
    branch_cov = unit_stage.get('coverage', {}).get('branchCoverage', '')
    if not branch_cov.startswith('≥ 90%'):
        errors.append("Unit testing branch coverage must be ≥ 90%")
    
    # Check safety goal coverage
    system_stage = vv_plan['testStrategy'].get('stage3_systemTesting', {})
    sg_cov = system_stage.get('coverage', {}).get('safetyGoalCoverage', '')
    if not sg_cov.startswith('100%'):
        errors.append("System testing safety goal coverage must be 100%")
    
    return {"valid": len(errors) == 0, "errors": errors}
```

### 4.4 Traceability Validation

**每個 TSR 須至少被 1 個 Test Case 驗證**:

```python
def validate_tsr_coverage(vv_plan, tsr_doc):
    errors = []
    tsr_ids = set([req['requirementId'] for req in tsr_doc['requirements']])
    tsr_covered = set()
    
    for tc in vv_plan.get('testCases', []):
        tsr_covered.update(tc.get('traceToConcept', {}).get('tsr', []))
    
    uncovered = tsr_ids - tsr_covered
    if uncovered:
        errors.append(f"TSRs not covered by test cases: {uncovered}")
    
    return {"valid": len(errors) == 0, "errors": errors}
```

---

## 5. Example Document (Minimal Valid)

```json
{
  "documentId": "VV-PLAN-v1.0-Draft-20250115",
  "version": "v1.0",
  "status": "Draft",
  "creationDate": "2025-01-15",
  "owner": {
    "name": "David Zhang",
    "email": "david.zhang@company.com",
    "role": "Quality Engineer"
  },
  "testStrategy": {
    "stage1_unitTesting": {
      "name": "Unit Testing",
      "scope": "CRC check, ECC, link-down detection, power monitoring modules",
      "coverage": {
        "statementCoverage": "≥ 95%",
        "branchCoverage": "≥ 90%"
      },
      "asilRequirement": "ASIL_B",
      "tools": ["Verilog simulator", "VCS coverage tool"],
      "estimatedEffort": "80 hours"
    },
    "stage2_integrationTesting": {
      "name": "Integration Testing",
      "scope": "PCIe TX/RX + error detection + safe state control integration",
      "coverage": {
        "interfaceCoverage": "100%",
        "scenarioCoverage": "≥ 90%"
      },
      "asilRequirement": "ASIL_B",
      "tools": ["SystemVerilog testbench", "Protocol analyzer"],
      "estimatedEffort": "120 hours"
    },
    "stage3_systemTesting": {
      "name": "System Testing",
      "scope": "End-to-end data integrity, fault tolerance validation",
      "coverage": {
        "safetyGoalCoverage": "100%",
        "faultInjectionCoverage": "≥ 95%"
      },
      "asilRequirement": "ASIL_B",
      "tools": ["Fault injection tool", "System test framework"],
      "estimatedEffort": "160 hours"
    },
    "stage4_regressionTesting": {
      "name": "Regression Testing",
      "scope": "Re-run affected test cases after code changes",
      "coverage": {
        "modifiedCodeCoverage": "≥ 95%",
        "affectedTestCaseRerun": "100%"
      },
      "asilRequirement": "ASIL_B",
      "tools": ["Jenkins CI", "Git version control"],
      "estimatedEffort": "40 hours per cycle"
    }
  },
  "verificationMethods": [
    {
      "methodId": "VM-001",
      "methodName": "Test",
      "applicableStages": ["Unit Testing", "Integration Testing", "System Testing", "Regression Testing"],
      "asilRecommendation": "Highly Recommended (++)",
      "coverage": "Statement ≥ 95%, Branch ≥ 90%"
    },
    {
      "methodId": "VM-002",
      "methodName": "Review",
      "applicableStages": ["Unit Testing", "Integration Testing"],
      "asilRecommendation": "Recommended (+)",
      "coverage": "100% safety-critical modules reviewed"
    },
    {
      "methodId": "VM-003",
      "methodName": "Analysis",
      "applicableStages": ["Unit Testing"],
      "asilRecommendation": "Recommended (+)",
      "coverage": "100% code analyzed for MISRA-C compliance"
    },
    {
      "methodId": "VM-004",
      "methodName": "Simulation",
      "applicableStages": ["System Testing"],
      "asilRecommendation": "Recommended (+)",
      "coverage": "≥ 95% fault scenarios simulated"
    }
  ],
  "testCases": [
    {
      "testCaseId": "TC-001",
      "description": "Verify CRC error detection with single-bit corruption",
      "type": "Unit",
      "traceToConcept": {
        "tsr": ["TSR-001"],
        "design": ["DSN-001"],
        "safetyGoal": ["SG-001"]
      },
      "verificationMethod": "Test",
      "testProcedure": "1. Inject single-bit error in packet; 2. Run CRC check; 3. Verify error flagged",
      "testInput": "PCIe packet with bit 15 flipped",
      "expectedResult": "CRC error flag set to 1",
      "acceptanceCriteria": "CRC error detection in 100% of 1000 trials",
      "status": "NotRun"
    },
    {
      "testCaseId": "TC-002",
      "description": "Verify CRC error detection with multi-bit corruption",
      "type": "Unit",
      "traceToConcept": {
        "tsr": ["TSR-001"],
        "design": ["DSN-001"],
        "safetyGoal": ["SG-001"]
      },
      "verificationMethod": "Test",
      "testProcedure": "1. Inject 5-bit errors in packet; 2. Run CRC check; 3. Verify error flagged",
      "testInput": "PCIe packet with bits 10,20,30,40,50 flipped",
      "expectedResult": "CRC error flag set to 1",
      "acceptanceCriteria": "CRC error detection in ≥ 99.99% of 10,000 trials",
      "status": "NotRun"
    }
  ]
}
```

---

**Last Updated**: 2025-01-18  
**Version**: v1.0  
**Schema Type**: V&V Plan Document
