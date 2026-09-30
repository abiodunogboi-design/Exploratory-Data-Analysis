# 🛒 Retail Store Sales Analysis — Python Data Cleaning & Exploratory Data Analysis

> **End-to-end data analytics project using Python to clean, validate, transform, and analyze retail sales data to uncover revenue, product, seasonal, and customer-level insights.**

---

**📌 Project Overview**

This project analyzes a retail sales dataset covering 2022–2025. The objective was to transform a raw, imperfect dataset into an analysis-ready dataset and use exploratory data analysis (EDA) to identify patterns in:

- Revenue performance
- Order volume
- Product and category performance
- Seasonal sales trends
- Customer purchasing behavior
- Revenue growth
- Quantity sold versus revenue
- Potential areas requiring further business investigation

Rather than treating the dataset as immediately analysis-ready, the project began with a dedicated data-cleaning and validation pipeline before moving into exploratory analysis.

The analysis ultimately focuses on 2022–2024, because the available 2025 records contain only one month of sales data and therefore would distort year-over-year comparisons.

---

**🎯 Business Questions**

The analysis was designed to answer questions such as:

1. How has revenue changed from year to year?
2. How have order volume and average order value changed?
3. Are there identifiable seasonal sales patterns?
4. Which product categories generate the most revenue?
5. Which individual products perform best by revenue and quantity sold?
6. Why can a product with high sales volume generate less revenue than another product?
7. Are revenue patterns consistent across customers?
8. Which products or categories require further investigation?
9. Are there indications of customer concentration or retention patterns?
10. What additional data would be required to explain unusual product or customer behavior?

---

**🛠️ Tools & Technologies**

Python| Data cleaning, transformation and analysis

Pandas| Data manipulation, aggregation and statistical analysis

NumPy| Numerical operations and data transformation

Matplotlib| Data visualization

Seaborn| Statistical visualization

Jupyter Notebook| Interactive analysis environment

GitHub| Project documentation and version control


**🔄 Project Workflow**

---
        Raw Retail Dataset
                ↓
        Data Inspection
                ↓
      Data Quality Assessment
                ↓
     Missing-Value Investigation
                ↓
    Data Cleaning & Transformation
                ↓
            Validation
                ↓
     Analysis-Ready Dataset
                ↓
    Exploratory Data Analysis
                ↓
        Revenue Analysis
                ↓
        Seasonality Analysis
                ↓
    Product & Category Analysis
                ↓
        Customer Analysis
                ↓
        Business Insights
                ↓
    Recommendations & Further Investigation

---

🧹 1. Data Cleaning & Preparation

The raw dataset contained missing values and structural irregularities that required investigation before analysis.

Initial data validation

The cleaning process included:

- Inspecting dataset structure with "df.info()"
- Converting "Transaction Date" to a proper datetime format
- Checking transaction ID uniqueness
- Inspecting unique product categories
- Checking missing values across important variables
- Investigating relationships between "Quantity", "Price Per Unit", and "Total Spent"
- Checking for duplicate records
- Validating product-price relationships
- Cleaning accidental array/list-like values in "Customer ID" and "Item"

Missing-value treatment

Rather than filling every missing value indiscriminately, missing values were investigated based on relationships between existing columns.

For example:

df['Price Per Unit'] = df['Price Per Unit'].fillna(
    df['Total Spent'] / df['Quantity']
)

Missing quantities were also reconstructed where the required information was available:

df['Quantity'] = df['Quantity'].fillna(
    df['Total Spent'] / df['Price Per Unit']
)

Similarly, missing "Total Spent" values were reconstructed where possible:

df['Total Spent'] = df['Total Spent'].fillna(
    df['Quantity'] * df['Price Per Unit']
)

For remaining missing values, customer-and-item-level averages were used where sufficient historical information existed:

df['Quantity'] = (
    df['Quantity']
    .fillna(
        df.groupby(['Customer ID', 'Item'])['Quantity']
        .transform('mean')
    )
    .round(1)
)

The same approach was applied to "Total Spent".

Importantly, values were not blindly imputed when a reliable basis was unavailable, because unnecessary imputation could introduce artificial patterns into the analysis.

---

🔎 2. Data Quality Validation

Several validation checks were performed after cleaning.

Transaction uniqueness

Transaction IDs were investigated for uniqueness to understand whether each transaction represented a distinct sales record.

Product-price validation

The relationship between "Category", "Price Per Unit", and "Item" was investigated to ensure that products were not ambiguously associated with identical prices within the same category.

Duplicate detection

Duplicate records were explicitly checked:

