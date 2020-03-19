Window Functions
--------

# Window Function Basic

## MySQL 8.0 Window Functions Intro

MySQL has supported window functions since version 8.0. The window functions allow you to solve query problems in new, 
easier ways, and with better performance.


**Window functions?**
- 행과 행간의 관계를 쉽게 정의하기 위해 만든 함수가 바로 WINDOW FUNCTION이다.
- 윈도우 함수를 활용하면 복잡한 프로그램을 하나의 SQL 문장으로 쉽게 해결할 수 있다.
- 분석 함수(ANALYTIC FUNCTION)나 순위 함수(RANK FUNCTION)로도 알려져 있는 윈도우 함수 (ANSI/ISOSQL 표준은 WINDOW FUNCTION이란 용어를 사용함)는 데이터웨어하우스에서 발전한 기능이다.
- WINDOW 함수는 다른 함수와는 달리 중첩(NEST)해서 사용하지는 못하지만, 서브쿼리에서는 사용할 수 있다.
- 출처 : http://www.gurubee.net/lecture/2382


**Window functions 종류**
- CUME_DIST : Calculates the cumulative distribution of a value in a set of values.
- DENSE_RANK : Assigns a rank to every row within its partition based on the ORDER BY clause. It assigns the same rank to the rows with equal values. If two or more rows have the same rank, then there will be no gaps in the sequence of ranked values.
- FIRST_VALUE : Returns the value of the specified expression with respect to the first row in the window frame.
- LAG : Returns the value of the Nth row before the current row in a partition. It returns NULL if no preceding row exists.
- LAST_VALUE : Returns the value of the specified expression with respect to the last row in the window frame.
- LEAD : Returns the value of the Nth row after the current row in a partition. It returns NULL if no subsequent row exists.
- NTH_VALUE : Returns value of argument from Nth row of the window frame
- NTILE : Distributes the rows for each window partition into a specified number of ranked groups.
- PERCENT_RANK : Calculates the percentile rank of a row in a partition or result set
- RANK : Similar to the DENSE_RANK() function except that there are gaps in the sequence of ranked values when two or more rows have the same rank.
- ROW_NUMBER : Assigns a sequential integer to every row within its partition


## 기본적인 window function 예제
- https://www.mysqltutorial.org/mysql-window-functions/ 를 바탕으로 공부하였다.
- 아래 쿼리에서 사용하는 sales라는 테이블은 위 링크를 참고하여 만들었다.

````sql
SELECT 
    fiscal_year, 
    sales_employee,
    sale,
    SUM(sale) OVER (PARTITION BY fiscal_year) as total_sales
FROM
    sales;
````

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-Window-Function-SUM-window-function.png)


fiscal_year을 기준으로 partition 하는 것이 아니라, sales_employee를 기준으로 다시 쿼리를 작성해보았다.

```sql
select
  fiscal_year,
  sales_employee,
  sale,
  sum(sale) over (partition by sales_employee) as total_sales
from sales;
```

이름 순으로 묶여서 정리가 되었다.

```bash
#,fiscal_year,sales_employee,sale,total_sales
1,2016,Alice,150.00,450.00
2,2017,Alice,100.00,450.00
3,2018,Alice,200.00,450.00
4,2016,Bob,100.00,450.00
5,2017,Bob,150.00,450.00
6,2018,Bob,200.00,450.00
7,2016,John,200.00,600.00
8,2017,John,150.00,600.00
9,2018,John,250.00,600.00
```

## window function 구조
```bash
window_function_name(expression) 
    OVER (
        [partition_defintion]
        [order_definition]
        [frame_definition]
    )
```

- First, specify the window function name followed by an expression.
- Second, specify the OVER clause which has three possible elements
  - partition definition - 이해함
  - order definition - 이해함
  - [ ] frame definition - 이해 못 함


![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/09/mysql-window-functions-frame-clause-bound.png)


## Window functions 종류

### CUME_DIST()
The CUME_DIST() is a window function that returns the cumulative distribution of a value within a set of values. 
It represents the number of rows with values less than or equal to that row’s value divided by the total number of rows.
- 요약하면 누적분포를 구할 수 있는 함수


The returned value of the CUME_DIST() function is greater than zero and less than or equal one (0 < CUME_DIST() <= 1). 
The repeated column values receive the same CUME_DIST() value.
- 리턴되는 값은 0보다 크고, 1보다는 작거나 같은 값을 준다.

CUME_DIST()의 기본적인 구조

