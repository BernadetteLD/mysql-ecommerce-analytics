**Exploratory Data Analysis**

After cleaning the dataset, I performed Exploratory Data Analysis (EDA) to gain insights into sales,
customers, products, and overall business performance. The goal of EDA was to identify patterns, trends,
and actionable insights that can support business decisions, marketing strategies, and inventory management. 
Each query was designed to provide a clear view of the data, and the results are illustrated with tables or visual
outputs wherever relevant.

**1.** How many invoices and customers are in the dataset? This tells us the dataset size and how many distinct customers are placing orders.

SELECT 
    COUNT(DISTINCT InvoiceNo) AS total_invoices,
    COUNT(DISTINCT CustomerID) AS total_customers
FROM ecommerce_analytics.online_retail_dataset;

<img width="157" height="36" alt="image" src="https://github.com/user-attachments/assets/3f4ac0db-fb44-4f31-9ffe-f5eed3e20652" />

The dataset contains 69 invoices from 51 distinct customers, which means some customers placed multiple orders.

**2.** Total number of items sold and total revenue. Gives a sense of overall sales volume and business revenue.

SELECT 
    SUM(Quantity) AS total_items_sold,
    SUM(Quantity * UnitPrice) AS total_revenue
FROM ecommerce_analytics.online_retail_dataset;

<img width="188" height="33" alt="image" src="https://github.com/user-attachments/assets/3e79dc00-88a1-47f4-b3b8-8ce233ddc6f6" />

Across all invoices, 12,774 items were sold, generating a total revenue of 24,659.13. This gives an idea of the scale of business activity in the dataset.

**3.** Top-selling products. This highlights the most popular products and helps understand customer preferences.

SELECT Description, SUM(Quantity) AS total_sold
FROM ecommerce_analytics.online_retail_dataset
GROUP BY Description
ORDER BY total_sold DESC
LIMIT 10;

<img width="296" height="172" alt="image" src="https://github.com/user-attachments/assets/7199c426-119f-4da6-ab1f-aeb2a77e4e81" />

These products are the most frequently purchased and could be prioritized in inventory, marketing, or promotion strategies.

**4.** Top customers by revenue. Identifies the biggest contributors to revenue, useful for loyalty or marketing strategies

SELECT CustomerID, SUM(Quantity * UnitPrice) AS revenue
FROM ecommerce_analytics.online_retail_dataset
GROUP BY CustomerID
ORDER BY revenue DESC
LIMIT 10;

<img width="143" height="171" alt="image" src="https://github.com/user-attachments/assets/edc26665-437b-47ce-bceb-f28efc7e86b3" />


The top 10 customers account for a significant portion of revenue. This insight helps the business focus on high-value customers and tailor marketing or promotional strategies accordingly.

**5.** Revenue and total items by country.Shows geographical distribution of sales — helps focus on key markets.

SELECT Country, SUM(Quantity * UnitPrice) AS revenue, SUM(Quantity) AS total_items
FROM ecommerce_analytics.online_retail_dataset
GROUP BY Country
ORDER BY revenue DESC;

<img width="242" height="73" alt="image" src="https://github.com/user-attachments/assets/f1d5780e-87c0-409d-89fd-483db2949505" />

The United Kingdom dominates sales, followed by France and Australia. This insight highlights key markets and helps assess where business efforts should be focused.

**6.** Number of cancelled invoices.Provides insight into order cancellations and potential issues with products or processes.

SELECT COUNT(*) AS cancelled_invoices
FROM ecommerce_analytics.online_retail_dataset
WHERE InvoiceNo LIKE 'C%';

<img width="104" height="40" alt="image" src="https://github.com/user-attachments/assets/5f8441d7-a4d4-4dae-9de6-2944545afba7" />

The total number of cancelled orders are 10. I further am interested to know, which iteams where cancelled.

SELECT CustomerID, InvoiceNo, InvoiceDate, Description, Quantity, UnitPrice, Country
FROM ecommerce_analytics.online_retail_dataset
WHERE InvoiceNo LIKE 'C%';

<img width="617" height="169" alt="image" src="https://github.com/user-attachments/assets/e0e8310b-1fd4-47ad-bcbd-eafb8ab93a84" />

Cancelled invoices represent returns, order corrections, or discounts in the dataset. Negative quantities indicate that items were reversed from the original sale. Multiple cancellations by the same customer or across products are normal in real-world retail data and help the business monitor returns, customer behavior, and potential issues in sales or fulfillment.