duplicates = df[df.duplicated(keep=False)]

The analysis found no duplicate entries in the cleaned dataset.

Structural irregularities

Some "Customer ID" and "Item" values contained array/list-like structures rather than simple scalar values. These were flattened into usable scalar values before further analysis.

---

📊 3. Dataset Scope

After cleaning and validation, the exploratory analysis focused on 2022, 2023 and 2024.

The available 2025 data contained only one month of observations, so it was excluded from the three-year comparison.

Annual performance

Year| Revenue| Orders| Quantity Sold| Customers| Average Order Value
2022| $535,569.29| 4,134| 22,950.1| 25| $129.55
2023| $513,065.05| 3,987| 21,996.1| 25| $128.68
2024| $548,830.67| 4,241| 23,251.6| 25| $129.41

Revenue growth

- 2023: Revenue decreased by 4.20%
- 2024: Revenue increased by 6.97%

The 2024 revenue therefore recovered above the 2022 level after the decline recorded in 2023.

---

📈 4. Revenue & Sales Trend Analysis

Revenue was analyzed across yearly and monthly periods to identify changes in business performance.

Key observation

Revenue remained within a relatively similar range across the three complete years:

- 2022: $535.6K
- 2023: $513.1K
- 2024: $548.8K

This indicates that the business experienced a decline in 2023 followed by recovery in 2024, rather than sustained rapid revenue growth.

The number of customers also remained at 25 across all three years, making customer acquisition an important area for further investigation.

---

📅 5. Seasonality Analysis

Monthly revenue and order volumes were compared across 2022, 2023 and 2024.

A recurring pattern was observed around the beginning and end of each year.

January

January recorded the highest monthly revenue in each of the three years:

Year| January Revenue
2022| $55,748.76
2023| $49,100.34
2024| $50,588.03

January also recorded the highest monthly order count in each year.

December → January pattern

Order volume increased toward the end of the year and peaked in January before dropping substantially in February.

This recurring pattern suggests a seasonal sales cycle that deserves further investigation.

Potential explanations could include:

- Holiday purchasing
- Year-end promotions
- New-year purchasing behavior
- Seasonal product demand
- Customer purchasing cycles

However, the available dataset does not contain enough contextual information to establish the underlying cause.

---

🏷️ 6. Category Performance Analysis

Eight product categories were analyzed.

Category| Revenue| Share of Revenue| Orders
Butchers| $212,255.26| 13.29%| 1,543
Electric household essentials| $210,848.37| 13.20%| 1,566
Beverages| $201,647.36| 12.62%| 1,541
Furniture| $200,561.18| 12.55%| 1,567
Food| $200,044.78| 12.52%| 1,551
Computers & electric accessories| $195,709.38| 12.25%| 1,526
Patisserie| $190,167.62| 11.90%| 1,507
Milk Products| $186,231.06| 11.66%| 1,561

Category-level observations

Butchers generated the highest three-year revenue at approximately $212.3K, representing 13.29% of total revenue.

Food recorded the highest total quantity sold at approximately 8,668.4 units.

Furniture recorded the highest number of orders with 1,567 transactions.

An important finding is that the category generating the most revenue is not necessarily the category generating the most units or orders.

This demonstrates why revenue, quantity and order volume should be analyzed together rather than relying on a single KPI.

---

🏆 7. Product Performance Analysis

Individual products were analyzed using:

- Revenue
- Quantity sold
- Number of orders
- Price per unit

Highest revenue-generating products

The highest-revenue products included:

Product| Category| Revenue| Quantity Sold| Unit Price
Item_25_FUR| Furniture| $26,561.17| 647.8| $41.00
Item_25_EHE| Electric household essentials| $26,400.57| 643.9| $41.00
Item_25_BUT| Butchers| $23,265.16| 567.5| $41.00
Item_24_FUR| Furniture| $23,071.96| 584.0| $39.50
Item_25_FOOD| Food| $22,276.67| 543.3| $41.00

Highest quantity-selling products

The highest quantities included:

Product| Category| Quantity Sold| Revenue| Unit Price
Item_2_BEV| Beverages| 706.8| $4,594.58| $6.50
Item_16_MILK| Milk Products| 689.0| $18,947.50| $27.50
Item_19_CEA| Computers & electric accessories| 650.8| $20,824.00| $32.00
Item_19_MILK| Milk Products| 650.6| $20,819.91| $32.00
Item_25_FUR| Furniture| 647.8| $26,561.17| $41.00

Key analytical insight

High quantity does not automatically mean high revenue.

For example:

