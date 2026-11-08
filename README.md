# 🛍️ Customer Shopping Behavior Analysis

> End-to-end analytics project: **Python → PostgreSQL → Power BI**

<p align="center">
  <img src="dashboard/dashboard_preview.png" alt="Dashboard Preview" width="800"/>
</p>

![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-14+-336791?logo=postgresql&logoColor=white)
![Power BI](https://img.shields.io/badge/Power_BI-F2C811?logo=powerbi&logoColor=black)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

---

## 📖 Overview

This project analyzes **3,900 customer purchase transactions** to uncover insights about spending patterns, customer segments, product performance, and subscription behavior.

The goal: **turn raw data into business decisions** using a clean, end-to-end analytics pipeline.

---

## 🔄 Pipeline
📄 Raw CSV → 🐍 Python (clean & prep) → 🐘 PostgreSQL (analyze) → 📊 Power BI (visualize)

| Stage | Tool | Purpose |
|---|---|---|
| 1. Cleaning | Python (pandas) | Handle missing values, standardize columns, engineer features |
| 2. Analysis | PostgreSQL | Answer 10 business questions with SQL |
| 3. Visualization | Power BI | Interactive dashboard for stakeholders |

---

## 🧹 Data Preparation

| Issue | Solution |
|---|---|
| 37 missing `Review Rating` values | Imputed with **median rating per product category** |
| Inconsistent column names | Renamed to `snake_case` |
| Redundant fields | `discount_applied` and `promo_code_used` were identical → dropped one |
| Raw ages | Binned into `age_group` buckets |

---

## 🐘 SQL Analysis — Key Findings

| # | Business Question | Insight |
|---|---|---|
| 1 | Revenue by gender | **Men $157,890** vs Women $75,191 |
| 2 | High-spending discount users | 839 customers spent above average *with* discounts |
| 3 | Top-rated products | Gloves (3.86), Sandals (3.84), Boots (3.82) |
| 4 | Shipping type impact | Express ($60.48) > Standard ($58.46) |
| 5 | Subscribers vs non-subscribers | Similar avg spend (~$59.5) — **conversion opportunity** |
| 6 | Most discounted products | Hat (50%), Sneakers (49.7%), Coat (49.1%) |
| 7 | Customer segments | **3,116 Loyal** · 833 New · 701 Returning |
| 8 | Top products per category | Jewelry, Blouse, Sandals, Jacket lead their categories |
| 9 | Repeat buyers & subscriptions | 958 of 3,476 repeat buyers subscribed (**~28%**) |
| 10 | Revenue by age group | Young Adults drive the most revenue |

> 💡 **Key insight:** Non-subscribers generate **2.7× more revenue** simply because there are more of them — not because they spend more. The real opportunity is **conversion**, not upselling.

---