````sql
CUME_DIST() OVER (
    PARTITION BY expr, ...
    ORDER BY expr [ASC | DESC], ...
)
````

학습하기 위해서 아래와 같은 테이블을 추가한다.

```sql
CREATE TABLE scores (
    name VARCHAR(20) PRIMARY KEY,
    score INT NOT NULL
);

INSERT INTO
    scores(name, score)
VALUES
    ('Smith',81),
    ('Jones',55),
    ('Williams',55),
    ('Taylor',62),
    ('Brown',62),
    ('Davies',84),
    ('Evans',87),
    ('Wilson',72),
    ('Thomas',72),
    ('Johnson',100);
```

아래와 같은 쿼리를 실행해보았다.
- ROW_NUMBER()는 추후 알아본다.
- score 컬럼에 대해 CUME_DIST()를 확인할 수 있다. 

```sql
SELECT
    name,
    score,
    ROW_NUMBER() OVER (ORDER BY score) row_num,
    CUME_DIST() OVER (ORDER BY score) cume_dist_val
FROM
    scores;   
```

```bash
#,name,score,row_num,cume_dist_val
1,Jones,55,1,0.2
2,Williams,55,2,0.2
3,Brown,62,3,0.4
4,Taylor,62,4,0.4
5,Thomas,72,5,0.6
6,Wilson,72,6,0.6
7,Smith,81,7,0.7
8,Davies,84,8,0.8
9,Evans,87,9,0.9
10,Johnson,100,10,1
```


CUME_DIST() 는 어떤 원리로 계산이 될까?

> For the first row, the function finds the number of rows in the result set, which have value less than or equal to 55. 
The result is 2. Then CUME_DIST() function divides 2 by the total number of rows which is 10: 2/10. 
the result is 0.2 or 20%. The same logic is applied to the second row.

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-CUME_DIST-Function-First-Row.png)


- 정리하면 같은 값을 가지는 개수를 구한 후, 전체 개수에서 그 비율을 구한다.
- 10개 중 최하값이 2개가 존재하므로 2/10 = 0.2 로 정리할 수 있다.

> For the third row, the function finds the number of rows with the values less than or equal to 62. 
There are four rows. Then the result of the CUME_DIST() function is: 4/10 = 0.4 which is 40%.

- 62까지 살펴보면 총 4개이고, 전체 10개중 4개는 40%이다.


### DENSE_RANK()
The DENSE_RANK() is a window function that assigns a rank to each row 
within a partition or result set with no gaps in ranking values.

The rank of a row is increased by one from the number of distinct rank values which come before the row.

- 말을 보면 무슨 말이지 생각할 수 있으나, 생각해보면 다음과 같다.
- partition이나 result set의 각각의 row에 랭크를 할당하는 함수이다.
- 예제를 보면 좀 더 이해가 쉽다.


```sql
DENSE_RANK() OVER (
    PARTITION BY <expression>[{,<expression>...}]
    ORDER BY <expression> [ASC|DESC], [{,<expression>...}]
)
```


```sql
SELECT
    sales_employee,
    fiscal_year,
    sale,
    DENSE_RANK() OVER (
      PARTITION BY fiscal_year
      ORDER BY sale DESC
      ) sales_rank
FROM
    sales;
```

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-DENSE_RANK-Assign-Rank-to-sales-employees.png)


step by step
- First, the PARTITION BY clause divided the result sets into partitions using fiscal year.
  - 빨간, 보라, 노랑으로 구간 나누기
- Second, the ORDER BY clause specified the order of the sales employees by sales in descending order.
  - 각 색깔별 박스 내에서 내림차순 정렬하기
- Third, the DENSE_RANK() function is applied to each partition with the rows order specified by the ORDER BY clause.
  - 정렬된 것을 기준으로 각각의 partition에 랭크함수를 적용


### FIRST_VALUE()

The FIRST_VALUE() is a window function that allows you to select the first row of a window frame, partition, or result set.
- window frame, partition, or result set의 가장 첫 번째 row에 오는 값을 리턴하는 함수이다.
- 이것도 예제를 보면 이해가 쉽다.


```sql
FIRST_VALUE(expression) OVER (
        [partition_clause]
        [order_clause]
        [frame_clause]
)
```