> `Item_2_BEV` sold approximately **706.8 units** but generated only **$4,594.58**, while `Item_25_FUR` sold approximately **647.8 units** and generated **$26,561.17**.

The difference is largely explained by unit price:

- Item_2_BEV → $6.50
- Item_25_FUR → $41.00

This demonstrates the importance of analyzing volume and price together when evaluating product performance.

---

📊 8. Year-over-Year Category Analysis

Revenue by category was compared across the three years.

Some notable movements included:

Beverages

- 2022: $66,529.88
- 2023: $57,370.69
- 2024: $77,746.79

This category experienced a substantial recovery in 2024.

Furniture

- 2022: $64,407.79
- 2023: $65,270.68
- 2024: $70,882.71

Furniture showed relatively consistent growth across the three-year period.

Butchers

- 2022: $81,623.95
- 2023: $62,342.27
- 2024: $68,289.04

The category experienced a significant decline in 2023 followed by partial recovery in 2024.

These variations demonstrate why three-year trend analysis provides more context than looking at a single year's performance.

---

👥 9. Customer Analysis

Customer-level analysis examined:

- Total revenue
- Total quantity purchased
- Number of orders
- Average order value
- Yearly purchasing patterns
- First purchase dates

There were 25 customers represented in the analysis period.

Customer revenue

The highest total customer revenue recorded was:

CUST_24 — $70,791.30

Other high-value customers included:

- CUST_08 — $69,760.65
- CUST_05 — $69,486.36
- CUST_13 — $66,957.29
- CUST_16 — $66,674.94

Customer order frequency

The highest order counts included:

Customer| Orders
CUST_05| 540
CUST_24| 534
CUST_08| 526
CUST_13| 524
CUST_15| 510

The relatively narrow range of customer revenue and order frequency suggests that the dataset does not show extreme customer concentration.

However, customer acquisition cannot be evaluated properly without historical sales data beyond the available period.

---

📐 10. Customer Revenue Distribution

The analysis also examined the statistical distribution of transaction revenue.

Calculated metrics included:

- Skewness: 0.8276
- Kurtosis: -0.1004

These statistics were used as supporting evidence when examining the shape of transaction-level spending rather than treating averages alone as representative of the entire dataset.

---

🔍 11. Interesting Data Finding: Customer Start Dates

The first recorded purchase date for each customer was analyzed.

Most customers have first recorded transactions within the first few days of January 2022.

This raises an important data-quality/business-context question:

> Are these genuinely new customers, or were they existing customers whose historical transactions occurred before the dataset began?

The available data cannot answer this definitively.

Additional historical sales data would be required to distinguish between:

- genuinely new customers,
- existing customers whose previous transactions are unavailable, and
- customers who were already active before the dataset's starting date.

This is an example of an important analytical principle:

A pattern in the data does not automatically explain the reason behind the pattern.

---

💡 Key Business Insights

1. Revenue is relatively stable

Revenue remained around the $500K–$550K range across 2022–2024, with a decline in 2023 followed by recovery in 2024.

2. Customer count remained unchanged

The dataset contains 25 customers in each analyzed year. This makes customer acquisition an important area for further investigation if the business aims to grow beyond its current revenue range.

3. January is consistently strong

January recorded the highest monthly revenue and order count in each analyzed year.

4. February shows a recurring decline

Order volume drops considerably in February following January's peak.

5. Revenue and quantity tell different stories

The highest-volume products are not necessarily the highest-revenue products because unit price has a substantial effect on revenue.

6. Category performance is relatively distributed

No single category dominates the entire revenue mix. The eight categories each contribute roughly 11.7%–13.3% of total revenue.

7. Product-level investigation is more informative than category-level analysis alone

Category-level results can hide major differences between individual products.

8. Some anomalies require additional data

For example, "Item_3_EHE" generated zero revenue in 2022 but generated revenue in later years.

The current dataset cannot determine whether this resulted from:

- inventory availability,
- product launch timing,
- low demand,
- marketing activity,
- or another operational factor.

Inventory and marketing data would be required to investigate further.

---

📌 Recommendations for Further Analysis

Based strictly on the patterns identified in this dataset, the next analytical steps would include:

Customer acquisition analysis

Investigate:

- New versus returning customers
- Customer acquisition by month
- Customer retention
- Customer lifetime value
- Customer churn

Product analysis

Investigate:

- Product-level profitability
- Price changes
- Discounting
- Product availability
- Product lifecycle
- Cross-selling relationships

Seasonality analysis

Investigate:

- Holiday effects
- Promotional periods
- Monthly purchasing cycles
- Seasonal product categories
- Year-end campaigns

