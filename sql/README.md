**Data cleaning**

This was the initial step. I noticed that the column name "InvoiceNo" appeared as “ï»¿InvoiceNo” and the InvoiceDate column was messy, so I corrected them to match normal SQL standards.I also checked the dataset for NULL values.
Regarding duplicates, I decided not to remove them because I wanted to treat this dataset like a real-world example. Duplicates in a sales dataset can indicate cancellations, incorrect entries, or even potential fraud. In such cases, it is better to investigate and bring the issue to management, so appropriate actions such as training or controls can be introduced.
I also chose not to remove cancelled orders. It is important to understand which items were cancelled and why, because this helps evaluate customer behavior, operational issues, and whether certain products should continue to be stocked or promoted. These insights support better business decisions.

--  Explore table

SELECT * FROM ecommerce_analytics.online_retail_dataset;
DESCRIBE ecommerce_analytics.online_retail_dataset;

-- Clean corrupted InvoiceNo column

ALTER TABLE ecommerce_analytics.online_retail_dataset
CHANGE COLUMN `ï»¿InvoiceNo` `InvoiceNo` TEXT;

-- Check InvoiceDate values 

SELECT InvoiceDate FROM online_retail_dataset LIMIT 20; 

-- Add new column for converted dates

ALTER TABLE ecommerce_analytics.online_retail_dataset
ADD COLUMN InvoiceDate_ISO DATETIME;

-- Check conversion works 

SELECT 
    `InvoiceDate`,
    STR_TO_DATE(`InvoiceDate`, '%d/%m/%Y %H:%i') AS iso_datetime
FROM ecommerce_analytics.online_retail_dataset
LIMIT 10;

-- Temporarily disable safe updates to allow UPDATE

SET SQL_SAFE_UPDATES = 0;

-- Update InvoiceDate_ISO with proper DATETIME conversion

UPDATE ecommerce_analytics.online_retail_dataset
SET InvoiceDate_ISO = STR_TO_DATE(`InvoiceDate`, '%d/%m/%Y %H:%i');

-- Re-enable safe updates

SET SQL_SAFE_UPDATES = 1;

-- Verify new column

SELECT InvoiceDate, InvoiceDate_ISO
FROM ecommerce_analytics.online_retail_dataset
LIMIT 10;

-- Remove old InvoiceDate column

ALTER TABLE ecommerce_analytics.online_retail_dataset
DROP COLUMN `InvoiceDate`;

-- Rename InvoiceDate_ISO back to InvoiceDate

ALTER TABLE ecommerce_analytics.online_retail_dataset
CHANGE COLUMN `InvoiceDate_ISO` `InvoiceDate` DATETIME;

-- Review cancelled invoices

SELECT *
FROM ecommerce_analytics.online_retail_dataset
WHERE ï»¿InvoiceNo LIKE 'C%';

-- Review  null values in all columns

SELECT
    SUM(InvoiceNo IS NULL) AS null_invoiceno,
    SUM(StockCode IS NULL)      AS null_stockcode,
    SUM(Description IS NULL)    AS null_description,
    SUM(Quantity IS NULL)       AS null_quantity,
    SUM(UnitPrice IS NULL)      AS null_unitprice,
    SUM(CustomerID IS NULL)     AS null_customerid,
    SUM(Country IS NULL)        AS null_country,
    SUM(InvoiceDate IS NULL)    AS null_invoicedate
FROM ecommerce_analytics.online_retail_dataset;

<img width="509" height="45" alt="image" src="https://github.com/user-attachments/assets/20e1f15a-628f-497a-9268-be9681c91a15" />


-- Check for duplicates

I reviewed the duplicate entries in the dataset. While some rows share the same CustomerID or InvoiceNo, most differ in InvoiceDate, Quantity, or Product Description.
This is expected in a real-world sales dataset: One invoice can contain multiple products. Customers may purchase the same product multiple times with slightly different quantities. Slight differences in invoice time are common for sequential transactions.Therefore, these “duplicates” are not errors, but reflect actual business behavior. Identifying them helps understand purchase patterns, repeated products, and customer behavior.

SELECT *, COUNT(*) AS duplicate_count
FROM ecommerce_analytics.online_retail_dataset
GROUP BY InvoiceNo, StockCode, Description, Quantity, InvoiceDate, UnitPrice, CustomerID, Country
HAVING duplicate_count > 1
ORDER BY duplicate_count DESC;


<img width="581" height="331" alt="image" src="https://github.com/user-attachments/assets/24201b3d-5f8b-4e60-b06f-18a598feba5e" />


