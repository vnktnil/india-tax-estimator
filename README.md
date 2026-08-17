# India Tax Estimator — FY 2025-26 (AY 2026-27)

A mobile-friendly income tax calculator for India, covering salary, Section 44ADA presumptive income, rental/property income, capital gains, home loans, HRA, NPS, and advance tax planning.

**Live tool:** https://vnktnil.github.io/india-tax-estimator/

Built for personal tax planning and year-end estimation — not a substitute for professional advice. See the in-app disclaimers and Important Notes for details.

---

## Changelog

All notable changes to this tool are logged here, most recent first. Dates are in dd-mm-yyyy format.

### 17-08-2026
- Clarified rental income guidance for properties let for part of the year and vacant the rest (Section 23(1)(c)) — the rent field and part-year projector now explicitly say to enter only the *actual rent received*, not a full-year estimate, since entering a full-year figure was a realistic way to overstate taxable income.
- Added a new Important Notes entry explaining this "let then vacant" rule, distinct from the "never let at all" case.
- Added guidance for properties that are genuinely vacant all year (never let, not a residence) — clarified these count as Self-Occupied (Nil annual value) within the first 2 such properties, and only become "deemed let-out" (taxed on notional rent) from the 3rd such property onward, per Section 23.

### 16-08-2026
- Initial build and full iterative development of the calculator, including:
  - Core tax engine: New Regime vs Old Regime slab comparison, surcharge with marginal relief, health & education cess.
  - Income sources: salary (with standard deduction), Section 44ADA presumptive consultancy income, rental/property income, other income — each with part-year projectors for mid-year changes.
  - Capital gains by tax-rate bucket: equity/MF STCG and LTCG, slab-rate gains (debt MF, short-term property/gold/unlisted), gold/unlisted LTCG, and property LTCG with automatic indexation comparison (pre/post 23 July 2024 rules) using the CBDT Cost Inflation Index table.
  - Deductions: 80C (with detailed guidance on what counts), 80D (self/family and parents, separate buckets), 80CCD(1B) and 80CCD(2) (NPS, including the employer-contribution benefit that survives under New Regime), 80TTA (savings interest), with live over-limit warnings on every field.
  - Home loan interest (Section 24b): full self-occupied vs let-out logic, support for multiple properties, automatic "deemed let-out" treatment for a 3rd+ self-occupied/vacant property, pre-construction interest spread over 5 years, and an "active loan" toggle to skip the section entirely for paid-off loans.
  - HRA (Section 10(13A)) and Section 80GG for those without HRA, including the correct FY 2025-26 metro city list and the "no property in your own city" condition for 80GG.
  - Multi-source TDS tracking (multiple employers/clients) under Sections 192 and 194J.
  - Advance tax planner with New vs Old Regime toggle, Section 234B/234C interest estimation (including support for late payments after 31 March), and a simple combined balance-due figure.
  - Automated ITR form indicator (ITR-1/2/3/4) based on income types entered.
  - A four-state Compute button (initial / dirty / computed / needs recompute) so it's always clear whether displayed results match current inputs.
  - Structural reorganisation into three groups (Income & Earnings; Deductions, Reliefs & Tax Already Paid; Computation) with consistent section numbering (1.1, 1.2, 2.1 etc.) and plain-language guidance notes throughout, each tagged to the section it explains.
  - Fixed a state-management bug where several Yes/No toggles (property occupancy, active loan, HRA/80GG) defaulted to a pre-selected answer that looked identical to "not yet chosen" — these now require an explicit tap and clearly flag when a property isn't yet counted in the calculation.
  - Extensive automated testing throughout development (calculation accuracy, edge cases, full realistic user sessions) after each change.
  - Hosted on GitHub Pages.

---

## Known limitations

See the in-app "Known Gaps" section and the CA review briefing document for a full list. Notable items not yet covered: HRA for those with multiple rented homes in a year, capital gains exemptions under Sections 54/54B/54EC/54F, foreign income/assets, and virtual digital assets (crypto).

## Disclaimer

This tool is for personal planning and estimation only. It has not been formally reviewed by a qualified Chartered Accountant. Please verify all figures independently before making tax payments or filing decisions.
