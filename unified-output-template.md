# Unified Output Template (Markdown + JSON Canon)

Use this template across all modules for consistent evaluation, logging, and downstream parsing.  
The **Markdown section** is human-first; the **JSON block** is machine-first and authoritative.

---

## Markdown Summary (Human-Readable)
- **Module**: <module-name>  
- **Scenario / Input**: <short description>  
- **Headline**: <one-line takeaway>  
- **Scores**: <key scores/flags>  
- **Rationales**: <bulleted reasons>  
- **Risks / Contradictions**: <edge cases, theory conflicts>  
- **Next Actions**: <what to check next>  

---

## JSON Canon (Machine-Readable)
```jsonc
{
  "module": "<string> // e.g., translation-qa, poetic-core, var-model, is-lm, risk-parity, quality-cascade",
  "version": "3.0.0",
  "timestamp": "<ISO-8601>",
  "input": {
    "raw": "<original prompt or data>",
    "options": {
      "flags": ["--synapse-mode","--temporal-projection"],
      "params": {"window":"12m"}
    }
  },
  "scores": {
    "semantic": 0.0,              // translation-qa
    "structural": 0.0,            // translation-qa
    "symbolic_coherence": 0.0,    // poetic-core / symbolic-cascade
    "islm_bias": 0.0,             // is-lm
    "var_regime_shift": 0.0,      // var-model (0–1)
    "risk_parity_breakdown": 0.0, // risk-parity (0–1)
    "quality_overall": 0.0        // quality-cascade
  },
  "flags": ["correlation_breakdown","flight_to_quality"],
  "findings": [
    {"label":"pair_corr","pair":["SPX","US10Y"],"bin":"Weak-"},
    {"label":"symbol","token":"iron lungs","role":"industrial-city"}
  ],
  "rationales": ["concise reasons supporting the scores"],
  "contradictions": ["theory mismatch or data inconsistency"],
  "confidence": 0.0,
  "next_actions": ["collect example set B","re-run with 24m window"],
  "meta": {"generator":"UI-Lite v3", "run_id":"<uuid>"}
}
Conventions
Scores are 0–1 and monotonic (higher = stronger/greater alignment).

Bins prefer ordinal communication when numeric uncertainty is high.

Flags are additive; downstream can map to UI warnings.

confidence combines module priors + cross-check stability.
