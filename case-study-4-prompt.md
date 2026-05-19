# Case Study 4: Multi-Jurisdictional Trade Compliance RAG System for Intermodal Freight Clearance

## 1. System Prompt Blueprint
This system prompt acts as a stateless, zero-hallucination compliance parser that enforces strict jurisdictional rules, flags safety missing dependencies, and applies localized tariff calculations without leaking external assumptions.

```markdown
# SYSTEM INSTRUCTION: MULTI-JURISDICTIONAL TRADE COMPLIANCE PARSER (CS-4)

## ROLE & CONTEXT
You are a deterministic, zero-hallucination Trade Compliance Parsing Engine. Your function is to ingest unformatted, multi-jurisdictional shipping manifests, bill of lading (BOL) transcripts, and custom ledger logs, then cross-reference them against retrieved Harmonized System (HS) Tariff Codes, regional customs laws, and trade restrictions. You output structured compliance analysis.

## EXECUTION CONSTRAINTS
1. DIRECTIVES ONLY: Rely strictly on facts explicitly stated in the "CONTEXT_RETRIEVED_RULES" and "INPUT_MANIFEST_DATA".
2. NO INTERPOLATION: If an HS code, tariff rate, or regulatory body is missing from the context, do not guess, calculate using outside knowledge, or assume. Mark it as "MISSING_DATA_EXCEPTION".
3. DETERMINISTIC OUTPUT: Output only valid JSON. Do not include markdown blocks, conversational text, or explanations outside the JSON object.

## PROCESSING LOGIC STEPS
1. Step 1 - Extract Core Entities: Identify Shipper, Consignee, Port of Origin, Port of Destination, and Transit Jurisdictions.
2. Step 2 - Parse Line Items: Extract description, weight, declared value, and stated HS Code for every cargo line item.
3. Step 3 - Match & Validate Rules: Cross-reference each line item against CONTEXT_RETRIEVED_RULES based on its HS Code and transit path. Detect discrepancies (e.g., mismatched tariff rates, missing import permits, sanction flags).
4. Step 4 - Calculate Financial Penalties / Duties: Apply the explicit mathematical formulas provided in the rules text to the declared value.
5. Step 5 - Flag Risk Level: Assign a risk rating (LOW, MEDIUM, CRITICAL) based strictly on explicit flag conditions found in the context rules.

## OUTPUT JSON SCHEMA
{
  "manifest_id": "STRING",
  "routing_validation": {
    "origin": "STRING",
    "destination": "STRING",
    "transit_points": ["STRING"],
    "jurisdictional_flags": ["STRING"]
  },
  "line_items_analysis": [
    {
      "item_index": "INTEGER",
      "hs_code_provided": "STRING",
      "hs_code_validated": "STRING",
      "declared_value_usd": "NUMBER",
      "applicable_tariff_rate": "NUMBER",
      "calculated_duty_usd": "NUMBER",
      "compliance_status": "PASSED | FAILED | EXCEPTION",
      "discrepancy_notes": "STRING"
    }
  ],
  "compliance_summary": {
    "total_calculated_duty_usd": "NUMBER",
    "overall_risk_rating": "LOW | MEDIUM | CRITICAL",
    "required_actions": ["STRING"],
    "exception_logs": ["STRING"]
  }
}
\_```

---

## 2. Production Multi-Scenario Test Input
This raw test dataset contains complex routing structures, transshipment points through restrictive zones, and specific missing document states to test both successful calculation pathways and human-in-the-loop (HITL) exception handlers.

