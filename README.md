# 🍔 Food Delivery Analytics Dashboard (Power BI)

## 🚀 Overview
This project is a Food Delivery Analytics Dashboard built using Power BI and Power Query Editor. It transforms raw food delivery data into meaningful business insights through interactive dashboards and KPI-driven analysis.

The dashboard helps track order performance, delivery efficiency, customer behavior, and restaurant analytics to support data-driven decision-making.

---

## 🎯 Objectives
- Analyze overall food delivery business performance
- Track order status (Delivered, Cancelled, Late)
- Improve delivery efficiency insights
- Understand customer retention and repeat behavior
- Evaluate restaurant performance and revenue contribution

---

## 📸 Dashboard Screenshots

### 📊Executive Overview Dashboard
![Executive Overview](Screenshots/1.png)

### 💰 Customer Analysis
![Customer Analysis](Screenshots/2.png)

### 👥 Resstaurant Performance 
![Resstaurant Performance](Screenshots/3.png)

### 📦Delivery & Operations
![Delivery & Operations](Screenshots/4.png)

### Restaurant Details
![Restaurant Details](Screenshots/5.png)

## 📊 Key KPIs
- Total Orders  
- Total Revenue  
- Average Order Value (AOV)  
- Average Delivery Time  
- Late Delivery %  
- Cancellation %  
- Repeat Customers %  
- Revenue per Restaurant  
- Average Rating  

---

## 📈 Dashboard Features
- Order performance tracking (Delivered / Cancelled / Late)
- Revenue and trend analysis
- Customer segmentation (New vs Repeat)
- Restaurant performance comparison
- City-wise insights
- Time-based analysis (Monthly / YTD)
- Interactive slicers and filters

---

## 🧠 Data Transformation (Power Query)
- Removed duplicates and null values
- Standardized categorical fields
- Created calculated columns
- Merged multiple datasets (Orders, Customers, Restaurants)
- Built a structured data model for analytics

---

## 🔢 Calculated Columns (DAX)

FilterCity:
VAR val = 'Customer'[city]
VAR wordCount = LEN(val) - LEN(SUBSTITUTE(val, " ", "")) + 1
RETURN
IF(
    ISBLANK(val) || wordCount > 4 || LEN(val) > 50,
    BLANK(),
    val
)

Cost Bucket:
SWITCH(
    TRUE(),
    Restaurants[CostForTwo] < 500, "Budget",
    Restaurants[CostForTwo] < 1000, "Mid Range",
    "Premium"
)

Rating Bucket:
SWITCH(
    TRUE(),
    Restaurants[Rating] >= 4.5, "Excellent",
    Restaurants[Rating] >= 4.0, "Good",
    Restaurants[Rating] >= 3.0, "Average",
    "Low"
)

Delivery Status:
IF(Orders[DeliveryTimeMins] > 45, "Late", "On Time")

Order Month:
FORMAT(Orders[OrderDate], "MMM YYYY")

---

## 📊 Core DAX Measures

Total Orders = COUNT(Orders[OrderID])

Total Revenue = SUM(Orders[OrderValue])

Avg Order Value = DIVIDE([Total Revenue], [Total Orders])

Avg Delivery Time = AVERAGE(Orders[DeliveryTimeMins])

Total Customers = DISTINCTCOUNT(Orders[CustomerID])

Total Restaurants = DISTINCTCOUNT(Restaurants[RestaurantID])

---

Delivered Orders =
CALCULATE([Total Orders], Orders[OrderStatus] = "Delivered")

Cancelled Orders =
CALCULATE([Total Orders], Orders[OrderStatus] = "Cancelled")

Late Orders =
CALCULATE([Total Orders], Orders[Delivery Status] = "Late")

Cancellation % =
DIVIDE([Cancelled Orders], [Total Orders])

Late Delivery % =
DIVIDE([Late Orders], [Total Orders])

---

Repeat Customers =
COUNTROWS(
    FILTER(
        VALUES(Orders[CustomerID]),
        CALCULATE(COUNT(Orders[OrderID])) > 1
    )
)

Repeat Customer % =
DIVIDE([Repeat Customers], [Total Customers])

Orders Per Customer =
DIVIDE([Total Orders], [Total Customers])

---

Revenue per Restaurant =
DIVIDE([Total Revenue], [Total Restaurants])

Avg Rating = AVERAGE(Restaurants[Rating])

Avg Votes = AVERAGE(Restaurants[Votes])

---

Revenue YTD = TOTALYTD([Total Revenue], DateTable[Date])

Orders YTD = TOTALYTD([Total Orders], DateTable[Date])

Revenue Growth % =
DIVIDE(
    [Total Revenue] - CALCULATE([Total Revenue], DATEADD(DateTable[Date], -1, MONTH)),
    CALCULATE([Total Revenue], DATEADD(DateTable[Date], -1, MONTH))
)

---

## 📊 Business Impact
- Improved visibility into delivery performance and delays
- Identified high-performing restaurants
- Enhanced customer retention insights
- Enabled KPI-based decision making
- Built executive-level analytics dashboard

---

## 🛠️ Tools Used
- Power BI
- Power Query Editor
- DAX (Data Modeling & KPIs)
- Excel / CSV Dataset

---

## ⭐ Author
Built as a portfolio project to demonstrate Power BI, data modeling, and business intelligence skills.

---

## 🔥 Note
This dashboard is designed for real-world KPI tracking, operational insights, and business performance monitoring in food delivery systems.
