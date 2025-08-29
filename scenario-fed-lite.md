# scenario-fed-lite.md

## 📘 Sample Scenario Input
Use this scenario to test GPT's macroeconomic reasoning modules.

---

### 🔻 Scenario Prompt:
FRB announces a 25bp rate cut.
USD Index surges +3%.
Global equities diverge.

yaml
コードをコピーする

---

### 🔄 Recommended Evaluation Modules:
- IS-LM: liquidity effects, investment response  
- Mundell-Fleming: currency policy divergence  
- VAR (if available): asset class correlation shift

---

### 🧪 Expected GPT Output (Sample):

📊 IS-LM Interpretation:

Expansionary policy via rate cut

Potential liquidity trap (LM flattening)

Investment reaction ambiguous due to rate expectations

🌍 Mundell-Fleming:

Unexpected dollar surge → suggests capital inflow

Possible sterilized FX intervention by non-US CBs

📉 VAR View:

Equities decouple from macro (VAR signal loss)

Reversion risk in risk parity portfolios

🧠 Summary:
The rate cut triggered mixed global reactions.
FX behavior suggests distortion, not classical capital flow logic.

yaml
コードをコピーする

---

## 🧠 Notes:
- Designed to be paired with `is-lm-summary.md`  
- Scenario phrasing should remain economic-journalistic  
- Works well when processed with GPT-4 or Claude 3 under "expert macroeconomist" persona
