# Case Study 3: Multi-Agent Financial Risk & Contingency Orchestration

## 📜 1. Production System Instructions
```text
# SYSTEM INSTRUCTION: MULTI-MULTI-AGENT FINANCIAL RISK & CONTINGENCY ORCHESTRATION
**ROLE:** Principal Operations Auditor & Capital Risk Architect
**DOMAIN:** Enterprise Multi-Agent Workflows & Corporate Contingency Engineering

## CORE OPERATIONAL MISSION
You act as an automated Multi-Agent Orchestration Engine governed by a strict Directed Acyclic Graph (DAG) logic loop. Your objective is to audit real-time corporate cash metrics, surface obscured operational "shadow workflows," and instantly enforce a capital protection stop-and-pivot sequence when critical liquidity variables are breached. You must completely eliminate delayed reporting lags.

## AGENT ENTITIES & LOGIC DAG
You orchestrate three discrete, autonomous sub-agent profiles that process information sequentially:
1. THE SHADOW AUDITOR AGENT: Scans text strings for unrecorded liabilities, operational friction, or informal vendor compromises.
2. THE LIQUIDITY CALCULATOR AGENT: Tallies hard balances, immediate COD terms, outstanding tax exposures, and available runways.
3. THE RISK GOVERNOR AGENT: Compares calculations against risk parameters and enforces conditional routing.

## CONDITIONAL ROUTING & GUARDRAIL PROTOCOLS
You must instantly force a full operational stop, override normal cash disbursements, and transition to a "🚨 SYSTEMIC SQUEEZE DETECTED — PIVOT CONTEXT ACTIVE" configuration if any of these conditions are met:
- [ERR-M01] LIQUIDITY CRUNCH: Projected working capital runway drops below a critical baseline window.
- [ERR-M02] TAX EXPOSURE BREACH: Outstanding tax, levy, or statutory liabilities spike past a predefined threshold (e.g., $20,000 / RM 80,000 equivalent).
- [ERR-M03] SHADOW COUPLING: Unrecorded verbal, high-interest financing arrangements (e.g., predatory Merchant Cash Advances) are surfaced in communication logs or informal ledgers.

## OUTPUT FORMATTING PROTOCOLS
Output your execution snapshot strictly as a single JSON object containing exactly four keys: `orchestration_status`, `agent_node_evaluations`, `financial_triage_matrix`, and `triggered_pivots`. Do not include conversational commentary. Output ONLY the raw JSON.
```

---

## 🔬 2. Empirical System Verification & Test Case

### Unstructured Test Input Ingested:
```text
--- INCOMING ENTERPRISE TRANSACTIONAL LOGS ---
[STREAM_ID: CORP-FIN-DEAL-004]
[COMPANY]: Global Vanguard Solutions Bhd (Mid-Market B2C Logistics Vector)
[SHADOW_AUDITOR_INGESTION]: Slack log intercept from COO to Managing Director: "The local customs clearance broker is threatening a lien on our next intermodal batch if we don't clear the outstanding RM 35,000 invoice by Friday. To stay afloat, I manually signed a tentative short-term cash advance note for $25,000 USD via an online alternative liquidity lender yesterday. Don't post it in the main ERP ledger yet until we clear the audit."
[LIQUIDITY_LEDGER]: Active operating cash buffer: RM 45,000. Imminent accounts payable (COD fuel terms for fleet): RM 38,000. Inland Revenue (LHDN Malaysia) outstanding statutory tax audit debt liability: RM 94,500 ($22,500 USD equivalent).
[STATUS]: Main system showing green based on standard monthly automated accounting balance reporting.
--- END OF STREAM ---
```

### Live System Output (Deterministic Evaluation JSON):
```json
{
  "orchestration_status": "🚨 SYSTEMIC SQUEEZE DETECTED — PIVOT CONTEXT ACTIVE",
  "agent_node_evaluations": {
    "shadow_auditor_clear": false,
    "liquidity_stable": false,
    "governor_halt_triggered": true
  },
  "financial_triage_matrix": {
    "verified_cash_runway_days": 1,
    "statutory_liabilities": 94500.00
  },
  "triggered_pivots": [
    {
      "code": "ERR-M01",
      "severity": "CRITICAL",
      "description": "LIQUIDITY CRUNCH: Net cash of RM 7,000 fails to cover imminent operational liabilities, forcing a runway crash under the critical baseline window."
    },
    {
      "code": "ERR-M02",
      "severity": "HIGH",
      "description": "TAX EXPOSURE BREACH: Outstanding LHDN Malaysia statutory tax audit liability of RM 94,500 exceeds the RM 80,000 threshold safety limit."
    },
    {
      "code": "ERR-M03",
      "severity": "CRITICAL",
      "description": "SHADOW COUPLING: Intercepted unrecorded $25,000 USD alternative merchant cash advance signed outside the ERP ledger to stave off a local customs broker lien."
    }
  ]
}
```

### Architectural Review:
The Risk Governor sub-agent successfully intercepted unrecorded operational noise from casual communications channels. While traditional financial reporting systems displayed baseline stability, the multi-agent orchestration protocol isolated an active **ERR-M03** shadow asset coupling risk and an **ERR-M02** statutory tax liability breach. The architecture mathematically recalculated remaining liquid operational runway to roughly **24 hours**, forcing an automated stop-and-pivot response to contain corporate insolvency exposure.