```sql
CREATE TABLE overtime (
    employee_name VARCHAR(50) NOT NULL,
    department VARCHAR(50) NOT NULL,
    hours INT NOT NULL,
    PRIMARY KEY (employee_name , department)
);
INSERT INTO overtime(employee_name, department, hours)
VALUES('Diane Murphy','Accounting',37),
('Mary Patterson','Accounting',74),
('Jeff Firrelli','Accounting',40),
('William Patterson','Finance',58),
('Gerard Bondur','Finance',47),
('Anthony Bow','Finance',66),
('Leslie Jennings','IT',90),
('Leslie Thompson','IT',88),
('Julie Firrelli','Sales',81),
('Steve Patterson','Sales',29),
('Foon Yue Tseng','Sales',65),
('George Vanauf','Marketing',89),
('Loui Bondur','Marketing',49),
('Gerard Hernandez','Marketing',66),
('Pamela Castillo','SCM',96),
('Larry Bott','SCM',100),
('Barry Jones','SCM',65);
```

```sql
SELECT
    employee_name,
    hours,
    FIRST_VALUE(employee_name) OVER (
        ORDER BY hours
    ) least_over_time
FROM
    overtime;
```


![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-FIRST_VALUE-Function-Example.png)

- 이해가 잘 된다... :)
- hours로 정렬하였을 때 employee_name 이라는항목에서 가장 첫 번째 값을 least_over_time으로 할당한다.
 

또 다른 예제는 구간 별로 설정되었을 때 각 구간의 첫 번쨰 값을 할당하는 방식을 안내한다.

```sql
SELECT
    employee_name,
    department,
    hours,
    FIRST_VALUE(employee_name) OVER (
        PARTITION BY department
        ORDER BY hours
    ) least_over_time
FROM
    overtime;
```

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-FIRST_VALUE-Function-Over-Partition-Example.png)

- department 별로 partition을 나눈 후, hours를 기준으로 정렬한 뒤 각 department의 첫 번째 값을 할당하였다.


### LAG()

The LAG() function is a window function that allows you to look back a number of rows and access data of that row from the current row.
- LAG()는 이전 행을 현재 행에 배치시킬 수 있는 기능을 하는 함수이다.
- 사실 이렇게 말을 해도 잘 와닿지는 않은데 결과를 살펴보면 단번에 어떤 의미인지 알 수 있다.

```bash
LAG(<expression>[,offset[, default_value]]) OVER (
    PARTITION BY expr,...
    ORDER BY expr [ASC|DESC],...
)
```


The LAG() function ...
- returns the value of the expression from the row that precedes the current row by offset number of rows within its partition or result set.

**offset**
- The offset is the number of rows back from the current row from which to get the value. 
The offset must be zero or a literal positive integer. If offset is zero, then the LAG() function evaluates the expression for the current row. 
If you don’t specify the offset, then the LAG() function uses one by default.
- offset은 0이나 양수여야 한다.
- 0일 경우에는 이전 행이 아닌 자기 자신을 구하고자 하는 행으로 할당한다.
- 양수일 경우 해당 숫자만큼 내려가서 구하고자 하는 행에 값을 할당한다.


````sql
WITH productline_sales AS (
    SELECT productline,
           YEAR(orderDate) order_year,
           ROUND(SUM(quantityOrdered * priceEach),0) order_value
    FROM orders
    INNER JOIN orderdetails USING (orderNumber)
    INNER JOIN products USING (productCode)
    GROUP BY productline, order_year
)
SELECT
    productline,
    order_year,
    order_value,
    LAG(order_value, 1) OVER (
        PARTITION BY productLine
        ORDER BY order_year
    ) prev_year_order_value
FROM
    productline_sales;
````

```bash
#,productline,order_year,order_value,prev_year_order_value
1,Classic Cars,2003,1374832,
2,Classic Cars,2004,1763137,1374832
3,Classic Cars,2005,715954,1763137
4,Motorcycles,2003,348909,
5,Motorcycles,2004,527244,348909
6,Motorcycles,2005,245273,527244
7,Planes,2003,309784,
8,Planes,2004,471971,309784
9,Planes,2005,172882,471971
10,Ships,2003,222182,
11,Ships,2004,337326,222182
12,Ships,2005,104490,337326
13,Trains,2003,65822,
14,Trains,2004,96286,65822
15,Trains,2005,26425,96286
16,Trucks and Buses,2003,376657,
17,Trucks and Buses,2004,465390,376657
18,Trucks and Buses,2005,182066,465390
19,Vintage Cars,2003,619161,
20,Vintage Cars,2004,854552,619161
21,Vintage Cars,2005,323846,854552
```

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-LAG-function-current-and-previous-year-example.png)

example summary
- First, we used a common table expression to get the order value of every product in every year.
- Then, we divided the products using the product lines into partitions, sorted each partition by order year, and applied the LAG() function to each sorted partition to get the previous year’s order value of each product.


