# Validated SQL Patterns
```sql
-- Customer acquisition by month
SELECT DATE_TRUNC('month', created_date) as month,
       COUNT(*) as new_customers
FROM customers 
WHERE created_date >= '2024-01-01'
GROUP BY month
ORDER BY month;
```
