# Quick-Start Guide for Modifying the Simulator

This guide points to the 10 highest-impact edit locations in [rent_vs_buy_simulation.html](rent_vs_buy_simulation.html).

## 1. Global assumptions and constants

- Where: [Constants](rent_vs_buy_simulation.html#L392)
- What to edit:
  - Selling transaction cost (currently 5%).
  - Annual housing inflation assumption (currently 2%).
- Why it matters: These values influence final buy-path outcomes across all years.

## 2. Input schema and defaults

- Where: [fieldConfig](rent_vs_buy_simulation.html#L395)
- What to edit:
  - Add/remove inputs.
  - Change labels, step sizes, defaults, and help text.
- Why it matters: This is the source of truth for rendered inputs, parse keys, save/load fields, and validation targeting.

## 3. Annual maintenance input definition

- Where: [Annual maintenance field](rent_vs_buy_simulation.html#L468)
- What to edit:
  - Default annual maintenance amount.
  - Step size and help wording.
- Why it matters: Maintenance is dollar-based and inflation-adjusted in simulation logic.

## 4. Monthly housing budget definition

- Where: [Monthly allocation field](rent_vs_buy_simulation.html#L489)
- What to edit:
  - Rename/repurpose the housing budget concept.
  - Default and step sizing for planning scenarios.
- Why it matters: This value drives both rent-path and buy-path monthly contributions.

## 5. Dynamic field rendering and input events

- Where: [createFields](rent_vs_buy_simulation.html#L605) and [input event hook](rent_vs_buy_simulation.html#L654)
- What to edit:
  - Input type behavior.
  - Event triggers for live re-simulation.
- Why it matters: Any UI behavior change for inputs usually starts here.

## 6. Validation rules and gating

- Where: [validateInputs](rent_vs_buy_simulation.html#L689)
- What to edit:
  - Allowed value ranges.
  - Business-rule checks (down payment bounds, budget minimum checks, tax bracket limits).
  - Validation messaging.
- Why it matters: Invalid inputs block simulation reruns.

## 7. Core simulation engine

- Where: [runSimulation](rent_vs_buy_simulation.html#L732)
- What to edit:
  - Cash-flow formulas.
  - Growth factors.
  - Mortgage/tax behavior.
  - Yearly inflation application.
- Why it matters: This function controls all scenario math and yearly result arrays.

## 8. Net-worth chart behavior

- Where: [renderChart](rent_vs_buy_simulation.html#L840) and [non-animated update](rent_vs_buy_simulation.html#L870)
- What to edit:
  - Dataset styling (colors, line width, points).
  - Tooltip text and axis formatting.
  - Interactivity behavior.
- Why it matters: This is the primary visual summary users trust.

## 9. Difference chart behavior

- Where: [renderDifferenceChart](rent_vs_buy_simulation.html#L926) and [non-animated update](rent_vs_buy_simulation.html#L956)
- What to edit:
  - Difference calculation presentation.
  - Break-even line style.
  - Hover/click detail behavior.
- Why it matters: This chart communicates who leads and by how much.

## 10. Save/load compatibility and migrations

- Where: [applyLoadedInputs](rent_vs_buy_simulation.html#L1034), [legacy monthly mapping](rent_vs_buy_simulation.html#L1035), [legacy maintenance mapping](rent_vs_buy_simulation.html#L1039), [file load event](rent_vs_buy_simulation.html#L1064)
- What to edit:
  - Backward compatibility for renamed keys.
  - Input-file migration rules.
  - Load-time error handling.
- Why it matters: Prevents older saved JSON files from breaking after schema changes.

## Safe change sequence

1. Update schema in fieldConfig.
2. Update validation in validateInputs.
3. Update formulas in runSimulation.
4. Update save/load migration in applyLoadedInputs.
5. Verify chart labels and status text still describe the new behavior.

## Common pitfalls to avoid

- Changing a field key in fieldConfig but not updating validateInputs or runSimulation.
- Mixing annual and monthly units in formulas.
- Re-introducing positive-only contribution clamping for monthly contributions.
- Removing legacy key mapping and breaking older saved input files.
