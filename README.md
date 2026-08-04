# Olist Brazilian E-Commerce, Data Wrangling & Exploratory Analysis

Independent, unguided data cleaning and analysis project built on the [Olist Brazilian E-Commerce Public Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (Kaggle), a real-world relational dataset covering orders, customers, sellers, products, payments, and reviews.

## Objective

Practice handling messy, real-world tabular data without a guided tutorial, identifying data quality issues on my own and deciding how to treat them, then using the cleaned data to run an exploratory analysis and a simple baseline machine learning model.

Full reasoning behind every decision (nulls, duplicates, transformations, and modeling choices) is documented in [`step-by-description.md`](./step-by-description.md). This README is a summary, the notebook (`main.ipynb`) has the full implementation.

## What's in this repo

| File | Content |
|---|---|
| `main.ipynb` | Full notebook: cleaning, transformation, EDA, and modeling |
| `step-by-description.md` | Detailed write-up of every decision and assumption made |
| `images/` | Charts generated during the analysis |
| `olist_*.csv` | Raw source data (9 relational tables) |

## Data cleaning highlights

- **Missing values** treated per-column with different strategies depending on context (explicit "no_information" placeholders for descriptive text, category-mean imputation for numeric product measures, and intentional non-treatment where nulls carried real business meaning, e.g. an order that was never delivered has no delivery date).
- **Duplicates**: identified and resolved two real structural issues in the dataset, installment payments splitting a single order across multiple rows, and reviews not being strictly one-to-one with orders.
- **Geolocation cleaning**: normalized city/state names (accents, casing) and validated coordinates against Brazil's real geographic bounds before aggregating ZIP-level data to city level, fixing silent merge mismatches caused by inconsistent text formatting.

## Exploratory analysis: key findings

- São Paulo (SP) accounts for **over one-third** of total spending among all 27 Brazilian states.
- Order volume grows steadily from January and peaks in **August**, followed by a sharp drop in September.
- Customer-seller distance, computed via the **Haversine formula**, is concentrated at short ranges with a long tail reflecting Brazil's size.
- **Computers** is the highest average-price product category, nearly double the second-highest.
- **~77%** of orders received a positive review (4 or 5 stars).

## Machine Learning (baseline)

A simple **Linear Regression** was trained to predict `review_score` from order characteristics (delivery time, distance, price, freight, payment type/installments), with proper preprocessing:

- One-Hot Encoding for the categorical payment method
- Train/test split performed before scaling to avoid data leakage
- `StandardScaler` fit only on the training set

**Result:** R² train = 0.1265 / R² test = 0.1437, low explanatory power, as expected for a simple baseline with limited features and no hyperparameter tuning. The goal here was practicing the full supervised learning workflow correctly, not maximizing performance.

## Tech stack

Python · pandas · NumPy · Matplotlib · scikit-learn · Jupyter Notebook