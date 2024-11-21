# SQL Task 3 Market-Badge Total Sold Qty (Finance Analytics)

Task 3: To create a stored Procedure that can determine the market badge. If total sold quantity > 5 million that market is considered Gold else is silver.

Inputs will be: 
- Market
- Fiscal Year
  
Output:
- Market Badge


```sql


CREATE PROCEDURE `get_market_badge`(
		in in_market varchar(45),
        in in_fiscal_year YEAR,
        out out_market_badge VARCHAR (45)
)
BEGIN
		DECLARE qty INT DEFAULT 0;
		# Declare default Market to be india
        
		
        IF in_market = "" THEN 
           SET in_market = "india";
        END IF; 
        
        # Retrive total qty sold for market and fiscal year 2021
        
		select sum(f.sold_quantity) INTO qty
		from fact_sales_monthly f 
        join dim_customer c
		on f.customer_code = c.customer_code
		where get_fiscal_year(f.date) = in_fiscal_year
        and c.market = in_market 
		group by c.market;

		# Determining market badge
        
        if qty > 5000000 THEN SET out_market_badge = 'GOLD';
        ELSE  SET out_market_badge ='SILVER';
        END IF;
END

```
