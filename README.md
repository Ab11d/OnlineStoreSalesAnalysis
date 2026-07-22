# Online Store Sales Analysis

A Python data analysis project that examines online store sales data using **Pandas** and **Matplotlib**. The project covers data loading, exploration, feature engineering, business analysis, filtering, sorting, visualization, and customer segmentation.

## Dataset

The project uses the following file:

```text
sales.xlsx
```

The dataset contains **200 orders** and the following original columns:

| Column | Description |
|---|---|
| `Order_ID` | Unique order identifier |
| `Date` | Date the order was placed |
| `Customer` | Customer name |
| `City` | Customer city |
| `Category` | Product category |
| `Product` | Product name |
| `Quantity` | Number of items purchased |
| `Price` | Price per item |
| `Discount` | Discount percentage |

## Technologies Used

- Python
- Pandas
- Matplotlib
- OpenPyXL
- Google Colab or Jupyter Notebook

## Installation

Install the required Python libraries:

```bash
pip install pandas matplotlib openpyxl
```

## Running the Project

### Google Colab

1. Open a new Google Colab notebook.
2. Upload `sales.xlsx`.
3. Import the required libraries.
4. Run each project section in order.

```python
import pandas as pd
import matplotlib.pyplot as plt

sales_df = pd.read_excel("sales.xlsx")
```

### Jupyter Notebook

Place `sales.xlsx` in the same folder as the notebook and run:

```python
import pandas as pd
import matplotlib.pyplot as plt

sales_df = pd.read_excel("sales.xlsx")
```

## Project Tasks

### Part 1 — Load the Dataset

- Import the required libraries
- Load the Excel dataset
- Display the first five rows
- Display the last five rows

### Part 2 — Explore the Dataset

The dataset was examined for:

- Number of rows and columns
- Column names
- Data types
- Missing values
- Duplicate records
- Summary statistics

### Dataset Quality Results

- Rows: **200**
- Columns: **9**
- Missing values: **0**
- Duplicate records: **0**

### Part 3 — Create New Columns

The following columns were created:

#### Total Price

```text
Total Price = Quantity × Price
```

#### Discount Amount

```text
Discount Amount = Total Price × Discount ÷ 100
```

#### Final Amount

```text
Final Amount = Total Price − Discount Amount
```

#### Month

The month name was extracted from the `Date` column.

```python
sales_df["Date"] = pd.to_datetime(sales_df["Date"])

sales_df["Total Price"] = (
    sales_df["Quantity"] * sales_df["Price"]
)

sales_df["Discount Amount"] = (
    sales_df["Total Price"] * sales_df["Discount"] / 100
)

sales_df["Final Amount"] = (
    sales_df["Total Price"] - sales_df["Discount Amount"]
)

sales_df["Month"] = sales_df["Date"].dt.month_name()
```

### Part 4 — Business Analysis

The analysis answered the following questions:

1. What is the total revenue?
2. How many orders were placed?
3. How many unique customers made purchases?
4. Which product generated the highest revenue?
5. Which product sold the highest quantity?
6. Which category generated the highest revenue?
7. Which city generated the highest revenue?
8. Which month had the highest sales?
9. What was the average order value?
10. Which order had the highest final amount?

## Key Findings

| Metric | Result |
|---|---|
| Total revenue | ₹6,891,254.05 |
| Total orders | 200 |
| Unique customers | 15 |
| Highest-revenue product | Laptop |
| Laptop revenue | ₹3,282,565.35 |
| Highest-quantity product | Phone |
| Phone quantity sold | 103 units |
| Highest-revenue category | Electronics |
| Electronics revenue | ₹6,118,067.35 |
| Highest-revenue city | Calicut |
| Calicut revenue | ₹1,542,166.45 |
| Highest-sales month | June |
| June revenue | ₹1,524,157.85 |
| Average order value | ₹34,456.27 |
| Highest-value order | ORD0194 |
| Highest order value | ₹317,426.55 |

## Part 5 — Grouping and Aggregation

Pandas `groupby()` was used to calculate:

- Revenue by category
- Revenue by city
- Revenue by month
- Average discount by category
- Average quantity sold for each product

### Revenue by Category

| Category | Revenue |
|---|---:|
| Electronics | ₹6,118,067.35 |
| Clothing | ₹563,139.20 |
| Grocery | ₹210,047.50 |

