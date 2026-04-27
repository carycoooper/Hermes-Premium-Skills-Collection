---
name: finance-tracker
description: Personal and business financial data processing including bank statement categorization, spending pattern analysis, budget tracking, invoice extraction, anomaly detection, and tax preparation assistance. Works with CSV exports, PDF invoices, and financial APIs.
version: 2.0.0
author: Hermes Skills Team
license: MIT
platforms: [macos, linux, windows]
metadata:
  hermes:
    tags: [finance, budgeting, expenses, categorization, invoices, personal-finance]
    category: finance
    related_skills: [email-management, summarize, mission-control]
    config:
      - key: currency
        description: "Default currency for analysis"
        default: "USD"
        prompt: "What currency should be used for financial reports?"
      - key: categories
        description: "Custom expense categories"
        default: "housing,food,transportation,utilities,entertainment,healthcare,shopping,savings,income,other"
        prompt: "What expense categories should be tracked?"
      - key: privacy_mode
        description: "Enhanced privacy (local processing only, no cloud)"
        default: "true"
        prompt: "Enable maximum privacy mode (recommended)?"
required_environment_variables: []
---

# Finance Tracker Skill

Intelligent financial data analysis that transforms raw bank statements and invoices into actionable money intelligence.

## When to Use

Trigger this skill for any financial analysis task:

**Expense Tracking:**
- "Categorize my bank statement"
- "Analyze my spending this month"
- "Compare spending to budget"
- "Where am I spending the most?"

**Income & Budget:**
- "Create a monthly budget"
- "Track income vs expenses"
- "Am I saving enough?"
- "Project savings for end of year"

**Invoice Processing:**
- "Extract data from this invoice PDF"
- "Categorize business expense receipts"
- "Prepare expense report for [trip/project]"

**Financial Health:**
- "Give me a financial health check"
- "Find unusual transactions"
- "Tax deduction opportunities"
- "Net worth snapshot"

**Reporting:**
- "Monthly financial summary"
- "Year-over-year comparison"
- "Spending trends over time"
- "Export report for accountant"

## Quick Reference

| Task | Command | Input Format |
|------|---------|--------------|
| Categorize expenses | `/finance categorize file.csv` | Bank export CSV |
| Monthly summary | `/finance summary month YYYY-MM` | Transaction history |
| Budget analysis | `/finance budget analyze` | Budget + actuals |
| Invoice extract | `/finance extract invoice.pdf` | PDF/image invoice |
| Anomaly detect | `/finance anomalies [period]` | Transaction list |
| Tax prep | `/finance tax deductions year YYYY` | Categorized expenses |

## Procedure

### Phase 1: Data Import & Parsing (1-3 minutes)

1. **Accept Multiple Input Formats**
   
   **Bank Statement CSV:**
   ```csv
   Date,Description,Amount,Category,Balance
   2026-01-15,"AMAZON.COM",-45.99,,15234.56
   2026-01-14,"PAYROLL DEPOSIT",4500.00,,15280.55
   2026-01-13,"WHOLE FOODS #123",-87.32,,10780.55
   ```
   
   Supported variations:
   - Different date formats (MM/DD/YYYY, DD/MM/YYYY, ISO 8601)
   - Amount in one column or separate debit/credit columns
   - Various delimiter types (comma, semicolon, tab)
   - Encoded descriptions (sometimes garbled Unicode)
   
   **PDF Invoices:**
   - Text-based PDFs: Direct text extraction
   - Scanned/image PDFs: OCR processing required
   - Structured extraction: Vendor, date, line items, totals, tax
   
   **Other Formats:**
   - OFX/QFX (Quicken format)
   - JSON (from financial apps/APIs)
   - Excel (.xlsx) spreadsheets
   - Copy-pasted transaction lists

