# 📊 Power BI Restaurant Sales & User Analysis Dashboard

This Power BI project provides a comprehensive analysis of restaurant orders, sales trends, user performance, and dynamic city rankings using DAX and interactive visuals.

## 🔖 Description

**"Restaurant Sales & User Behavior Dashboard using Power BI"**  
This Power BI report analyzes restaurant sales, customer orders, and performance metrics across different cities and years. It includes dynamic rankings, top-N filters, user segmentation, and restaurant rating insights—powered by DAX measures and advanced visualizations.

## 🛠️ Tools & Technologies

- **Power BI Desktop**
- **DAX (Data Analysis Expressions)**
- **Power Query**
- **Data Modeling & Relationships**

## 📂 Project Structure

| File Name               | Description                           |
|------------------------|---------------------------------------|
| `Overview.png`         | Dashboard overview screen             |
| `City_Overview.png`    | City-wise breakdown and trends        |
| `Insights.png`         | Key business insights                 |
| `User_Performance.png` | Detailed user-level analysis          |
| `Restaurant_Analysis.png` | Restaurant metrics and ratings     |

<img width="1472" height="773" alt="image" src="https://github.com/user-attachments/assets/06ed8c15-a1b6-4417-b792-c0a5de145422" />

---

## 🗂️ Data Source

This Power BI project is built using a custom dataset composed of restaurant order transactions, user data, and restaurant information.

- **Orders Table**: Transaction-level data with city, year, order type, quantity, and value.
- **Users Table**: User-level data with unique identifiers and total sales.
- **Restaurant Table**: Details like restaurant ID, ratings, and location.
- **Rank Table**: Manually created to filter top-N cities dynamically.



## 🧩 Data Model – ERD (Entity Relationship Diagram)

#### 🔗 Relationships (Star Schema)
```
        ┌────────────┐       ┌────────────┐
        │  Users     │       │ Restaurant │
        │ (Dim Table)│       │ (Dim Table)│
        └────┬───────┘       └────┬───────┘
             │                      │
             │                      │
        ┌────▼───────┐         ┌────▼───────┐
        │  Orders    │◄────────┤ Rank Table │
        │ (Fact)     │         │ (Dim Table)│
        └────────────┘         └────────────┘
```

### 🔗 Relationships & Keys

| From Table  | Key Column         | To Table     | Key Column         | Cardinality    |
|-------------|--------------------|--------------|---------------------|----------------|
| Orders      | `User_id`          | Users        | `User_id`           | Many-to-One    |
| Orders      | `Restaurant_id`    | Restaurant   | `Restaurant_id`     | Many-to-One    |
| Orders      | (Disconnected)     | Rank Table   | —                   | No Relationship|

## 📐 DAX Measures & Calculations

### 🔸 General Metrics
- `Sale_Value = SUM(Orders[Value])`
- `Total_Quantity_Amount = SUM(Orders[Value])`
- `Order_Count = COUNTROWS(Orders)`
- `Users_Count_Order = DISTINCTCOUNT(Orders[User_id])`
- `Restaurant_Counts = DISTINCTCOUNT(Restaurant[Restaurant_id])`
- `Users_count = DISTINCTCOUNT(Users[User_id])`
- `Rating_Counts = COUNT(Restaurant[Rating])`
- `Average_Ratings = AVERAGE(Restaurant[Rating])`
- `Total_Cities = DISTINCTCOUNT(Orders[City])`

### 🔸 Year-based Calculations
```dax
Current_Year = MAX(Orders[Year])
Previous_Year = [Current_Year] - 1

Current_Yr_Sale = 
VAR yr = [Current_Year]
RETURN CALCULATE([Sale_Value], Orders[Year] = yr)

Previous_Yr_Sale = 
VAR yr = [Previous_Year]
RETURN CALCULATE([Sale_Value], Orders[Year] = yr)
```

### 🔸 Dynamic Titles
```dax
Dynamic Title Top Sales = 
VAR selectrank = SELECTEDVALUE('Rank Table'[Type])
VAR selecttype = SELECTEDVALUE(Orders[Type])
RETURN selectrank & " City " & selecttype

Dynamic Title Year = 
VAR selectedtype = SELECTEDVALUE(Orders[Type])
RETURN selectedtype & " by Year"
```

### 🔸 Ranking & Filtering Logic
```dax
Rank Table = 
DATATABLE("Sort", INTEGER, "Type", STRING, "NO", INTEGER, {
  {0,"Default",0}, {3, "Top 5", 5}, {2, "Top 10", 10},
  {3, "Top 20",20}, {4, "Top 30", 30}, {5, "Top 100",100}
})

Top_N_Sales = 
VAR rankValue = RANKX(ALL(Orders[City]), [Sale_Value], , DESC)
VAR selectedRank = SELECTEDVALUE('Rank Table'[NO])
RETURN IF(
  selectedRank = 0, [Sale_Value],
  IF(rankValue <= selectedRank, [Sale_Value], BLANK())
)

Rank = RANKX(ALL(Users), [Total_Sales], , DESC, DENSE)

Top_10%_Customers = 
VAR TotalUsers = CALCULATE(DISTINCTCOUNT(Users[User_id]))
VAR TopCount = INT(TotalUsers * 0.1)
RETURN CALCULATE(SUM([Total_Sales]), FILTER(Users, Users[Rank] <= TopCount))

Total_Sales = 
CALCULATE(SUM(Orders[Value]), RELATEDTABLE(Orders), Orders[Type] = "Amount")
```

## 🔍 Key Insights

- **Top Performing Cities**: Dynamic rankings highlight which cities lead in sales.
- **User Contribution**: Top 10% of users contribute a large share of revenue.
- **Restaurant Performance**: Ratings show which restaurants are most liked.
- **Sales Trends**: Year-over-year comparisons reveal growth or dips.
- **Order Types**: Helps identify product mix and upsell opportunities.
- **Customer Engagement**: Tracks active users and total orders.

## 🚀 Enhancements for Future

- **Customer Segmentation** using RFM or clustering.
- **Geospatial Maps** to analyze performance by area.
- **Time Intelligence** with rolling averages and forecasts.
- **Sentiment Analysis** on review data.
- **Mobile-optimized View** for on-the-go access.
- **Real-time Data** using APIs or live connections.
- **Export Options** for filtered views.
- **Row-Level Security** to protect sensitive data.

## 📌 Author

**Souvik Ghosh**  
[https://www.linkedin.com/in/souvik-ghosh-/] 
[https://github.com/SouvikAnalysis]
