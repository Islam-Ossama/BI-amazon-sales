# Amazon Sales Data Analysis Project

## Project Overview

This project focuses on analyzing Amazon sales data in India for the year 2022 using Power BI to derive actionable insights. The goal is to identify sales trends, profit margins, and category performance to support business decision-making.

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

- Power BI Desktop for data analysis and visualization

## Setup

1. **Clone the Repository**:

   ```bash
   git clone https://github.com/Islam-Ossama/BI-amazon-sales.git
   ```

2. **Configure Power BI**:

   - Open Power BI Desktop and import the `Amazon Sale Report.csv` file directly.
   - Use Power BI's data transformation tools (Power Query Editor) to clean and prepare the data.

## Analysis Workflow

1. **Data Cleaning**: Use Power BI's Power Query Editor to handle missing values (e.g., `Amount` for cancelled orders), duplicates, and inconsistencies in `Status` and `Category`.
2. **Exploratory Analysis**: Leverage Power BI to identify patterns in sales quantities, profit by category, and quarterly trends through data modeling and DAX calculations.
3. **Visualization**: Create interactive dashboards in Power BI to visualize key metrics like total profit, category-wise sales, and size-wise profit distribution.

## Key Queries

In this project, analysis was performed directly in Power BI using DAX (Data Analysis Expressions) instead of SQL. Below are examples of DAX measures used for the analysis:

- **Measure 1**: Calculate total quantity sold by category

  ```
  Total Quantity by Category = 
  CALCULATE(
      SUM('sales_data'[Qty]),
      ALLEXCEPT('sales_data', 'sales_data'[Category])
  )
  ```

- **Measure 2**: Calculate total profit by category (using Amount as profit proxy for shipped orders)

  ```
  Total Profit by Category = 
  CALCULATE(
      SUM('sales_data'[Amount]),
      'sales_data'[Status] IN {"Shipped", "Shipped - Delivered to Buyer"},
      ALLEXCEPT('sales_data', 'sales_data'[Category])
  )
  ```

- **Measure 3**: Quarterly sales quantity and amount

  ```
  Quarter = 
  SWITCH(
      TRUE(),
      MONTH('sales_data'[Date]) <= 3, "Qtr 1",
      MONTH('sales_data'[Date]) <= 6, "Qtr 2",
      MONTH('sales_data'[Date]) <= 9, "Qtr 3",
      "Qtr 4"
  )
  
  Total Quantity by Quarter = 
  CALCULATE(
      SUM('sales_data'[Qty]),
      'sales_data'[Status] IN {"Shipped", "Shipped - Delivered to Buyer"},
      ALLEXCEPT('sales_data', 'sales_data'[Quarter])
  )
  
  Total Amount by Quarter = 
  CALCULATE(
      SUM('sales_data'[Amount]),
      'sales_data'[Status] IN {"Shipped", "Shipped - Delivered to Buyer"},
      ALLEXCEPT('sales_data', 'sales_data'[Quarter])
  )
  ```

## Results

- Total quantity sold in 2022: 117K units, with T-shirts leading at 45K units.
- Total profit: ₹78.59M, with T-shirts contributing ₹39M.
- Quarterly analysis shows a consistent increase in sales volume and amount throughout the year.
- Size-wise profit distribution highlights M size as the highest contributor at ₹12.3M.
- A Power BI dashboard was created to visualize these insights, including charts for category-wise sales, quarterly trends, and size-wise profit.

## Contributing

Contributions are welcome! Please fork the repository and submit a pull request with your changes. For major updates, open an issue to discuss first.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.
