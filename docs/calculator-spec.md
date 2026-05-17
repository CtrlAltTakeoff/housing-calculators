# House Affordability Calculator Specification

## Scope

This specification describes a standalone house affordability calculator that estimates a maximum purchase price from either household income or a fixed monthly housing budget. The design target is a client-side HTML file with embedded JavaScript and CSS.

## Product Summary

The calculator:

- Supports two distinct calculation modes:
  - affordability based on household income and lender DTI rules
  - affordability based on a fixed monthly budget
- Uses common U.S. mortgage affordability conventions.
- Accepts recurring ownership costs such as property tax, HOA/co-op fees, and insurance.
- Exposes preset DTI profiles:
  - conventional 28/36
  - FHA 31/43
  - VA 41 back-end only
  - custom ratios
- Treats down payments below 20% as PMI-eligible with a 0.5% annual PMI assumption.
- Assumes maintenance and repairs cost 0.5% of the house price per year.
- Presents a maximum affordable house price and a payment breakdown.

## Functional Requirements

### 1. Modes

The calculator shall support:

1. Income-based affordability mode
2. Budget-based affordability mode

Only one mode is active at a time.

### 2. Inputs

#### Income-Based Inputs

- `annualIncome`
  - number
  - gross annual household income before tax
- `monthlyDebt`
  - number
  - recurring monthly non-housing debt
- `loanTerm`
  - integer years
- `interestRate`
  - annual nominal mortgage interest rate in percent
- `dtiProfile`
  - enum: `conventional | fha | va | custom`
- `customFrontRatio`
  - percent
  - only visible when `dtiProfile = custom`
- `customBackRatio`
  - percent
  - only visible when `dtiProfile = custom`
- `downPayment`
  - numeric value plus unit
  - units: `percent | amount`
- `propertyTax`
  - numeric value plus unit
  - units: `percent-year | amount-year | amount-month`
- `hoa`
  - numeric value plus unit
  - units: `amount-month | amount-year | percent-year`
- `insurance`
  - numeric value plus unit
  - units: `percent-year | amount-year | amount-month`

#### Budget-Based Inputs

- `monthlyBudget`
  - number
  - monthly affordability ceiling
- `budgetLoanTerm`
  - integer years
- `budgetInterestRate`
  - annual nominal mortgage interest rate in percent
- `budgetDownPayment`
  - numeric value plus unit
  - units: `percent | amount`
- `includeFeesInBudget`
  - boolean-like enum: `yes | no`
- `budgetPropertyTax`
  - numeric value plus unit
  - units: `percent-year | amount-year | amount-month`
- `budgetHoa`
  - numeric value plus unit
  - units: `amount-month | amount-year | percent-year`
- `budgetInsurance`
  - numeric value plus unit
  - units: `percent-year | amount-year | amount-month`
- `maintenance`
  - numeric value plus unit
  - units: `percent-year | amount-year | amount-month`

## Calculation Rules

### 1. Mortgage Payment Formula

Use the standard amortizing loan payment formula:

- monthly rate `r = annualRate / 12`
- number of payments `n = years * 12`
- payment factor `f = r(1+r)^n / ((1+r)^n - 1)`
- monthly principal-and-interest payment `PI = loanAmount * f`

Special case:

- if interest rate is `0`, then `PI = loanAmount / n`

### 2. DTI Presets

Use these affordability constraints in income-based mode:

- Conventional:
  - front-end = `28%`
  - back-end = `36%`
- FHA:
  - front-end = `31%`
  - back-end = `43%`
- VA:
  - front-end = not enforced
  - back-end = `41%`
- Custom:
  - front-end = user supplied
  - back-end = user supplied

### 3. Income-Based Housing Budget

For income mode:

- `grossMonthlyIncome = annualIncome / 12`
- `frontCap = grossMonthlyIncome * frontRatio`
- `backCap = grossMonthlyIncome * backRatio - monthlyDebt`
- if a front-end ratio exists, allowed housing spend is `min(frontCap, backCap)`
- if no front-end ratio exists, allowed housing spend is `backCap`
- negative affordability is clamped to `0`

### 4. Converting Cost Inputs to Monthly

For any cost expressed as:

- `percent-year`
  - `monthlyCost = homePrice * percent / 100 / 12`
