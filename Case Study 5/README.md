# 🥐 Bakeshop Inventory, Sales, and Transaction Management  
Author: **JOHN REY ORTIGAS BSCS 3B**

📚 **Table of Contents**

Business Task

This database manages **bakeshop operations**, covering products, suppliers, customers, employees, and financial tracking.  

Main tasks supported:

1. Maintain bakeshop branches (ID, name, address).  
2. Manage categories (cakes, bread, beverages, etc.).  
3. Track suppliers and their products.  
4. Monitor product details: stock, price, expiration, and cost.  
5. Store customer and order data.  
6. Record employee roles and assignments.  
7. Handle payments, invoices, and order details.  
8. Generate business reports:  
   - Total sales & revenue  
   - Profit margins  
   - Sales by product category / supplier  
   - Inventory and low-stock alerts  
   - Employee performance  
   - Expired products  
   - Customer purchase patterns  

---

## Entity Relationship Diagram
[Link]

Database Schema: https://www.db-fiddle.com/f/ozNkHxaJKsRhBY67ye72Rc/7

---

***
## Question and Solution

### **1. Total Sales for a Specific Date Range**

````sql
SELECT 
    o.OrderDate AS SaleDate,
    SUM(p.AmountPaid) AS TotalSales,
    COUNT(DISTINCT o.OrderID) AS TotalOrders
FROM Orders as o
JOIN Payment p ON o.OrderID = p.OrderID
WHERE o.OrderDate BETWEEN '2024-06-01' AND '2024-06-30'
GROUP BY o.OrderDate
ORDER BY o.OrderDate;
````
####🔎 Steps
 - Join Orders with Payment by OrderID.
 - Filter orders between 2024-06-01 and 2024-06-30.
 - Sum payments and count distinct orders per date.

👉*Shows daily sales and number of orders within June 2024.*

***
### **1. Total Sales for a Specific Date Range**

````sql
SELECT 
    o.OrderDate AS SaleDate,
    SUM(p.AmountPaid) AS TotalSales,
    COUNT(DISTINCT o.OrderID) AS TotalOrders
FROM Orders as o
JOIN Payment p ON o.OrderID = p.OrderID
WHERE o.OrderDate BETWEEN '2024-06-01' AND '2024-06-30'
GROUP BY o.OrderDate
ORDER BY o.OrderDate;
````
####📊 Answer
| SaleDate   | TotalSales | TotalOrders |
| ---------- | ---------- | ----------- |
| 2024-06-01 | 50.75      | 1           |
| 2024-06-02 | 22.00      | 1           |

####🔎 Steps
 - Join Orders with Payment by OrderID.
 - Filter orders between 2024-06-01 and 2024-06-30.
 - Sum payments and count distinct orders per date.

👉*Shows daily sales and number of orders within June 2024.*

***

### **2. Total Revenue Report**

````sql
SELECT SUM(OrderTotalAmount) AS TotalRevenue
FROM Orders
WHERE OrderDate BETWEEN '2024-01-01' AND '2024-12-31';

````
####📊 Answer
| TotalRevenue |
| ------------ |
| 193.25       |

####🔎 Steps
 - Aggregate total revenue for all orders in 2024.
 - 
👉*Gives the overall revenue for the year 2024.*

***

### **3. Profit Margins Report**

````sql
SELECT P.ProductName, 
       SUM(OD.Subtotal) AS TotalRevenue,
       SUM(OD.Quantity * P.Price) AS TotalCost,
       (SUM(OD.Subtotal) - SUM(OD.Quantity * P.Price)) AS ProfitMargin
FROM OrderDetails as OD
JOIN Product P ON OD.ProductID = P.ProductID
GROUP BY P.ProductName
ORDER BY ProfitMargin DESC;

````
####📊 Answer
| SaleDate   | TotalSales | TotalOrders |
| ---------- | ---------- | ----------- |
| 2024-06-01 | 50.75      | 1           |
| 2024-06-02 | 22.00      | 1           |

####🔎 Steps
 - Calculate total revenue (sum of subtotals).
 - Estimate cost using product price × quantity.
 - Profit = revenue − cost.
  
👉*Ranks products by profitability.*

***

