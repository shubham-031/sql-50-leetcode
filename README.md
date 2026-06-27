# SQL 50 - LeetCode
Solutions for [SQL 50 Study Plan](https://leetcode.com/studyplan/top-sql-50/) on LeetCode
1.Outer Query + Subquery Pattern LeetCode SQL
---

[1757 - Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/)
```sql
SELECT product_id
FROM Products
WHERE low_fats = 'Y'
AND recyclable = 'Y'
```

[584 - Find Customer Referee](https://leetcode.com/problems/find-customer-referee)
```sql
SELECT name 
FROM Customer 
WHERE referee_id != 2 OR referee_id IS null
```

[595 - Big Countries](https://leetcode.com/problems/big-countries/)
```sql
SELECT name, population, area
FROM WORLD
WHERE area >= 3000000
OR population >= 25000000
```

[1148 - Article Views I](https://leetcode.com/problems/article-views-i)
```sql
SELECT DISTINCT author_id as id
FROM Views
WHERE
author_id = viewer_id
ORDER BY author_id
```

[1683 - Invalid Tweets](https://leetcode.com/problems/invalid-tweets/)
```sql
SELECT tweet_id
FROM Tweets
WHERE length(content) > 15
```

[1378 - Replace Employee ID With The Unique Identifier](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier)
```sql
SELECT unique_id, name
FROM Employees AS a
LEFT JOIN EmployeeUNI AS b
ON a.id = b.id
```

[1068 - Product Sales Analysis I](https://leetcode.com/problems/product-sales-analysis-i/)
```sql
SELECT product_name, year, price
FROM Sales AS a
LEFT JOIN Product  AS b
ON a.product_id = b.product_id
```

[1581 - Customer Who Visited but Did Not Make Any Transactions](https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/)
```sql
SELECT
    v.customer_id,
    COUNT(*) AS count_no_trans
FROM Visits v
LEFT JOIN Transactions t
ON v.visit_id = t.visit_id
WHERE t.visit_id IS NULL
GROUP BY v.customer_id;
```

[197 - Rising Temperature](https://leetcode.com/problems/rising-temperature/) 
```sql
SELECT w1.id 
FROM Weather w1, Weather w2
WHERE DATEDIFF(w1.recordDate, w2.recordDate) = 1
AND w1.temperature > w2.temperature

-- OR
SELECT w1.id
FROM Weather w1, Weather w2
WHERE w1.temperature > w2.temperature
AND SUBDATE(w1.recordDate, 1) = w2.recordDate
```

[1661 - Average Time of Process per Machine](https://leetcode.com/problems/average-time-of-process-per-machine/)
```sql
select 
a.machine_id, 
ROUND(AVG(b.timestamp-a.timestamp), 3) AS processing_time
from Activity a
JOIN Activity b
ON a.machine_id = b.machine_id
AND a.process_id = b.process_id
WHERE a.activity_type = "start"
AND b.activity_type = "end"
Group BY machine_id;
```

[577 - Employee Bonus](https://leetcode.com/problems/employee-bonus/solutions/)
```sql
SELECT name, bonus
FROM Employee e
LEFT JOIN Bonus b
ON e.empId = b.empId
WHERE bonus < 1000
OR bonus IS NULL
```

[1280 - Students and Examinations](https://leetcode.com/problems/students-and-examinations/)


🎯 Question एका वाक्यात

Question हे नाही विचारत:

कोणत्या Students ने Exam दिला?

तो विचारतो:

प्रत्येक Student ने प्रत्येक Subject चा Exam किती वेळा दिला?


```sql

SELECT a.student_id, 
a.student_name, 
b.subject_name,
 COUNT(c.student_id) AS attended_exams

FROM  Students AS a
CROSS JOIN Subjects  AS b
LEFT JOIN Examinations AS c
ON a.student_id = c.student_id
AND b.subject_name = c.subject_name
GROUP BY 
a.student_id ,
a.student_name ,
b.subject_name 

ORDER BY 
a.student_id,
b.subject_name;
```
[570. Managers with at Least 5 Direct Reports](https://leetcode.com/problems/managers-with-at-least-5-direct-reports)

1️⃣ Question Translate

👉 ज्या Manager कडे किमान 5 Employees directly report करतात, त्या Manager चे नाव द्या.

```sql
SELECT name 
FROM Employee 
WHERE id IN
  (SELECT managerId 
   FROM Employee 
   GROUP BY managerId 
   HAVING COUNT(*) >= 5
  )

-- OR
SELECT a.name
FROM Employee a
JOIN Employee b
WHERE a.id = b.managerId
GROUP BY b.managerId
HAVING COUNT(*) >= 5
```

[1934. Confirmation Rate](https://leetcode.com/problems/confirmation-rate/)

1️⃣ Question Translate

Question म्हणतो:

👉 प्रत्येक User चा Confirmation Rate काढा.


⭐ Revision Formula
SUM(condition)   → Count TRUE rows

COUNT(column)    → Count total rows

IFNULL(x,0)      → If x is NULL, return 0

ROUND(x,2)       → Round to 2 decimal places

One-line memory trick:

Confirmation Rate = Confirmed Count / Total Count
                  = SUM(action='confirmed') / COUNT(action)

                  
```sql

SELECT a.user_id ,
ROUND (IFNULL(SUM(b.action ="confirmed")/ COUNT(b.action),0), 2)
AS confirmation_rate 
FROM Signups AS a 
LEFT JOIN Confirmations AS b
ON a.user_id = b.user_id
GROUP BY a.user_id ;
```

[620. Not Boring Movies](https://leetcode.com/problems/not-boring-movies)

1️⃣ Question Translate

👉 Odd ID असलेले movies घ्या.

👉 Description = "boring" नसावा.

👉 Rating descending मध्ये sort करा.

```sql
-- odd id, "boring", rating desc
SELECT *
FROM Cinema
WHERE id % 2 != 0
AND description != 'boring'
ORDER BY rating DESC;
```

[1251. Average Selling Price](https://leetcode.com/problems/average-selling-price/)

1️⃣ Question Translate

👉 प्रत्येक Product चा Average Selling Price काढा.


Formula:

Average Price = Total Amount / Total Units Sold

```sql
-- avg(selling), round 2
SELECT
    a.product_id,
    ROUND(
        IFNULL(SUM(a.price * b.units) / SUM(b.units), 0),
        2
    ) AS average_price
FROM Prices AS a
LEFT JOIN UnitsSold AS b
ON a.product_id = b.product_id
AND b.purchase_date BETWEEN a.start_date AND a.end_date
GROUP BY a.product_id;
```

[1075. Project Employees I](https://leetcode.com/problems/project-employees-i)


1️⃣ Question Translate

👉 प्रत्येक Project वर काम करणाऱ्या Employees चा Average Experience काढा.

```sql
-- avg(exp_yr), round 2, by project
SELECT project_id,
ROUND(AVG(experience_years),2) AS average_years
from Project AS a 
LEFT JOIN Employee AS b
ON a.employee_id = b.employee_id
GROUP BY project_id
```

[1633. Percentage of Users Attended a Contest](https://leetcode.com/problems/percentage-of-users-attended-a-contest)

1️⃣ Question Translate

👉 प्रत्येक Contest मध्ये किती % Users register झाले ते काढा.

Formula

Percentage  = Registered Users × 100 / Total Users

```sql
-- % desc, contest_id asc, round 2
SELECT 
       contest_id,
       ROUND(COUNT(b.user_id)* 100/
        (SELECT COUNT(*) from Users), 2) AS percentage
FROM Register AS b
GROUP BY b.contest_id
ORDER BY percentage DESC, b.contest_id ASC; 
```

[1211 Queries Quality and Percentage](https://leetcode.com/problems/queries-quality-and-percentage)


1️⃣ Question Translate

प्रत्येक query_name साठी 2 गोष्टी काढायच्या आहेत.

Quality
Poor Query Percentage
2️⃣ Formula 1 : Quality

Question मध्ये Formula दिला आहे.

Quality = AVG(rating / position)
3️⃣ Formula 2 : Poor Query %

Poor म्हणजे

rating < 3


```sql
--quality - avg(rating/position), poor query % - %(rating < 3), round 2


SELECT query_name, 
 ROUND(AVG(rating/position),2) AS quality,
 ROUND(SUM(IF(rating < 3, 1, 0)) * 100 / Count(rating), 2) AS poor_query_percentage

 FROM Queries
 GROUP BY query_name;
 


-- OR

SELECT query_name, 
    ROUND(AVG(rating/position), 2) AS quality, 
    ROUND(SUM(
        CASE WHEN rating < 3 THEN 1 ELSE 0 END
    ) * 100/ COUNT(rating), 2) AS poor_query_percentage
FROM Queries
GROUP BY query_name
```

[1193. Monthly Transactions I](https://leetcode.com/problems/monthly-transactions-i/)

-- 1️⃣ Question Translate

-- प्रत्येक Month + Country साठी काढा:

-- Total Transactions
-- Approved Transactions
-- Total Amount
-- Approved Amount

```sql

SELECT 
 DATE_FORMAT(trans_date, '%Y-%m') AS month,
country, 
COUNT(state) AS trans_count,
SUM(IF(state="approved", 1,0)) AS approved_count,
SUM(amount) AS trans_total_amount,
SUM(IF(state= "approved", amount, 0)) AS approved_total_amount

FROM Transactions

GROUP BY month , country;

-- OR
SELECT DATE_FORMAT(trans_date, '%Y-%m') month, country, 
        COUNT(state) trans_count, 
        SUM(CASE WHEN state = 'approved' THEN 1 ELSE 0 END) approved_count, 
        SUM(amount) trans_total_amount,
        SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) approved_total_amount
FROM Transactions
GROUP BY 1, 2
```

[1174. Immediate Food Delivery II](https://leetcode.com/problems/immediate-food-delivery-ii/)


-- 1️⃣ Question Translate

-- 👉 प्रत्येक Customer चा पहिला Order शोधा.

-- 👉 त्या पहिल्या Order पैकी किती Immediate आहेत त्याची Percentage काढा.

-- 2️⃣ Immediate म्हणजे काय?
-- order_date == customer_pref_delivery_date

-- उदा.

-- Order Date	Preferred Date	Type
-- 1 Aug	1 Aug	✅ Immediate
-- 1 Aug	2 Aug	❌ Schedule

```sql


SELECT
ROUND(
AVG(order_date = customer_pref_delivery_date) * 100,
2
) AS immediate_percentage
FROM Delivery
WHERE (customer_id, order_date) IN
(
SELECT
customer_id,
MIN(order_date)
FROM Delivery
GROUP BY customer_id
);

-- OR
SELECT ROUND(AVG(temp.order_date=temp.customer_pref_delivery_date) * 100, 2) immediate_percentage
FROM (
    SELECT *, RANK() OVER(partition by customer_id ORDER BY order_date) od
    FROM Delivery) temp
WHERE temp.od = 1
```

[550. Game Play Analysis IV](https://leetcode.com/problems/game-play-analysis-iv/)


1️⃣ Question Translate

👉 प्रत्येक Player चा First Login शोधा.

👉 तो Player दुसऱ्या दिवशी (Next Day) परत Login झाला का?

👉 अशा Players ची Fraction काढा.



⭐ Query Flow (याच क्रमाने विचार कर)
Activity Table

        │
        ▼
Find First Login
(MIN + GROUP BY)

        │
        ▼
Next Day
(DATE_ADD)

        │
        ▼
WHERE IN

        │
        ▼
Matched Players
(Logged in next day)

        │
        ▼
COUNT(*)

        │
        ▼
Divide by Total Players

        │
        ▼
ROUND(...,2)


```sql
SELECT
    ROUND(
        COUNT(DISTINCT player_id) /
        (SELECT COUNT(DISTINCT player_id) FROM Activity),
        2
    ) AS fraction
FROM Activity
WHERE (player_id, event_date) IN
(
    SELECT
        player_id,
        DATE_ADD(MIN(event_date), INTERVAL 1 DAY)
    FROM Activity
    GROUP BY player_id
);


```
[2356. Number of Unique Subjects Taught by Each Teacher](https://leetcode.com/problems/number-of-unique-subjects-taught-by-each-teacher)


1️⃣ Question Translate

👉 प्रत्येक Teacher किती वेगवेगळे (Unique) Subjects शिकवतो ते काढा.

⚠️ Department मोजायचा नाही.

```sql
SELECT teacher_id, 
COUNT(DISTINCT subject_id) cnt
FROM Teacher
GROUP BY teacher_id

```

[1141. User Activity for the Past 30 Days I](https://leetcode.com/problems/user-activity-for-the-past-30-days-i/)


1️⃣ Question Translate

👉 2019-07-27 पर्यंतच्या मागील 30 दिवसांतील प्रत्येक दिवसासाठी किती Unique Users active होते ते काढा.

⚠️ User ने एका दिवशी 10 activities केल्या तरी 1 active user म्हणूनच मोजायचा.


```sql
SELECT
    activity_date AS day,
    COUNT(DISTINCT user_id) AS active_users
FROM Activity
WHERE activity_date BETWEEN '2019-06-28' AND '2019-07-27'
GROUP BY activity_date;
```

[1070. Product Sales Analysis III
](https://leetcode.com/problems/product-sales-analysis-iii/)


1️⃣ Question Translate

👉 प्रत्येक Product चा First Sale Year शोधा.

👉 त्या First Year मधील Sale ची quantity आणि price द्या.

⚠️ सगळ्या Sales नाहीत, फक्त First Year च्या Sales.

```sql
SELECT
    a.product_id,
    a.year AS first_year,
    a.quantity,
    a.price
FROM Sales AS a
JOIN
(
    SELECT
        product_id,
        MIN(year) AS year
    FROM Sales
    GROUP BY product_id
) AS b
ON a.product_id = b.product_id
AND a.year = b.year;


```
[596. Classes More Than 5 Students](https://leetcode.com/problems/classes-more-than-5-students/)


1️⃣ Question Translate

👉 ज्या Class मध्ये 5 किंवा त्यापेक्षा जास्त Students आहेत, त्या Class चे नाव द्या.

```sql
SELECT
    class
FROM Courses
GROUP BY class
HAVING COUNT(student) >= 5;
```

[1729. Find Followers Count](https://leetcode.com/problems/find-followers-count/)

1️⃣ Question Translate

👉 प्रत्येक User चे किती Followers आहेत ते काढा.

```sql
SELECT
    user_id,
    COUNT(follower_id) AS followers_count
FROM Followers
GROUP BY user_id
ORDER BY user_id;
```

[619. Biggest Single Number](https://leetcode.com/problems/biggest-single-number/)

1️⃣ Question Translate

👉 जो Number फक्त एकदाच आला आहे (Single Number), त्यांच्यामधला सर्वात मोठा Number द्या.

```sql
SELECT
    MAX(num) AS num
FROM
(
    SELECT
        num
    FROM MyNumbers
    GROUP BY num
    HAVING COUNT(*) = 1
) AS a;
```

[1045. Customers Who Bought All Products](https://leetcode.com/problems/customers-who-bought-all-products/)

1️⃣ Question Translate

👉 ज्या Customer ने Product Table मधले सर्व Products विकत घेतले आहेत, त्यांचा customer_id द्या.

```sql
SELECT
    customer_id
FROM Customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) =
(
    SELECT COUNT(*)
    FROM Product
);
```
[1731. The Number of Employees Which Report to Each Employee
](https://leetcode.com/problems/the-number-of-employees-which-report-to-each-employee/)

1️⃣ Question Translate

👉 प्रत्येक Manager साठी:

किती Employees त्याला report करतात?
त्या Employees चं Average Age किती आहे?

```sql
SELECT
    a.employee_id,
    a.name,
    COUNT(b.employee_id) AS reports_count,
    ROUND(AVG(b.age)) AS average_age
FROM Employees AS a
JOIN Employees AS b
ON a.employee_id = b.reports_to
GROUP BY
    a.employee_id,
    a.name
ORDER BY
    a.employee_id;
    
```
[1789. Primary Department for Each Employee
](https://leetcode.com/problems/primary-department-for-each-employee/?envType=study-plan-v2&envId=top-sql-50)


Question काय म्हणतो?

प्रत्येक Employee साठी एकच Department Return करायचा.

पण कोणता?

Rule आहेत.

Rule 1

जर

primary_flag = 'Y'

असेल

👉 तो Department Return कर.

Rule 2

जर Employee फक्त एका Department मध्ये असेल

👉 तोच Department Return कर.

```sql
SELECT employee_id,department_id
FROM Employee
WHERE primary_flag = 'Y' OR employee_id IN
    (SELECT employee_id
     FROM employee
     GROUP BY employee_id
     HAVING COUNT(department_id) = 1
    )

-- OR

SELECT employee_id, department_id
FROM Employee 
WHERE primary_flag = 'Y'
UNION
SELECT employee_id, department_id
FROM Employee
GROUP BY employee_id
HAVING COUNT(employee_id)=1
```

[610. Triangle Judgement](https://leetcode.com/problems/triangle-judgement/)


1️⃣ Question Translate

👉 प्रत्येक Row साठी Check करा:

हे 3 sides Triangle बनवू शकतात का?

जर हो

↓

Yes

नाही

↓

No
2️⃣ Triangle Rule

Triangle बनण्यासाठी

तीनही Conditions True पाहिजेत.

x + y > z

x + z > y

y + z > x

```sql
SELECT
    x,
    y,
    z,
    IF(
        x + y > z
        AND x + z > y
        AND y + z > x,
        'Yes',
        'No'
    ) AS triangle
FROM Triangle;
```

[180. Consecutive Numbers
](https://leetcode.com/problems/consecutive-numbers/)


1️⃣ Question Translate

👉 सलग (Consecutive) 3 वेळा आलेले Number शोधा.

⚠️ फक्त 3 वेळा Number आला म्हणजे चालणार नाही.

सलग (Continuous IDs) पाहिजेत.
```
PATTERN

SELECT DISTINCT column
FROM
(
    SELECT
        column,
        LAG(column) OVER(ORDER BY id_column) AS prev,
        LEAD(column) OVER(ORDER BY id_column) AS next
    FROM table
) AS t
WHERE column = prev
AND column = next;


```sql
SELECT DISTINCT
    num AS ConsecutiveNums
FROM
(
    SELECT
        id,
        num,
        LEAD(num) OVER(ORDER BY id) AS next,
        LAG(num) OVER(ORDER BY id) AS prev
    FROM Logs
) AS a
WHERE num = next
AND num = prev;
```


[1164. Product Price at a Given Date](https://leetcode.com/problems/product-price-at-a-given-date)

1️⃣ Question Translate

2019-08-16 रोजी प्रत्येक Product ची Price काय होती?

⚠️ 16 नंतरचे Changes Ignore करायचे.

⚠️ जर 16 पर्यंत Price Change झालाच नसेल

↓

Default Price = 10


```sql
SELECT product_id, new_price AS price
FROM products
WHERE (product_id, change_date) IN
(   
    SELECT product_id, MAX(change_date)
    FROM products
    WHERE change_date <= '2019-08-16'
    GROUP BY product_id
)
UNION

SELECT product_id, 10 AS price
FROM products
WHERE product_id NOT IN
(
  SELECT product_id
  FROM products
  WHERE change_date <= '2019-08-16'
)
```
[1204. Last Person to Fit in the Bus](https://leetcode.com/problems/last-person-to-fit-in-the-bus/)


1️⃣ Question Translate

Bus ची Weight Limit = 1000 kg

लोक turn नुसार Bus मध्ये चढतात.

👉 1000 kg च्या आत बसू शकणारा शेवटचा Person कोण?
```sql
-- 1000 kg limit
-- name of last person

SELECT person_name
FROM
(
    SELECT
        person_name,
        turn,
        SUM(weight) OVER(ORDER BY turn) AS total_weight
    FROM Queue
) AS a
WHERE total_weight <= 1000
ORDER BY turn DESC
LIMIT 1;
```

[1907. Count Salary Categories](https://leetcode.com/problems/count-salary-categories/)

⚠️ Output मध्ये तिन्ही Categories हव्यात, जरी एखाद्या Category मध्ये 0 Accounts असले तरी.

1️⃣ Question Translate

प्रत्येक Account ला Salary Category मध्ये टाका.

Rules:

income < 20000

↓

Low Salary
20000 <= income <= 50000

↓

Average Salary
income > 50000

↓

High Salary
```sql

SELECT 'Low Salary' AS category, 
SUM(IF(income<20000,1,0)) AS accounts_count 
FROM Accounts

UNION

SELECT 'Average Salary' AS category,
SUM(IF(income>=20000 AND income<=50000,1,0)) AS accounts_count 
FROM Accounts

UNION

SELECT 'High Salary' AS category, 
SUM(IF(income>50000,1,0)) AS accounts_count 
FROM Accounts
```
OR 

```sql
SELECT
    'Low Salary' AS category,
    COUNT(*) AS accounts_count
FROM Accounts
WHERE income < 20000

UNION ALL

SELECT
    'Average Salary',
    COUNT(*)
FROM Accounts
WHERE income BETWEEN 20000 AND 50000

UNION ALL

SELECT
    'High Salary',
    COUNT(*)
FROM Accounts
WHERE income > 50000;

 ```
 [1978. Employees Whose Manager Left the Company](https://leetcode.com/problems/employees-whose-manager-left-the-company)

 1️⃣ Question Translate

असे Employees शोधा ज्यांचे:

✅ Salary < 30000
✅ Manager कंपनी सोडून गेला आहे.

Manager कंपनी सोडून गेला म्हणजे

👉 त्याचा manager_id Employees Table मध्ये नाही.
```sql
SELECT employee_id
FROM Employees
WHERE manager_id NOT IN (
    SELECT employee_id 
    FROM Employees
)
AND salary < 30000
ORDER BY employee_id
```
[626. Exchange Seats](https://leetcode.com/problems/exchange-seats/)

1️⃣ Question Translate

प्रत्येक 2 Students चे Seats Swap करा.

1 ↔ 2

3 ↔ 4

5 ↔ 6

पण

जर शेवटी एकच Student उरला

↓

त्याला Swap करायचं नाही.

SYNTAX --

LEAD(column, offset, default) OVER(ORDER BY id)
column → कोणता Column हवा? (student)
offset → किती Row पुढे? (1)
default → पुढची Row नसेल तर काय Return करायचं? (student)
```sql
-- id, student
-- swap every two consecutives
-- num(students): odd? no swap for last one

SELECT
 id, 
IF(
    id % 2 != 0,
    LEAD(student, 1, student) OVER (ORDER BY id),
    LAG(student) OVER (ORDER BY id)
) AS student
FROM Seat
```

[1327. List the Products Ordered in a Period](https://leetcode.com/problems/list-the-products-ordered-in-a-period/)
```sql
-- name, amt
-- >= 100 units, feb 2020

SELECT p.product_name, SUM(o.unit) AS unit
FROM Products p
LEFT JOIN Orders o
ON p.product_id = o.product_id
WHERE DATE_FORMAT(order_date, '%Y-%m') = '2020-02'
GROUP BY p.product_name
HAVING SUM(o.unit) >= 100
```

[1484. Group Sold Products By The Date](https://leetcode.com/problems/group-sold-products-by-the-date/)
```sql
SELECT sell_date, 
COUNT(DISTINCT product) AS num_sold,
GROUP_CONCAT(DISTINCT product) AS 'products' 
FROM Activities
GROUP BY sell_date
ORDER BY sell_date
```

[1341. Movie Rating](https://leetcode.com/problems/movie-rating/)

User ने किती Movies Rate केले?

MovieRating Table

user	movie
Daniel	Avengers
Daniel	Frozen 2
Daniel	Joker
Monica	Avengers
Monica	Frozen 2
Monica	Joker
Maria	Avengers
Maria	Frozen 2
James	Avengers

Count करू.

User	Movies Rated
Daniel	3
Monica	3
Maria	2
James	1

Maximum = 3

पण दोन जण आहेत.

Daniel
Monica

Question म्हणतो

Tie?

↓

Alphabetically Smaller
Daniel ✅

```sql
(SELECT name AS results
FROM Users u
LEFT JOIN MovieRating mr
ON u.user_id = mr.user_id
GROUP BY name
ORDER BY COUNT(rating) DESC, name ASC
LIMIT 1)

UNION ALL

(SELECT title
FROM Movies m
LEFT JOIN MovieRating mr
ON m.movie_id = mr.movie_id
WHERE DATE_FORMAT(created_at, '%Y-%m') = '2020-02'
GROUP BY title
ORDER BY AVG(rating) DESC, title ASC
LIMIT 1 
)
```

[1321. Restaurant Growth](https://leetcode.com/problems/restaurant-growth/)

-- pay: last 7 days (today inclusive) - avg.amt (round, 2)

SELECT visited_on, amount, ROUND(amount/7, 2) AS average_amount
FROM (
    SELECT DISTINCT visited_on,
    SUM(amount) OVER(ORDER BY visited_on RANGE BETWEEN INTERVAL 6 DAY PRECEDING AND CURRENT ROW) AS amount,
    MIN(visited_on) OVER() day_1
    FROM Customer
) t
WHERE visited_on >= day_1+6;

```sql
-- pay: last 7 days (today inclusive) - avg.amt (round, 2)

SELECT
    visited_on,
    amount,
    ROUND(amount / 7, 2) AS average_amount
FROM
(
    SELECT DISTINCT
        visited_on,

        SUM(amount) OVER(
            ORDER BY visited_on
            RANGE BETWEEN INTERVAL 6 DAY PRECEDING
            AND CURRENT ROW
        ) AS amount,

        MIN(visited_on) OVER() AS first_day

    FROM Customer
) t
WHERE visited_on >= first_day + 6;
```


[602. Friend Requests II: Who Has the Most Friends](https://leetcode.com/problems/friend-requests-ii-who-has-the-most-friends)

```sql
-- `union` selects only unique vals, so we use `union all` here

WITH CTE AS (
    SELECT requester_id AS id FROM RequestAccepted
    UNION ALL
    SELECT accepter_id AS id FROM RequestAccepted
)

SELECT id, COUNT(id) AS num
FROM CTE
GROUP BY id
ORDER BY num DESC
LIMIT 1
```

[585. Investments in 2016](https://leetcode.com/problems/investments-in-2016)
```sql
SELECT
    ROUND(SUM(tiv_2016),2) AS tiv_2016
FROM insurance
WHERE tiv_2015 IN (SELECT tiv_2015 FROM insurance GROUP BY tiv_2015 HAVING COUNT(*) > 1)
AND (lat,lon) IN (SELECT lat,lon FROM insurance GROUP BY lat,lon HAVING COUNT(*) = 1)
```

[185. Department Top Three Salaries](https://leetcode.com/problems/department-top-three-salaries)
```sql
WITH RankedSalaries AS 
(SELECT 
    e.Id AS employee_id,
    e.name AS employee,
    e.salary,
    e.departmentId,
    DENSE_RANK() OVER (PARTITION BY e.departmentId ORDER BY e.salary DESC) AS salary_rank 
FROM Employee e)
SELECT d.name AS Department,
r.employee,
r.salary
FROM Department d
JOIN RankedSalaries r ON r.departmentId = d.id
WHERE r.salary_rank <=3;
```


[1667. Fix Names in a Table](https://leetcode.com/problems/fix-names-in-a-table)
```sql
SELECT user_id, CONCAT(UPPER(LEFT(name, 1)), LOWER(RIGHT(name, LENGTH(name)-1))) AS name
FROM Users
ORDER BY user_id
```

[1527. Patients With a Condition](https://leetcode.com/problems/patients-with-a-condition)
```sql
SELECT patient_id, patient_name, conditions 
FROM patients 
WHERE conditions LIKE '% DIAB1%' 
OR conditions LIKE 'DIAB1%'
```

[196. Delete Duplicate Emails](https://leetcode.com/problems/delete-duplicate-emails)
```sql
DELETE p
FROM Person p, Person q
WHERE p.id > q.id
AND q.Email = p.Email
```

[176. Second Highest Salary](https://leetcode.com/problems/second-highest-salary)
```sql
SELECT
(SELECT DISTINCT Salary 
FROM Employee
ORDER BY Salary DESC
LIMIT 1 OFFSET 1)
AS SecondHighestSalary

-- HINT: subquery is used to return null if there is no SecondHighestSalary
```


 
[1517. Find Users With Valid E-Mails](https://leetcode.com/problems/find-users-with-valid-e-mails)
```sql
SELECT *
FROM Users
WHERE mail REGEXP '^[A-Za-z][A-Za-z0-9_\.\-]*@leetcode\\.com$'
```


