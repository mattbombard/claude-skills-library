---
name: data-analysis-template
description: Creates Excel or Google Sheets templates for simple data analysis tasks with formulas, formatting, and structure
---

# Data Analysis Template Creator

Generate well-structured spreadsheet templates for common data analysis tasks with proper formatting, formulas, and data validation.

## Purpose

This skill creates ready-to-use spreadsheet templates optimized for data analysis, including:
- Pre-configured column headers and data types
- Built-in formulas for calculations and summaries
- Conditional formatting for insights
- Data validation rules
- Pivot table suggestions
- Chart recommendations

## Instructions

When this skill is invoked:

1. **Understand the Use Case**
   - Ask what type of data will be analyzed (sales, expenses, inventory, time tracking, etc.)
   - Identify key metrics and KPIs needed
   - Determine the time period (daily, weekly, monthly, yearly)
   - Clarify if this is for Excel (.xlsx) or Google Sheets

2. **Design the Template Structure**
   - Create a main data entry sheet with:
     - Clear, descriptive column headers
     - Appropriate data types (text, number, date, currency, percentage)
     - Data validation where applicable (dropdowns, date ranges, number limits)
     - Freeze panes for headers
   - Add a summary/dashboard sheet for key metrics
   - Include a documentation/instructions sheet if complex

3. **Add Formulas and Calculations**
   - Use appropriate functions: SUM, AVERAGE, COUNT, COUNTIF, SUMIF, VLOOKUP, etc.
   - Create calculated columns (totals, percentages, variances, growth rates)
   - Add summary statistics (min, max, average, median)
   - Include period-over-period comparisons when relevant
   - Use named ranges for clarity

4. **Apply Formatting**
   - Use conditional formatting for:
     - Highlighting thresholds (red for negative, green for positive)
     - Data bars or color scales for visual comparison
     - Duplicate detection
   - Format numbers appropriately (currency, percentages, decimals)
   - Use consistent font and color scheme
   - Add borders and shading for readability

5. **Provide Output**
   - If creating a file: Generate the spreadsheet using Python with libraries like:
     - `openpyxl` for Excel files
     - `gspread` for Google Sheets API
     - `pandas` with `to_excel()` for data-heavy templates
   - If providing instructions: Give clear step-by-step guide with:
     - Column structure
     - Exact formulas to use
     - Formatting rules
     - Sample data rows

6. **Include Usage Instructions**
   - Explain how to use the template
   - Describe what each column/sheet is for
   - Provide example data entries
   - Note any important formulas or features

## Examples

**Example 1: Monthly Expense Tracker**
```
User: /data-analysis-template Create a monthly expense tracker
Claude: I'll create a monthly expense tracker template with:

Sheets:
1. "Expenses" - Main data entry
2. "Summary" - Monthly breakdown and categories
3. "Charts" - Spending visualizations

Expenses Sheet Columns:
- Date (Date format, data validation: current month only)
- Category (Dropdown: Food, Transport, Housing, Entertainment, Other)
- Description (Text)
- Amount (Currency)
- Payment Method (Dropdown: Cash, Credit Card, Debit Card)
- Notes (Text, optional)

Formulas:
- Total Spent: =SUM(D:D)
- Category Totals: =SUMIF(B:B,"Food",D:D)
- Daily Average: =AVERAGE(D:D)

[Generates Python code or detailed setup instructions]
```

**Example 2: Sales Performance Dashboard**
```
User: /data-analysis-template I need to track sales team performance
Claude: I'll create a sales performance template with:

Data Sheet Columns:
- Sales Rep (Dropdown: Team member names)
- Date (Date validation)
- Customer Name
- Product (Dropdown: Product list)
- Quantity (Number, min: 1)
- Unit Price (Currency)
- Total (Formula: =Quantity × Unit Price)
- Status (Dropdown: Won, Lost, Pending)

Dashboard Sheet:
- Top Performers (RANK + VLOOKUP)
- Total Revenue by Rep (SUMIF)
- Win Rate % (COUNTIF Won / COUNTIF Total)
- Monthly Trend (Sparklines or mini charts)

[Provides implementation code or setup guide]
```

**Example 3: Project Time Tracker**
```
User: /data-analysis-template time tracking for projects
Claude: Creating a project time tracking template...

Time Log Sheet:
- Date (Date picker)
- Project (Dropdown validation)
- Task (Text)
- Start Time (Time format)
- End Time (Time format)
- Duration (Formula: =End-Start, formatted as hours)
- Billable (Checkbox/Yes-No)
- Rate (Currency)
- Total (Formula: =Duration × Rate)

Summary Sheet:
- Hours by Project (SUMIF)
- Billable vs Non-billable (SUMIFS)
- Weekly/Monthly totals
- Revenue calculations

[Implementation details follow]
```

## Guidelines

### Formula Best Practices
- Use absolute references ($) for fixed ranges
- Name your ranges for readability (e.g., =SUM(MonthlyExpenses))
- Avoid circular references
- Add error handling with IFERROR() or IFNA()
- Document complex formulas with comments

### Data Validation
- Always use dropdowns for categorical data to ensure consistency
- Set appropriate constraints (min/max for numbers, date ranges)
- Add input messages to guide users
- Create error alerts for invalid entries

### Performance Considerations
- Limit volatile functions (NOW, TODAY, RAND) when possible
- Use tables/structured references for dynamic ranges
- Avoid excessive conditional formatting rules
- Consider data size - suggest pivot tables for large datasets

### Compatibility
- Note Excel vs Google Sheets formula differences:
  - ARRAYFORMULA (Sheets only)
  - XLOOKUP vs VLOOKUP/INDEX-MATCH
  - Dynamic arrays (Excel 365)
- Test formulas in target platform
- Provide alternatives when platform-specific

### User Experience
- Always freeze header rows
- Use consistent color coding
- Add clear instructions within the sheet
- Protect formula cells to prevent accidental changes
- Include sample data in first few rows

## Output Formats

Choose the most appropriate format:

1. **Python Script** - For complex templates requiring automation
   - Use `openpyxl` for Excel
   - Use `gspread` for Google Sheets
   - Include all formatting and formulas in code

2. **Step-by-Step Instructions** - For simpler templates
   - Clear column setup guide
   - Exact formulas with cell references
   - Formatting rules explained
   - Screenshots or mockups if helpful

3. **CSV + Formula Guide** - For basic structures
   - CSV with headers and sample data
   - Separate document with formulas to add
   - Formatting instructions

## Notes

- Always ask clarifying questions before creating complex templates
- Suggest appropriate chart types for the data (line, bar, pie, scatter)
- Recommend pivot tables when data has multiple dimensions
- Consider suggesting Google Sheets Apps Script or Excel VBA for automation if needed
- Provide templates that are beginner-friendly but professionally structured
- Include data privacy considerations if template handles sensitive information
