# SQL_HW1
## My Tables
```sql
-- Customers table
+-------------+------------+-----------+-------------+
| customer_id | first_name | last_name | referral_id |
+-------------+------------+-----------+-------------+
|           1 | Fred       | Fish      |        NULL |
|           2 | Larry      | Lobster   |           1 |
|           3 | Bubble     | Bass      |           2 |
|           4 | Poppy      | Puff      |           2 |
+-------------+------------+-----------+-------------+

-- employees table
+-------------+------------+-------------+------------+------------+
| employee_id | first_name | last_name   | hourly_pay | hire_date  |
+-------------+------------+-------------+------------+------------+
|           1 | Eugene     | Krabs       |      25.50 | 2023-01-02 |
|           2 | Squidward  | Tentacles   |      15.00 | 2023-01-04 |
|           3 | Spongebob  | Squarepants |      12.50 | 2023-01-03 |
|           4 | Patrick    | Star        |      12.50 | 2023-01-05 |
|           5 | Sandy      | Cheeks      |      17.25 | 2023-01-06 |
+-------------+------------+-------------+------------+------------+

-- transactions table
+----------------+--------+-------------+
| transaction_id | amount | customer_id |
+----------------+--------+-------------+
|           1000 |   4.99 |           3 |
|           1001 |   2.89 |           2 |
|           1002 |   3.38 |           3 |
|           1003 |   4.99 |           1 |
|           1006 |   1.00 |        NULL |
+----------------+--------+-------------+

```
## Queries
```sql
-- Inner-Joins
mysql> SELECT *
    -> FROM transactions
    -> INNER JOIN customers
    -> ON transactions.customer_id = customers.customer_id;
+----------------+--------+-------------+-------------+------------+-----------+-------------+
| transaction_id | amount | customer_id | customer_id | first_name | last_name | referral_id |
+----------------+--------+-------------+-------------+------------+-----------+-------------+
|           1000 |   4.99 |           3 |           3 | Bubble     | Bass      |           2 |
|           1001 |   2.89 |           2 |           2 | Larry      | Lobster   |           1 |
|           1002 |   3.38 |           3 |           3 | Bubble     | Bass      |           2 |
|           1003 |   4.99 |           1 |           1 | Fred       | Fish      |        NULL |
+----------------+--------+-------------+-------------+------------+-----------+-------------+

-- Left-Joins
mysql> SELECT *
    -> FROM transactions LEFT JOIN customers
    -> ON transactions.customer_id = customers.customer_id;
+----------------+--------+-------------+-------------+------------+-----------+-------------+
| transaction_id | amount | customer_id | customer_id | first_name | last_name | referral_id |
+----------------+--------+-------------+-------------+------------+-----------+-------------+
|           1000 |   4.99 |           3 |           3 | Bubble     | Bass      |           2 |
|           1001 |   2.89 |           2 |           2 | Larry      | Lobster   |           1 |
|           1002 |   3.38 |           3 |           3 | Bubble     | Bass      |           2 |
|           1003 |   4.99 |           1 |           1 | Fred       | Fish      |        NULL |
|           1006 |   1.00 |        NULL |        NULL | NULL       | NULL      |        NULL |
+----------------+--------+-------------+-------------+------------+-----------+-------------+

-- Right-Joins
mysql> SELECT *
    -> FROM transactions RIGHT JOIN customers
    -> ON transactions.customer_id = customers.customer_id;
+----------------+--------+-------------+-------------+------------+-----------+-------------+
| transaction_id | amount | customer_id | customer_id | first_name | last_name | referral_id |
+----------------+--------+-------------+-------------+------------+-----------+-------------+
|           1003 |   4.99 |           1 |           1 | Fred       | Fish      |        NULL |
|           1001 |   2.89 |           2 |           2 | Larry      | Lobster   |           1 |
|           1002 |   3.38 |           3 |           3 | Bubble     | Bass      |           2 |
|           1000 |   4.99 |           3 |           3 | Bubble     | Bass      |           2 |
|           NULL |   NULL |        NULL |           4 | Poppy      | Puff      |           2 |
+----------------+--------+-------------+-------------+------------+-----------+-------------+

-- Self-Joins
mysql> SELECT *
    -> FROM customers AS a
    -> INNER JOIN customers AS b
    -> ON a.referral_id = b.customer_id;
+-------------+------------+-----------+-------------+-------------+------------+-----------+-------------+
| customer_id | first_name | last_name | referral_id | customer_id | first_name | last_name | referral_id |
+-------------+------------+-----------+-------------+-------------+------------+-----------+-------------+
|           2 | Larry      | Lobster   |           1 |           1 | Fred       | Fish      |        NULL |
|           3 | Bubble     | Bass      |           2 |           2 | Larry      | Lobster   |           1 |
|           4 | Poppy      | Puff      |           2 |           2 | Larry      | Lobster   |           1 |
+-------------+------------+-----------+-------------+-------------+------------+-----------+-------------+

mysql> Select a.customer_id, a.first_name, a.last_name,
    -> CONCAT(b.first_name, " ", b.last_name) AS "referred_by"
    -> FROM customers AS a
    -> INNER JOIN customers AS b
    -> ON a.referral_id = b.customer_id;
+-------------+------------+-----------+---------------+
| customer_id | first_name | last_name | referred_by   |
+-------------+------------+-----------+---------------+
|           2 | Larry      | Lobster   | Fred Fish     |
|           3 | Bubble     | Bass      | Larry Lobster |
|           4 | Poppy      | Puff      | Larry Lobster |
+-------------+------------+-----------+---------------+

-- NEW employees table
+-------------+------------+-------------+------------+---------------+------------+---------------+
| employee_id | first_name | last_name   | hourly_pay | job           | hire_date  | supervisor_id |
+-------------+------------+-------------+------------+---------------+------------+---------------+
|           1 | Eugene     | Krabs       |      25.50 | manager       | 2023-01-02 |          NULL |
|           2 | Squidward  | Tentacles   |      15.00 | cashier       | 2023-01-04 |             5 |
|           3 | Spongebob  | Squarepants |      12.50 | cook          | 2023-01-03 |             5 |
|           4 | Patrick    | Star        |      12.50 | cook          | 2023-01-05 |             5 |
|           5 | Sandy      | Cheeks      |      17.25 | asst. manager | 2023-01-06 |             1 |
|           6 | Sheldon    | Plankton    |      10.00 | janitor       | 2023-01-07 |             5 |
+-------------+------------+-------------+------------+---------------+------------+---------------+

-- anotrher example of self joins
mysql> SELECT a.first_name, a.last_name,
    -> CONCAT(b.first_name, " ", b.last_name) AS "reports to"
    -> FROM employees AS a
    -> INNER JOIN employees AS b
    -> ON a.supervisor_id = b.employee_id;
+------------+-------------+--------------+
| first_name | last_name   | reports to   |
+------------+-------------+--------------+
| Sandy      | Cheeks      | Eugene Krabs |
| Sheldon    | Plankton    | Sandy Cheeks |
| Patrick    | Star        | Sandy Cheeks |
| Spongebob  | Squarepants | Sandy Cheeks |
| Squidward  | Tentacles   | Sandy Cheeks |
+------------+-------------+--------------+