```sql
-- change LAG(order_value, 1->2)
WITH productline_sales AS (
    SELECT productline,
           YEAR(orderDate) order_year,
           ROUND(SUM(quantityOrdered * priceEach),0) order_value
    FROM orders
    INNER JOIN orderdetails USING (orderNumber)
    INNER JOIN products USING (productCode)
    GROUP BY productline, order_year
)
SELECT
    productline,
    order_year,
    order_value,
    LAG(order_value, 2) OVER (
        PARTITION BY productLine
        ORDER BY order_year
    ) prev_year_order_value
FROM
    productline_sales;
```

```bash
#,productline,order_year,order_value,prev_year_order_value
1,Classic Cars,2003,1374832,
2,Classic Cars,2004,1763137,
3,Classic Cars,2005,715954,1374832
4,Motorcycles,2003,348909,
5,Motorcycles,2004,527244,
6,Motorcycles,2005,245273,348909
7,Planes,2003,309784,
8,Planes,2004,471971,
9,Planes,2005,172882,309784
10,Ships,2003,222182,
11,Ships,2004,337326,
12,Ships,2005,104490,222182
13,Trains,2003,65822,
14,Trains,2004,96286,
15,Trains,2005,26425,65822
16,Trucks and Buses,2003,376657,
17,Trucks and Buses,2004,465390,
18,Trucks and Buses,2005,182066,376657
19,Vintage Cars,2003,619161,
20,Vintage Cars,2004,854552,
21,Vintage Cars,2005,323846,619161
```


### LAST_VALUE()

- The LAST_VALUE() function is a window function that allows you to select the last row in an ordered set of rows.
- 각 partition의 마지막 값을 구한다고 생각하면 쉽다.
- FIRST_VALUE()의 원리를 기억하자.


```sql
CREATE TABLE overtime (
    employee_name VARCHAR(50) NOT NULL,
    department VARCHAR(50) NOT NULL,
    hours INT NOT NULL,
     PRIMARY KEY (employee_name , department)
);
INSERT INTO overtime(employee_name, department, hours)
VALUES('Diane Murphy','Accounting',37),
('Mary Patterson','Accounting',74),
('Jeff Firrelli','Accounting',40),
('William Patterson','Finance',58),
('Gerard Bondur','Finance',47),
('Anthony Bow','Finance',66),
('Leslie Jennings','IT',90),
('Leslie Thompson','IT',88),
('Julie Firrelli','Sales',81),
('Steve Patterson','Sales',29),
('Foon Yue Tseng','Sales',65),
('George Vanauf','Marketing',89),
('Loui Bondur','Marketing',49),
('Gerard Hernandez','Marketing',66),
('Pamela Castillo','SCM',96),
('Larry Bott','SCM',100),
('Barry Jones','SCM',65);
```

```sql
SELECT
    employee_name,
    hours,
    LAST_VALUE(employee_name) OVER (
        ORDER BY hours
        RANGE BETWEEN
            UNBOUNDED PRECEDING AND
            UNBOUNDED FOLLOWING
    ) highest_overtime_employee
FROM
    overtime;
```
 
 
```bash
#,employee_name,hours,highest_overtime_employee
1,Steve Patterson,29,Larry Bott
2,Diane Murphy,37,Larry Bott
3,Jeff Firrelli,40,Larry Bott
4,Gerard Bondur,47,Larry Bott
5,Loui Bondur,49,Larry Bott
6,William Patterson,58,Larry Bott
7,Barry Jones,65,Larry Bott
8,Foon Yue Tseng,65,Larry Bott
9,Anthony Bow,66,Larry Bott
10,Gerard Hernandez,66,Larry Bott
11,Mary Patterson,74,Larry Bott
12,Julie Firrelli,81,Larry Bott
13,Leslie Thompson,88,Larry Bott
14,George Vanauf,89,Larry Bott
15,Leslie Jennings,90,Larry Bott
16,Pamela Castillo,96,Larry Bott
17,Larry Bott,100,Larry Bott
```

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-LAST_VALUE-example.png)


```sql
SELECT
    employee_name,
    department,
    hours,
    LAST_VALUE(employee_name) OVER (
        PARTITION BY department
        ORDER BY hours
        RANGE BETWEEN
        UNBOUNDED PRECEDING AND
            UNBOUNDED FOLLOWING
    ) most_overtime_employee
FROM
    overtime;
```