### **3. Profit Margins Report**

````sql
SELECT P.ProductName, 
       SUM(OD.Subtotal) AS TotalRevenue,
       SUM(OD.Quantity * P.Price) AS TotalCost,
       (SUM(OD.Subtotal) - SUM(OD.Quantity * P.Price)) AS ProfitMargin
FROM OrderDetails as OD
JOIN Product P ON OD.ProductID = P.ProductID
GROUP BY P.ProductName
ORDER BY ProfitMargin DESC;

````
####📊 Answer
| ProductName | TotalRevenue | TotalCost | ProfitMargin |
| ----------- | ------------ | --------- | ------------ |
| Hopia       | 45.50        | 42.00     | 3.50         |
| Ensaymada   | 30.00        | 30.00     | 0.00         |
| Coffee      | 45.00        | 50.00     | -5.00        |
| Coke        | 24.00        | 40.00     | -16.00       |
| Pandesal    | 30.00        | 60.00     | -30.00       |

####🔎 Steps
 - Calculate total revenue (sum of subtotals).
 - Estimate cost using product price × quantity.
 - Profit = revenue − cost.
  
👉*Ranks products by profitability.*

***

### **4. Sales by Product Category**

````sql
SELECT C.CategoryName, 
       SUM(OD.Quantity) AS TotalQuantitySold, 
       SUM(OD.Subtotal) AS TotalSales
FROM OrderDetails as OD
JOIN Product as P ON OD.ProductID = P.ProductID
JOIN Category as C ON P.CategoryID = C.CategoryID
GROUP BY C.CategoryName;

````
####📊 Answer
| CategoryName   | TotalQuantitySold | TotalSales |
| -------------- | ----------------- | ---------- |
| Beverages      | 4                 | 69.00      |
| Filipino Bread | 22                | 105.50     |


####🔎 Steps

 - Join OrderDetails → Product → Category.
 - Aggregate sales per category.
  
👉*Shows which product category contributes most to sales.*

***
### **5. Sales by Supplier**

````sql
SELECT S.SupplierName, 
       SUM(OD.Quantity) AS TotalQuantitySold, 
       SUM(OD.Subtotal) AS TotalSales
FROM OrderDetails as OD
JOIN Product as P ON OD.ProductID = P.ProductID
JOIN Suppliers as S ON P.SupplierID = S.SupplierID
GROUP BY S.SupplierName;

````
####📊 Answer
| SupplierName          | TotalQuantitySold | TotalSales |
| --------------------- | ----------------- | ---------- |
| Bakery Essentials Co. | 22                | 105.50     |
| Refreshments Ltd.     | 4                 | 69.00      |

####🔎 Steps

 - Join OrderDetails → Product → Supplier.
 - Summarize sales by supplier.
  
👉*Identifies top suppliers by sales volume and revenue.*

***

### **6. Inventory Monitoring**

````sql
SELECT 
    P.ProductID, 
    P.ProductName, 
    P.QuantityInStock, 
    IFNULL(SUM(OD.Quantity), 0) AS TotalSold, 
    (P.QuantityInStock - IFNULL(SUM(OD.Quantity), 0)) AS UpdatedStock
FROM Product as P
LEFT JOIN OrderDetails as OD ON P.ProductID = OD.ProductID
GROUP BY P.ProductID, P.ProductName, P.QuantityInStock;

````
####📊 Answer
| ProductID | ProductName         | QuantityInStock | TotalSold | UpdatedStock |
| --------- | ------------------- | --------------- | --------- | ------------ |
| P-003     | Pandesal            | 100             | 10        | 90           |
| P-004     | Ensaymada           | 50              | 5         | 45           |
| P-005     | Spanish Bread       | 80              | 0         | 80           |
| P-006     | Monay               | 60              | 0         | 60           |
| P-007     | Hopia               | 70              | 7         | 63           |
| P-008     | Kababayan           | 90              | 0         | 90           |
| P-009     | Cheese Roll         | 55              | 0         | 55           |
| P-010     | Ube Cheese Pandesal | 40              | 0         | 40           |
| P-011     | Taisan              | 45              | 0         | 45           |
| P-012     | Pianono             | 50              | 0         | 50           |
| P-013     | Coke                | 60              | 2         | 58           |
| P-014     | Calamansi Juice     | 50              | 0         | 50           |
| P-015     | Royal               | 40              | 0         | 40           |
| P-016     | Coffee              | 70              | 2         | 68           |
| P-017     | Mountain Dew        | 30              | 0         | 30           |

