Customer Shopping Behavior Analysis
An end-to-end data analytics project that takes raw transactional data through Python cleaning → PostgreSQL SQL analysis → Power BI dashboard to uncover customer spending patterns, segments, and product preferences.

📌 Project Overview
This project analyzes 3,900 customer purchases across product categories to answer ten business questions around revenue, customer segmentation, discount behavior, product performance, and subscription trends. The output is a set of SQL-backed insights and an interactive Power BI dashboard designed to support strategic business decisions.

🗂️ Dataset Summary
Attribute	Detail
Rows	3,900
Columns	18
Missing Values	37 (Review Rating)
Key feature groups:

Demographics — Age, Gender, Location, Subscription Status

Purchase details — Item Purchased, Category, Purchase Amount (USD), Season, Size, Color

Behavioral — Discount Applied, Promo Code Used, Previous Purchases, Frequency of Purchases, Review Rating, Shipping Type

🔄 Pipeline Architecture
text
Raw CSV
   │
   ▼
┌──────────────────────┐
│  Python (pandas)     │  Loading → Cleaning → Feature Engineering
└──────────────────────┘
   │  SQLAlchemy + psycopg2-binary
   ▼
┌──────────────────────┐
│  PostgreSQL          │  10 business-question queries
└──────────────────────┘
   │  Query results
   ▼
┌──────────────────────┐
│  Power BI            │  Interactive dashboard
└──────────────────────┘
🧹 Data Preparation (Python)
Load — imported the CSV with pandas.

Explore — df.info() and df.describe() to profile structure and distributions.

Missing data — imputed the 37 nulls in Review Rating using the median rating per product category (preserves category-level rating distributions rather than a global median).

Standardization — renamed all columns to snake_case.

Feature engineering

age_group — binned customer ages.

purchase_frequency_days — derived from purchase data.

Consistency check — verified discount_applied and promo_code_used were redundant; dropped promo_code_used.

Load to database — wrote the cleaned DataFrame to PostgreSQL via SQLAlchemy.

🗄️ SQL Analysis (10 Business Questions)
#	Question	Key Finding
1	Revenue by Gender	Male: $157,890** · Female: **$75,191
2	High-spending discount users	839 customers used a discount while spending above average
3	Top 5 products by rating	Gloves (3.86), Sandals (3.84), Boots (3.82), Hat (3.80), Skirt (3.78)
4	Standard vs. Express shipping	Standard: $58.46** avg · Express: **$60.48 avg
5	Subscribers vs. non-subscribers	Subscribers: 1,053 customers, $59.49 avg, **$62,645** total · Non-subscribers: 2,847 customers, $59.87 avg, **$170,436** total
6	Discount-dependent products	Hat (50.0%), Sneakers (49.66%), Coat (49.07%), Sweater (48.17%), Pants (47.37%)
7	Customer segmentation	Loyal: 3,116 · New: 833 · Returning: 701
8	Top 3 products per category	Accessories: Jewelry/Sunglasses/Belt · Clothing: Blouse/Pants/Shirt · Footwear: Sandals/Shoes/Sneakers · Outerwear: Jacket/Coat
9	Repeat buyers (>5 purchases) & subscriptions	2,518 non-subscribers vs. 958 subscribers among repeat buyers
10	Revenue by age group	Young Adults lead revenue; Senior group smallest
Note on Q5: Average spend is nearly identical between segments ($59.49 vs $59.87). Non-subscribers dominate total revenue simply because they outnumber subscribers ~2.7:1 — an acquisition/conversion story, not a "subscribers spend more" story.

📊 Power BI Dashboard
Headline KPIs

Number of Customers: 3.9K

Average Purchase Amount: $59.76

Average Review Rating: 3.75

Visuals

Donut — % of customers by subscription status (Yes 27% / No 73%)

Bar — Revenue and Sales by Category

Bar — Revenue and Sales by Age Group

Slicers — Subscription Status, Gender, Category, Shipping Type

💡 Business Recommendations
Boost Subscriptions — promote exclusive subscriber benefits; only 27% of customers subscribe despite near-identical average spend.

Customer Loyalty Programs — reward repeat buyers to migrate the 701 "Returning" customers into the 3,116-strong "Loyal" segment.

Review Discount Policy — products like Hat (50%), Sneakers (49.7%), and Coat (49.1%) are heavily discounted; balance sales lift against margin erosion.

Product Positioning — feature top-rated (Gloves, Sandals, Boots) and best-selling items in campaigns.

Targeted Marketing — focus on high-revenue age groups (Young Adults) and Express-shipping users, who spend slightly more per order ($60.48 vs $58.46).

🛠️ Setup & Reproduce
Prerequisites
Python 3.11+

PostgreSQL 14+

Power BI Desktop

1. Install dependencies
bash
python -m pip install pandas sqlalchemy psycopg2-binary jupyter
2. Configure the database connection
Create a .env file (never commit credentials):

env
DB_USER=your_user
DB_PASSWORD=your_password
DB_HOST=localhost
DB_PORT=5432
DB_NAME=customer_behavior
3. Create the schema and load data
python
from sqlalchemy import create_engine
import pandas as pd, os
from dotenv import load_dotenv

load_dotenv()
engine = create_engine(
    f"postgresql+psycopg2://{os.getenv('DB_USER')}:{os.getenv('DB_PASSWORD')}"
    f"@{os.getenv('DB_HOST')}:{os.getenv('DB_PORT')}/{os.getenv('DB_NAME')}"
)
df = pd.read_csv("customer_shopping_behavior.csv")
# ... cleaning steps ...
df.to_sql("transactions", engine, if_exists="replace", index=False)
4. Run the SQL analysis
Execute the queries in sql/analysis.sql against the transactions table.

5. Open the dashboard
Open dashboard/Customer_Behavior_Dashboard.pbix in Power BI Desktop and refresh the data source to point at your local PostgreSQL instance.

📁 Suggested Project Structure
text
customer-shopping-behavior/
├── data/
│   └── customer_shopping_behavior.csv
├── notebooks/
│   ├── 01_cleaning.ipynb
│   └── 02_load_to_postgres.ipynb
├── sql/
│   └── analysis.sql
├── dashboard/
│   └── Customer_Behavior_Dashboard.pbix
├── reports/
│   └── Customer_Shopping_Behavior_Analysis.pdf
├── .env.example
├── requirements.txt
└── README.md
⚠️ Known Issues / TODO
□ Verify Q2 (high-spending discount users): reported purchase amounts (up to ~$22,600) exceed the dataset maximum of $100 — confirm the correct column is being selected.
□ Add percentage columns to the Q9 repeat-buyer table for readability.
□ Render all result tables (Q3, Q10) in the final PDF report.
□ Add a "so what" takeaway sentence beneath each SQL result.
