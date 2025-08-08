# Trading Entry Checklist

This Pine Script creates a comprehensive checklist table displayed in the bottom-right corner of the chart. It evaluates multiple factors to help assess potential trading entry points.

## Checklist Items and Calculations

### 1. Earnings Period Check
**Shows:** Whether the next earnings announcement is within 2 weeks
**Calculation:** 
- Retrieves future earnings date using `earnings.future_time`
- Calculates days until earnings: `(earningsUTC - todayUTC) / 86400000`
- **Warning (❌):** If earnings are 0-14 days away
- **Display:** Earnings date and time in YYYY-MM-DD HH:MM format

### 2. Weak Month Check
**Shows:** Whether current month has historically weak performance
**Calculation:**
- Checks if current month is May (5), August (8), or September (9)
- **Warning (❌):** During May, August, or September
- **Display:** Month number and abbreviated name

### 3. Greed/Fear Index (VIX)
**Shows:** Market sentiment based on VIX volatility index
**Calculation:**
- Retrieves CBOE VIX daily close price
- **Green (✅):** VIX ≤ 15 (extreme greed, potential buying opportunity)
- **Red (❌):** VIX ≥ 25 (fear/uncertainty, potential risk)
- **Display:** Current VIX value

### 4. Sector Strength
**Shows:** How the stock's sector performs vs. SPY over 20 days
**Calculation:**
- Maps stock sector to corresponding sector ETF (XLK, XLF, XLV, etc.)
- Calculates 20-day performance: `(current_close / close_20_days_ago) - 1`
- Compares sector performance vs. SPY performance
- **Green (✅):** Sector outperforming SPY by >2%
- **Red (❌):** Sector underperforming SPY by >2%
- **Display:** Sector name, ETF symbol, and relative performance percentage

### 5. Industry Strength
**Shows:** How the stock's specific industry performs vs. SPY
**Calculation:**
- Maps to industry-specific ETF (SMH for semiconductors, GOAU for precious metals, etc.)
- Same calculation method as sector strength
- Only displays if industry ETF differs from sector ETF
- **Green (✅):** Industry outperforming SPY by >2%
- **Red (❌):** Industry underperforming SPY by >2%

### 6. RS Rating (Relative Strength)
**Shows:** Stock's performance relative to S&P 500 index
**Calculation:**
- Weighted performance calculation:
  - 40% weight: 63-day performance
  - 20% weight: 126-day performance  
  - 20% weight: 189-day performance
  - 20% weight: 252-day performance
- Formula: `(stock_weighted_perf / spx_weighted_perf) * 100`
- **Rating Scale:**
  - ≥120: 99 rating
  - ≥110: 90 rating
  - ≥100: 80 rating
  - ≥95: 70 rating
  - And so on...
- **Green (✅):** RS Rating ≥ 80 (strong)
- **Red (❌):** RS Rating < 65 (weak)

### 7. Sales & EPS Growth
**Shows:** Year-over-year growth in sales and earnings per share
**Calculation:**
- Retrieves TTM (trailing twelve months) financial data
- Sales Growth: `(current_TTM_sales / TTM_sales_1_year_ago) - 1`
- EPS Growth: `(current_TTM_EPS / TTM_EPS_1_year_ago) - 1`
- **Green (✅):** Both sales AND EPS growing (>0%)
- **Red (❌):** Either sales OR EPS declining
- **Display:** "S:X.X% E:X.X%" format

### 8. Earnings Acceleration
**Shows:** Whether growth is accelerating compared to previous quarter
**Calculation:**
- Compares current quarter's YoY growth vs. previous quarter's YoY growth
- Sales Acceleration: `current_sales_growth - previous_quarter_sales_growth`
- EPS Acceleration: `current_EPS_growth - previous_quarter_EPS_growth`
- **Green (✅):** Both sales AND EPS growth accelerating
- **Red (❌):** Either metric decelerating
- **Display:** Acceleration percentages in "S:X.X% E:X.X%" format

## Visual Indicators
- **❌ Red:** Warning/negative condition
- **✅ Green:** Positive/favorable condition  
- **White:** Neutral condition
- **N/A:** Data not available

This checklist helps quickly assess multiple risk factors and growth metrics before entering a trade position.