```bash
#,employee_name,department,hours,most_overtime_employee
1,Diane Murphy,Accounting,37,Mary Patterson
2,Jeff Firrelli,Accounting,40,Mary Patterson
3,Mary Patterson,Accounting,74,Mary Patterson
4,Gerard Bondur,Finance,47,Anthony Bow
5,William Patterson,Finance,58,Anthony Bow
6,Anthony Bow,Finance,66,Anthony Bow
7,Leslie Thompson,IT,88,Leslie Jennings
8,Leslie Jennings,IT,90,Leslie Jennings
9,Loui Bondur,Marketing,49,George Vanauf
10,Gerard Hernandez,Marketing,66,George Vanauf
11,George Vanauf,Marketing,89,George Vanauf
12,Steve Patterson,Sales,29,Julie Firrelli
13,Foon Yue Tseng,Sales,65,Julie Firrelli
14,Julie Firrelli,Sales,81,Julie Firrelli
15,Barry Jones,SCM,65,Larry Bott
16,Pamela Castillo,SCM,96,Larry Bott
17,Larry Bott,SCM,100,Larry Bott
```

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-LAST_VALUE-OVER-partitions-example.png)


### LEAD()
The LEAD() function is a window function that allows you to look forward a number of rows and access data of that row from the current row.

Similar to the LAG() function, the LEAD() function is very useful for calculating 
the difference between the current row and the subsequent row within the same result set.
- LAG() function의 반대로 생각하면 이해하기 쉽다.
- 특히 날짜 차이 계산을 할 때 이용하면 유용하다.


```sql
LEAD(<expression>[,offset[, default_value]]) OVER (
    PARTITION BY (expr)
    ORDER BY (expr)
)
```

**offset**
- The offset is the number of rows forward from the current row from which to obtain the value.
- The offset must be a non-negative integer. If offset is zero, then the LEAD() function evaluates the expression for the current row.
- In case you omit offset, then the LEAD() function uses one by default.


```sql
SELECT
    customerName,
    orderDate,
    LEAD(orderDate,1) OVER (
        PARTITION BY customerNumber
        ORDER BY orderDate ) nextOrderDate
FROM
    orders
INNER JOIN customers USING (customerNumber);
```

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-LEAD-Function-Example.png)



### Nth value
The NTH_VALUE() is a window function that allows you to get a value from the Nth row in an ordered set of rows.
- 정렬된 row 내에서 N 번째 값을 할당할 수 있는 함수

```sql
NTH_VALUE(expression, N)
FROM FIRST
OVER (
    partition_clause
    order_clause
    frame_clause
)
```


- The NTH_VALUE() function returns the value of expression from the Nth row of the window frame. 
If that Nth row does not exist, the function returns NULL. N must be a positive integer e.g., 1, 2, and 3.
- The FROM FIRST instructs the NTH_VALUE() function to begin calculation at the first row of the window frame.
- Note that SQL standard supports both FROM FIRST and FROM LAST. However, MySQL only supports FROM FIRST. 
If you want to simulate the effect of FROM LAST, then you can use the ORDER BY in the over_clause to sort the result set in reverse order.


```sql
-- make a example table
CREATE TABLE basic_pays(
    employee_name VARCHAR(50) NOT NULL,
    department VARCHAR(50) NOT NULL,
    salary INT NOT NULL,
    PRIMARY KEY (employee_name , department)
);

-- insert example data
INSERT INTO
    basic_pays(employee_name,
               department,
               salary)
VALUES
    ('Diane Murphy','Accounting',8435),
    ('Mary Patterson','Accounting',9998),
    ('Jeff Firrelli','Accounting',8992),
    ('William Patterson','Accounting',8870),
    ('Gerard Bondur','Accounting',11472),
    ('Anthony Bow','Accounting',6627),
    ('Leslie Jennings','IT',8113),
    ('Leslie Thompson','IT',5186),
    ('Julie Firrelli','Sales',9181),
    ('Steve Patterson','Sales',9441),
    ('Foon Yue Tseng','Sales',6660),
    ('George Vanauf','Sales',10563),
    ('Loui Bondur','SCM',10449),
    ('Gerard Hernandez','SCM',6949),
    ('Pamela Castillo','SCM',11303),
    ('Larry Bott','SCM',11798),
    ('Barry Jones','SCM',10586);
```

```sql
SELECT
    employee_name,
    salary,
    NTH_VALUE(employee_name, 2) OVER  (
        ORDER BY salary DESC
    ) second_highest_salary
FROM
    basic_pays;
```

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-NTH_VALUE-Function-Example-1.png)



