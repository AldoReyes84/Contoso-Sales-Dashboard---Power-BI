# Contoso Sales PowerBI Dashboard 


Source Model, Contoso Sales Sample for Power BI data Model 

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

## 🧮 Base Metrics (Explicit Measures)

This section defines the foundational metrics used in the Contoso Sales Dashboard. All metrics are implemented as explicit DAX measures to ensure compatibility with Calculation Groups and maintain clarity across visuals.

| Metric              | Description                                                                 | Suggested Formula               | DAX Example |
|---------------------|-----------------------------------------------------------------------------|----------------------------------|-------------|
| **UnitPrice**        | Unit price of the product. Retrieved from the `Product` table for consistency. | `(Product[UnitPrice])` | `UnitPrice = SUM('Product'[UnitPrice])` |
| **UnitCost**         | Unit cost of the product. Retrieved from the `Product` table.              | `(Product[UnitCost])` | `UnitCost = SUM(Product[UnitCost])` |
| **SalesQuantity**    | Total quantity of products sold. Defined as an explicit measure.           | `SUM(Sales[SalesQuantity])`      | `SalesQuantity = SUM(Sales[SalesQuantity])` |
| **ReturnQuantity**   | Total quantity of products returned.                                       | `SUM(Sales[ReturnQuantity])`     | `ReturnQuantity = SUM(Sales[ReturnQuantity])` |
| **DiscountAmount**   | Total discount amount applied to sales.                                    | `SUM(Sales[DiscountAmount])`     | `DiscountAmount = SUM(Sales[DiscountAmount])` |
| **DiscountQuantity** | Quantity of products sold with discount applied.                          | `SUM(Sales[DiscountQuantity])`   | `DiscountQuantity = SUM(Sales[DiscountQuantity])` |

---

### 🧠 Notes

- All metrics above are defined as **explicit DAX measures** to ensure compatibility with Calculation Groups and advanced Time Intelligence logic.
- Avoid using raw columns directly in visuals or calculations when dynamic logic (e.g., YTD, YoY) is required.
- These measures serve as the foundation for derived metrics such as revenue, cost, and profitability.

## 💰 Revenue and Cost Metrics

| Metric         | Description                                                                 | Suggested Formula                                  | DAX Example |
|----------------|-----------------------------------------------------------------------------|----------------------------------------------------|-------------|
| **NetSales**     | Gross revenue before discounts.                                            | `UnitPrice × SalesQuantity`                        | `NetSales = [SalesAmount]+[DiscountAmount]` |
| **SalesAmount**  | Net revenue after discounts.                                               | `SUM(Sales[SalesAmount])`                      | `SalesAmount = SUM(Sales[SalesAmount]` |
| **ReturnAmount** | Monetary value of returned products.                                       | `SUM(Sales[ReturnAmount]`                       | `ReturnAmount = SUM(Sales[DiscountAmount]` |
| **TotalCost**    | Total cost of sold products (excluding returns).                           | `SUM(Sales[TotalCost]`      | `TotalCost = (SUM(Sales[TotalCost]` |

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


## ⏱️ Time Intelligence Calculation Group

This section defines reusable time-based transformations using Calculation Groups in Power BI. These allow dynamic application of logic (YTD, MTD, YoY, etc.) to any measure using `SELECTEDMEASURE()`.

| Calculation Item     | Description                                               | Suggested Formula                                  | DAX Example |
|----------------------|-----------------------------------------------------------|----------------------------------------------------|-------------|
| YTD                  | Year-to-date total from Jan 1 to current date.            | `TOTALYTD(SELECTEDMEASURE(), 'Date'[Date])`        | `YTD = TOTALYTD(SELECTEDMEASURE(), 'Calendar'[DateKey])` |
| MTD                  | Month-to-date total from 1st of month to current date.    | `TOTALMTD(SELECTEDMEASURE(), 'Date'[Date])`        | `MTD = TOTALMTD(SELECTEDMEASURE(), 'Calendar'[DateKey])` |
| QTD                  | Quarter-to-date total from start of quarter to today.     | `TOTALQTD(SELECTEDMEASURE(), 'Date'[Date])`        | `QTD = TOTALQTD(SELECTEDMEASURE(), 'Calendar'[DateKey])` |
| YoY                  | Same period last year.                                    | `CALCULATE(SELECTEDMEASURE(), SAMEPERIODLASTYEAR('Date'[Date]))` | `YoY = CALCULATE(SELECTEDMEASURE(), SAMEPERIODLASTYEAR('Calendar'[DateKey]))` |
| Previous Month       | Same period in previous month.                            | `CALCULATE(SELECTEDMEASURE(), PREVIOUSMONTH('Date'[Date]))` | `PreviousMonth = CALCULATE(SELECTEDMEASURE(), PREVIOUSMONTH('Calendar'[DateKey]))` |
| YoY % Change         | Year-over-year percentage change.                         | `(Current - LastYear) / LastYear`                  | `YoY % = DIVIDE(SELECTEDMEASURE() - [YoY], [YoY])` |
| MoM % Change         | Month-over-month percentage change.                       | `(Current - PreviousMonth) / PreviousMonth`        | `MoM % = DIVIDE(SELECTEDMEASURE() - [PreviousMonth], [PreviousMonth])` |

---

> **Technical Notes**  
> - Metrics are calculated using data from the `Product` and `Sales` tables.  
> - Ensure consistency between `UnitPrice` and `UnitCost` across tables.  
> - Formulas are adaptable to DAX, SQL, or other BI environments.


