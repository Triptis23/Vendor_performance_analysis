# 🧾 Vendor Performance Analysis | SQLite + Python + Power BI

## 📘 Project Overview

This project performs Vendor Performance Analysis by integrating multiple data sources (purchases, sales, freight, and prices), cleaning and transforming them into a unified summary table, and analyzing vendor-level performance using Python (Pandas, SQL, Matplotlib, Seaborn).
The final summarized data is used for Power BI dashboarding to visualize business KPIs like sales, profit margin, and vendor contribution.

## 🧩 Project Architecture
database/
│── purchases.csv
│── purchase_prices.csv
│── sales.csv
│── vendor_invoice.csv
│── (other CSVs)

logs/
│── ingestion_db.log
│── get_vendor_summary.log

scripts/
│── ingestion_db.py
│── get_vendor_summary.py
│── performance_analysis.ipynb
│── inventory.db

## ⚙️ Workflow Summary
1. Data Ingestion (ingestion_db.py)

Reads all .csv files from the database folder.

Loads them into an SQLite database (inventory.db) as tables.

Tracks ingestion progress in log files for debugging and audit trails.

for file in os.listdir('database'):
    if file.endswith('.csv'):
        df = pd.read_csv('database/' + file)
        ingest_db(df, file[:-4], engine)

 ✅ Output:
All raw data stored as SQLite tables — purchases, sales, purchase_prices, vendor_invoice, etc.

2. Data Integration (get_vendor_summary.py)

Merges all relevant tables into a vendor_sales_summary table using SQL WITH clauses.

Adds calculated business metrics like:

GrossProfit

ProfitMargin

StockTurnover

SalesToPurchaseRatio

vendor_sales_summary = pd.read_sql_query("""
WITH FreightSummary AS (...),
PurchaseSummary AS (...),
SalesSummary AS (...)
SELECT ...
FROM PurchaseSummary ps
LEFT JOIN SalesSummary ss
LEFT JOIN FreightSummary fs
""", conn)

 ✅ Output:
A clean, structured table with vendor-wise aggregated performance data.

3. Data Cleaning

Fills missing values with zeros.

Converts numeric columns to correct datatypes.

Strips extra spaces from text columns.

df['VendorName'] = df['VendorName'].str.strip()
df.fillna(0, inplace=True)

4. Exploratory Data Analysis (performance_analysis.ipynb)

Performed in Jupyter Notebook:

Distribution analysis via histograms & boxplots

Correlation heatmaps between numeric features

Vendor contribution Pareto chart

Donut chart showing top vendor contribution

Statistical hypothesis testing (T-Test) to compare top vs. low-performing vendors’ profit margins

Example visualizations:

sns.heatmap(df.corr(), annot=True, cmap="coolwarm")
sns.barplot(x='VendorName', y='Purchase_Contribution%', data=top_vendors)

## 📊 Key KPIs
Metric	Description
Total Sales Dollars	Total revenue generated per vendor
Total Purchase Dollars	Total cost incurred for purchases
Gross Profit	Difference between sales and purchase dollars
Profit Margin (%)	Profitability ratio for each vendor
Stock Turnover	Efficiency of inventory movement
Sales-to-Purchase Ratio	Sales generated for each unit of purchase cost
Purchase Contribution (%)	Vendor’s contribution to total purchase volume
## 💡 Insights Derived

Top Vendors contribute ~80% of total purchases — classic Pareto distribution.

High-turnover vendors don’t always yield higher profits.

Bulk buyers receive ~70% price advantage per unit.

Low-performing vendors maintain higher margins — focus on volume growth.

Freight cost variation highlights potential logistics optimization.

## 🧰 Tools & Technologies Used
Category	Tools
Data Processing	Python (Pandas, NumPy, SQLAlchemy)
Database	SQLite
Data Visualization	Matplotlib, Seaborn, Power BI
Logging	Python Logging module
EDA Environment	Jupyter Notebook
Version Control	Git & GitHub
## 🧠 Statistical Analysis

A two-sample T-Test compared profit margins between top and low-performing vendors.

t_stat, p_value = ttest_ind(top_vendors, low_vendors, equal_var=False)


Result:

p-value < 0.05 → Statistically significant difference.
→ Low-performing vendors had higher average profit margins.

## 📦 How to Run This Project
Step 1: Clone the Repo
git clone https://github.com/your-username/vendor-performance-analysis.git
cd vendor-performance-analysis

Step 2: Install Dependencies
pip install pandas numpy sqlalchemy matplotlib seaborn scipy

Step 3: Load Raw Data

Make sure your .csv files are inside a /database folder, then run:

python ingestion_db.py

Step 4: Create Summary Table
python get_vendor_summary.py

Step 5: Run EDA

Open Jupyter Notebook and execute:

performance_analysis.ipynb

## 📚 Future Enhancements

Automate Power BI refresh via API.

Integrate anomaly detection for vendor outliers.

Deploy as a Streamlit or Flask dashboard.

Add scheduling using Apache Airflow.

👩‍💻 Author

 Tripti Singh
📧 Data Analytics Enthusiast