####🔎 Steps

 - Compare stock with total sold.
 - Calculate updated stock.

👉 *Tracks remaining stock after sales.*

***


### **7. Low-Stock Products**

````sql
SELECT ProductID, ProductName, QuantityInStock
FROM Product
WHERE QuantityInStock < 50;
````
####📊 Answer
| ProductID | ProductName         | QuantityInStock |
| --------- | ------------------- | --------------- |
| P-010     | Ube Cheese Pandesal | 40              |
| P-011     | Taisan              | 45              |
| P-015     | Royal               | 40              |
| P-017     | Mountain Dew        | 30              |

####🔎 🔎 Steps
 - Select products where stock is under 50 units.

👉 *Highlights items that need restocking.*

***


### **8. Employee Sales Performance**

````sql
SELECT 
    E.EmployeeName, 
    COUNT(P.PaymentID) AS TotalSalesHandled, 
    SUM(P.AmountPaid) AS TotalAmountCollected
FROM Employee as E
JOIN Payment as P ON E.EmployeeID = P.EmployeeID
GROUP BY E.EmployeeName
ORDER BY TotalAmountCollected DESC;

````
####📊 Answer
| EmployeeName | TotalSalesHandled | TotalAmountCollected |
| ------------ | ----------------- | -------------------- |
| John Doe     | 2                 | 125.75               |
| Jane Smith   | 2                 | 67.50                |

####🔎 Steps
 - Join Employee and Payment.
 - Count sales handled and sum collections.
   
👉 *Ranks employees by sales performance.*

***


### **9. Expired Products**

````sql
SELECT 
    ProductID, 
    ProductName, 
    ExpirationDate, 
    QuantityInStock
FROM Product
WHERE ExpirationDate < CURDATE();

````
####📊 Answer
| ProductID | ProductName         | ExpirationDate | QuantityInStock |
| --------- | ------------------- | -------------- | --------------- |
| P-003     | Pandesal            | 2024-12-31     | 100             |
| P-004     | Ensaymada           | 2024-12-15     | 50              |
| P-005     | Spanish Bread       | 2024-12-20     | 80              |
| P-006     | Monay               | 2024-12-25     | 60              |
| P-007     | Hopia               | 2024-12-10     | 70              |
| P-008     | Kababayan           | 2024-12-18     | 90              |
| P-009     | Cheese Roll         | 2024-12-22     | 55              |
| P-010     | Ube Cheese Pandesal | 2024-12-12     | 40              |
| P-011     | Taisan              | 2024-12-17     | 45              |
| P-012     | Pianono             | 2024-12-14     | 50              |
| P-013     | Coke                | 2024-12-30     | 60              |
| P-014     | Calamansi Juice     | 2024-12-28     | 50              |
| P-015     | Royal               | 2024-12-29     | 40              |
| P-016     | Coffee              | 2024-12-27     | 70              |
| P-017     | Mountain Dew        | 2024-12-31     | 30              |


####🔎Steps
 - Identify products past their expiration date.
   
👉 *Helps with waste management and compliance.*

***



### **10. Customer Purchase Patterns**

````sql
SELECT 
    C.CustomerName, 
    COUNT(O.OrderID) AS TotalOrders, 
    SUM(O.OrderTotalAmount) AS TotalSpent
FROM Customers as C
JOIN Orders as O ON C.CustomerID = O.CustomerID
GROUP BY C.CustomerName
ORDER BY TotalSpent DESC;

````
####📊 Answer
| CustomerName                 | TotalOrders | TotalSpent |
| ---------------------------- | ----------- | ---------- |
| Nathaniel Montenegoro Ibarra | 3           | 171.25     |
| Emanuel Buenavista Guerrero  | 1           | 22.00      |



####🔎Steps
 - Join Customers with Orders.
 - Count orders and total spending.
   
👉 *Profiles customer loyalty and spending behavior.*
