## SQL Task 3: Market Badge Total Sold Quantity (Finance Analytics)

### Task Overview
Create a stored procedure to determine a market badge based on total sold quantity for a specific market and fiscal year.  
- **Gold Badge**: Total sold quantity > 5 million.  
- **Silver Badge**: Total sold quantity ≤ 5 million.

### Inputs and Outputs
- **Inputs**:
  - `in_market`: Market name (default is "India").
  - `in_fiscal_year`: Fiscal year for analysis.
- **Output**:
  - `out_market_badge`: Market badge (Gold or Silver).

---

### Stored Procedure Code
```sql
CREATE PROCEDURE `get_market_badge`(
    IN in_market VARCHAR(45), -- Market name input.
    IN in_fiscal_year YEAR,   -- Fiscal year input.
    OUT out_market_badge VARCHAR(45) -- Market badge output.
)
BEGIN
    DECLARE qty INT DEFAULT 0; -- Variable to store total sold quantity.

    -- Default market set to India if input is empty.
    IF in_market = "" THEN 
        SET in_market = "india";
    END IF;

    -- Retrieve total quantity sold for the specified market and fiscal year.
    SELECT SUM(f.sold_quantity) INTO qty -- Aggregate sold quantity.
    FROM fact_sales_monthly f
    JOIN dim_customer c
    ON f.customer_code = c.customer_code -- Match customer codes.
    WHERE get_fiscal_year(f.date) = in_fiscal_year -- Filter by fiscal year.
      AND c.market = in_market -- Filter by market.
    GROUP BY c.market;

    -- Determine market badge based on total sold quantity.
    IF qty > 5000000 THEN 
        SET out_market_badge = 'GOLD'; -- Gold badge if quantity > 5M.
    ELSE  
        SET out_market_badge = 'SILVER'; -- Silver badge otherwise.
    END IF;
END;
```
