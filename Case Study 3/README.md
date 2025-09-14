# 🎨 Art Gallery Sales and Inventory Database
Author: **JOHN REY ORTIGAS BSCS 3B**

📚 **Table of Contents**

Business Task

This database manages art gallery operations, including artists, artworks, customers, and purchases.  

Main tasks supported:

1. Maintain artist information (ID, name).  
2. Store artwork details (art number, title, artist).  
3. Manage customer records (ID, first name, last name).  
4. Track purchases:  
   - Customer who bought the artwork.  
   - Artwork purchased.  
   - Purchase date.  
5. Provide analytical queries:  
   - Count customers buying specific artworks.  
   - Retrieve customer purchase counts.  
   - Display purchases with artwork and artist details.  
   - Show full purchase history per customer.  

***

## Entity Relationship Diagram
[Link]

Database Schema: https://www.db-fiddle.com/f/cGaPb64KViAMYrEmHt5sCi/5

***

## Question and Solution

### **1. Number of Customers Who Bought "The Last Supper"**

````sql
SELECT COUNT(DISTINCT custID) AS Number_of_Customers 
FROM purchase
WHERE artNo = '01-1498-02';
````
#### Steps
 - Filter the purchase table for artwork 01-1498-02 (The Last Supper).
 - Count distinct custID values. 

#### 📊 Answer
| Number_of_Customers |
| ------------------- |
| 4                   |

👉 *Four customers purchased "The Last Supper".*

***
### **2. Customer Purchase Count**

````sql
SELECT CONCAT(cust_Fname, ' ', cust_Lname) AS Customer_Name, COUNT(artNo) AS Number_of_Artworks
FROM customer
JOIN purchase ON customer.custID = purchase.custID
GROUP BY customer.custID, cust_Fname, cust_Lname;
````
#### 🔎 Steps
 - Join customer and purchase tables on custID.
 - Count artworks per customer.

#### 📊 Answer
| Customer_Name   | Number_of_Artworks |
| --------------- | ------------------ |
| Ryan Leblanc    | 2                  |
| David Varga     | 4                  |
| Matthew Tomas   | 1                  |
| Annie Brown     | 4                  |
| Nathan Burberry | 1                  |
| Melissa Erin    | 2                  |
| Sarah George    | 1                  |

👉 *Shows how many artworks each customer purchased.*

***

### **3. Detailed Purchase Info (Customer, Artwork, Artist)**

````sql
SELECT CONCAT(cust_Fname, ' ', cust_Lname) AS Customer_Name, art_work.title AS Artwork_Title, artist.name AS Artist_Name
FROM purchase
INNER JOIN customer ON purchase.custID = customer.custID
INNER JOIN art_work ON purchase.artNo = art_work.artNo
INNER JOIN artist ON art_work.artistID = artist.artistID;

````
#### 🔎 Steps
 - Join purchase, customer, art_work, and artist tables.
 - Retrieve customer, artwork, and artist details.

#### 📊 Answer
| Customer_Name   | Artwork_Title        | Artist_Name       |
| --------------- | -------------------- | ----------------- |
| Ryan Leblanc    | Café Terrace         | Vincent van Gogh  |
| Ryan Leblanc    | The Kiss             | Gustav Klimt      |
| David Varga     | Mona Lisa            | Leonardo da Vinci |
| David Varga     | Café Terrace         | Vincent van Gogh  |
| David Varga     | The Last Supper      | Leonardo da Vinci |
| David Varga     | The Creation of Adam | Michelangelo      |
| Matthew Tomas   | The Last Supper      | Leonardo da Vinci |
| Annie Brown     | The Last Supper      | Leonardo da Vinci |
| Annie Brown     | Mona Lisa            | Leonardo da Vinci |
| Annie Brown     | Salvator Mundi       | Leonardo da Vinci |
| Annie Brown     | The Birth of Venus   | Sandro Botticelli |
| Nathan Burberry | Mona Lisa            | Leonardo da Vinci |
| Melissa Erin    | Café Terrace         | Vincent van Gogh  |
| Melissa Erin    | The Night Watch      | Rembrandt         |
| Sarah George    | The Last Supper      | Leonardo da Vinci |


👉 *Provides detailed purchase information for all customers.*

***


### **4. Complete Purchase History**

````sql
SELECT 
    purchase.orderNo AS Order_Number,
    CONCAT(customer.cust_Fname, ' ', customer.cust_Lname) AS Customer_Name,
    art_work.title AS Artwork_Title,
    artist.name AS Artist_Name,
    purchase.purchDate AS Date_Purchased
FROM purchase
NATURAL JOIN customer
NATURAL JOIN art_work
NATURAL JOIN artist
ORDER BY Customer_Name ASC;
````
#### 🔎 Steps
 - Use NATURAL JOIN across all tables (purchase, customer, art_work, artist).
 - Retrieve order number, customer, artwork, artist, and purchase date.
 - Order results by customer name.

#### 📊 Answer
| Order_Number | Customer_Name   | Artwork_Title        | Artist_Name       | Date_Purchased |
| ------------ | --------------- | -------------------- | ----------------- | -------------- |
| OR-01        | Annie Brown     | The Last Supper      | Leonardo da Vinci | 2020-02-24     |
| OR-07        | Annie Brown     | Mona Lisa            | Leonardo da Vinci | 2021-08-18     |
| OR-12        | Annie Brown     | Salvator Mundi       | Leonardo da Vinci | 2022-10-22     |
| OR-15        | Annie Brown     | The Birth of Venus   | Sandro Botticelli | 2023-06-10     |
| OR-02        | David Varga     | Mona Lisa            | Leonardo da Vinci | 2020-03-02     |
| OR-09        | David Varga     | Café Terrace         | Vincent van Gogh  | 2022-01-24     |
| OR-10        | David Varga     | The Last Supper      | Leonardo da Vinci | 2022-01-24     |
| OR-14        | David Varga     | The Creation of Adam | Michelangelo      | 2023-03-14     |
| OR-06        | Matthew Tomas   | The Last Supper      | Leonardo da Vinci | 2021-01-20     |
| OR-03        | Melissa Erin    | Café Terrace         | Vincent van Gogh  | 2020-03-02     |
| OR-11        | Melissa Erin    | The Night Watch      | Rembrandt         | 2022-05-11     |
| OR-05        | Nathan Burberry | Mona Lisa            | Leonardo da Vinci | 2020-12-21     |
| OR-08        | Ryan Leblanc    | Café Terrace         | Vincent van Gogh  | 2021-12-18     |
| OR-13        | Ryan Leblanc    | The Kiss             | Gustav Klimt      | 2022-11-06     |
| OR-04        | Sarah George    | The Last Supper      | Leonardo da Vinci | 2020-10-17     |




👉 *Gives a complete purchase history with customer names, artworks, artists, and purchase dates.*

