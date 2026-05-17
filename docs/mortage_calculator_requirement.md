# Mortgage Calculator Application Requirements

## 1. Purpose
Build a simple HTML application that calculates and displays the estimated monthly and yearly cost of owning a home based on user-provided mortgage and property expenses.

## 2. Goal
The application should help a user understand:
- the monthly mortgage payment
- how much of that payment goes to principal versus interest
- the additional monthly cost of property tax, home insurance, and HOA
- the yearly and lifetime cost breakdown of the mortgage

## 3. Required Inputs
The application must allow the user to enter:
- home price in USD
- down payment in USD
- annual interest rate in percent
- loan term in years, such as 15 or 30
- annual property tax rate in percent
- annual home insurance cost in USD
- monthly HOA cost in USD

## 4. Core Calculations
The application must calculate:
- loan amount = home price - down payment
- monthly mortgage payment for principal and interest
- monthly property tax cost for year 1
- monthly home insurance cost for year 1
- monthly HOA cost for year 1
- total monthly housing cost = mortgage payment + property tax + home insurance + HOA

The mortgage calculation must:
- use a standard fixed-rate amortization formula
- separate each payment into principal and interest
- support a full amortization schedule across the selected loan term

Hardcoded cost-growth assumptions:
- property tax is based on assessed home value
- assessed home value starts from the input home price
- assessed home value increases by 1% per year
- home insurance starts from the input annual insurance value
- home insurance increases by 2% per year
- HOA starts from the input monthly HOA value
- HOA increases by 2% per year
- these growth rates are fixed assumptions and are not user inputs

## 5. Required Outputs
The application must display:
- monthly mortgage payment for principal and interest
- monthly property tax amount for year 1
- monthly home insurance amount for year 1
- monthly HOA amount for year 1
- total monthly housing cost for year 1

The application must also display first-year summary values:
- total paid in year 1
- total principal paid in year 1
- total interest paid in year 1
- total property tax paid in year 1
- total home insurance paid in year 1
- total HOA paid in year 1

## 6. Amortization Table
The application must generate a yearly breakdown table for the full loan term.

Each row should represent one year and include:
- year number
- assessed home value for that year
- total principal paid that year
- total interest paid that year
- total property tax paid that year
- total home insurance paid that year
- total HOA paid that year
- total annual cost
- remaining loan balance at the end of the year

## 7. Charts and Visualizations
The application must include chart-based visualizations showing mortgage cost over time.

Required charts:
- a stacked plot that shows principal and interest over the life of the loan
- the stacked plot must allow the user to switch between:
- yearly amounts
- monthly amounts
- the stacked plot must allow the user to optionally include:
- property tax
- home insurance
- HOA
- the stacked plot must show the selected point or bar value when the user clicks the chart
- a total cost chart that shows the running total paid over time
- the total cost chart must allow the user to choose which cost components are included in the running total
- the total cost chart must show the selected point value when the user clicks the chart
- selectable cost components must include:
- principal
- interest or loan cost
- property tax
- home insurance
- HOA

Optional chart enhancements:
- a chart showing total annual housing cost
- a chart showing remaining loan balance by year

## 8. User Interface Requirements
The application must provide:
- a clearly visible assumptions section near the top of the page
- a simple form for entering all required inputs
- a summary section for monthly cost results
- a summary section for first-year totals
- a table for yearly amortization results
- chart areas for visualizing mortgage costs
- controls for switching the stacked chart between yearly and monthly views
- controls for selecting which cost components are included in the stacked chart
- controls for selecting which cost components are included in the total cost chart
- click interaction on charts so selected plotted values are displayed to the user

The interface should:
- clearly label all fields and results
- format currency values in USD
- show comma-separated formatting for currency input fields such as home price, down payment, annual home insurance, and monthly HOA
- display currency results as whole-dollar amounts without decimals
- format percentage inputs clearly
- handle invalid or missing input with user-friendly validation messages
- use sans-serif fonts throughout the application
- clearly state that property tax and insurance growth rates are hardcoded assumptions

## 9. Validation Rules
The application should enforce:
- home price must be greater than 0
- down payment must be 0 or greater and cannot exceed home price
- annual interest rate must be 0 or greater
- loan term must be greater than 0
- property tax rate must be 0 or greater
- annual home insurance cost must be 0 or greater
- monthly HOA must be 0 or greater

## 10. Technical Scope
- output format: a standalone HTML application file named `mortage_calculator.html`
- calculation type: fixed-rate mortgage only
- no backend is required unless needed for future expansion

## 11. Out of Scope
The first version does not need to support:
- adjustable-rate mortgages
- PMI
- extra monthly payments
- refinancing scenarios
- closing costs

## 12. Acceptance Criteria
The application is complete when:
- a user can enter all required inputs in the UI
- currency input fields display comma-separated values for easier reading
- currency outputs are rounded and displayed to the nearest whole dollar
- the application calculates monthly mortgage, tax, insurance, and total cost correctly
- the application applies a 1% yearly assessed-value increase for property tax calculations
- the application applies a 2% yearly increase for home insurance calculations
- the application applies a 2% yearly increase for HOA calculations
- the application shows first-year totals
- the application shows a yearly amortization table for the full term
- the application renders the required stacked principal-versus-interest chart
- the stacked chart can be switched between yearly and monthly views
- the stacked chart can optionally include property tax, home insurance, and HOA
- the total cost chart updates based on the selected cost components
- clicking a plotted item displays its value on each chart
- invalid inputs are blocked or clearly explained

## 13. Documentation Maintenance
- the requirements document must be updated whenever application behavior, UI, outputs, charts, controls, or technical deliverables change
- code changes and requirement changes should be kept in sync within the same update whenever practical
