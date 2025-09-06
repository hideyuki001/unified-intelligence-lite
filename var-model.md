# VAR Model — Multi-Asset Correlation & Shock Analysis (Lite)

**Purpose**: Provide a promptable Vector Autoregression (VAR) abstraction for multi-asset correlation analysis, regime-shift detection, and shock propagation using language-model reasoning (no code runtime required).

## Inputs
- **Assets**: Tickers or descriptors (e.g., `SPX, NDX, BTC, GOLD, WTI, US10Y, DXY`).
- **Window**: Narrative lookback (e.g., `short=3m, mid=12m, long=36m`).
- **Shock** (optional): Exogenous delta (e.g., `Fed -25bp`, `Oil +10%`).

## Outputs
- **Pairwise Correlation Matrix** (qualitative bins): Strong+/Weak+/Neutral/Weak-/Strong-
- **Granger-like Directionality** (heuristic): A→B, B→A, Bi-dir, None
- **Regime Flags**: `correlation_breakdown`, `flight_to_quality`, `risk_on`, `risk_off`
- **Shock Propagation Map**: primary/secondary effects with attenuation
- **Confidence**: 0–1 with rationales

## Heuristic Scoring
- Base correlation is inferred from recent macro narrative + asset co-movements.
- Directionality inferred from typical lead/lag relations (rates→FX→equities, energy→inflation→rates, etc.).
- Regime-shift if ≥30% of pairs switch sign vs previous window or dispersion ↑.

### Binning
- **Strong+** (ρ≈0.6–1.0), **Weak+** (ρ≈0.2–0.6), **Neutral** (−0.2–0.2), **Weak-** (−0.6–−0.2), **Strong-** (−1.0–−0.6)

## Prompt
/var "Assets: SPX, NDX, BTC, GOLD, WTI, US10Y, DXY; Window: 12m; Shock: Fed -25bp" --regime-check --propagate


## Output Template (Markdown + JSON)
- Use **unified-output-template.md** for the full schema; quick view below.

```json
{
  "module": "var-model",
  "assets": ["SPX","NDX","BTC","GOLD","WTI","US10Y","DXY"],
  "window": "12m",
  "correlation_bins": [["SPX","NDX","Strong+"], ["SPX","US10Y","Weak-"]],
  "directionality": [["US10Y","DXY","A->B"], ["WTI","CPI_proxy","A->B"]],
  "regime_flags": ["correlation_breakdown"],
  "shock": "Fed -25bp",
  "propagation": [
    {
      "source":"US10Y",
      "targets":[
        {"asset":"DXY","effect":"Weak-","lag":"short"},
        {"asset":"SPX","effect":"Weak+","lag":"short"}
      ]
    }
  ],
  "confidence": 0.76,
  "rationales": ["Rates cut supports risk assets; dollar mixed; gold neutral"]
}
## Interpretation Notes

Use bins to communicate uncertainty robustly; avoid false precision.

When correlation_breakdown is on, prefer regime narrative over historical averages.

Combine with risk-parity.md for weight sanity checks.
