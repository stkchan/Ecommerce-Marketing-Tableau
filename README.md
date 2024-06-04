# Ecommerce-Marketing-Tableau

![image](https://github.com/stkchan/Ecommerce-Marketing-Tableau/blob/main/Munchys%20Pet%20Supply%20Executive%20Dashboard%20(1).png)

## Table of Contents
* [Project Link](#Project-Link)
* [Project Objective](#Project-Objective)
* [Data Connection in Tableau](#Data-Connection-in-Tableau)
* [Calculation Field](#Calculation-Field)


## Project Link
See on Tableau Public: [Here](https://public.tableau.com/app/profile/anupong.mueangngam/viz/TableuaMarketingAnalysis/MunchysPetSupplyExecutiveDashboard)

## Project Objective
In this project, we aim to find insights from the Ecommerce Pet Munchies dataset. The primary objective is visualize data in Tableau to generate insights based on defined goals.

## Data Connection in Tableau
In this project, we have two data sources.
- In Product Sales, 
  - In sales and customer table,               Joining data by using **Customer ID** 
  - In sales and product table,                Joining data by using **Stock Code**
  - In sales and state_region_mappping table,  Joining data by using **Order State**
 
    
![image](https://github.com/stkchan/Ecommerce-Marketing-Tableau/blob/main/1.png)

- In Market Basket, 
  - First condition  = Invoice No
  - Second condition = Stock Code
  
 
![image](https://github.com/stkchan/Ecommerce-Marketing-Tableau/blob/main/1.1.png)


## Calculation Field

**1.Number of Customers**
```DAX
COUNTD([Customer ID])
```

**2.Customer LTV (avg)**
```DAX
AVG({FIXED [Customer ID]: SUM([Sales])})
```

**3.COGS**
```DAX
[Landed Cost] * [Quantity]
```

**4.Blended Shipping Cost Factor**
```DAX
IF ([What if Quantity])     <= 1 THEN 1
ELSEIF ([What if Quantity]) <=2 THEN 0.8
ELSEIF ([What if Quantity]) <=4 THEN 0.6
ELSEIF ([What if Quantity]) <=7 THEN 0.5
ELSEIF ([What if Quantity]) <=9 THEN 0.4
ELSE 0.3
END
```

**5.Shipping (Baseline)**
```DAX
IF [Quantity] = 1 THEN [Shipping Cost 1000 mile]
ELSE [Shipping Cost 1000 mile] + (([Quantity]-1) * [Shipping Cost 1000 mile] * 0.7)
END
```

**6.Shipping (What-if)**
```DAX
IF [Quantity] <= 1 THEN [Shipping Cost 1000 mile]
ELSE [Shipping Cost 1000 mile] + (([Quantity] - 1) * [Shipping Cost 1000 mile] * [Blended Shipping Cost Factor])
END
```

**7.Shipping (Difference Baseline)**
```DAX
[Shipping (Baseline)] - [Shipping (What-if)]
```

**8.Shipping (per unit)**
```DAX
[Shipping Cost 1000 mile] * [Quantity]
```

**9.Profit %**
```DAX
SUM([Profit (Baseline)]) / SUM([Sales])
```

**10.Warehouse-LAX**
```DAX
MAKEPOINT(34.0544, -118.2439)
```

**10.Distance-LAX**
```DAX
AVG(DISTANCE([Warehouse-LAX], [Destination], "mi"))
```
