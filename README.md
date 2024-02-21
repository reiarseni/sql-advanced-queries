
Advanced SQL queries
====================

Table name: **employee**

<table cellspacing="0" summary="">
<thead>
  <tr>
    <th>employee_id</th><th>first_name</th><th>last_name</th><th>dept_id</th><th>manager_id</th><th>salary</th><th>expertise</th>
  </tr>
</thead>
<tfoot>
</tfoot>
<tbody>
  <tr>
    <td>100</td><td>John</td><td>White</td><td>IT</td><td>103</td><td>120000</td><td>Senior</td>
  </tr>
  <tr>
    <td>101</td><td>Mary</td><td>Danner</td><td>Account</td><td>109</td><td>80000</td><td>junior</td>
  </tr>
  <tr>
    <td>102</td><td>Ann</td><td>Lynn</td><td>Sales</td><td>107</td><td>140000</td><td>Semisenior</td>
  </tr>
  <tr>
    <td>103</td><td>Peter</td><td>O'connor</td><td>IT</td><td>110</td><td>130000</td><td>Senior</td>
  </tr>
  <tr>
    <td>106</td><td>Sue</td><td>Sanchez</td><td>Sales</td><td>107</td><td>110000</td><td>Junior</td>
  </tr>
  <tr>
    <td>107</td><td>Marta</td><td>Doe</td><td>Sales</td><td>110</td><td>180000</td><td>Senior</td>
  </tr>
  <tr>
    <td>109</td><td>Ann</td><td>Danner</td><td>Account</td><td>110</td><td>90000</td><td>Senior</td>
  </tr>
  <tr>
    <td>110</td><td>Simon</td><td>Yang</td><td>CEO</td><td>null</td><td>250000</td><td>Senior</td>
  </tr>
  <tr>
    <td>111</td><td>Juan</td><td>Graue</td><td>Sales</td><td>102</td><td>37000</td><td>Junior</td>
  </tr>
</tbody>
</table>

* Example #1 - Listing the First 5 Rows of a Result Set
- CTE 
- Rank

```sql
WITH employee_ranking AS (
  SELECT
    employee_id,
    last_name,
    first_name,
    salary,
    RANK() OVER (ORDER BY salary DESC) as ranking
  FROM employee
)
SELECT
  employee_id,
  last_name,
  first_name,
  salary
FROM employee_ranking
WHERE ranking <= 5
ORDER BY ranking
```
The WITH clause in the previous query creates a CTE called employee_ranking, which is like a virtual table that is used in the main query. The subquery in the CTE uses the RANK() function to get the position of each row in the ranking. The OVER clause (ORDER BY salary DESC) indicates how the RANK() value should be calculated. The RANK() function for the row with the highest salary will return 1, and so on.

Finally, in the WHERE clause of the main query, we ask for those rows with a ranking value less than or equal to 5. This allows us to only get the top 5 rows by ranking value. Once again, we use an ORDER BY clause to display the result set, which is sorted by rank in ascending order.


* Example #2 - Join a Table to Itself

```sql
 SELECT 
  concat(m.first_name,' ', m.last_name) AS manager_name,
  concat(e.first_name,' ', e.last_name) AS employee_name
FROM employee m
JOIN employee e
ON m.employee_id = e.manager_id
order by m.first_name;
```

* Show All Rows with a Value Above Average
- Subquery

```sql
SELECT
  first_name,
  last_name,
  salary
FROM employee 
WHERE salary > ( SELECT AVG(salary) FROM employee )
```


