# NHTSA Defect Signal Agent — Evaluation Protocol v1

**Status:** Pre-registered before experimental results.

## 1. Scope

- Manufacturers: Toyota and Honda.
- Target: 10–15 recall campaigns with recall report dates from 2020–2024.
- Select campaigns chronologically using documented eligibility rules, not detection outcomes.
- Exclude campaigns with insufficient identifying information; record all exclusions.

## 2. Temporal Rules

- Evaluate detection decisions at monthly intervals.
- Only use complaints available by each simulated decision date, conservatively considering both NHTSA receipt and database addition dates.
- No complaint received after a recall's report date can contribute to detecting that recall.
- Recall narratives and later outcomes are unavailable to the detection agent.
- Use pre-2020 data for development. Treat 2020–2024 recalls as held-out evaluation cases.
- Do not change thresholds or matching rules after inspecting held-out results.

## 3. Alert Thresholds

Group complaints by manufacturer, model, model year, and normalized component.

Generate an alert when both conditions hold:

1. At least **5 complaints** occur within the previous 90 days.
2. The recent complaint count is at least **2 times** the historical rate normalized from the preceding 365 days, using a pseudocount of 0.5 in numerator and denominator.

These thresholds remain fixed for the final evaluation.

## 4. Custom LLM Judges

**Judge 1 — Extraction Fidelity**

Pass = 1 when extracted component, failure mode, severity, crash, and injury information is supported by the original complaint, including appropriate unknown values.

Fail = 0 if any extracted field contradicts or invents material information.

**Judge 2 — Evidence Grounding**

Pass = 1 when the proposed defect explanation is supported by its cited complaint narratives and does not add unsupported causal claims.

Fail = 0 when supporting evidence is absent, contradictory, or materially overstated.

Complaint identity and date validity will also be checked deterministically.

Judge criteria are fixed before evaluation.

## 5. Historical Backtest

- Replay monthly detection decisions using only eligible historical information.
- A recall is detected when a matching alert occurs before its report date, within an 18-month lookback.
- Measure detection rate across all eligible recalls.
- Measure median lead time across successfully detected recalls.
- Measure observed false-positive rate over eligible component-months without a matching recall in the following 12 months.
- Check negative labels against all available relevant recalls, not only the selected evaluation campaigns.

## 6. Reporting Rules

- Report all eligible recall outcomes, including misses.
- Report false alerts and unmatched signals.
- Do not optimize on the held-out period.
- Document data limitations, including changes to historical NHTSA records and the absence of vehicle exposure denominators.
- Report weak results without modifying the evaluation protocol.
