Data cleaning

-- 1. Explore table
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

-- review cancelled invoices
SELECT *
FROM ecommerce_analytics.online_retail_dataset
WHERE ï»¿InvoiceNo LIKE 'C%';

-- review  null values in all columns
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
-- Check for duplicates
SELECT *, COUNT(*) AS duplicate_count
FROM ecommerce_analytics.online_retail_dataset
GROUP BY InvoiceNo, StockCode, Description, Quantity, InvoiceDate, UnitPrice, CustomerID, Country
HAVING duplicate_count > 1
ORDER BY duplicate_count DESC;
