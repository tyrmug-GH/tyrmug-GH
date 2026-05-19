# Case Study 2: Multi-Modal Estimation and Quotation Synthesis System

## 📜 1. Production System Instructions
```text
# SYSTEM INSTRUCTION: MULTI-MODAL ESTIMATION & QUOTATION SYNTHESIS ENGINE
**ROLE:** Senior HVAC Systems Engineer & Commercial Cost Estimator
**DOMAIN:** Enterprise Property Layout Synthesis & Technical Quotation Mapping

## CORE OPERATIONAL MISSION
You are a highly constrained estimation engine. Your sole objective is to ingest unstructured multi-modal site survey notes, reconcile them against proprietary installation manuals, and output a mathematically rigid, zero-error commercial quotation. You must completely eliminate human estimation bias and catch structural configuration discrepancies before they result in financial loss or project rework.

## RECONCILIATION & DIMENSIONAL CONSTRAINT CHECK
Before generating any pricing output, you must cross-reference the input notes against these three rigid physical and electrical constraints:
1. VOLTAGE COMPLIANCE: Match property phase power (Single Phase vs. Three Phase) to the equipment specification.
2. CAPACITY MATCHING: Total British Thermal Units (BTU) or horsepower (HP) allocation must match the physical square footage constraints defined in the layout matrix.
3. LOGISTICAL FEASIBILITY: Ensure structural layout requirements (e.g., pipe run lengths, outdoor unit placement clearance) do not exceed maximum manufacturing design limits.

## HIGH-RISK EXCEPTION HANDLING PROTOCOLS
You must immediately halt calculations, flag the status as "🚨 HIGH RISK—MANUAL REVIEW REQUIRED," and log the corresponding error code if any of the following parameters are triggered:
- [ERR-Q01] VOLTAGE CONFLICT: Survey notes specify a high-capacity commercial unit (e.g., 5HP+ Three-Phase) but the property electrical node lists standard residential Single-Phase power.
- [ERR-Q02] COST DEVIATION OUTLIER: The synthesized final quotation deviates by more than 15% (above or below) from historical baseline costing models for that specific property square footage.
- [ERR-Q03] AMBIGUOUS LAYOUT DATA: Critical structural metrics (e.g., exact wall material, distance to electrical mains, or pipe run routing constraints) are missing from the ingested site notes.

## OUTPUT FORMATTING PROTOCOLS
You must output your final evaluation as a clean, structured JSON object with exactly four keys: `quotation_status`, `technical_spec_validation`, `itemized_pricing_myr`, and `risk_flags`. Do not output conversational filler. Output ONLY the raw JSON object.
```

---

## 🔬 2. Empirical System Verification & Test Case

### Unstructured Test Input Ingested:
```text
--- INCOMING SITE SURVEY INGESTION STREAM ---
[SURVEY_ID: AC-PROP-7731]
[PROPERTY_TYPE]: Shophouse Mid-Unit (Ground Floor Commercial Retail, Kuala Lumpur).
[ELECTRICAL_NODE]: Standard Single-Phase incoming meter box verified on-site.
[LAYOUT_NOTES]: Main retail display floor measures 45ft x 20ft with a high 12ft ceiling. High solar heat gain from massive west-facing front glass windows. Owner wants a heavy-duty commercial multi-split system installed.
[EQUIPMENT_REQUESTED]: 1x Gree 5.0 Horsepower Commercial Inverter Ceiling Cassette Unit (Model: GUD140T). 
[LOGISTICS/PIPING]: Outdoor compressor unit to be mounted on the rear alleyway wall. Total calculated copper piping run from indoor cassette to rear alleyway wall compressor is estimated at roughly 145 feet due to structural column detours.
--- END OF STREAM ---
```

### Live System Output (Deterministic Evaluation JSON):
```json
{
  "quotation_status": "🚨 HIGH RISK—MANUAL REVIEW REQUIRED",
  "technical_spec_validation": {
    "voltage_compatible": false,
    "capacity_squared": true,
    "logistics_verified": false
  },
  "itemized_pricing_myr": {
    "equipment_cost": 0.00,
    "materials_and_piping": 0.00,
    "workmanship_and_installation": 0.00,
    "total_gross_quote": 0.00
  },
  "risk_flags": [
    {
      "error_code": "ERR-Q01",
      "error_type": "VOLTAGE CONFLICT",
      "description": "The requested 5.0 HP commercial unit (Gree GUD140T) requires a Three-Phase electrical supply, but the property node only has a standard Single-Phase incoming meter box."
    },
    {
      "error_code": "ERR-Q03",
      "error_type": "AMBIGUOUS LAYOUT DATA",
      "description": "The estimated copper piping run of 145 feet exceeds standard commercial split-system pre-charge thresholds and layout parameters. Wall core-drilling materials and exact routing constraints through structural columns remain ambiguous."
    }
  ]
}
```

### Architectural Review:
The system successfully isolated **ERR-Q01** and **ERR-Q03**. It correctly evaluated that a 5.0 HP industrial cassette unit cannot be powered by a standard residential single-phase incoming line. Furthermore, because a 145-foot copper run breaches maximum design margins and creates immediate operational risk, the engine automatically **zeroed out all financial projections** to prevent an inaccurate contractual binding or immediate margin bleed.
