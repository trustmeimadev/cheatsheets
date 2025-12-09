# SQL Conditions & Operators Reference

## 📋 **BASIC COMPARISON OPERATORS**

```sql
=           -- Equal
!=  or <>   -- Not equal
<           -- Less than
>           -- Greater than
<=          -- Less than or equal
>=          -- Greater than or equal
```

**Examples:**
```sql
WHERE age = 25
WHERE salary != 50000
WHERE price > 100
WHERE quantity <= 50
```

## 🔍 **PATTERN MATCHING (LIKE)**

```sql
LIKE        -- Pattern matching with wildcards
NOT LIKE    -- Negative pattern matching
%           -- Matches any sequence of characters
_           -- Matches any single character
[]          -- Matches any single character within brackets (SQL Server)
[^]         -- Matches any single character NOT in brackets (SQL Server)
```

**Examples:**
```sql
WHERE name LIKE 'John%'         -- Starts with John
WHERE email LIKE '%@gmail.com'  -- Ends with @gmail.com
WHERE code LIKE 'A_B%'          -- A, any char, B, then anything
WHERE name NOT LIKE '%test%'    -- Doesn't contain 'test'
```

## 📦 **RANGE CONDITIONS**

```sql
BETWEEN     -- Within a range (inclusive)
NOT BETWEEN -- Outside a range
IN          -- Match any value in a list
NOT IN      -- Match none of the values in a list
```

**Examples:**
```sql
WHERE age BETWEEN 18 AND 65
WHERE price NOT BETWEEN 10 AND 20
WHERE status IN ('Active', 'Pending', 'Approved')
WHERE country NOT IN ('USA', 'Canada')
```

## ❓ **NULL CONDITIONS**

```sql
IS NULL         -- Value is NULL
IS NOT NULL     -- Value is not NULL
ISNULL()        -- Function to check/replace NULL (SQL Server)
IFNULL()        -- Function to check/replace NULL (MySQL)
COALESCE()      -- Returns first non-NULL value
```

**Examples:**
```sql
WHERE email IS NULL
WHERE phone IS NOT NULL
WHERE COALESCE(salary, 0) > 50000
WHERE IFNULL(bonus, 0) = 0
```

## ⚡ **LOGICAL OPERATORS**

```sql
AND     -- Both conditions must be true
OR      -- At least one condition must be true
NOT     -- Negates a condition
()      -- Parentheses for grouping
```

**Examples:**
```sql
WHERE age >= 18 AND status = 'Active'
WHERE department = 'Sales' OR department = 'Marketing'
WHERE NOT (status = 'Terminated')
WHERE (age < 30 AND salary > 50000) OR experience_years > 10
```

## 🔢 **ARITHMETIC OPERATORS**

```sql
+       -- Addition
-       -- Subtraction
*       -- Multiplication
/       -- Division
%       -- Modulo (remainder)
```

**Examples:**
```sql
WHERE salary + bonus > 100000
WHERE (price * quantity) <= 1000
WHERE MOD(id, 2) = 0  -- Even numbers
```

## 🔗 **STRING CONDITIONS**

```sql
|| or CONCAT()  -- String concatenation
||              -- Concatenation (Oracle, PostgreSQL)
+               -- Concatenation (SQL Server)
```

**Examples:**
```sql
WHERE first_name || ' ' || last_name = 'John Smith'
WHERE CONCAT(city, ', ', country) LIKE '%York%'
```

## 📅 **DATE/TIME CONDITIONS**

```sql
DATE()          -- Extract date part
TIME()          -- Extract time part
DATE_ADD()      -- Add to date
DATE_SUB()      -- Subtract from date
DATEDIFF()      -- Difference between dates
```

**Examples:**
```sql
WHERE DATE(order_date) = '2024-01-15'
WHERE order_date >= DATE_SUB(NOW(), INTERVAL 7 DAY)
WHERE DATEDIFF(NOW(), hire_date) > 365
WHERE EXTRACT(YEAR FROM birthdate) = 1990
```

## 🎯 **SPECIAL CONDITIONS**

### **EXISTS / NOT EXISTS**
```sql
WHERE EXISTS (SELECT 1 FROM orders WHERE customer_id = customers.id)
WHERE NOT EXISTS (SELECT 1 FROM payments WHERE invoice_id = invoices.id)
```

### **ANY / SOME / ALL**
```sql
WHERE salary > ANY (SELECT salary FROM managers)
WHERE rating >= ALL (SELECT rating FROM competitors)
```