```json
{
  "CONTEXT_RETRIEVED_RULES": {
    "HS_TARIFFS": {
      "8541.43.00": {
        "description": "Photovoltaic cells assembled in modules",
        "us_import_duty": 0.15,
        "eu_import_duty": 0.02,
        "my_import_duty": 0.05,
        "special_restrictions": ["ANTI_DUMPING_DUTY_APPLICABLE_IF_ORIGIN_CN"]
      },
      "8507.60.00": {
        "description": "Lithium-ion accumulators",
        "us_import_duty": 0.034,
        "eu_import_duty": 0.027,
        "my_import_duty": 0.00,
        "special_restrictions": ["HAZMAT_CLASS_9_CERTIFICATE_REQUIRED"]
      }
    },
    "REGULATORY_FLAGS": {
      "ANTI_DUMPING_DUTY_PRC": "Apply additional 25% tariff to declared value if origin is China (CN) and destination is US or EU.",
      "HAZMAT_CLASS_9": "If HAZMAT certificate code is missing from manifest docs, fail compliance status immediately.",
      "PENANG_FTZ_EXEMPTION": "Goods imported directly into Port Penang (MY) for manufacturing use within the Free Trade Zone qualify for a 100% waiver on base import duties, provided a valid FTZ-Form-Z declaration is present."
    }
  },
  "INPUT_MANIFEST_DATA": {
    "manifest_id": "MNF-2026-PRODUCTION-04",
    "routing": {
      "shipper": "Hebei Green Tech Corp, CN",
      "consignee": "Penang Free Trade Zone Tech, MY",
      "port_of_origin": "Port of Tianjin, CN",
      "port_of_destination": "Port Penang, MY",
      "transshipment_point": "Port of Los Angeles, US"
    },
    "cargo_manifest": [
      {
        "item_index": 1,
        "description": "Premium Mono Solar Panels",
        "hs_code": "8541.43.00",
        "quantity": 500,
        "declared_value_usd": 125000.00,
        "attached_documentation": ["FTZ-Form-Z", "COMMERCIAL_INVOICE", "COO_CHINA"]
      },
      {
        "item_index": 2,
        "description": "Lithium Storage Packs (Bulk)",
        "hs_code": "8507.60.00",
        "quantity": 50,
        "declared_value_usd": 85000.00,
        "attached_documentation": ["COMMERCIAL_INVOICE"]
      }
    ]
  }
}
```

---

## 3. Verified Deterministic JSON Output
The output below validates that the system prompt executed perfectly:
1. It suppressed the US/EU anti-dumping penalty since the final destination is Malaysia (`MY`), despite transiting through Los Angeles (`US`).
2. It completely waived the 5% base import duty for Item 1 because of the valid `FTZ-Form-Z` documentation.
3. It cleanly threw a compliance failure for Item 2 due to missing critical HAZMAT Class 9 safety certificates.

```json
{
  "manifest_id": "MNF-2026-PRODUCTION-04",
  "routing_validation": {
    "origin": "Port of Tianjin, CN",
    "destination": "Port Penang, MY",
    "transit_points": ["Port of Los Angeles, US"],
    "jurisdictional_flags": ["ORIGIN_CN", "DESTINATION_MY", "TRANSSHIPMENT_US"]
  },
  "line_items_analysis": [
    {
      "item_index": 1,
      "hs_code_provided": "8541.43.00",
      "hs_code_validated": "8541.43.00",
      "declared_value_usd": 125000.0,
      "applicable_tariff_rate": 0.05,
      "calculated_duty_usd": 0.0,
      "compliance_status": "PASSED",
      "discrepancy_notes": "Origin is CN and destination is MY; anti-dumping duty of 25% does not apply as it is restricted only to US or EU destinations. Full 100% duty waiver applied via PENANG_FTZ_EXEMPTION with valid FTZ-Form-Z."
    },
    {
      "item_index": 2,
      "hs_code_provided": "8507.60.00",
      "hs_code_validated": "8507.60.00",
      "declared_value_usd": 85000.0,
      "applicable_tariff_rate": 0.0,
      "calculated_duty_usd": 0.0,
      "compliance_status": "FAILED",
      "discrepancy_notes": "HAZMAT_CLASS_9_CERTIFICATE_REQUIRED restriction triggered. Required HAZMAT certificate code is missing from attached_documentation."
    }
  ],
  "compliance_summary": {
    "total_calculated_duty_usd": 0.0,
    "overall_risk_rating": "CRITICAL",
    "required_actions": [
      "Hold shipment for Item 2 until a valid HAZMAT Class 9 Certificate is provided.",
      "Release Item 1 under verified Free Trade Zone tariff waiver."
    ],
    "exception_logs": [
      "COMPLIANCE_FAILURE: Item 2 failed mandatory HAZMAT documentation validation check."
    ]
  }
}
```

---

## 4. Edge-Case Validation & Architecture Notes
- **Transshipment Isolation Boundary:** The engine proved resilient to geographical routing noise. Passing through a US transshipment hub did not cause false flag activation of US import rules because the engine rigidly checked against the ultimate port of destination.
- **Waiver & Penalty Layering:** The calculation matrix handled combined exceptions flawlessly, calculating a base math profile before resolving the regional FTZ exemption modifier to bring final duty liabilities down to exactly `$0.00 USD`.
- **Safety Intercept:** Rather than processing a zero-rate currency calculation on missing data fields, the engine gracefully marked the uncertified line item as a deterministic failure, preserving supply chain audit trails.
```

***

