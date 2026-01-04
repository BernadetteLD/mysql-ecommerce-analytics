**E-commerce Analysis- Online Retail Dataset**
                              
***Project Overview:**
This project focuses on cleaning, exploring, and analyzing the Online Retail Dataset which I took from Kaggle. The dataset contains transactional data for an online store, including information about invoices, products, customers, and countries.

**The main objectives of this project were to:**
* Clean and prepare the dataset for analysis by correcting corrupted columns, handling nulls, removing duplicates, and converting date formats.
* Perform exploratory data analysis (EDA) to understand sales trends, customer behavior, product popularity, and geographic distribution of revenue.
* Generate actionable business insights that could inform marketing, inventory, and operational decisions.
* Simulate a trigger-based stock management system to experiment with automated inventory alerts.

**Data Cleaning**
The dataset required several cleaning steps before meaningful analysis could be performed:
* Fix corrupted InvoiceNo column due to encoding issues.
* Convert InvoiceDate from text to proper DATETIME format to allow temporal analysis.
* Check for null values across all columns and review incomplete records.
* Identify duplicate entries to ensure accurate aggregations and statistics.
* Reviewed Cancelled (marked with 'C' in InvoiceNo) invoices. I did not remove the cancelled as I believe it would be usefull to review it for sales and returns.
These steps ensured that the dataset was consistent, accurate, and ready for analysis. 

**Exploratory Data Analysis (EDA)**
Key EDA tasks included:
* Dataset overview: Counting total invoices, customers, items sold, and overall revenue.
* Product performance: Identifying top-selling products and quantities sold.
* Customer analysis: Determining top customers by revenue and segmenting them into High, Average, and Low spenders.
* Geographic insights: Analyzing revenue and sales distribution across countries.
* Time trends: Observing monthly revenue patterns to detect seasonality.
* Order issues: Reviewing cancelled invoices to detect potential problems in orders or fulfillment.
These queries helped uncover patterns in customer behavior, product popularity, and revenue distribution, which are essential for business decisions.

**Business Insights**

From the EDA, several key insights were derived:
* Top-selling products: Highlighting which items generate the most sales helps guide inventory and marketing strategies.
* Customer segmentation: High-value customers can be targeted for loyalty programs, while lower-value customers could receive promotions to increase engagement.
* Repeat purchases: Identifying products bought multiple times by the same customer supports recommendations and bundling strategies.
* Revenue by country: Reveals top markets and helps assess the impact of cancelled orders geographically.
Overall, the analysis supports data-driven business strategies in sales optimization, customer retention, and operational planning.

**Trigger Simulation — Stock Management**
To explore automated operational insights, a trigger simulation was implemented:
*Objective: Generate warnings for products with potentially high stock usage (quantity > 10) to prevent stockouts.
*Implementation:
Created a log table (trigger_log) to record alerts.
Built a trigger that logs a message whenever an insert meets the simulated stock warning condition.
*Benefit: Provides a learning example of automated alerts for inventory management, though this is a simulation and does not enforce real stock constraints.
This exercise allowed experimentation with database automation and proactive operational monitoring.

**What Interested Me**
During this project, I was particularly interested in:
* Customer and product behavior: Using SQL to find top customers, repeat purchases, and best-selling products gave a sense of real-world business dynamics.
* Revenue insights: Analyzing revenue trends by month and by country helped me understand seasonality and market segmentation.
* Data cleaning challenges: Fixing corrupted columns and converting date formats was satisfying because it immediately improved the quality and usability of the dataset.
* Trigger simulation: Experimenting with automated stock alerts was engaging because it connected SQL analysis to practical operational applications.

**Technologies Used**
* SQL (MySQL): For cleaning, EDA, and insights generation.
* Kaggle Dataset: Online Retail Dataset (https://www.kaggle.com/datasets/ulrikthygepedersen/online-retail-dataset/data)
* Data cleaning techniques: Handling nulls, duplicates, and encoding issues.
* Analytical techniques: Aggregation, ranking, segmentation, and time series analysis.
