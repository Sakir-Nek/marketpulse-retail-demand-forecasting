# MarketPulse Retail – Data Dictionary

## 1. Project Overview

This project uses synthetic data for a fictional Indian multi-category e-commerce retailer, MarketPulse Retail.

The datasets cover historical sales, inventory availability, suppliers and purchase orders from 1 January 2024 to 31 December 2025.

The purpose is to analyse historical demand, compare forecasting methods and develop practical purchasing and inbound planning recommendations.

All records and business figures are simulated for this portfolio project. They do not represent an actual retailer.

## 2. Dataset Summary

| Dataset | Records | Purpose |
|---|---:|---|
| Products.csv | 1,100 | Product and SKU information |
| Suppliers.csv | 10 | Supplier details and lead times |
| Product_Supplier_Mapping.csv | 1,100 | Preferred supplier for each SKU |
| Daily_Sales.csv | 804,100 | Daily sales and inventory movement |
| Purchase_Orders.csv | 26,400 | Purchase order details and outstanding quantities |
| Purchase_Order_Receipts.csv | 29,522 | Individual purchase order receipt records |

## 3. Dataset Definitions

### 3.1 Products.csv

Contains the product catalogue and basic SKU attributes.

| Column | Description |
|---|---|
| SKU_ID | Unique product identifier |
| Category | Product category |
| Demand_Pattern | Simulated demand behaviour: Stable, Seasonal, Growing, Declining or Intermittent |
| Product_Name | Synthetic product name |
| Unit_Selling_Price | Simulated selling price per unit |
| Unit_Purchase_Cost | Simulated purchase cost per unit |
| Supplier_ID | Preferred supplier assigned to the SKU |
| Base_Daily_Demand | Estimated baseline daily demand used during data generation |

### 3.2 Suppliers.csv

Contains supplier information.

| Column | Description |
|---|---|
| Supplier_ID | Unique supplier identifier |
| Supplier_Name | Synthetic supplier name |
| Lead_Time_Days | Simulated supplier lead time in calendar days |
| Supplier_Reliability | Simulated supplier reliability classification: High, Medium or Low |

### 3.3 Product_Supplier_Mapping.csv

Maps each SKU to its preferred supplier.

| Column | Description |
|---|---|
| SKU_ID | Unique product identifier |
| Supplier_ID | Assigned preferred supplier |
| Supplier_Role | Supplier's role for the SKU; all records are Preferred |

Each SKU has one preferred supplier, while each supplier can supply multiple SKUs.

### 3.4 Daily_Sales.csv

Contains one record per SKU per calendar day for the two-year historical period.

| Column | Description |
|---|---|
| Date | Calendar date |
| SKU_ID | Unique product identifier |
| Category | Product category |
| Year | Calendar year |
| Opening_Inventory | Inventory available before that day's receipts |
| Units_Received | Quantity received on that day |
| Units_Sold | Simulated actual units sold, limited by available inventory |
| Closing_Inventory | Inventory remaining after receipts and sales |
| In_Stock_Flag | Indicates whether inventory was available at any point when the day's sales began |

The dataset includes zero-sales days, including days when a product was available but had no demand.

### 3.5 Purchase_Orders.csv

Contains purchase order information, including receipt reconciliation.

| Column | Description |
|---|---|
| PO_ID | Unique purchase order identifier |
| SKU_ID | Product identifier |
| Supplier_ID | Supplier assigned to the purchase order |
| PO_Date | Date the purchase order was placed |
| Order_Quantity | Total quantity ordered |
| Expected_Delivery_Date | Expected delivery date based on supplier lead time |
| Total_Received_Quantity | Total quantity received against the purchase order |
| First_Receipt_Date | Date of the first receipt, where applicable |
| Last_Receipt_Date | Date of the last receipt, where applicable |
| Receipt_Count | Number of receipt records against the purchase order |
| Outstanding_Quantity | Ordered quantity not yet received |
| PO_Status | Closed if fully received; otherwise Open |

### 3.6 Purchase_Order_Receipts.csv

Contains individual receipt transactions.

| Column | Description |
|---|---|
| Receipt_ID | Unique receipt identifier |
| PO_ID | Related purchase order |
| SKU_ID | Product identifier |
| Receipt_Date | Date of receipt |
| Received_Quantity | Quantity received in that transaction |

A purchase order can have multiple receipt records when deliveries are made in parts.

## 4. Data Generation Assumptions

- The product catalogue contains 1,100 SKUs across Electronics, Home & Kitchen, Clothing and Beauty.
- Each SKU is assigned one of five simulated demand patterns: Stable, Seasonal, Growing, Declining or Intermittent.
- Historical sales cover 731 calendar days from January 2024 through December 2025.
- Daily sales records include zero-sales days.
- Demand patterns include simulated seasonal increases, gradual growth or decline, and intermittent demand.
- Ten suppliers are used, with simulated lead times between 7 and 35 calendar days.
- Purchase orders are generated monthly, with order quantities based on 25–40 days of baseline demand.
- Deliveries can be complete, partial, delayed or missing.
- Opening inventory is simulated as 15–30 days of baseline demand for each SKU.

## 5. Inventory Availability and Sales Assumptions

Historical demand and actual sales are treated separately during data generation.

Potential demand is generated internally and then constrained by available inventory to calculate simulated actual sales. Potential demand is not included in the client-facing Daily_Sales.csv file.

The following rules are applied:

1. **Receipts:** All quantities received on a given date are assumed to become available before sales for that day.
2. **Actual sales:** Units sold cannot exceed the inventory available for that day.
3. **In-stock days:** A day is classified as in-stock if inventory is available at the start of the sales period, including stock received that day.
4. **Partial-day stockouts:** If a SKU runs out of stock during the day, the day is still classified as in-stock.
5. **Zero-sales days:** A zero-sales day is treated as zero demand when the SKU is available throughout the day.
6. **Out-of-stock days:** Days with no inventory available throughout the sales period are classified as out-of-stock.

### Limitation

The dataset has daily rather than hourly inventory information. It cannot identify the exact time a product became unavailable or estimate sales lost during the unavailable part of a day.

Consequently, even an in-stock day may have some unfulfilled demand if the SKU sells out during that day. This can lead to an underestimation of actual customer demand.

For demand analysis, average daily demand should therefore be calculated using in-stock days rather than all calendar days, while recognising this limitation.

## 6. Data Quality and Reconciliation

The following checks were completed during data generation:

- All 1,100 SKUs have one preferred supplier.
- Daily sales contain 804,100 records, representing 1,100 SKUs across 731 days.
- Daily inventory balances reconcile: opening inventory plus receipts minus actual sales equals closing inventory.
- Inventory balances are continuous between consecutive days for each SKU.
- Purchase order quantities reconcile with received and outstanding quantities.
- No purchase order has a negative outstanding quantity.
- Total year-end inventory reconciles to opening inventory plus receipts minus actual sales.

## 7. Important Notes

- All data is synthetic and intended for portfolio demonstration.
- Product names and supplier names are placeholders.
- Prices, demand, inventory, supplier reliability and delivery behaviour are simulated.
- The purchasing process used to generate historical purchase orders is simplified and is not itself a forecasting-based purchasing recommendation.
- Forecasting and purchasing recommendations will be developed separately during the analysis phase.