2. **Data Cleaning & Normalization**
   
   Standardize all transactions:
   ```python
   # Pseudo-code for normalization
   
   def normalize_transaction(raw):
       return {
           'date': parse_date(raw['date']),           # Standardize to ISO 8601
           'description': clean_description(raw['desc']), # Remove noise
           'amount': float(raw['amount']),             # Negative for expenses
           'currency': detect_currency(raw) or default_currency,
           'category': None,                            # To be classified
           'subcategory': None,
           'tags': [],
           'is_recurring': False,
           'is_income': raw['amount'] > 0,
           'merchant': extract_merchant(raw['desc']),
           'payment_method': infer_payment_method(raw),
           'notes': ''
       }
   ```

   Cleaning steps:
   - Trim whitespace, normalize case
   - Remove transaction IDs, reference numbers from description
   - Standardize merchant names ("AMZN MKTP" → "Amazon")
   - Handle pending transactions (mark as tentative)
   - Detect and flag duplicates

### Phase 2: Intelligent Categorization (2-5 minutes)

3. **Apply Multi-Layer Classification**
   
   **Layer 1: Rule-Based Matching**
   ```python
   rules = {
       'housing': [
           r'(rent|mortgage|lease)',
           r'(apartment|condo|housing auth)',
           r'(home insurance|property tax)'
       ],
       'food': [
           r'(whole foods|trader joe|grocery|supermarket)',
           r'(restaurant|cafe|coffee|starbucks|dunkin)',
           r'(ubereats|doordash|grubhub|delivery)',
           r'(mcdonald|burger king|chipotle|subway)'
       ],
       'transportation': [
           r'(uber|lyft|taxi|cab)',
           r'(gas station|shell|exxon|chevron)',
           r'(parking|toll|metro|transit)',
           r'(tesla|charging station|ev charging)'
       ],
       # ... more categories
   }
   ```
   
   **Layer 2: Merchant Database Lookup**
   - Known merchants mapped to categories
   - Community-maintained database (like Plaid's category taxonomy)
   - Updates as new merchants encountered
   
   **Layer 3: Contextual AI Classification**
   - When rules don't match, use description understanding
   - Consider amount ranges typical for categories
   - Look at temporal patterns (e.g., monthly subscriptions)
   - Learn from user corrections over time

4. **Sub-Categorization & Tagging**
   
   Beyond main category, add granularity:
   
   ```
   Main Category: Food
   ├── Sub-category: Groceries
   │   Tags: [weekly-shopping, essentials]
   │
   ├── Sub-category: Dining Out
   │   Tags: [lunch, social]
   │
   └── Sub-category: Coffee
       Tags: [habit, daily]
   
   Main Category: Transportation
   ├── Sub-category: Fuel
   ├── Sub-category: Public Transit
   ├── Sub-category: Rideshare
   └── Sub-category: Parking/Tolls
   ```

5. **Detect Special Patterns**
   
   Identify transaction types automatically:
   
   **Recurring Transactions:**
   - Same merchant, similar amount, monthly/weekly cadence
   - Mark as subscription/recurring bill
   - Track for budget forecasting
   
   **Large/Unusual Transactions:**
   - Flag amounts > 3x median for that category
   - Flag any single transaction > $500 (configurable threshold)
   - Require user confirmation for anomaly categorization
   
   **Income Sources:**
   - Payroll deposits (bi-weekly/monthly pattern)
   - Freelance payments (various sources)
   - Investment returns, refunds, transfers
   
   **Transfer/Internal:**
   - Between own accounts (savings ↔ checking)
   - Credit card payments
   - Exclude from spending analysis (double-count prevention)

### Phase 3: Analysis & Insights (3-8 minutes)

6. **Generate Financial Reports**
   
   ### Report A: Monthly Spending Summary
   ```markdown
   # 📊 Monthly Financial Report: [Month Year]
   **Period:** [start_date] – [end_date] | **Currency:** [currency]
   
   ---
   
   ## 💰 Income & Expense Overview
   
   | Metric | This Month | Last Month | Change | YTD Total |
   |--------|-----------|------------|--------|-----------|
   | **Total Income** | $[X,XXX.XX] | $[X,XXX.XX] | [+/-X%] | $[XX,XXX] |
   | **Total Expenses** | $[X,XXX.XX] | $[X,XXX.XX] | [+/-X%] | $[XX,XXX] |
   | **Net Savings** | $[X,XXX.XX] | $[X,XXX.XX] | [+/-X%] | $[XX,XXX] |
   | **Savings Rate** | [X.X]% | [X.X]% | [+/-X pts] | [X.X]% |
   
   ---
   
   ## 📈 Spending Breakdown by Category
   
   | Category | Amount | % of Total | Budget | Variance | Trend |
   |----------|--------|------------|--------|----------|-------|
   | 🏠 Housing | $X,XXX | XX% | $X,XXX | +$XX/-$XX | 📈📉➡️ |
   | 🍕 Food | $XXX | XX% | $XXX | +$XX/-$XX | ... |
   | 🚗 Transportation | $XXX | XX% | $XXX | ... | ... |
   | 💊 Healthcare | $XXX | XX% | $XXX | ... | ... |
   | 🎭 Entertainment | $XXX | XX% | $XXX | ... | ... |
   | 🛒 Shopping | $XXX | XX% | $XXX | ... | ... |
   | 💡 Utilities | $XXX | XX% | $XXX | ... | ... |
   | 📚 Other | $XXX | XX% | $XXX | ... | ... |
   
   **Visual:** [Text-based bar chart or recommendation to generate chart]
   
   **Top 5 Expenses:**
   1. **[Merchant/Description]** - $[amount] ([category]) - [date]
   2. **[Merchant/Description]** - $[amount] ([category]) - [date]
   3. ...
   
   ---
   
   ## 🎯 Budget Performance
   
   | Category | Budgeted | Actual | Status | Insight |
   |----------|----------|--------|--------|---------|
   | Housing | $X,XXX | $X,XXX | ✅ Under / ⚠️ Over | [commentary] |
   | Food | $XXX | $XXX | ✅ / ⚠️ | [commentary] |
   
   **Overall Budget Status:** ✅ On Track / ⚠️ Warning / 🔴 Over Budget
   
   **Biggest Overspends:**
   - [Category]: $[X] over budget ([X]% over)
     *Main contributor: [specific transaction or pattern]*
   
   **Biggest Underspends:**
   - [Category]: $[X] under budget ([X]% under)
     *Opportunity to reallocate or save more*
   
   ---
   
   ## 🔍 Spending Insights
   
   ### Patterns Detected
   
   **📊 Highest Spending Day:** [Day of week] (avg $[X]/transaction)
   **🏪 Most Visited Merchant:** [Merchant] ([N] transactions, $[total])
   **📱 Digital Subscriptions Total:** $[X]/month ([N] services)
   
   ### Changes from Last Month
   
   **Increased Spending:**
   - **[Category]**: +$[X] ([+X]%)
     *Likely cause: [identify reason if possible]*
   
   **Decreased Spending:**
   - **[Category]**: -$[X] ([-X]%)
     *Possible reason: [speculate or note]*
   
   ---
   
   ## ⚠️ Alerts & Anomalies
   
   ### Unusual Transactions This Month
   
   | Date | Amount | Merchant | Reason Flagged |
   |------|--------|----------|----------------|
   | [date] | $[X,XXX] | [merchant] | [X]x avg for category |
   | [date] | $[X,XXX] | [merchant] | New merchant, large amount |
   
   **User Action Required:**
   - [Transaction 1]: Please confirm category is correct
   - [Transaction 2]: Do you recognize this charge?
   
   ---
   
   ## 💡 Recommendations
   
   Based on your spending patterns:
   
   1. **Immediate Opportunity:** Save ~$[X]/month by [specific action]
      - Example: Cancel unused subscription [service] ($[X]/mo)
      - Example: Switch [expense] to cheaper alternative
   
   2. **Budget Adjustment:** Consider reallocating $[X] from [over-budget cat] to [under-budget cat]
   
   3. **Savings Goal Progress:**
      - Current trajectory: $[X]/month → $[XX,XXX]/year
      - To reach goal of $[XX,XXX]: Need +$[X]/month (increase savings rate by X%)
   
   ---
   
   ## 📋 Recurring Expenses (Subscriptions & Bills)
   
   | Service | Amount | Frequency | Next Charge | Since | Annual Cost |
   |---------|--------|-----------|-------------|-------|-------------|
   | [Name] | $[X]/mo | Monthly | [date] | [date] | $[XX]/yr |
   | [Name] | $[X]/yr | Annually | [date] | [date] | $[XX]/yr |
   
   **Total Monthly Recurring:** $[X]/month ($[XX,XXX]/year)
   
   **Subscription Audit Recommendations:**
   - [Service]: Last used [X] days ago → Consider canceling
   - [Service]: Duplicate service with [other service]? → Consolidate?
   
   ---
   
   *Report generated: [timestamp] | Data source: [filename]*
   *Privacy mode: [enabled/disabled] | All processing local*
   ```

   ### Report B: Invoice/Receipt Extraction
   ```markdown
   # 🧾 Invoice Extraction Results
   **File:** [filename.pdf] | **Processed:** [timestamp]
   
   ---
   
   ## Extracted Data
   
   **Vendor Information:**
   - Company Name: [vendor name]
   - Address: [street, city, state zip]
   - Contact: [phone, email]
   - Tax ID / VAT Number: [ID if found]
   
   **Invoice Details:**
   - Invoice Number: [INV-XXXXX]
   - Invoice Date: [date]
   - Due Date: [date]
   - Payment Terms: [Net 30, etc.]
   - PO Number: [if present]
   
   **Line Items:**
   | # | Description | Quantity | Unit Price | Amount | Tax |
   |---|-------------|----------|------------|--------|-----|
   | 1 | [item desc] | [qty] | $[price] | $[amt] | $[tax] |
   | 2 | [item desc] | [qty] | $[price] | $[amt] | $[tax] |
   
   **Financial Summary:**
   - Subtotal: $[X,XXX.XX]
   - Tax: $[XXX.XX] ([X]% rate)
   - Shipping/Handling: $[XX.XX]
   - Discount: -$[XX.XX] ([X]% off)
   - **Total: $[X,XXX.XX]**
   
   **Payment Information:**
   - Payment Method: [card ending, bank account]
   - Last 4 digits: [XXXX]
   
   ---
   
   ## Categorization
   
   **Suggested Category:** [category]
   **Sub-category:** [subcategory]
   **Tags:** [business, client-project-a, deductible]
   
   **Expense Type:** [Capital Expense / Operating Expense / COGS]
   
   **Tax Treatment:**
   - Deductible: Yes/No/Partial
   - If deductible: Category: [IRC section or business expense type]
   - Documentation retained: Yes (this invoice)
   
   ---
   
   **Confidence Score:** [X]% (based on extraction clarity)
   **Manual Review Required:** [Yes/No - if low confidence fields]
   
   **Discrepancies Found:**
   - [Any math errors detected: subtotal + tax ≠ total, etc.]
   ```

### Phase 4: Advanced Analytics (Optional Deep Dive)

7. **Trend Analysis** (when historical data available)
   
   ```markdown
   ## 📈 12-Month Trends
   
   **Spending Trajectory:**
   - 6-month moving average: [increasing/stable/decreasing]
   - Average monthly spend: $[X] (σ = $[Y])
   - Seasonal patterns identified:
     * High spending months: [months] (reason: [holidays/vacation/etc.])
     * Low spending months: [months]
   
   **Category Trends:**
   | Category | 6mo Avg | Direction | MoM Change |
   |----------|---------|-----------|------------|
   | Food | $XXX | 📈↑ | +X% |
   | Transport | $XXX | ➡️→ | 0% |
   | Entertainment | $XXX | 📉↓ | -X% |
   
   **Savings Rate Trend:**
   - Starting point (12 mo ago): [X]%
   - Current: [X]%
   - Trajectory: improving/stable/concerning
   ```

8. **Anomaly Detection**
   
   Statistical outlier detection:
   ```python
   # Z-score or IQR method for anomaly detection
   
   def find_anomalies(transactions, category):
       amounts = [t.amount for t in transactions if t.category == category]
       mean = np.mean(amounts)
       std = np.std(amounts)
       
       anomalies = []
       for t in transactions:
           if t.category == category:
               z_score = abs((t.amount - mean) / std)
               if z_score > 3:  # 3 standard deviations
                   anomalies.append({
                       'transaction': t,
                       'z_score': z_score,
                       'reason': f'{z_score:.1f}σ from mean of ${mean:.2f}'
                   })
       return anomalies
   ```
   
   Present findings with context:
   - Is it truly anomalous or one-time legitimate expense?
   - Does pattern suggest fraud or unauthorized use?
   - Are there seasonal explanations?

## Configuration & Customization

### Budget Setup
```yaml
budget:
  currency: USD
  period: monthly  # or bi-weekly, annual
  
  categories:
    housing:
      budget: 2000
      alert_threshold: 0.9  # Warn at 90% of budget
      
    food:
      budget: 800
      subcategories:
        groceries: 500
        dining_out: 200
        coffee: 100
        
    transportation:
      budget: 400
      # ... etc
      
  savings_goal:
    target_monthly: 1000
    target_percentage: 20  # % of income
    
  rules:
    round_up_savings: true  # Round purchases to next dollar, save difference
    subscription_alert: 50  # Alert if monthly subs exceed $50
    large_transaction_alert: 500  # Flag single txns > $500
```

### Privacy Controls
```yaml
privacy:
  mode: enhanced  # or standard
  
  enhanced_features:
    local_processing_only: true  # No data leaves machine
    anonymize_reports: true      # Remove names, exact amounts in summaries
    auto_delete_raw_data: 7     # Delete imported files after N days
    encrypted_storage: true      # Encrypt parsed data at rest
    
  data_retention:
    keep_summaries: forever      # Keep aggregated insights
    keep_transactions: 365_days  # Keep individual txns for 1 year
    keep_imported_files: 0_days  # Don't keep original files
```

## Pitfalls & Security

### 🛡️ Security Best Practices:

**Financial Data Sensitivity**
- Treat all financial data as highly confidential
- Never store passwords or banking credentials
- Use environment variables or secure vault for any tokens
- Recommend running locally with Ollama for maximum privacy

**Accuracy Considerations**
- Categorization is best-effort; always allow user override
- Flag low-confidence classifications for human review
- Never make financial decisions autonomously (analysis only, not action)
- Clearly distinguish between facts and AI-generated insights

**Common Errors to Avoid:**
- Double-counting credit card payments (expense when charged AND when paid)
- Misclassifying transfers as income/expenses
- Missing recurring transactions that vary slightly in amount
- Currency conversion errors when dealing with multiple currencies
- Not accounting for pending transactions

### ✅ Best Practices:

**Regular Maintenance:**
- Weekly: Review and correct miscategorized transactions
- Monthly: Generate full report, review budget performance
- Quarterly: Analyze trends, adjust budgets if needed
- Annually: Export data for tax preparation, year-end review

**Data Quality:**
- Import regularly (don't wait until tax season)
- Review uncategorized items promptly (better context when fresh)
- Split transactions appropriately (one grocery run may have household + personal items)
- Add memo notes to ambiguous transactions for future reference

**Integration with Other Tools:**
- Export to spreadsheet software for deeper analysis
- Connect to tax software categories for easy filing
- Share summarized reports (not raw data) with accountant/partner
- Sync with calendar to correlate spending with events/travel

## Verification Checklist

**Before Presenting Financial Analysis:**
- [ ] All transactions accounted for (import count matches source)
- [ ] Income and expenses properly signed (credits positive, debits negative)
- [ ] No duplicate transactions in dataset
- [ ] Categories assigned to all transactions (0 uncategorized)
- [ ] Math checks out (sum of categorized = total)
- [ ] Budget comparisons use same date range
- [ ] Percentages add up correctly
- [ ] Anomalies flagged with explanation, not just marked
- [ ] Recommendations are grounded in data shown
- [ ] Privacy disclaimers included if sharing reports

---

*Based on OpenClaw's Finance Tracker skill with enhanced privacy controls, intelligent categorization, and comprehensive reporting for Hermes.*
