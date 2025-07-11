
# 📄 Case Study: Restaurant Sales & User Analysis Dashboard – Power BI

## 🔍 Project Overview

This case study outlines the development of a comprehensive Power BI dashboard that analyzes restaurant orders, customer performance, sales trends, and city-based insights. The dashboard enables business stakeholders to monitor KPIs and make data-driven decisions based on dynamic visuals and DAX-powered calculations.

---

## 🎯 Objectives

- Analyze restaurant sales across cities and years
- Identify top-performing cities based on revenue
- Segment users by their contribution to sales
- Evaluate restaurant ratings and user engagement
- Enable dynamic filtering and ranking of insights

---

## 🧾 Data Source

The data consists of four primary tables:

- **Orders (Fact Table)**: Contains order-level data including year, city, value, quantity, type, and user ID.
- **Users (Dimension Table)**: Holds user details and total sales.
- **Restaurant (Dimension Table)**: Contains restaurant ID, rating, and metadata.
- **Rank Table (Disconnected Table)**: Used for dynamic Top-N ranking logic.

---

## 🧩 Data Model

The model follows a star schema:

```
        ┌────────────┐       ┌────────────┐
        │  Users     │       │ Restaurant │
        │ (Dim Table)│       │ (Dim Table)│
        └────┬───────┘       └────┬───────┘
             │                      │
             │                      │
        ┌────▼───────┐         ┌────▼───────┐
        │  Orders    │◄────────┤ Rank Table │
        │ (Fact)     │         │ (Disconnected) │
        └────────────┘         └────────────┘
```

Relationships are set up using user and restaurant IDs to maintain data integrity.

---

## ⚙️ DAX Measures & Logic

Key DAX measures were developed to enable dynamic insights:

- `Total_Sales`, `Sale_Value`, `Order_Count`, `Users_Count_Order`
- Yearly comparisons: `Current_Yr_Sale`, `Previous_Yr_Sale`
- Top-N filtering: `Top_N_Sales`, `Rank`
- User segmentation: `Top_10%_Customers`
- Dynamic titles and slicers using `SELECTEDVALUE()`

> A custom `Rank Table` enables user-controlled top-N city filtering.

---

## 📊 Visuals & Interactivity

The dashboard includes multiple pages:
- **Overview**: Summary metrics, KPIs, and year-over-year comparison
- **City Overview**: Top-N cities by sales with dynamic filter
- **User Performance**: Rankings and segmentation of users
- **Restaurant Analysis**: Ratings, count, and sales contribution
- **Insights Page**: Key takeaways and trends

Dynamic titles and measures enhance storytelling and interactivity.

---

## 🔑 Key Insights

1. **Top Cities**: Certain cities dominate revenue generation.
2. **Top Users**: Top 10% of users drive a significant share of total sales.
3. **Restaurant Ratings**: Higher-rated restaurants correlate with higher sales.
4. **Order Trends**: Clear growth patterns and seasonality observed.
5. **User Engagement**: Tracking quantity, value, and repeat orders.

---

## 🚀 Future Enhancements

- Add RFM-based customer segmentation
- Integrate geospatial visuals (maps)
- Enable real-time data updates via APIs
- Include sentiment analysis from user reviews
- Enhance mobile responsiveness and UX
- Implement Row-Level Security (RLS)

---

## 🧠 Skills Demonstrated

- Power BI data modeling & visualization
- Advanced DAX (RANKX, CALCULATE, VAR, SELECTEDVALUE)
- Dynamic report design and UX
- Analytical thinking and business insight extraction

---

## 🙌 Acknowledgements
- Developed by Souvik Ghosh as part of a Business Intelligence case study.

