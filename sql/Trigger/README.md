**Trigger Simulation — Stock Management**

This section demonstrates a trigger-based simulation for stock management in the online_retail_dataset. The goal
is to automatically log potential out-of-stock situations for products when large quantities are being inserted.


Implement an automated system to warn if products may be out of stock. (This is a simulation for learning purposes.)
Benefits:
1. Avoids customer dissatisfaction due to unfulfilled orders.
2. Ensures accurate inventory management.
3. Automatically alerts the fulfillment/logistics team.
4. Supports data-driven operational decisions.
this trigger is a simulation and does not prevent real stock issues

**1.** Create a log table to store trigger warnings
CREATE TABLE IF NOT EXISTS trigger_log (
    ID INT AUTO_INCREMENT PRIMARY KEY,
    Product TEXT,
    Quantity INT,
    Message TEXT,
    CreatedAt DATETIME
);

**2.** Create a trigger that simulates out-of-stock warnings
   
DELIMITER //
CREATE TRIGGER simulate_out_of_stock_warning
BEFORE INSERT ON online_retail_dataset
FOR EACH ROW
BEGIN
    -- Only log a warning if quantity > 10 (simulated stock check)
    IF NEW.Quantity > 10 THEN
        INSERT INTO trigger_log(Product, Quantity, Message, CreatedAt)
        VALUES (NEW.Description, NEW.Quantity, 'Simulated: Product may be out of stock', NOW());
    END IF;
END;
//

DELIMITER ;

**3.** Insert sample data to test the trigger
   
Example 1
INSERT INTO online_retail_dataset
(InvoiceNo, StockCode, Description, Quantity, UnitPrice, CustomerID, Country)
VALUES
('536600', '85184C', 'VALENTINE BOX', 12, 2.95, 17920, 'United Kingdom');

Example 2
INSERT INTO online_retail_dataset
(InvoiceNo, StockCode, Description, Quantity, UnitPrice, CustomerID, Country)
VALUES
('TEST100', 'TEST01', 'TEST PRODUCT', 12, 1.99, 99999, 'Testland');
-- Example 3: multiple inserts
INSERT INTO online_retail_dataset
(InvoiceNo, StockCode, Description, Quantity, UnitPrice, CustomerID, Country)
VALUES
('TEST101', 'TEST02', 'ANOTHER PRODUCT', 5, 3.50, 99998, 'Testland'),
('TEST102', 'TEST03', 'BIG PRODUCT', 15, 4.99, 99997, 'Testland')

**4.** Check the triggers
    
SHOW TRIGGERS LIKE 'online_retail_dataset';

**5.** View the trigger log to see warnings
   
SELECT * FROM trigger_log ORDER BY CreatedAt DESC;

<img width="436" height="52" alt="image" src="https://github.com/user-attachments/assets/390d72ae-ed6e-4f36-be11-3a3e5e05045f" />

This shows us the trigger worked when we tried to place an order.