```sql
SELECT
    employee_name,
    department,
    salary,
    NTH_VALUE(employee_name, 2) OVER  (
        PARTITION BY department
        ORDER BY salary DESC
        RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
    ) second_highest_salary
FROM
    basic_pays;
```

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-NTH_VALUE-Function-OVER-partition-example.png)


### NTILE

The MySQL NTILE() function divides the rows in a sorted partition into a specific number of groups. Each group is assigned a bucket number starting at one. 
For each row, the NTILE() function returns a bucket number representing the group to which the row belongs.
- 이게 무슨 말일까?... 고민이 되겠지만...
- NTILE(숫자)을 설정하면 설정된 숫자만큼 그룹을 나눈다.
  - 그 다음에 각 그룹에 번호를 할당한다.
  - 전체 숫자가 지정된 숫자로 딱 나누어 떨어지지 않으면 번호 할당이 가장 먼저된 그룹이 가장 많은 비율을 차지하는 구조이다.
- Partition 작업을 하는 것과 어떤 차이가 있는지는 아래 예제를 통해 이해할 수 있다.



```sql
NTILE(n) OVER (
    PARTITION BY <expression>[{,<expression>...}]
    ORDER BY <expression> [ASC|DESC], [{,<expression>...}]
)
```

- n is a literal positive integer. The bucket number is in the range from 1 to n.
- The PARTITION BY divides the result set returned from the FROM clause into partitions to which the NTILE() function is applied.
- The ORDER BY clause specifies the order in which the NTILE() values are assigned to the rows in a partition.

```sql
CREATE TABLE ntiletest (
    val INT NOT NULL
);

INSERT INTO ntiletest(val)
VALUES(1),(2),(3),(4),(5),(6),(7),(8),(9);


SELECT * FROM ntiletest;
```

```sql
SELECT
    val,
    NTILE (4) OVER (
        ORDER BY val
    ) bucket_no
FROM
    ntiletest;
```

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-NTILE-function-groups-with-the-different-number-of-rows.png)

```sql
SELECT
    val,
    NTILE (3) OVER (
        ORDER BY val
    ) bucket_no
FROM
    ntiletest;
```

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-NTILE-function-groups-with-difference-in-rows.png)


- 다음 주어지는 쿼리를 실행하면 NTILE()과 함께 사용되는 PARTITION을 이해할 수 있다.
  - 우선 지정된 컬럼에 따라 PARTITION을 나눈다.
  - PARTITION으로 나누어진 상태에서 각각의 그룹을 NTILE에 지정된 숫자만큼 또 나눈다.
  - 그래서 아래와 같은 결과를 얻는다.

````sql
WITH productline_sales AS (
    SELECT productline,
           year(orderDate) order_year,
           ROUND(SUM(quantityOrdered * priceEach),0) order_value
    FROM orders
    INNER JOIN orderdetails USING (orderNumber)
    INNER JOIN products USING (productCode)
    GROUP BY productline, order_year
)
SELECT
    productline,
    order_year,
    order_value,
    NTILE(3) OVER (
        PARTITION BY order_year
        ORDER BY order_value DESC
    ) product_line_group
FROM
    productline_sales;
````

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-NTILE-function-with-CTE-example.png)


### PERCENT_RANK

The PERCENT_RANK() is a window function that calculates the percentile rank of a row within a partition or result set.
- https://www.mysqltutorial.org/mysql-window-functions/mysql-percent_rank-function/
- 예시로 나온 부분이 잘 안되어서 수정을 조금 하였고, 이해하였다.

```sql
PERCENT_RANK()
    OVER (
        PARTITION BY expr,...
        ORDER BY expr [ASC|DESC],...
    )
```


```sql
with sample_table as (
  select
    productline,
    orderYear,
    orderValue
  from (
  SELECT
      productline,
      YEAR(orderdate) as orderYear,
      quantityordered * priceEach as orderValue
  FROM orderdetails
      INNER JOIN orders USING (orderNumber)
      INNER JOIN products USING (productcode)
  ) as sample
  GROUP BY 1, 2, 3)


select
    productline,
    orderYear,
    orderValue,
    ROUND(
    PERCENT_RANK()
    OVER (
        PARTITION BY orderYear
        ORDER BY orderValue
    ),2) percentile_rank
FROM sample_table;
```

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-PERCENT_RANK-function-over-partition-example-1.png)

### RANK

- Note that MySQL has been supporting the RANK() function and other window functions since version 8.0
- 랭크 함수는 이전부터 지원 해왔당.