- `amount-year`
  - `monthlyCost = amount / 12`
- `amount-month`
  - `monthlyCost = amount`

### 5. PMI Rule

For both modes:

- if down payment is below `20%`, add PMI
- PMI assumption:
  - annual PMI rate = `0.5%`
- monthly PMI:
  - `PMI = loanAmount * 0.005 / 12`
- The down payment ratio is based on the final solved house price, including fixed-dollar down payments.

This keeps the estimate transparent while avoiding loan-program-specific PMI underwriting details.

### 6. Maintenance and Repairs

For both modes:

- default annual maintenance and repair rate = `0.5%`
- monthly maintenance:
  - `maintenance = homePrice * 0.005 / 12`
- in budget mode, maintenance counts against the monthly budget when ownership costs are included

### 7. Solving Maximum House Price

#### Income-Based Mode

Solve for `homePrice` such that:

- `totalHousingMonthly = allowedHousingBudget`

Where:

- `loanAmount = homePrice - downPaymentAmount`
- `downPaymentAmount = homePrice * downPercent` when unit is percent
- `downPaymentAmount = fixedAmount` when unit is amount
- `totalHousingMonthly = PI + tax + hoa + insurance + maintenance + PMI`

Because PMI can depend on the solved purchase price when the down payment is a fixed dollar amount, solve for the maximum `homePrice` with a monotonic search over the monthly total.

Where:

- `fixedMonthlyCosts` are dollar-denominated monthly or annual inputs converted to monthly
- `variableMonthlyRate` is the sum of percentage-based annual ownership costs converted to monthly rates, excluding PMI because PMI is evaluated from the solved down-payment share

#### Budget-Based Mode

Solve for `homePrice` using the same direct method, but replace the income-derived housing budget with:

- `monthlyBudget`

If `includeFeesInBudget = no`:

- property tax, HOA, insurance, and maintenance are excluded from the affordability budget
- PMI still remains part of the loan-related monthly burden

If `includeFeesInBudget = yes`:

- property tax, HOA, insurance, maintenance, and PMI all count against the monthly budget

### 8. Output Metrics

The calculator shall compute:

- `maxHousePrice`
- `loanAmount`
- `downPaymentAmount`
- `principalInterestMonthly`
- `propertyTaxMonthly`
- `hoaMonthly`
- `insuranceMonthly`
- `maintenanceMonthly`
- `pmiMonthly`
- `totalHousingMonthly`
- `grossMonthlyIncome` for income mode
- `frontRatioUsed`
- `backRatioUsed`

## Output Requirements

The UI shall show:

- maximum affordable house price as the primary result
- a monthly payment breakdown
- a financing summary
- a text note describing:
  - which DTI rule or budget rule was applied
  - whether PMI was added
  - the 0.5% annual PMI and 0.5% annual maintenance assumptions
  - whether front-end or back-end DTI was the limiting factor in income mode

## GUI Specification

### Layout

- single responsive page
- two-column desktop layout
  - left: inputs
  - right: results
- single-column mobile layout

### Visual Structure

- top hero/title section
- mode toggle with two tabs:
  - `Income based`
  - `Budget based`
- grouped fieldsets for inputs
- persistent result card stack on the right for desktop

### Form Controls

- numeric text inputs for all numeric values
- select controls for:
  - mode-specific options
  - DTI profile
  - unit selectors
- `Reset defaults` button

### Result Blocks

- hero result card
  - prominent max house price
  - one-sentence summary
- monthly budget allocation card
  - line items for each cost component
- financing snapshot card
  - loan amount
  - down payment
  - income and DTI values where applicable
- rules applied card
  - status text
  - explanatory note

## UX Rules

- calculations should update whenever a field value or unit selector changes
- hidden custom DTI fields should appear only when `Custom` is selected
- invalid or negative effective affordability should render a valid zero-state result, not `NaN`
- currency values should display in U.S. dollars
- percent values should display with one decimal place

## Assumptions

- intended for U.S.-style mortgage affordability estimates
- does not include closing costs, reserves, or underwriting edge cases
- prioritizes transparent formulas and a standalone client-side implementation

## Deliverables

- `index.html`
  - standalone calculator with embedded CSS and JavaScript
- `calculator-spec.md`
  - ruleset and implementation specification
