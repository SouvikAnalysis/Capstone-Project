🚀 Swiggy Power BI Ranking Dashboard

This Power BI project provides ranking-based performance analysis for Swiggy's sales and delivery data across various metrics like order quantity, customer types, sales trends, and product categories. The 

dashboard helps stakeholders quickly assess top-performing products, customer trends, delivery insights, and seasonal variations.

📂 Data Source:

All data tables (Customers, Orders, Order_Details, Products, Product_Lines, Offices, Employees, Payments, Rank Table) are sourced from Excel files.

🗂️ Data Model

The model follows a snowflake schema with proper normalization to optimize performance and ensure data accuracy.

📊 Fact Table:

Order_Details: Contains transactional sales and delivery data including order numbers, product codes, quantities, prices, and profits.

🗃️ Dimension Tables:

Orders: Order metadata (order date, year, month)

Customers: Customer information (city, country, customer type)

Products: Product details (product name, product line, pricing)

Product_Lines: Product categories

Offices: Company office locations

Employees: Sales reps and their office assignments

Payments: Payment data for each order

Rank Table: Custom ranking logic (Top 5, Top 10, Top 20, Top 30, Top 100)

🔗 Key Relationships:
Orders ➔ Order_Details on orderNumber

Customers ➔ Orders on customerNumber

Products ➔ Order_Details on productCode

Product_Lines ➔ Products on productLine

Employees ➔ Offices on officeCode

📑 Report Pages & Visuals Breakdown

📄 Page 1: Executive Summary

✅ Total Profit, Total Orders, Total Customers

✅ Top Ranked Products based on dynamic selection (Top 5, Top 10, etc.)

📈 Order Trends: Monthly order trends and YoY comparisons

🌍 Customer Geo Distribution: Visualizes orders by region and customer type

🔝 Ranking Table: Allows users to dynamically filter product performance based on custom rank selections (Top 5, Top 10, etc.)

📄 Page 2: Detailed Product Ranking & Trends

📊 Visuals: Sales and Quantity by Product, Product Line, and Customer Type

🗓️ Monthly order breakdown by Product Line and Ranking Category

📦 Low Stock Items identified using stock_requirement DAX measure

🧮 DAX Measures Analysis

📌 Top Ranked Products (Dynamic Selection via Rank Table)
 
     Rank Table = DATATABLE(
    "Sort", INTEGER, "Type", STRING, "NO", INTEGER,
    {
        {0, "Default", 0},
        {1, "Top 5", 5},
        {2, "Top 10", 10},
        {3, "Top 20", 20},
        {4, "Top 30", 30},
        {5, "Top 100", 100}
    }
    )
    
📌 MostOrderedProduct:
 
    MostOrderedProduct = 
    SELECTCOLUMNS(
        TOPN(
            1, 
            SUMMARIZE('Products', Products[productName], "TotalOrdered", SUM('Order_Details'[quantityOrdered])),
            [TotalOrdered], DESC
        ), 
        "MostOrderedProduct", 'Products'[productName]
    )
    
📌 Total Profit:

    Total_Profit = SUM(Order_Details[Profit])

📌 Monthly and Yearly Breakdown:

    Month_Number = Orders[orderDate].[MonthNo]
    Order_Year = Orders[orderDate].[Year]
    
📌 Stock Requirement (Low Stock Flag):

    stock_requirement = IF(Products[quantityInStock] >= 500, "No", "Yes")
    
💡 Key Insights

Dynamic Ranking: Users can switch between Top 5, Top 10, Top 20, etc., to see how rankings impact visual trends.

Total Profit & Order Trends: Seasonal peaks and dips are easily identified.

Customer Insights: Concentrated demand in specific cities and customer types.

Low Stock Products: Visual flags for products that require inventory attention.

Top Product Performance: High-demand products drive most of the revenue.

✅ Suggestions for Enhancement

➕ Add Year-over-Year (YoY) KPI visuals with conditional formatting.

📅 Introduce a dynamic date slicer for more granular trend analysis.

🔍 Implement Drillthrough for product-level or customer-level detail.

📦 Build a dedicated Inventory Dashboard using stock requirement insights.

🗂️ Consider adding Tooltips for deeper context on key visuals.