Operational analysis

Combine sales data with:

- Inventory records
- Marketing campaigns
- Discounts
- Stock-outs
- Supplier data
- Product launch dates

This would allow the analysis to move from identifying what happened toward understanding why it happened.

---

⚠️ Data Limitations

This analysis has several important limitations.

Limited customer population

Only 25 customers are represented in the dataset, so customer-level conclusions should not be generalized beyond this dataset.

Incomplete 2025 data

Only one month of 2025 data was available, so 2025 was excluded from the three-year comparison.

Missing historical customer data

The first recorded transaction dates suggest that many customers may have existed before the dataset's starting point, but this cannot be confirmed.

No inventory data

The analysis cannot establish whether low or zero sales were caused by stock availability.

No marketing data

The analysis cannot determine whether promotional activity influenced product or seasonal performance.

No cost data

Revenue was analyzed, but profitability cannot be calculated because product costs and operating expenses were not provided.

---

📂 Project Structure

Retail-Store-Sales-Analysis/
│
├── EDA data cleaning.ipynb
│   └── Data inspection, cleaning and validation
│
├── Exploratory Data analysis.ipynb
│   └── Revenue, product, category and customer analysis
│
├── python cleaned.csv
│   └── Cleaned analysis-ready dataset
│
└── README.md
    └── Project documentation

---

🧪 Core Python Techniques Demonstrated

Data manipulation

pd.read_csv()
df.info()
df.describe()
df.groupby()
pd.pivot_table()
pd.merge()

Data cleaning

pd.to_datetime()
fillna()
drop()
duplicated()
astype()

Feature engineering

df['Year'] = df['Transaction Date'].dt.year
df['Month'] = df['Transaction Date'].dt.month

Statistical analysis

skew()
kurt()
describe()

Aggregation

sum()
mean()
count()
nunique()

Visualization

plot()
bar()
line()

---

📈 Visualizations Created

The project includes visual analysis of:

- Monthly revenue by year
- Monthly order volume
- Revenue by category
- Year-over-year category revenue
- Revenue per customer
- Orders per customer
- Customer revenue by year

These visualizations were used to support the numerical analysis rather than functioning as standalone charts.

---

🧠 Analytical Approach

A key principle throughout this project was:

> **Investigate the data before interpreting it.**

Instead of immediately producing charts, the workflow first established:

1. What the data contains
2. Which fields contain missing values
3. Whether relationships between fields could reconstruct missing information
4. Whether duplicate records existed
5. Whether the available time period supported the intended comparison
6. Which patterns were supported by the data
7. Which questions required additional datasets

This approach helps reduce the risk of drawing conclusions from incomplete or incorrectly structured data.

---

🚀 Future Improvements

Potential extensions to this project include:

- Build an interactive Tableau dashboard
- Add SQL-based analysis
- Perform RFM customer segmentation
- Calculate customer lifetime value
- Analyze product profitability
- Add cohort retention analysis
- Investigate price elasticity
- Perform correlation analysis
- Build automated data-quality checks
- Add statistical hypothesis testing
- Integrate inventory and marketing datasets
- Automate the cleaning pipeline
- Build a predictive sales model

---

🎓 Skills Demonstrated

This project demonstrates practical experience in:

Data Cleaning

- Missing-value investigation
- Data type conversion
- Structural data cleaning
- Duplicate detection
- Data validation
- Conditional imputation

Exploratory Data Analysis

- Descriptive statistics
- Trend analysis
- Seasonality analysis
- Product analysis
- Customer analysis
- Year-over-year analysis

Business Analysis

- KPI development
- Revenue analysis
- Product performance evaluation
- Customer behavior analysis
- Identifying business questions
- Distinguishing observed patterns from possible explanations

Python

- Pandas
- NumPy
- Matplotlib
- Seaborn
- Jupyter Notebook

---

👨‍💻 About the Analyst

Sunday Abiodun Ogboi

Data Analyst | Python | SQL | Excel | Tableau

I am building practical data analytics projects focused on transforming raw data into structured analysis, meaningful insights, and business-focused recommendations.

My portfolio focuses on demonstrating not only the ability to create visualizations, but also the complete analytical workflow:

Raw Data → Cleaning → Validation → Analysis → Insights → Business Questions

---

⭐ Project Takeaway

This project demonstrates an end-to-end approach to exploratory data analysis—from dealing with imperfect raw data to identifying meaningful revenue, product, seasonal, and customer patterns.

The most important outcome is not simply the charts produced, but the analytical process used to determine what the data
