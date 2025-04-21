# Amazon Sales Data Analysis Project

## Project Overview

This project focuses on analyzing Amazon sales data in India for the year 2022 using SQL, Excel, Power BI, and Tableau to derive actionable insights. The goal is to identify sales trends, profit margins, and category performance to support business decision-making.

## Table of Contents

- Project Overview
- Dataset
- Prerequisites
- Setup
- Analysis Workflow
- Key Queries
- Results
- Contributing
- License

## Dataset

- **Source**: The dataset is sourced from the provided "Amazon Sale Report.csv" file.
- **Description**: The dataset contains Amazon sales records in India for 2022, including columns like `Order ID`, `Date`, `Status`, `Category`, `Size`, `Qty`, `Amount`, `B2B`, and more, representing sales transactions, quantities sold, and profit metrics.
- **Schema**:
  - `index`: Unique identifier for each record
  - `Order ID`: Unique order identifier
  - `Date`: Order date (MM/DD/YYYY)
  - `Status`: Order status (e.g., Shipped, Cancelled)
  - `Category`: Product category (e.g., T-shirt, Shirt)
  - `Size`: Product size (e.g., S, M, L, XL)
  - `Qty`: Quantity sold
  - `Amount`: Sale amount in INR
  - `B2B`: Boolean indicating if the sale is business-to-business
  - Additional columns for shipping details and fulfillment.

## Prerequisites

- SQL-compatible database (e.g., PostgreSQL, MySQL, SQLite)
- SQL client (e.g., DBeaver, pgAdmin, or command-line interface)
- Microsoft Excel for data preprocessing
- Power BI Desktop and Tableau for visualization

## Setup

1. **Clone the Repository**:

   ```bash
   git clone https://github.com/Islam-Ossama/BI-amazon-sales.git
   ```

2. **Set Up the Database**:

   - Import the dataset into your SQL database using the provided `Amazon Sale Report.csv`.

   - Example for PostgreSQL:

     ```bash
     psql -U username -d amazon_sales_db -c "\copy sales_data FROM 'Amazon Sale Report.csv' DELIMITER ',' CSV HEADER;"
     ```

3. **Preprocess with Excel**:

   - Open `Amazon Sale Report.csv` in Excel to clean data (e.g., handle missing `Amount` values, remove duplicates).
   - Save the cleaned file for import into SQL or directly into Power BI/Tableau.

4. **Configure Power BI and Tableau**:

   - Open Power BI Desktop or Tableau, connect to your SQL database, and load the `sales_data` table.
   - Alternatively, directly import the cleaned CSV file into Power BI or Tableau.

5. **Configure Connection**:

   - Update connection settings in your SQL client, Power BI, or Tableau (e.g., host, port, username, password).

## Analysis Workflow

1. **Data Cleaning**: Use Excel to handle missing values (e.g., `Amount` for cancelled orders), duplicates, and inconsistencies in `Status` and `Category`.
2. **Exploratory Analysis**: Identify patterns in sales quantities, profit by category, and quarterly trends using SQL queries.
3. **Query Development**: Write SQL queries to calculate total quantities sold, profit by category, and quarterly performance.
4. **Visualization**: Import query results into Power BI and Tableau to create dashboards visualizing key metrics like total profit, category-wise sales, and size-wise profit distribution.

## Key Queries

Below are examples of SQL queries used in the analysis:

- **Query 1**: Calculate total quantity sold by category

  ```sql
  SELECT Category, SUM(Qty) AS total_quantity
  FROM sales_data
  GROUP BY Category
  ORDER BY total_quantity DESC;
  ```

- **Query 2**: Calculate total profit by category (using Amount as profit proxy for shipped orders)

  ```sql
  SELECT Category, SUM(Amount) AS total_profit
  FROM sales_data
  WHERE Status LIKE 'Shipped%'
  GROUP BY Category
  ORDER BY total_profit DESC;
  ```

- **Query 3**: Quarterly sales quantity and amount

  ```sql
  SELECT 
      CASE 
          WHEN EXTRACT(MONTH FROM TO_DATE(Date, 'MM/DD/YYYY')) BETWEEN 1 AND 3 THEN 'Qtr 1'
          WHEN EXTRACT(MONTH FROM TO_DATE(Date, 'MM/DD/YYYY')) BETWEEN 4 AND 6 THEN 'Qtr 2'
          WHEN EXTRACT(MONTH FROM TO_DATE(Date, 'MM/DD/YYYY')) BETWEEN 7 AND 9 THEN 'Qtr 3'
          ELSE 'Qtr 4'
      END AS Quarter,
      SUM(Qty) AS total_quantity,
      SUM(Amount) AS total_amount
  FROM sales_data
  WHERE Status LIKE 'Shipped%'
  GROUP BY Quarter
  ORDER BY Quarter;
  ```

## Results

- Total quantity sold in 2022: 117K units, with T-shirts leading at 45K units.
- Total profit: ₹78.59M, with T-shirts contributing ₹39M.
- Quarterly analysis shows a consistent increase in sales volume and amount throughout the year.
- Size-wise profit distribution highlights M size as the highest contributor at ₹12.3M.
- Power BI and Tableau dashboards were created to visualize these insights, including charts for category-wise sales, quarterly trends, and size-wise profit.

## Contributing

Contributions are welcome! Please fork the repository and submit a pull request with your changes. For major updates, open an issue to discuss first.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.