### Revenue by City

| City | Revenue |
|---|---:|
| Calicut | ₹1,542,166.45 |
| Mumbai | ₹1,170,524.35 |
| Chennai | ₹1,085,625.60 |
| Trivandrum | ₹1,030,973.80 |
| Hyderabad | ₹763,920.15 |
| Delhi | ₹471,290.50 |
| Bangalore | ₹453,497.25 |
| Kochi | ₹373,255.95 |

### Revenue by Month

| Month | Revenue |
|---|---:|
| January | ₹883,225.15 |
| February | ₹1,252,307.20 |
| March | ₹1,133,391.45 |
| April | ₹1,267,436.75 |
| May | ₹830,735.65 |
| June | ₹1,524,157.85 |

## Part 6 — Filtering

The dataset was filtered to display:

- Orders with a final amount greater than ₹20,000
- Orders with a discount greater than 20%
- Electronics orders
- Orders placed in January
- Orders with a quantity greater than 5

| Filter | Matching Orders |
|---|---:|
| Final Amount greater than ₹20,000 | 56 |
| Discount greater than 20% | 52 |
| Electronics category | 66 |
| January orders | 37 |
| Quantity greater than 5 | 72 |

## Part 7 — Sorting

The dataset was sorted by:

- Highest final amount
- Lowest final amount
- Highest quantity sold
- Highest discount

Example:

```python
highest_final_amount = sales_df.sort_values(
    by="Final Amount",
    ascending=False
)
```

## Part 8 — Data Visualization

The following visualizations were created using Matplotlib:

1. Bar chart — Revenue by category
2. Pie chart — Revenue share by category
3. Line chart — Monthly revenue
4. Histogram — Distribution of final amount
5. Scatter plot — Price versus quantity
6. Bar chart — Top 10 products by revenue
7. Horizontal bar chart — Top 10 customers by spending

## Part 9 — Customer Segmentation

Each customer's total spending was calculated and applied to all their order records.

```python
sales_df["Total Customer Spending"] = (
    sales_df.groupby("Customer")["Final Amount"]
    .transform("sum")
)
```

Because the original project thresholds placed almost every customer in the highest segment, the ranges were adjusted to better fit this dataset.

### Revised Customer Segments

| Total Customer Spending | Segment |
|---|---|
| ₹600,000 or more | Platinum |
| ₹500,000–₹599,999 | Gold |
| ₹300,000–₹499,999 | Silver |
| Below ₹300,000 | Bronze |

```python
def assign_customer_segment(spending):
    if spending >= 600000:
        return "Platinum"
    elif spending >= 500000:
        return "Gold"
    elif spending >= 300000:
        return "Silver"
    else:
        return "Bronze"


sales_df["Customer Segment"] = (
    sales_df["Total Customer Spending"]
    .apply(assign_customer_segment)
)
```

The revised thresholds produce the following customer distribution:

| Segment | Number of Customers |
|---|---:|
| Platinum | 3 |
| Gold | 4 |
| Silver | 5 |
| Bronze | 3 |

## Main Business Insights

- Electronics generated most of the store's revenue.
- Laptop sales were the largest source of product revenue.
- Phones sold the highest total number of units.
- Calicut was the strongest city by revenue.
- June produced the highest monthly sales.
- The business relies heavily on electronics, indicating potential concentration risk.
- Customer segmentation can support targeted promotions and loyalty programs.
- High-value customers should receive retention-focused offers.
- Lower-spending customers could be targeted with bundles, discounts, or product recommendations.

## Suggested Project Structure

```text
online-store-sales-analysis/
│
├── sales.xlsx
├── online_store_sales_analysis.ipynb
├── README.md
└── charts/
    ├── revenue_by_category.png
    ├── revenue_share_by_category.png
    ├── monthly_revenue.png
    ├── final_amount_distribution.png
    ├── price_vs_quantity.png
    ├── top_products.png
    └── top_customers.png
```
## Future Improvements

Possible extensions include:

- Building an interactive dashboard
- Exporting the cleaned dataset to Excel or CSV
- Adding year-over-year sales comparisons
- Studying product profitability
- Measuring discount effectiveness
- Creating customer retention metrics
- Using clustering for data-driven customer segmentation
- Building predictive sales models

## Author

**Abid Hashim**
