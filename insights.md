
                     ----:Suspicious Transactions Queries:----
                     ------------------------------------------
Query 1: Find all transactions greater than ₹10,00,000 (10 lakhs)
SELECT * FROM transactions WHERE amount > 1000000;
----------------------
Query 2: List accounts with more than 3 high-value transactions (above ₹5,00,000)
SELECT account_id, COUNT(*) AS high_value_count 
FROM transactions 
WHERE amount > 500000 
GROUP BY account_id 
HAVING COUNT(*) > 3;
----------------------
Query 3: Find accounts that have transactions in more than 3 different cities
SELECT account_id 
FROM (
  SELECT account_id, COUNT(DISTINCT location) AS cities 
  FROM transactions 
  GROUP BY account_id
) t 
WHERE t.cities > 3;
---------------------
Query 4: Find accounts with multiple transaction types on a single day
SELECT account_id, DATE(transaction_date), COUNT(DISTINCT transaction_type) AS types 
FROM transactions 
GROUP BY account_id, DATE(transaction_date) 
HAVING types > 1;
----------------------
Query 5: Find transactions on weekends
SELECT * FROM transactions 
WHERE DAYOFWEEK(transaction_date) IN (1, 7);  -- Sunday (1), Saturday (7)
----------------------
Query 6: List top 5 transactions with the highest amount
SELECT * FROM transactions 
ORDER BY amount DESC 
LIMIT 5;
----------------------
Query 7: Accounts that did both Deposit and Withdrawal in the last 30 days
SELECT account_id 
FROM transactions 
WHERE transaction_date >= NOW() - INTERVAL 30 DAY 
GROUP BY account_id 
HAVING SUM(transaction_type = 'Deposit') > 0 
   AND SUM(transaction_type = 'Withdrawal') > 0;
 ----------------------  
Query 8: Transactions above ₹2,00,000 that occurred in Surat
SELECT * FROM transactions 
WHERE location = 'Surat' AND amount > 200000;
------------------------
Query 9: Accounts with more than 10 transactions in the last 6 months
SELECT account_id, COUNT(*) AS txn_count 
FROM transactions 
WHERE transaction_date >= NOW() - INTERVAL 6 MONTH 
GROUP BY account_id 
HAVING txn_count > 10;
-------------------------
Query 10: Accounts with a single transaction greater than 80% of total transaction volume
SELECT account_id 
FROM transactions 
GROUP BY account_id 
HAVING MAX(amount) > 0.8 * SUM(amount);
-------------------------
