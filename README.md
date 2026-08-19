# India Tax Estimator — FY 2025-26 (AY 2026-27)

A mobile-friendly income tax calculator for India, covering salary, Section 44ADA presumptive income, rental/property income, capital gains, home loans, HRA, NPS, and advance tax planning.

**Live tool:** https://vnktnil.github.io/india-tax-estimator/

Built for personal tax planning and year-end estimation — not a substitute for professional advice. See the in-app disclaimers and Important Notes for details.

---

## Changelog

All notable changes to this tool are logged here, most recent first. Dates are in dd-mm-yyyy format.

### 19-08-2026
- **Section 194-IB rental TDS** — when monthly rent on a let-out property exceeds Rs.50,000, the calculator now auto-flags this and estimates the 2% TDS the tenant is legally required to deduct, with an override field for the actual amount. Feeds into the same combined TDS credit pool as salary/consultancy TDS, correctly reducing the advance tax balance shown.
- **Fixed a live-update bug** — the rental TDS note/field originally only appeared after tapping Compute; now updates live as the rent figure is typed, matching every other reactive element in the calculator.
- **Vacant property guidance** — added plain-language guidance distinguishing three cases that were previously conflated: a property genuinely vacant all year (treated as Self-Occupied, Nil annual value, within the first 2 such properties); a 3rd+ such property (automatically "deemed let-out," taxed on notional rent per Sec 23); and a property let for only part of the year (Sec 23(1)(c) — use actual rent received, not a full-year estimate). Applied consistently to both single- and multi-property entry modes.
- **Date field auto-formatting** — typing digits into any date field (property purchase date, late advance-tax payment date) now auto-inserts slashes as you type (`26052026` → `26/05/2026`), since the numeric mobile keypad these fields use has no `/` key, making the literal placeholder format previously untypeable.
- **Fixed a text-rendering bug** — several recently-added lines were showing literal `\u2014`/`\u2019` characters instead of an em-dash/apostrophe, caused by using JavaScript-style escape sequences directly in static HTML (where they have no special meaning). Swept the whole file and fixed every instance.
- **Advance tax due-dates section now shows real rupee amounts**, not just percentages — each of the four due dates (15 Jun/Sep/Dec/Mar) now shows the actual cumulative amount that should have been paid by that date (percentage x total tax, minus TDS), or "Already covered" if TDS alone exceeds it at that point.
- **New: Guided, conversational entry mode** (`guided_flow_WIP.html`, not yet merged into the main `index.html`) — a full step-by-step alternative to the existing full form, covering every section (income, property, capital gains, deductions, HRA, TDS, advance tax) as plain-language questions, one at a time, writing directly into the same underlying fields the full form uses. No separate data model or duplicated calculation logic — this is purely a friendlier input method. Highlights:
  - Landing screen lets the person choose Guided or Full Form, with genuine unselected-by-default buttons and an explicit confirm step before proceeding (no accidental immediate navigation on tap).
  - Every guided question follows one consistent, fixed-layout template (question, plain-language tip, input, confirmation message, Next button) so nothing shifts position depending on what's been answered.
  - Section-aware skip logic throughout (e.g. TDS questions are skipped entirely if no salary/consultancy income was reported; deduction/HRA sub-questions skip cleanly based on earlier Yes/No answers).
  - Advance-tax awareness check — since "how much have you paid" is genuinely ambiguous partway through the year, the guided flow opens by asking whether to log only what's actually been paid so far (only showing due dates that have already passed, computed against today's real date) or skip straight to the full-year estimate.
  - A plain-language completion/summary screen at the end — personalised, states which regime wins and by how much in a sentence, a cash-flow section (TDS coverage, balance still due, explicitly timing-neutral so it reads correctly whether someone is confirming actuals or forecasting ahead), a scannable numbers breakdown with the recommended regime's figures shown by default and an expand/collapse toggle to see the other regime's numbers, a "keep this up to date" reminder, and a clear call-to-action into the full form to review or edit anything.
  - **Two-way navigation between Guided and Full Form** — a "Walk me through this" link on each full-form section (Property, Capital Gains, Deductions, TDS, HRA, Advance Tax) jumps into just that guided section, pre-filled with whatever's already entered, and returns to the full form (not the next guided section) when finished. A distinct "← Back to full form" button (never a plain, ambiguous "← Back") appears throughout this jumped-in mode, including on the very first question. An optional "Prefer this? Continue guided from here" link lets someone convert an exploratory one-section detour into the full ongoing guided flow, restoring every normal guided-flow affordance (top skip link, in-section undo, the eventual real summary screen) automatically. A "← Back to Guided/Full Form choice" link on the full form and a "Start over" link on the summary screen both return to a freshly-reset landing screen without discarding anything already entered.
  - Extensive real-browser (Playwright) testing throughout, not just simulated DOM stubs — this caught several real bugs during development (a crash from calling a function that had been refactored away, missing Back buttons, a stale-wiring bug on a dynamically-created element, tax-due wording that didn't correctly generalise across a person confirming past actuals versus forecasting ahead) before they reached the live version.
  - **Not yet merged into `index.html`** — currently a separate, actively-developed file. The stable `index.html` deployed on GitHub Pages is unaffected by any of the guided-flow work above.
- **Explicit privacy notice added** — a plain-language line near the top of the page stating that no data is saved, collected, or stored anywhere, and that a refresh clears the form rather than preserving it. Verified directly (not just asserted) with a real browser: confirmed empty localStorage/sessionStorage/cookies after entering real figures, confirmed data is genuinely gone after a reload, and logged every network request made during a real session (only the page itself and its Google Fonts stylesheet — nothing else, ever).
- **Guided-flow navigation refinements**, following real usage testing:
  - Returning to the full form from a "Walk me through this" detour now scrolls back to the specific section you launched from, instead of always jumping to the top of the page.
  - Fixed the Back button's meaning depending on how a screen was reached: jumped in via "Walk me through this", it always reads "← Back to full form" and exits immediately, on every question including the first — never a plain, ambiguous "← Back" that could be mistaken for undoing an answer within the section.
  - Added an optional "Prefer this? Continue guided from here" link during a jumped-in detour, letting someone convert a one-section detour into the full ongoing guided flow, with every normal guided-flow affordance (top skip link, in-section undo, the eventual summary screen) restored automatically.
- **Print/Save Computation Summary feature was built, then fully reverted** at the user's request after real-device testing showed it opening and immediately closing the print/share dialog on mobile (traced to an overly aggressive fallback timer racing a still-open native dialog). Removed cleanly — confirmed zero residual references and a full regression pass — rather than leave a partially-working feature in place. May be revisited later with a different approach (e.g. a genuinely separate print-preview page).
- **Fixed a systemic "duplicate Next/Finish button" bug**, found via user report and confirmed with a full automated audit across every guided-flow question. Root cause: the button-suppression logic tried to infer "does this screen already have its own advance button" from current on-screen state, which only worked *after* an answer was given — it had no way to know a screen was Yes/No-only *before* anything was selected, so an extra outer "Next" briefly appeared on several unanswered questions (and, on one screen, duplicated as two stacked "Finish" buttons). Fixed by having each screen type explicitly declare its own navigation behaviour up front, rather than inferring it from current DOM state. Verified with an automated pass over 40+ screens across every section (property, capital gains, deductions, HRA, TDS, advance tax), both before and after answering each one.

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

This tool covers the common cases for a mixed salary/44ADA/rental/capital-gains income profile, but there are real, deliberate gaps. Nothing below is calculated or estimated by the tool today — if any of these apply to you, treat the headline number with extra caution and consult a CA.

**Not modelled at all:**
- Fixed deposit interest and 80TTB (the higher savings/FD interest deduction for senior citizens) — only regular savings account interest (80TTA) is covered
- Cryptocurrency and other virtual digital assets (no Section 115BBH flat 30% treatment)
- Foreign income or foreign assets (no Schedule FA equivalent, no DTAA relief, no foreign tax credit)
- Agricultural income
- Capital gains reinvestment exemptions (Sections 54/54B/54EC/54F)
- Sec 80E (education loan interest) and Sec 80G (donations)
- Directorship income and unlisted share holdings as a distinct asset class

**Partially modelled or simplified:**
- Cost of improvement on property is not included in capital gains — only the original purchase price is used
- Pre-2001 property purchases default to the CII base year (100) rather than offering the fair-market-value-as-on-1-April-2001 alternative
- The Sec 111A/112/112A surcharge cap (15%) is not separately enforced — general surcharge slabs apply uniformly instead
- Sec 234C relief for capital-gains-driven shortfalls late in the year is not modelled — all shortfalls are treated uniformly
- 44ADA eligibility (turnover thresholds, the 95% digital-receipts condition) is explained in a note but not actively checked against entered figures
- Advance tax is shown as a single combined balance rather than split proportionally by income type — a deliberate simplification after an earlier attempt produced confusing results (see the CA briefing document for detail)

**Guided flow specifically** (see the Changelog above): built and extensively tested, but not yet merged into the live `index.html` — currently a separate file (`guided_flow_WIP.html`) under active development.

## Disclaimer

This tool is for personal planning and estimation only. It has not been formally reviewed by a qualified Chartered Accountant. Please verify all figures independently before making tax payments or filing decisions.
