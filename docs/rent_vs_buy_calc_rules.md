Rent vs Buy Simulation Tool: Requirements and Constraints

## 1. Purpose
Build a single-page HTML application that lets a user adjust financial inputs and compare two scenarios over time:
1. Rent and invest.
2. Buy a home and invest remaining cash flow.

The output program filename must be rent_vs_buy_simulation.html.

The tool must estimate which path produces higher net worth over the selected simulation horizon.

## 2. Required Inputs
All inputs are required unless explicitly marked optional.

1. Home value growth rate (year-over-year, percent). Step: 0.1%. Default: 5%.
2. Rent growth rate (year-over-year, percent). Step: 0.1%. Default: 5%.
3. Stock market growth rate (year-over-year, percent). Step: 0.1%. Default: 10%.
4. Current monthly rent (currency). Step: $100. Default: $3,000.
5. Home purchase price (currency). Step: $50,000. Default: $1,000,000.
6. Available down payment cash (currency). Step: $50,000. Default: $200,000.
7. Initial renovation cost (currency, one-time). Step: $50,000. Default: $50,000.
8. One-time purchase closing cost (currency). Step: $500. Default: $20,000.
9. Property tax rate (annual percent of taxable assessed value). Step: 0.1%. Default: 1.2%.
10. Monthly HOA cost (currency). Step: $100. Default: $0.
11. Annual maintenance cost (currency). Step: $100. Default: $10,000.
12. Maximum annual assessed value increase for tax purposes (percent). Step: 0.1%. Default: 1%.
13. Mortgage interest rate (annual percent). Step: 0.1%. Default: 6%.
14. Monthly allocation for housing (currency). Step: $1,000. Default: $10,000.
15. Annual home insurance cost (currency). Step: $100. Default: $2,000.
16. Tax bracket (annual percent). Step: 0.1%. Default: 30%.
17. Simulation length in years (integer). Step: 1 year. Default: 30 years.

Currency inputs must display thousands separators. Example: $1,000,000.


## 3. Input Validation Rules
1. Simulation runs automatically whenever any input value changes, but only when all required inputs are present and valid.
2. Percent fields must be numeric and interpreted as percentages (for example, 5 means 5%).
3. Tax bracket must be between 0% and 100% inclusive.
4. Currency and income fields must be numeric and non-negative.
5. Simulation length must be a positive integer.
6. Down payment must be less than or equal to purchase price.
7. Monthly allocation for housing must be greater than or equal to the higher of:
   1. Current monthly rent, or
   2. Monthly home ownership cost at simulation start, defined as monthly mortgage payment (principal + interest) + monthly property tax + monthly insurance + monthly HOA + monthly maintenance.
8. If computed loan amount is zero, mortgage payment is zero.
9. If simulation length exceeds 30 years, mortgage-related values continue after payoff with zero mortgage payment and zero remaining balance.

## 4. Shared Modeling Assumptions
1. Time step is monthly for cash flow and investment contributions.
2. Year-over-year rates are converted to equivalent monthly compounding rates.
3. Starting one-time cash in both scenarios is the same and equals down payment + renovation cost + one-time purchase closing cost.
4. Annual home insurance, monthly HOA cost, and annual maintenance cost each increase by 2% once per year to reflect inflation.
5. Negative monthly investable cash flow must be applied as a negative contribution (investment drawdown), not clamped to zero.
6. Pitfall to avoid: clamping negative contributions to zero while still crediting tax refunds can falsely make higher property-tax growth appear beneficial for buying.
7. Values are tracked through the end of each simulation year for charting.
8. **Timing of annual growth factors**: All annual adjustments (home value growth, rent growth, property tax assessed value growth, home insurance inflation, HOA inflation, maintenance inflation) are applied at the end of each year (after completing months 1–12, 13–24, etc.) and **before** recording the yearly snapshot. This ensures Year 1 data reflects one year of growth compared to Year 0, Year 2 reflects two years of growth, and so on.

## 5. Rent Scenario Rules
1. Initial investment principal is down payment + renovation cost + one-time purchase closing cost (since those amounts are not spent in the rent path).
2. Monthly rent starts at the user-provided current rent.
3. Rent increases once per year using the rent growth rate.
4. Monthly investable cash flow = monthly allocation for housing - monthly rent.
5. Monthly investable cash flow is contributed to stock investments each month and may be negative when expenses exceed income.
6. Investment balance grows monthly using the stock market growth assumption.
7. Net worth (rent scenario) = stock investment balance.

