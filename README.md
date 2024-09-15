## Inventory Management Report

### Business Task

The business request for this data analyst project was an executive warehouse report for sales team, warehouse team and executive team to understand and navigate company's inventory. Below are variables for the project: 

Inventory Costs: Report total inventory value and recognise any risks to business.

Turnover: Navigate stock ins and outs to action re-ordering.

Sales to Inventory: Understanding sales to inventory correlation and maintain the ratio as per business policies.

### Data Cleaning and Transformation

To create data model neccessary tables were cleaned and transformed with SQL.

Below are SQL queries for data cleaning and transforming.

#### Date Table

```--Data Cleaning for Date Table
SELECT [DateKey]
      ,[FullDateAlternateKey] AS Date
      --,[DayNumberofWeek]
      ,[EnglishDayNameOfWeek] AS Day
      --,[SpanishDayNameOfWeek]
      --,[FrenchDayNameOfWeek]
      --,[DayNumberOfMonth]
      --,[DayNumberOfYear]
      --,[WeekNumberOfYear]
      ,[EnglishMonthName] AS MonthName
	  ,LEFT ([EnglishMonthName], 3) AS MonthShort
      --,[SpanishMonthName]
      --,[FrenchMonthName]
      ,[MonthNumberOfYear] AS MonthNumber
      ,[CalendarQuarter] AS Quater
      ,[CalendarYear] AS Year
      --,[CalendarSemester]
      --,[FiscalQuarter]
      --,[FiscalYear]
      --,[FiscalSemester]
  FROM [AdventureWorksDW2022].[dbo].[DimDate]
  WHERE [CalendarYear] >= 2021
```

#### Products Table

```-- Data Cleaning for Products Table
SELECT 
	 A.[ProductKey]
	,A.[ProductAlternateKey] AS ItemCode
--	,[ProductSubcategoryKey]
--	,[WeightUnitMeasureCode]
--	,[SizeUnitMeasureCode]
	,A.[EnglishProductName] AS ProductName
	,C.[EnglishProductSubcategoryName] AS ProductSubCategory
 	,B.[EnglishProductCategoryName] AS ProductCategory
--	,[SpanishProductName]
--	,[FrenchProductName]
--	,A.[StandardCost]
--	,[FinishedGoodsFlag]
	,A.[Color]
--	,[SafetyStockLevel]
--	,A.[ReorderPoint]
--	,[ListPrice]
--	,[Size]
--	,[SizeRange]
--	,[Weight]
--	,[DaysToManufacture]
--	,[ProductLine]
--	,[DealerPrice]
--	,[Class]
--	,[Style]
	,A.[ModelName]
--	,[LargePhoto]
	,A.[EnglishDescription] AS Description
--	,[FrenchDescription]
--	,[ChineseDescription]
--	,[ArabicDescription]
--	,[HebrewDescription]
--	,[ThaiDescription]
--	,[GermanDescription]
--	,[JapaneseDescription]
--	,[TurkishDescription]
	,Cast ([StartDate] AS Date) AS StartDate
	,Cast ([EndDate] AS Date) AS EndDate
	,ISNULL ([Status], 'Terminate') AS Status
FROM
	[AdventureWorksDW2022].[dbo].[DimProduct] AS A
LEFT JOIN
	[AdventureWorksDW2022].[dbo].[DimProductSubCategory] AS C ON C.[ProductSubCategoryKey] = A.[ProductSubcategoryKey]
LEFT JOIN
	[AdventureWorksDW2022].[dbo].[DimProductCategory] AS B ON C.[ProductCategoryKey] = B.[ProductCategoryKey]
```

### Sales Table (Fact Table)

```-- Data Cleaning For Sales Table
SELECT [ProductKey]
      ,[OrderDateKey]
      ,[DueDateKey]
      ,[ShipDateKey]
      ,[CustomerKey]
      --,[PromotionKey]
      ,[CurrencyKey]
      ,[SalesTerritoryKey]
      ,[SalesOrderNumber]
      --,[SalesOrderLineNumber]
      --,[RevisionNumber]
      ,[OrderQuantity]
      ,[UnitPrice]
      --,[ExtendedAmount]
      --,[UnitPriceDiscountPct]
      --,[DiscountAmount]
      --,[ProductStandardCost]
      ,[TotalProductCost] AS ProductCost
      ,[SalesAmount]
      ,[TaxAmt]
      ,[Freight]
      --,[CarrierTrackingNumber]
      --,[CustomerPONumber]
      ,CAST ([OrderDate] AS Date) AS OrderDate
      ,CAST ([DueDate] AS Date) AS DueDate
      ,CAST ([ShipDate] AS Date) AS ShipDate
  FROM [AdventureWorksDW2022].[dbo].[FactInternetSales]
WHERE LEFT ([OrderDateKey], 4) >= 2021
```

### Inventory Table (Fact Table)

```SELECT [ProductKey]
      ,[DateKey]
      ,[MovementDate]
      ,[UnitCost]
      ,[UnitsIn]
      ,[UnitsOut]
      ,[UnitsBalance]
  FROM [AdventureWorksDW2022].[dbo].[FactProductInventory]
  WHERE [DateKey] >= 20210101
```

### Power BI Measures

#### Stock Turnover

```
DIVIDE([Total Sales], [Average Inventory])
```
#### Sales to Stock

```
Divide([Total Quantiies Sold], [Total Sales]) * 100
```
#### Average Inventory
```
AVERAGEX('Stock',[UnitsBalance])
```
#### Avg 30d Orders
```
[Total Units Ordered Per Product] / 30
```

### Data Model

![Data Model](https://github.com/user-attachments/assets/f7394a46-859e-4dec-b0ee-a2dfca425dfa)

![Data Model Screenshot  1](https://github.com/user-attachments/assets/64655ad5-cff2-42c2-8f77-90c2ed9b2421)

### Power BI Report

To further broswe the report please click the screenshot

[![image](https://github.com/user-attachments/assets/672f151a-5a0d-477f-8b52-c819fb869860)](https://app.powerbi.com/view?r=eyJrIjoiMzM4NmM4ZWMtZjI3NS00MzQxLTgwYmQtZmEzZGYzYTYwNWMxIiwidCI6IjgyYTIxMTg1LTQyMGUtNDFlOS05OGQ5LTM4YzQ2YzY1ZWZjYyJ9)

