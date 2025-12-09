# SQL Functions Quick Reference

## 📊 **AGGREGATE FUNCTIONS**
```sql
SELECT COUNT(*) FROM users;                     -- Count all rows
SELECT SUM(salary) FROM employees;              -- Add all values
SELECT AVG(age) FROM students;                  -- Calculate average
SELECT MIN(price) FROM products;                -- Find smallest value
SELECT MAX(price) FROM products;                -- Find largest value
```

## 📝 **STRING FUNCTIONS**
```sql
SELECT CONCAT(first_name, ' ', last_name) FROM customers;
SELECT UPPER(name) FROM products;               -- Convert to uppercase
SELECT LOWER(email) FROM users;                 -- Convert to lowercase
SELECT SUBSTRING(name, 1, 3) FROM products;     -- Get first 3 chars
SELECT LENGTH(address) FROM customers;          -- Get string length
SELECT TRIM('  Hello  ');                       -- Remove spaces
SELECT REPLACE(title, 'old', 'new') FROM books;
```

## 🔢 **NUMERIC FUNCTIONS**
```sql
SELECT ROUND(price, 2) FROM products;          -- Round to 2 decimals
SELECT CEIL(4.3);                               -- Round up → 5
SELECT FLOOR(4.9);                              -- Round down → 4
SELECT ABS(-10);                                -- Absolute value → 10
SELECT MOD(10, 3);                              -- Remainder → 1
SELECT POWER(2, 3);                             -- 2³ = 8
SELECT SQRT(25);                                -- Square root → 5
SELECT RAND();                                  -- Random number 0-1
```

## 📅 **DATE/TIME FUNCTIONS**
```sql
SELECT NOW();                                   -- Current date/time
SELECT CURDATE();                               -- Current date
SELECT CURTIME();                               -- Current time
SELECT YEAR('2024-01-15');                      -- Extract year → 2024
SELECT MONTH('2024-01-15');                     -- Extract month → 1
SELECT DAY('2024-01-15');                       -- Extract day → 15
SELECT DATE_ADD('2024-01-15', INTERVAL 7 DAY);  -- Add 7 days
SELECT DATEDIFF('2024-01-20', '2024-01-15');    -- Difference → 5
```

## 🔄 **CONVERSION FUNCTIONS**
```sql
SELECT CAST('123' AS INT);                      -- Convert string to int
SELECT CONVERT('2024-01-15', DATE);             -- Convert to date
SELECT COALESCE(NULL, NULL, 'default');         -- First non-null
SELECT IFNULL(salary, 0) FROM employees;        -- Replace NULL with 0
```

## 🪟 **WINDOW FUNCTIONS**
```sql
SELECT name, salary, 
       ROW_NUMBER() OVER (ORDER BY salary DESC) 
FROM employees;

SELECT name, salary,
       RANK() OVER (ORDER BY salary DESC)
FROM employees;

SELECT name, salary,
       LAG(salary) OVER (ORDER BY date)
FROM sales;
```

## ⚡ **CONDITIONAL FUNCTIONS**
```sql
-- CASE statement
SELECT name,
  CASE 
    WHEN age < 18 THEN 'Minor'
    WHEN age < 65 THEN 'Adult'
    ELSE 'Senior'
  END AS age_group
FROM users;

-- Simple IF (MySQL)
SELECT IF(age >= 18, 'Adult', 'Minor') FROM users;

-- IIF (SQL Server)
SELECT IIF(age >= 18, 'Adult', 'Minor') FROM users;
```

## 📋 **OTHER USEFUL FUNCTIONS**
```sql
-- Group concatenation
SELECT GROUP_CONCAT(name) FROM products GROUP BY category;

-- Type checking
SELECT TYPEOF(column) FROM table;

-- NULL handling
SELECT NVL(salary, 0) FROM employees;          -- Oracle

-- JSON functions (modern SQL)
SELECT JSON_EXTRACT(data, '$.name') FROM users;
```

## 🎯 **QUICK EXAMPLES**
```sql
-- Count active users
SELECT COUNT(*) FROM users WHERE active = 1;

-- Format full name
SELECT CONCAT(UPPER(first_name), ' ', UPPER(last_name)) FROM customers;

-- Get this month's sales
SELECT SUM(amount) FROM sales 
WHERE MONTH(sale_date) = MONTH(NOW());

-- Find duplicate emails
SELECT email, COUNT(*) 
FROM users 
GROUP BY email 
HAVING COUNT(*) > 1;

-- Calculate age from birthdate
SELECT name, 
       YEAR(NOW()) - YEAR(birthdate) AS age 
FROM people;
```

## ⚠️ **IMPORTANT NOTES**
1. **Syntax varies** between databases (MySQL, PostgreSQL, SQL Server, Oracle)
2. **Not all functions** work in every database
3. **Check documentation** for your specific database
4. **Use aliases** for better column names: `SELECT COUNT(*) AS total_users`

## 📚 **MOST COMMONLY USED**
```sql
-- Everyday functions
COUNT(), SUM(), AVG()
CONCAT(), UPPER(), LOWER()
NOW(), DATE(), YEAR()
ROUND(), CAST(), COALESCE()
CASE, ROW_NUMBER(), RANK()
```

---

*Pro Tip: Use `DESCRIBE table_name` or `SHOW COLUMNS FROM table_name` to see column types before using functions!*