## 6. Buy Scenario Rules
1. Home purchase occurs at month 0.
2. Down payment is subtracted from available starting cash.
3. Initial renovation cost and one-time purchase closing cost are paid at month 0.
4. Loan principal = purchase price - down payment.
5. Mortgage type is fixed-rate, 30-year fully amortizing loan.
6. Monthly mortgage payment (principal + interest) is computed using standard amortization formula.
7. Home value increases annually using home value growth rate.
8. Property tax base (assessed value) increases annually but cannot exceed the maximum annual assessed value increase input.
9. Annual property tax = property tax rate x assessed value; monthly property tax = annual property tax / 12.
10. Monthly insurance starts as annual home insurance / 12 and increases by 2% once per year.
11. Monthly maintenance starts as annual maintenance cost / 12 and increases by 2% once per year.
12. Monthly HOA starts as the user-entered monthly HOA cost and increases by 2% once per year.
13. Monthly investable cash flow = monthly allocation for housing - mortgage payment - monthly property tax - monthly insurance - monthly HOA - monthly maintenance.
14. Monthly investable cash flow is contributed to stock investments each month and may be negative when ownership expenses exceed income.
15. Stock investments grow monthly using stock market growth assumption.
16. Home equity each month = current home market value - remaining loan balance.
17. When the home is sold, selling closing cost is fixed at 5% of the home market value.
18. Annual mortgage interest and property taxes paid are federally deductible. At the end of each year, the tax refund = (annual mortgage interest paid + annual property taxes paid) × tax bracket %. Example: at 30%, every $1 of deductible expense returns $0.30. This refund is added to the buy-path stock investment balance at year-end.
19. Net sale proceeds = (home market value x 0.95) - remaining loan balance.
20. Net worth (buy scenario) = net sale proceeds + stock investment balance - initial renovation cost - one-time purchase closing cost.

## 7. Output Requirements
1. Show a line chart comparing yearly net worth for both scenarios.
2. Provide one data point per year for each scenario, including year 0.
3. User must be able to hover or click each point to see exact year and net worth value.
4. Display final year totals and the difference between scenarios.
5. Clearly indicate which option has the higher final net worth.
6. Show a second line chart for yearly net worth difference, defined as: buy net worth - rent net worth.
7. The difference chart must include one data point per year, including year 0.
8. User must be able to hover or click each point in the difference chart to see exact year and difference value.
9. In the difference chart, values above zero indicate buy is ahead; values below zero indicate rent is ahead.
10. Show a third selectable line chart for buy-side homeowner metrics and path comparisons over time.
11. The selectable chart must provide one data point per year, including year 0.
12. User must be able to switch the chart between these plot options:
    1. Home market value over time.
    2. Annual maintenance cost over time.
    3. Annual property tax over time, based on assessed value.
    4. Remaining mortgage balance over time.
    5. Annual home insurance cost over time.
    6. Cash flow comparison: both rent and buy paths' cumulative net expenditures over time, displayed as two series on one chart.
    7. Net gains and contributions comparison: both rent and buy paths' net stock investment balances over time (including contributions and market returns), displayed as two series on one chart.
13. User must be able to hover or click each point in the chart to see the exact year and plotted value.
14. For the cash flow comparison plot (option 6), net expenditure is defined as:
    1. Rent path: cumulative rent paid (monthly rent × months elapsed).
    2. Buy path: cumulative (monthly mortgage payment + monthly property tax + monthly home insurance + monthly HOA + monthly maintenance).
15. For the net gains and contributions comparison plot (option 7), net stock balance represents the current investment account balance for each path, including all monthly contributions and market returns (rentStock for rent scenario, buyStock for buy scenario).

## 8. UX and Interaction Requirements
1. Application must be delivered as an HTML-based tool (client-side web app).
2. Inputs must be editable and rerun the simulation immediately after each parameter update, without page reload.
3. Every required input must provide a visible, clickable up control and down control in the UI so the user can increment or decrement the value without typing.
4. The up/down controls for each input must use that input's step size exactly as defined in the Required Inputs section.
5. If validation fails, show clear field-level error messages and block simulation run.
6. Provide consistent currency and percentage formatting in UI outputs.
7. Each input label must include a clickable "?" help icon next to the label.
8. Clicking the "?" icon must display a short explanation of:
   1. What the input means.
   2. How increasing or decreasing it can affect the simulation outcome.
9. Help content must be available for all required inputs and must update correctly after repeated runs.
10. After a user enters a dollar amount and exits the field, the UI must display the value with comma separators (for example, 1000000 is displayed as 1,000,000).
11. A "Save Inputs" button must export all current input values to a downloadable JSON file named `rent_vs_buy_inputs.json`.
12. A "Load Inputs" button must open a file picker restricted to `.json` files. When a valid file is selected, all field values are populated from the file. Currency fields must display with comma formatting after loading. If the file is invalid or cannot be parsed, an error message is shown in the status area and no fields are changed.
13. The selectable homeowner-metric chart must provide an obvious control for switching plot type without reloading the page.
14. When the simulation reruns after an input change, after loading saved inputs, or after the user changes the homeowner-metric plot selection, charts must refresh in place without animated transitions, pans, zooms, or other motion effects.
15. The layout must consist of two independently scrollable panels side-by-side on desktop: input parameters on the left and results/plots on the right.
16. Both panels must be independently scrollable, with each maintaining its own scroll position to allow the user to view any content in either panel without affecting the other.

## 9. Explicit Exclusions (Out of Scope for v1)
Unless added later, do not include these factors in calculations:
1. Realtor fees and transaction taxes beyond the fixed 5% selling closing cost.
2. Utilities and moving costs.
3. Mortgage insurance (PMI), refinancing, or adjustable-rate loans.
4. Inflation-adjusted (real) dollars.

## 10. Consistency Requirements
1. All formulas and assumptions must be applied consistently across all years.
2. Charted values must exactly match values used in summary outputs.
3. Rounding should only affect displayed values, not internal calculations.