The RANK() function assigns a rank to each row within the partition of a result set. The rank of a row is specified by one plus the number of ranks that come before it.

```sql
RANK() OVER (
    PARTITION BY <expression>[{,<expression>...}]
    ORDER BY <expression> [ASC|DESC], [{,<expression>...}]
)
```

```sql
CREATE TABLE rank_test (
    val INT
);
 
INSERT INTO rank_test(val)
VALUES(1),(2),(2),(3),(4),(4),(5);
 
 
SELECT * FROM rank_test;

------------
-- Query....

SELECT
    val,
    RANK() OVER (
        ORDER BY val
    ) my_rank
FROM
    t;
```

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-RANK-function-example.png)



```sql
CREATE TABLE IF NOT EXISTS sales(
    sales_employee VARCHAR(50) NOT NULL,
    fiscal_year INT NOT NULL,
    sale DECIMAL(14,2) NOT NULL,
    PRIMARY KEY(sales_employee,fiscal_year)
);
 
INSERT INTO sales(sales_employee,fiscal_year,sale)
VALUES('Bob',2016,100),
      ('Bob',2017,150),
      ('Bob',2018,200),
      ('Alice',2016,150),
      ('Alice',2017,100),
      ('Alice',2018,200),
       ('John',2016,200),
      ('John',2017,150),
      ('John',2018,250);
 
SELECT * FROM sales;
```


````sql
SELECT
    sales_employee,
    fiscal_year,
    sale,
    RANK() OVER (PARTITION BY
                     fiscal_year
                 ORDER BY
                     sale DESC
                ) sales_rank
FROM
    sales;
````

```bash
-- 위의 쿼리 결과....
#,sales_employee,fiscal_year,sale,sales_rank
1,John,2016,200.00,1
2,Alice,2016,150.00,2
3,Bob,2016,100.00,3
4,Bob,2017,150.00,1
5,John,2017,150.00,1
6,Alice,2017,100.00,3
7,John,2018,250.00,1
8,Alice,2018,200.00,2
9,Bob,2018,200.00,2
```


```sql
WITH order_values AS(
    SELECT 
        orderNumber, 
        YEAR(orderDate) order_year,
        quantityOrdered*priceEach AS order_value,
        RANK() OVER (
            PARTITION BY YEAR(orderDate)
            ORDER BY quantityOrdered*priceEach DESC
        ) order_value_rank
    FROM
        orders
    INNER JOIN orderDetails USING (orderNumber)
)
SELECT 
    * 
FROM 
    order_values
WHERE 
    order_value_rank <=3;
```

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-RANK-function-order-values-example.png)

- 실제로는 수 백등까지 rank가 형성되어 있다.


### Row_number

MySQL introduced the ROW_NUMBER() function since version 8.0. The ROW_NUMBER() is a window function or analytic function that assigns a sequential number to each row to which it applied beginning with one.


**번외**

Row_number를 모르더라도 구현할 수 있는 방법이 있다.
- https://www.mysqltutorial.org/mysql-row_number/

```sql
SET @row_number = 0; 
SELECT 
    (@row_number:=@row_number + 1) AS num, 
    firstName, 
    lastName
FROM
    employees
ORDER BY firstName, lastName    
LIMIT 5;
```

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2019/08/mysql-row_number-emulation.png)

- 더 많은 내용은 별도 정리 할 것

본론으로 돌아와서......


**Assigning sequential numbers to rows**

```sql
SELECT 
    ROW_NUMBER() OVER (
        ORDER BY productName
    ) row_num,
    productName,
    msrp
FROM 
    products
ORDER BY 
    productName;
```

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-ROW_NUMBER-function-assign-sequential-numbers.png)


**Finding top N rows of every group**

```sql
WITH inventory
AS (SELECT 
       productLine,
       productName,
       quantityInStock,
       ROW_NUMBER() OVER (
          PARTITION BY productLine 
          ORDER BY quantityInStock DESC) row_num
    FROM 
       products
   )
SELECT 
   productLine,
   productName,
   quantityInStock
FROM 
   inventory
WHERE 
   row_num <= 3;
```

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-ROW_NUMBER-function-Top-N-rows-from-group.png)


위 쿼리를 살짝 뜯어 보면....

```sql
SELECT
       productLine,
       productName,
       quantityInStock,
       ROW_NUMBER() OVER (
          PARTITION BY productLine
          ORDER BY quantityInStock DESC) row_num
    FROM
       products;
```

