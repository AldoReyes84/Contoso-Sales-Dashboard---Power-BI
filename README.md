# Contoso-Sales-Dashboard---Power-BI
Contoso Sales Dashboard - Power BI

Contoso Sales Sample for Power BI data Model 

<img width="550" height="393" alt="image" src="https://github.com/user-attachments/assets/090237de-2860-467d-807c-d9399e9ba881" />

At first glance, one might assume that SalesAmount equals UnitPrice × SalesQuantity.

  Check SalesAmount = CALCULATE(SUM('Product'[UnitPrice])*SUM(Sales[SalesQuantity]))

However, when testing this logic, the result exceeds the actual SalesAmount, suggesting that discounts are already factored in. This implies that SalesAmount represents net revenue after applying DiscountAmount.

  SalesAmount Cal = [Check SalesAmount]-Sum(Sales[DiscountAmount])

<img width="880" height="247" alt="image" src="https://github.com/user-attachments/assets/bab0161c-eac4-491d-8fcd-694777031289" />

## Excel Metrics Analysis

<img width="452" height="620" alt="image" src="https://github.com/user-attachments/assets/bf6ecf70-1e95-419d-87e6-50a3c28a3b66" />


# 📘 Metrics Glossary – Contoso Sales Dashboard

This glossary defines the key metrics used in the Contoso Sales analysis. Each entry includes a description, a suggested formula, and a DAX implementation example for Power BI.

## 🧮 Base Metrics

| Metric              | Description                                                                 | Suggested Formula               | DAX Example |
|---------------------|-----------------------------------------------------------------------------|----------------------------------|-------------|
| **UnitPrice**        | Unit price of the product. Taken from the `Product` table for consistency. | `Product[UnitPrice]`             | `UnitPrice = SELECTEDVALUE(Product[UnitPrice])` |
| **UnitCost**         | Unit cost of the product. Taken from the `Product` table.                  | `Product[UnitCost]`              | `UnitCost = SELECTEDVALUE(Product[UnitCost])` |
| **SalesQuantity**    | Quantity of products sold.                                                 | `Sales[SalesQuantity]`           | `SalesQuantity = SUM(Sales[SalesQuantity])` |
| **ReturnQuantity**   | Quantity of products returned.                                             | `Sales[ReturnQuantity]`          | `ReturnQuantity = SUM(Sales[ReturnQuantity])` |
| **DiscountAmount**   | Total discount amount applied.                                             | `Sales[DiscountAmount]`          | `DiscountAmount = SUM(Sales[DiscountAmount])` |
| **DiscountQuantity** | Quantity of products sold with discount.                                  | `Sales[DiscountQuantity]`        | `DiscountQuantity = SUM(Sales[DiscountQuantity])` |

## 💰 Revenue and Cost Metrics

| Metric         | Description                                                                 | Suggested Formula                                  | DAX Example |
|----------------|-----------------------------------------------------------------------------|----------------------------------------------------|-------------|
| **NetSales**     | Gross revenue before discounts.                                            | `UnitPrice × SalesQuantity`                        | `NetSales = [UnitPrice] * [SalesQuantity]` |
| **SalesAmount**  | Net revenue after discounts.                                               | `NetSales - DiscountAmount`                        | `SalesAmount = [NetSales] - [DiscountAmount]` |
| **ReturnAmount** | Monetary value of returned products.                                       | `UnitPrice × ReturnQuantity`                       | `ReturnAmount = [UnitPrice] * [ReturnQuantity]` |
| **TotalCost**    | Total cost of sold products (excluding returns).                           | `(SalesQuantity - ReturnQuantity) × UnitCost`      | `TotalCost = ([SalesQuantity] - [ReturnQuantity]) * [UnitCost]` |

## 📊 Profitability Metrics

| Metric           | Description                                                               | Suggested Formula                                  | DAX Example |
|------------------|---------------------------------------------------------------------------|----------------------------------------------------|-------------|
| **GrossProfit**     | Gross profit before returns.                                              | `SalesAmount - TotalCost`                          | `GrossProfit = [SalesAmount] - [TotalCost]` |
| **NetProfit**       | Net profit after returns.                                                 | `SalesAmount - TotalCost - ReturnAmount`           | `NetProfit = [SalesAmount] - [TotalCost] - [ReturnAmount]` |
| **GrossMargin %**   | Gross margin as a percentage of sales.                                    | `GrossProfit / SalesAmount`                        | `GrossMargin % = DIVIDE([GrossProfit], [SalesAmount])` |
| **NetMargin %**     | Net margin as a percentage of sales.                                      | `NetProfit / SalesAmount`                          | `NetMargin % = DIVIDE([NetProfit], [SalesAmount])` |

## 📉 Discount and Return Metrics

| Metric             | Description                                                              | Suggested Formula                                  | DAX Example |
|--------------------|--------------------------------------------------------------------------|----------------------------------------------------|-------------|
| **DiscountRate %**   | Discount percentage applied over NetSales.                               | `DiscountAmount / NetSales`                        | `DiscountRate % = DIVIDE([DiscountAmount], [NetSales])` |
| **ReturnRate %**     | Return percentage over quantity sold.                                    | `ReturnQuantity / SalesQuantity`                   | `ReturnRate % = DIVIDE([ReturnQuantity], [SalesQuantity])` |

---

> **Technical Notes**  
> - Metrics are calculated using data from the `Product` and `Sales` tables.  
> - Ensure consistency between `UnitPrice` and `UnitCost` across tables.  
> - Formulas are adaptable to DAX, SQL, or other BI environments.