### **CASE Expressions**
```sql
WHERE CASE 
    WHEN age < 18 THEN 'Minor'
    WHEN age < 65 THEN 'Adult'
    ELSE 'Senior'
END = 'Adult'
```

## 🔄 **COMPOUND CONDITIONS (Advanced)**

### **Multiple AND/OR Combinations**
```sql
WHERE (department = 'IT' AND salary > 70000)
   OR (department = 'Sales' AND commission > 0.2)
   OR (experience_years >= 10 AND certification = 'Yes')
```

### **Nested Conditions**
```sql
WHERE status = 'Active'
  AND (
    (department = 'Engineering' AND level >= 3)
    OR
    (department = 'Management' AND reports_count > 5)
  )
  AND NOT (terminated OR suspended)
```

### **Complex Business Rules**
```sql
WHERE (
    (account_type = 'Premium' AND balance >= 1000)
    OR
    (account_type = 'Standard' AND balance >= 5000 AND credit_score > 700)
)
AND last_transaction_date >= DATE_SUB(NOW(), INTERVAL 90 DAY)
```

## 🎪 **REGULAR EXPRESSIONS**

```sql
REGEXP          -- Regular expression match (MySQL)
RLIKE           -- Synonym for REGEXP (MySQL)
~               -- Matches regular expression (PostgreSQL)
!~              -- Doesn't match regex (PostgreSQL)
SIMILAR TO      -- SQL-standard regex (PostgreSQL)
```

**Examples:**
```sql
WHERE email REGEXP '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$'
WHERE phone ~ '^[0-9]{3}-[0-9]{3}-[0-9]{4}$'
WHERE name SIMILAR TO '[A-Z][a-z]* [A-Z][a-z]*'
```

## 📊 **AGGREGATE CONDITIONS (HAVING)**

```sql
HAVING COUNT(*) > 1
HAVING SUM(amount) > 10000
HAVING AVG(score) >= 80
HAVING MAX(price) - MIN(price) > 50
```

**Examples:**
```sql
SELECT department, COUNT(*) 
FROM employees 
GROUP BY department
HAVING COUNT(*) > 5
  AND AVG(salary) > 60000;
```

## 🚨 **COMMON PITFALLS**

### **1. NULL Comparisons**
```sql
-- WRONG (always returns UNKNOWN)
WHERE salary = NULL

-- CORRECT
WHERE salary IS NULL
```

### **2. String vs Number**
```sql
-- May cause issues
WHERE id = '123'      -- Implicit conversion

-- Better
WHERE id = 123        -- Exact type match
```

### **3. Operator Precedence**
```sql
-- WRONG logic
WHERE status = 'Active' OR status = 'Pending' AND priority = 1
-- Evaluates as: Active OR (Pending AND priority=1)

-- CORRECT with parentheses
WHERE (status = 'Active' OR status = 'Pending') AND priority = 1
```

## 🎓 **BEST PRACTICES**

### **Readable Conditions:**
```sql
-- Hard to read
WHERE (a=1 AND b=2) OR (c=3 AND d=4) OR (e=5 AND f=6)

-- Better formatting
WHERE (a = 1 AND b = 2)
   OR (c = 3 AND d = 4)
   OR (e = 5 AND f = 6)
```

### **Use Variables/Parameters:**
```sql
-- Instead of hardcoded values
WHERE order_date >= @start_date 
  AND order_date <= @end_date
```

### **Optimize for Performance:**
```sql
-- Place most restrictive conditions first
WHERE active = 1           -- Eliminates many rows
  AND department_id = 5    -- Further filtering
  AND name LIKE 'A%'       -- Least restrictive
```

## 💡 **PRO TIPS**

1. **Use parentheses** for complex logic - they're free!
2. **Put indexed columns first** in WHERE clauses
3. **Avoid functions on columns** in WHERE clauses (can't use indexes)
4. **Use EXISTS instead of IN** for subqueries with large results
5. **Test NULL handling** - it's often the source of bugs
6. **Use comments** for complex business rules

## 📚 **QUICK REFERENCE CARD**

```
Comparison: = != < > <= >=
Pattern: LIKE NOT LIKE % _
Range: BETWEEN IN
NULL: IS NULL IS NOT NULL
Logic: AND OR NOT ()
Math: + - * / %
Exists: EXISTS NOT EXISTS
Regex: REGEXP RLIKE ~
Group: HAVING
```

---

**Remember:** Conditions define what data you get - write them carefully! Always test edge cases and NULL values.