```bash
#,productLine,productName,quantityInStock,row_num
1,Classic Cars,1995 Honda Civic,9772,1
2,Classic Cars,2002 Chevy Corvette,9446,2
3,Classic Cars,1976 Ford Gran Torino,9127,3
4,Classic Cars,1968 Dodge Charger,9123,4
5,Classic Cars,1965 Aston Martin DB5,9042,5
6,Classic Cars,1948 Porsche Type 356 Roadster,8990,6
7,Classic Cars,1948 Porsche 356-A Roadster,8826,7
....
....
39,Motorcycles,2002 Suzuki XREO,9997,1
40,Motorcycles,1982 Ducati 996 R,9241,2
41,Motorcycles,1969 Harley Davidson Ultimate Chopper,7933,3
42,Motorcycles,1957 Vespa GS150,7689,4
43,Motorcycles,1997 BMW R 1100 S,7003,5
44,Motorcycles,1982 Ducati 900 Monster,6840,6
45,Motorcycles,1996 Moto Guzzi 1100i,6625,7
46,Motorcycles,2003 Harley-Davidson Eagle Drag Bike,5582,8
....
....
52,Planes,America West Airlines B757-200,9653,1
53,Planes,American Airlines: MD-11S,8820,2
54,Planes,ATA: B757-300,7106,3
55,Planes,Corsair F4U ( Bird Cage),6812,4
56,Planes,1900s Vintage Bi-Plane,5942,5
....
....
```

위 결과에서 각 row_num이 3보다 작거나 같은 경우에만 추려서 쿼리를 실행한 것이다.


**Removing duplicate rows**

작업을 하기 위해 테이블을 만들어 보자

```sql
CREATE TABLE duplicated_test (
    id INT,
    name VARCHAR(10) NOT NULL
);
 
INSERT INTO duplicated_test(id,name) 
VALUES(1,'A'),
      (2,'B'),
      (2,'B'),
      (3,'C'),
      (3,'C'),
      (3,'C'),
      (4,'D');
```

```sql
SELECT
    id,
    name,
    ROW_NUMBER() OVER (
      PARTITION BY id, name ORDER BY id
    ) AS row_num
FROM duplicated_test;
```

다음과 같은 결과가 나온다.

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-ROW_NUMBER-function-Duplicate-Rows.png)


중복되는 걸 지워야 하니까...CTE를 써서 정리를 해보자.

As you can see from the output, the unique rows are the ones whose the row number equals one.
- you can use the common table expression (CTE) to return the duplicate rows and delete statement to remove
- Notice that the MySQL does not support CTE based delete, therefore, we had to join the original table with the CTE as a workaround.

```sql
WITH dups AS (SELECT
        id,
        name,
        ROW_NUMBER() OVER(PARTITION BY id, name ORDER BY id) AS row_num
    FROM duplicated_test)

DELETE FROM duplicated_test USING duplicated_test JOIN dups ON duplicated_test.id = dups.id
WHERE dups.row_num <> 1;
```

- 중복되는 항목을 지울 수 있다.
- to-do list : name이 중복되는 경우를 다 지워서 살짝 이해가 안 간다. 나중에 다시 확인 할 것 2020-02-28

```bash
#,id,name
1,1,A
2,4,D
```

### Pagination using  ROW_NUMBER() function

Because the ROW_NUMBER() assigns each row in the result set a unique number, you can use it for pagination. Suppose, you need to display a list of products with 10 products per page. To get the products for the second page, you use the following query:

- Pagination : 페이지 매김..1페이지, 2페이지...


```sql
SELECT *
FROM
    (SELECT productName,
         msrp,
         row_number()
        OVER (order by msrp) AS row_num
    FROM products) t
WHERE row_num BETWEEN 11 AND 20
```

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-ROW_NUMBER-function-Pagination-Example.png)



# Window Function을 이용한 실전 예제

## 누적합 구하기

누적합을 구하는 것을 연습하였다. 잊지 말고 잘 사용하자.

```sql
select
    distinct officeCode,
    sum(officeCode) over (order by officeCode asc)
from (select distinct officeCode as officeCode from employees) as distinct_employees;
```

```bash
#,officeCode,sum(officeCode) over (order by officeCode asc)
1,1,1
2,2,3
3,3,6
4,4,10
5,5,15
6,6,21
7,7,28
```

가장 중요한 부분은 `sum(column_name) over (order by column_name asc)` 항목이다.
- 합계를 구하는데 각 컬럼의 합계를 부분적으로 판단해서 구하는 것으로 보인다.
- [ ] 원리에 대해서는 조금 더 리서치 해볼 것 