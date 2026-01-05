**Business Insights**

After completing data cleaning and exploratory analysis, I generated key business insights to 
understand sales performance, customer behavior, and geographic trends. The insights focus on 
actionable findings that can support marketing strategies, inventory management, and operational decision-making.

**1.** Top-selling products (excluding cancelled orders).Understand product performance — useful for promotions, inventory planning, and marketing focus.

SELECT Description, SUM(Quantity) AS total_sold, SUM(Quantity*UnitPrice) AS revenue
FROM online_retail_dataset
WHERE InvoiceNo NOT LIKE 'C%'
GROUP BY Description
ORDER BY total_sold DESC
LIMIT 10;

<img width="354" height="171" alt="image" src="https://github.com/user-attachments/assets/abd4f5ed-b946-49f4-8dec-5e7214c56119" />

The analysis of top-selling products (excluding cancelled orders) reveals which items drive 
the most sales and revenue. The top 10 products are led by NAMASTE SWAGAT INCENSE with 600 units 
sold, generating €144 in revenue, followed by BLACK RECORD COVER FRAME with 480 units sold (€1,627.20). 
Other popular items include decorative lights and seasonal products, such as the RED TOADSTOOL LED NIGHT 
LIGHT (463 units, €591.15) and FAIRY TALE COTTAGE NIGHTLIGHT (432 units, €626.40).

These insights help identify products with strong customer demand, guiding inventory planning,
marketing focus, and promotional strategies. For example, high-selling decorative items and household 
products could be prioritized for stock replenishment and featured in campaigns to maximize sales.

**2.** Customer revenue and segmentation
Identify top, average, and low spenders to target marketing and loyalty strategies.
High: Focus on loyalty programs, exclusive offers
Average: Encourage upselling with targeted promotions
Low: Engage with discounts or personalized marketing

SELECT CustomerID,
       SUM(Quantity * UnitPrice) AS total_revenue,
       CASE
           WHEN SUM(Quantity * UnitPrice) >= 500 THEN 'High'
           WHEN SUM(Quantity * UnitPrice) BETWEEN 200 AND 499 THEN 'Average'
           ELSE 'Low'
       END AS revenue_group
FROM online_retail_dataset
WHERE InvoiceNo NOT LIKE 'C%'
GROUP BY CustomerID
ORDER BY total_revenue DESC;

<img width="205" height="314" alt="image" src="https://github.com/user-attachments/assets/f3db4133-c533-4985-895d-1e97830fb5e9" />


Customer Revenue and Segmentation: The analysis of customer spending reveals distinct segments based on total revenue. High spenders (≥ €500) include customers
like 16029 (€3,702.12) and 16210 (€2,474.74), representing the top contributors to revenue. Average spenders (€200–€499) make
up the largest portion of the customer base, with revenues ranging from €489.60 to €204. Low spenders (< €200) account for a
smaller group, including customers with revenues as low as €17.50.

Segmenting customers in this way provides actionable insights for targeted strategies: high spenders can be engaged with 
loyalty programs and exclusive offers, average spenders can be encouraged to purchase more through upselling and 
personalized promotions, and low spenders can be nurtured with discounts or targeted marketing campaigns. This approach 
helps maximize revenue while building long-term customer loyalty.

**4.** Repeat purchases per customer
Identify products purchased multiple times by the same customer. Useful for personalized recommendations, 
bundling, loyalty campaigns, and stock management.

SELECT o1.CustomerID, o1.Description, SUM(o1.Quantity) AS total_quantity
FROM online_retail_dataset o1
GROUP BY o1.CustomerID, o1.Description
HAVING total_quantity > 1
ORDER BY total_quantity DESC;

<img width="306" height="89" alt="image" src="https://github.com/user-attachments/assets/c0859a76-a827-48e0-baf7-8f6d3186d9cb" />

Repeat Purchases per Customer

The analysis of repeat purchases shows which products are frequently bought by the same customers, 
highlighting opportunities for personalized recommendations, bundling, loyalty campaigns, and stock planning. 
In total, there were 691 instances where a customer purchased the same product more than once.

Some key observations:
High-volume repeat purchases include:
Customer 13694 bought NAMASTE SWAGAT INCENSE 600 times.
Customer 16210 bought BLACK RECORD COVER FRAME 480 times.
Customer 16029 bought FAIRY TALE COTTAGE NIGHTLIGHT 432 times.
Many other products, such as RED RETROSPOT OVEN GLOVE, HAND WARMER SCOTTY DOG DESIGN, and JUMBO BAG RED RETROSPOT,
were purchased multiple times by different customers, indicating strong repeat demand.
These patterns suggest that popular products should be considered for special promotions, bundled offers, or loyalty 
rewards, while inventory levels should be adjusted to meet recurring demand. Understanding repeat purchase behavior can 
also inform targeted marketing campaigns, encouraging customers to buy complementary products.

**5.** Revenue by country (including cancelled orders)
Shows top and bottom performing countries and the impact of cancellations.
Useful for marketing strategy, stock allocation, and operational improvements.

SELECT Country, Valid_Revenue, Cancelled_Revenue, Total_Revenue,
       RANK() OVER (ORDER BY Valid_Revenue DESC) AS Revenue_Rank
FROM (
    SELECT 
        Country,
        SUM(CASE WHEN InvoiceNo NOT LIKE 'C%' THEN Quantity*UnitPrice ELSE 0 END) AS Valid_Revenue,
        SUM(CASE WHEN InvoiceNo LIKE 'C%' THEN Quantity*UnitPrice ELSE 0 END) AS Cancelled_Revenue,
        SUM(Quantity*UnitPrice) AS Total_Revenue
    FROM online_retail_dataset
    GROUP BY Country
) AS country_revenue
ORDER BY Revenue_Rank;

<img width="449" height="112" alt="image" src="https://github.com/user-attachments/assets/ae34065a-672d-496e-b102-a23cc692f860" />

Revenue by Country (Including Cancelled Orders):This analysis shows revenue generated per country, including the impact 
of cancelled orders. It helps identify top-performing markets, low-performing markets, and potential areas for marketing, 
stock allocation, or operational improvement.

Key findings:Top-performing country: United Kingdom with £23,335.32 valid revenue and only minor cancellations (£199.13).
Other notable countries:
France: £855.86
Australia: £358.25
Netherlands: £192.60
Impact of cancellations: Minimal overall; only the UK shows a small negative adjustment due to cancelled invoices.
Insights:
Focus marketing and loyalty campaigns on high-revenue countries like the UK.
Smaller markets such as Testland (£116.23) may benefit from promotional campaigns or targeted strategies to increase sales.
