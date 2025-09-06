# Quality Cascade — Unified Scoring for QA / Symbolic / Theory

**Goal**: Produce a single **quality_overall** score and detailed sub-scores integrating translation QA, symbolic coherence, and theory consistency.

---

## Inputs
- **Text / Output** to evaluate  
- Optional references: source text (for translation), symbol palette (for poetic), scenario/theory context  

---

## Sub-scores (0–1)
- **semantic** (translation meaning alignment)  
- **structural** (format, punctuation, style constraints)  
- **symbolic_coherence** (symbol chain preservation, ΔS drift control)  
- **islm_bias** (IS-LM consistency with scenario)  
- **var_regime_shift** (if VAR indicates unstable regime, dampen overall)  
- **risk_parity_breakdown** (penalize if diversification fails)  

---

## Aggregation
Let weights `w = {ws, wt, wc, wi, wv, wr}` correspond to the above order.

quality_overall = clip(
wssemantic + wtstructural + wcsymbolic_coherence +
wi(1 - |islm_bias|) +
(1 - 0.5wvvar_regime_shift) +
(1 - 0.6wrrisk_parity_breakdown),
0, 1
)


### Default Weights (Lite)
- `semantic 0.25`  
- `structural 0.20`  
- `symbolic_coherence 0.20`  
- `islm_bias 0.15`  
- `var_regime_shift 0.10`  
- `risk_parity_breakdown 0.10`  

### Heuristics
- Penalize severe contradictions: if any of `{var_regime_shift, risk_parity_breakdown} > 0.7`, cap `quality_overall ≤ 0.6`.  
- If `symbolic_cascade` depth ≥ 3 and coherence ≥ 0.7, small boost `+0.05` (max 1.0).  

---

## Prompt
/quality-cascade "<text or module output>" --with-translation --with-symbolic --with-theory


---

## Output (use unified-output-template)
```json
{
  "module": "quality-cascade",
  "scores": {
    "semantic": 0.82,
    "structural": 0.77,
    "symbolic_coherence": 0.71,
    "islm_bias": 0.12,
    "var_regime_shift": 0.30,
    "risk_parity_breakdown": 0.25,
    "quality_overall": 0.74
  },
  "rationales": [
    "semantic alignment strong; minor structure issues; regime moderately stable"
  ],
  "confidence": 0.78
}
Notes
Works as a meta-evaluator: feed outputs from other modules.

Keep rationales short and auditable; prefer bullet evidence.