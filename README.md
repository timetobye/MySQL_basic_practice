# MySQL_basic_practice
[![alt text](https://img.shields.io/badge/mysql-5.6-green.svg)](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/)
![alt text](https://img.shields.io/badge/result-idea%26query__solution-blue.svg)
[![alt text](https://img.shields.io/badge/data-web-red.svg)](https://www.w3resource.com/mysql-exercises/)

SQL 공부 내용을 담습니다.
- MySQL
  - 5.6 버전으로 학습하였습니다.
  - 기본적인 내용보다는, 얕게 알거나 몰랐던 것 위주로 정리하였습니다.
  - 참고 : [MySQL 5.6 공식 문서](https://dev.mysql.com/doc/refman/5.6/en/)
- Redshift 외 등
  - Redshift에는 좋은 가이드 문서들이 많이 올라와 있습니다. AWS 제품 뿐만 아니라, 어떠한 항목에 대한 개념,사용 방법,확장성 등이 안내 되어 있습니다.
  - [Redshift 공식 문서](https://docs.aws.amazon.com/ko_kr/redshift/latest/mgmt/welcome.html)
  - [열 기반 데이터베이스란 무엇입니까?](https://aws.amazon.com/ko/nosql/columnar/)
  - [NOSQL이란?](https://aws.amazon.com/ko/nosql/)
  - 분산 환경에서 작업되는 항목들은 별도의 페이지를 작성해서 학습 노트를 기록할 예정입니다.
  
  
# Table of Contents
  - [MySQL Quiz](https://github.com/timetobye/MySQL_basic_practice#mysql-quiz)
  - [MySQL 학습](https://github.com/timetobye/MySQL_basic_practice#mysql-학습-1)
    - [order by](https://github.com/timetobye/MySQL_basic_practice#odrer-by)
    - [IN vs OR](https://github.com/timetobye/MySQL_basic_practice#in-vs-or)
    - [IN 에서 subquery를 사용](https://github.com/timetobye/MySQL_basic_practice#in-%EC%97%90%EC%84%9C-subquery%EB%A5%BC-%EC%82%AC%EC%9A%A9)
    - [between](https://github.com/timetobye/MySQL_basic_practice#between)
    - [between with cast](https://github.com/timetobye/MySQL_basic_practice#between-with-cast)
    - [LIKE](https://github.com/timetobye/MySQL_basic_practice#like)
      - [escape in LIKE](https://github.com/timetobye/MySQL_basic_practice#escape-in-like)
    - [Null에 대한 이해](https://github.com/timetobye/MySQL_basic_practice#null%EC%97%90-%EB%8C%80%ED%95%9C-%EC%9D%B4%ED%95%B4)
    - [exists](https://github.com/timetobye/MySQL_basic_practice#exists)
      - [update-in-exists](https://github.com/timetobye/MySQL_basic_practice#update-in-exists)
    - [limit](https://github.com/timetobye/MySQL_basic_practice#limit)
    - [inner join](https://github.com/timetobye/MySQL_basic_practice#inner-join)
    - [left join](https://github.com/timetobye/MySQL_basic_practice#left-join)
    - [group by, having](https://github.com/timetobye/MySQL_basic_practice#group-by-having)
    - [Rollup](https://github.com/timetobye/MySQL_basic_practice#rollup)
    - [DEFAULT CHARSET=utf8](https://github.com/timetobye/MySQL_basic_practice#default-charsetutf8)
    - [now(), sysdate(), current_date()](https://github.com/timetobye/MySQL_basic_practice#now-sysdate-current_date)
    - [concat_ws vs concat](https://github.com/timetobye/MySQL_basic_practice#concat_ws-vs-concat)
    - [group_concat](https://github.com/timetobye/MySQL_basic_practice#group_concat)
    - [IFNULL](https://github.com/timetobye/MySQL_basic_practice#ifnull)
    - [Derived table](https://github.com/timetobye/MySQL_basic_practice#derived-table)
    - [CTE](https://github.com/timetobye/MySQL_basic_practice#derived-table)
    - [recursive CTE](https://github.com/timetobye/MySQL_basic_practice#recursive-cte)
    - [Union vs Union All](https://github.com/timetobye/MySQL_basic_practice#union-vs-union-all)
    - [INTERSECT](https://github.com/timetobye/MySQL_basic_practice#intersect)
    - [Minus](https://github.com/timetobye/MySQL_basic_practice#minus)
    - [Insert](https://github.com/timetobye/MySQL_basic_practice#insert)
    - [Insert into select](https://github.com/timetobye/MySQL_basic_practice#insert-into-select)
    - [IF](https://github.com/timetobye/MySQL_basic_practice#if)
    - [Update](https://github.com/timetobye/MySQL_basic_practice#update)
    - [tips](https://github.com/timetobye/MySQL_basic_practice#tips)
    - [Date Function](https://github.com/timetobye/MySQL_basic_practice#date-function)
      - [CURDATE](https://github.com/timetobye/MySQL_basic_practice#curdate)
      - [DATEDIFF](https://github.com/timetobye/MySQL_basic_practice#datediff)
      - [DAY](https://github.com/timetobye/MySQL_basic_practice#day)
      - [DATE_ADD](https://github.com/timetobye/MySQL_basic_practice#date_add)
      - [DATE_SUB](https://github.com/timetobye/MySQL_basic_practice#date_sub)
      - [DATE_FORMAT](https://github.com/timetobye/MySQL_basic_practice#date_format)
      - [STR_TO_DATE()](https://github.com/timetobye/MySQL_basic_practice#str_to_date)
    

---------------------------------

## MySQL Quiz
- 기초적인 Mysql Quiz에 대해 idea, solution, result를 정리해두었습니다. mysql을 remind 하기 위해 만들었습니다.
- 어떤 면에서는 무척 쉽습니다. 쿼리를 사용 할 때 idea, solution, result 3 단계를 차근차근 해보기 위해 작성하였습니다.
- 문제 풀이 아래에는 [www.mysqltutorial.org](http://www.mysqltutorial.org/basic-mysql-tutorial.aspx)에서 학습하면서 필요한 내용을 정리하였습니다.
- 문서 작성은 Notion을 이용하여 작성하였습니다. 그 후 HTML 파일, Markdown 파일을 받아서 git에 정리하였습니다.
> 연습이 필요하신 분은 HTML 파일을 다운 받아서 보거나 웹에서 바로 볼 수 있는 링크를 참고해주세요.

### :information_source: DB

연습에 필요한 DB는 [w3resource](https://www.w3resource.com/mysql-exercises//)의 [HR Database](https://www.w3resource.com/mysql-exercises//db.sql)를 이용하였습니다.

### :book: Quiz Result
- [MySQL Basic SELECT statement 19 Exercises](http://htmlpreview.github.io/?https://github.com/timetobye/MySQL_basic_practice/blob/master/MySQL%20Quiz/MySQL%20Basic%20SELECT%20statement%2019%20Exercises/MySQL%20Basic%20SELECT%20statement%2019%20Exercises.html)
- [MySQL Restricting and Sorting data 11 exercises](http://htmlpreview.github.io/?https://github.com/timetobye/MySQL_basic_practice/blob/master/MySQL%20Quiz/MySQL%20Restricting%20and%20Sorting%20data%20%5B11%20exercises%5D/MySQL%20Restricting%20and%20Sorting%20data%2011%20exercises.html)
- [MySQL Aggregate Functions and Group by 14 exercises](http://htmlpreview.github.io/?https://github.com/timetobye/MySQL_basic_practice/blob/master/MySQL%20Quiz/MySQL%20Aggregate%20Functions%20and%20Group%20by%20%5B14%20exercises%5D/Aggregate%20Functions%20and%20Group%20by%2014%20exercises.html)
- [MySQL Subquery 22 exercises](http://htmlpreview.github.io/?https://github.com/timetobye/MySQL_basic_practice/blob/master/MySQL%20Quiz/MySQL%20Subquery%20%5B22%20exercises%20with%20solution%5D/MySQL%20Subquery%2022%20exercises%20with%20solution.html)
- [MySQL Joins 13 exercises with solution](http://htmlpreview.github.io/?https://github.com/timetobye/MySQL_basic_practice/blob/master/MySQL%20Quiz/MySQL%20Joins%20%5B13%20exercises%20with%20solution%5D/MySQL%20Joins%2013%20exercises%20with%20solution.html)


**thanks to**
- SQL을 활용해서 멋지게 일하시는 나의 동료 DD님 감사합니다.
- SQL 고민 할 때 이거 보세요 하고 링크 건네주셨던 G.S Park에게도 감사합니다.
- SQL 처음 할 때 [sql 첫걸음](https://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9788968482311) 추천해주신 Noah 님 감사합니다.
--------------------------


# MySQL 학습
- [www.mysqltutorial.org](http://www.mysqltutorial.org/basic-mysql-tutorial.aspx)에 게시된 자료를 바탕으로 정리하였습니다.
- 모르는 것, 새롭게 알게 된 것, 애매하게 알고 있는 것을 적었습니다.
- ~~하면 할 수록 어렵냐...~~

## :information_source: DB
- 학습에 필요한 DB는 [이곳](http://www.mysqltutorial.org/mysql-sample-database.aspx)에서 받을 수 있습니다.
- 설치 방법은 별도로 적지 않습니다. 잘 모르실 경우 메일 주세요.

## schema
The MySQL sample database schema consists of the following tables
- Customers: stores customer’s data.
- Products: stores a list of scale model cars.
- ProductLines: stores a list of product line categories.
- Orders: stores sales orders placed by customers.
- OrderDetails: stores sales order line items for each sales order.
- Payments: stores payments made by customers based on their accounts.
- Employees: stores all employee information as well as the organization structure such as who reports to whom.
- Offices: stores sales office data.

![alt text](http://www.mysqltutorial.org/wp-content/uploads/2009/12/MySQL-Sample-Database-Schema.png)


### odrer by
- 아래와 같은 것을 새롭게 배웠다.

```
SELECT 
    orderNumber, status
FROM
    orders
ORDER BY FIELD(status,
        'In Process',
        'On Hold',
        'Cancelled',
        'Resolved',
        'Disputed',
        'Shipped');
```

- 특정 칼럼을 지정하여 칼럼 내 값들을 임의대로 순서 조정 할 수 있다.
- 이렇게 하면 정렬을 자유자재로 할 수 있다.

![Alt text](http://www.mysqltutorial.org/wp-content/uploads/2010/01/MySQL-ORDER-BY-and-FIELD-function.jpg)


문자와 숫자가 함께 섞여 있는 경우의 정렬은 어떻게 하면 좋을까?

![alt text](http://www.mysqltutorial.org/wp-content/uploads/2014/01/MySQL-Natural-Sorting-Example.jpg)
![alt text](http://www.mysqltutorial.org/wp-content/uploads/2014/01/MySQL-Natural-Sorting-Example-correct-order.jpg)

- 두번재 그림을 기대하였다.

```
SELECT 
    item_no
FROM
    items
ORDER BY CAST(item_no AS UNSIGNED) , item_no;
```
- item_no의 데이터를 cast를 사용하여 부호없는 정수로 변환
- 그 다음 정렬

```
TRUNCATE TABLE items;
 
INSERT INTO items(item_no)
VALUES('A-1'),
      ('A-2'),
      ('A-3'),
      ('A-4'),
      ('A-5'),
      ('A-10'),
      ('A-11'),
      ('A-20'),
      ('A-30');
```

```
SELECT 
    item_no
FROM
    items
ORDER BY item_no;
```

- truncate 후에 위의 쿼리를 날리면 아래와 같은 결과가 안 나온다...~~뭐야..~~

![alt text](http://www.mysqltutorial.org/wp-content/uploads/2014/01/MySQL-Natural-Sorting-Another-Good-Example.jpg)


### IN VS OR

```
SELECT 
    officeCode, 
    city, 
    phone, 
    country
FROM
    offices
WHERE
    country IN ('USA' , 'France');
```

```
SELECT 
    officeCode, 
    city, 
    phone
FROM
    offices
WHERE
    country = 'USA' OR country = 'France';
```

- In case the list has many values, you need to construct a very long statement with multiple OR operators. Hence, the IN operator allows you to shorten the query and make it more readable.
- 가독성의 중요성

### IN 에서 subquery를 사용

```
SELECT 
    orderNumber, 
    customerNumber, 
    status, 
    shippedDate
FROM
    orders
WHERE
    orderNumber IN (SELECT 
            orderNumber
        FROM
            orderdetails
        GROUP BY orderNumber
        HAVING SUM(quantityOrdered * priceEach) > 60000);
```

- 구동방식은 아래와 같은 원리로 진행되었다.
```
SELECT 
    orderNumber, 
    customerNumber, 
    status, 
    shippedDate
FROM
    orders
WHERE
    orderNumber IN (10165,10287,10310);
```

### between

- between 에서 not은 이렇게 사용한다.

```
select 
	productcode,
    productname,
    buyprice
From
	products
where
	buyprice not between 90 and 100;
```

- between은 경계 값에 대해 생각해보고 써야 한다.
  - 예를 들면 시간 계산, 날짜 계산
  - [Q&A](https://stackoverflow.com/questions/749615/does-ms-sql-servers-between-include-the-range-boundaries/749663)
  

### between with cast

- When you use the BETWEEN operator with date values, to get the best result, you should use the type cast to explicitly convert the type of column or expression to the DATE type.

- For example, to get the orders whose required dates are from 01/01/2003 to 01/31/2003, you use the following query:

```
SELECT 
   orderNumber,
   requiredDate,
   status
FROM 
   orders
WHERE 
   requireddate BETWEEN 
     CAST('2003-01-01' AS DATE) AND 
     CAST('2003-01-31' AS DATE);
```

![alt text](http://www.mysqltutorial.org/wp-content/uploads/2009/12/MySQL-BEETWEEN-with-Dates-Example.png)

- Because the data type of the required date column is DATE so we used the cast operator to convert the literal strings ‘2003-01-01 ‘ and ‘2003-12-31 ‘ to the DATE data type.

- **convert the literal strings ‘2003-01-01 ‘ and ‘2003-12-31 ‘ to the DATE data type.**


### LIKE

```
SELECT 
    employeeNumber, 
    lastName, 
    firstName
FROM
    employees
WHERE
    firstname LIKE 'T_m';
```

![alt text](http://www.mysqltutorial.org/wp-content/uploads/2009/12/mysql-like-with-_-pattern.png)

- 'T_M 이렇게 하면 Tom, Tim 등 일치하는 문자열 검색 가능

- not like도 사용 가능
```
select
	employeeNumber,
    lastname,
    firstname
from employees
where lastname not Like 'B%'
```

#### escape in LIKE

- 원래 '\'만 되는 줄 알았다.(디폴트 값이다.)
- 그런데 escape를 지정하면 그 문자열도 지정할 수 있다.

- For example, if you want to find product whose product code contains string _20 , you can use the pattern %\_20% as the following query:

```
SELECT 
    productCode, 
    productName
FROM
    products
WHERE
    productCode LIKE '%\_20%';
```

```
SELECT 
    productCode, 
    productName
FROM
    products
WHERE
    productCode LIKE '%$_20%' ESCAPE '$';
```

![alt text](http://www.mysqltutorial.org/wp-content/uploads/2009/12/MySQL-LIKE-ESCAPE-example.png)


### Null에 대한 이해

```
value IS NULL
```

If the value is NULL, the expression returns true. Otherwise, it returns false.

Note that MySQL does not have the built-in BOOLEAN type. It uses the TINYINT(1) to represent the BOOLEAN values i.e.,  true means 1 and false means 0.

```
SELECT 1 IS NULL,  -- 0
       0 IS NULL,  -- 0
       NULL IS NULL; -- 1
```

```
SELECT 1 IS NOT NULL, -- 1
       0 IS NOT NULL, -- 1
       NULL IS NOT NULL; -- 0
```


- http://www.mysqltutorial.org/mysql-is-null/
  - 위 부분이 이해가 잘 되지 않았다.
  - 이해가 되면 업데이트 할 것
  
### exists

- Introduction to MySQL EXISTS operator
The EXISTS operator is a Boolean operator that returns either true or false. The EXISTS operator is often used the in a subquery to test for an “exist” condition.

- where, update 등 다양하게 사용
- 서브쿼리 할 때 도 사용

```
select customernumber, customername
from customers
where
	exists(
    select 1
    from orders
    where orders.customernumber = customers.customernumber);
```
 
 
- sql 첫걸음 책에서도 본 내용이다. 두 개의 table에서 가져온 걸 비교할 때는 table0.column = table1.column

```
SELECT 
    employeenumber, firstname, lastname, extension
FROM
    employees
WHERE
    EXISTS( SELECT 
            1
        FROM
            offices
        WHERE
            city = 'San Francisco'
                AND offices.officeCode = employees.officeCode);
```
  
- select 1 은 왜 쓸까?
  - http://blog.naver.com/PostView.nhn?blogId=aidpost123&logNo=110180721327&parentCategoryNo=9&categoryNo=&viewDate=&isShowPopularPosts=true&from=search

```
Select 1 from table

위처럼 select 절에 1이 오는 것은 해당 테이블의 숫자만큼 1의 행을 만들어 낸다.

즉 table 의 데이터수가 N개면 1이 N행에 거쳐서 반환된다.

이것이 중요한 이유는 1은 TRUE의 다른 말이기 때문이다.

그래서 보통 WHERE 절의 (NOT) EXISTS 안의 내포 SELECT 문으로 사용된다.

또한 SELECT * FROM TABLE 이나 SELECT 1 FROM TABLE이나 논리식으로 사용될 때는 존재유무가 중요하기 때문에 보다 간단하게 사용하려면 SELECT 1 FROM TABLE을 사용하지만 접근에 대한 정확성은 떨어질 수 밖에 없다.
```

- exists 예제는 어려워서 링크로 대체한다.
- 핵심은 1을 반환하는 것이 True이며 결과에 매칭시킨다.
- http://www.mysqltutorial.org/mysql-exists/


#### update in exists

```
update employees
set extension = concat(extension,'1')
where
	exists(
    select 1
    from offices
    where
    city = 'San Francisco'
    and offices.officeCode = employees.officeCode);
```

```
select * from employees
inner join offices on offices.officeCode = employees.officeCode
```

- extension이라는 항목에 1을 추가 해야 하는데 exitsts를 이용하여 employees라는 항목에 1을 넣었다.

#### in vs exists

- exists(첫번째 그림), IN(두번째 그림)

![alt text](http://www.mysqltutorial.org/wp-content/uploads/2016/01/MySQL-EXISTS-vs-IN-EXISTS-performance.jpg)

![alt text](http://www.mysqltutorial.org/wp-content/uploads/2016/01/MySQL-EXISTS-vs-IN-IN-performance.jpg)


- 하위 쿼리가 많을 수록 in이 느리다.
- 풀스캔해야 하기 때문이다.
- 적으면 in이 낫다.
- http://yahwang.tk/posts/35

#### length 함수를 이용하여 작업

- item_no의 문자열 길이를 반환한 다음에 길이별로 정렬
- 아이디어일뿐 해결책은 아님

```
SELECT 
    item_no
FROM
    items
ORDER BY LENGTH(item_no), item_no;
```

### limit

#### MySQL LIMIT을 사용하여 n 번째로 높은 값 얻기

```
SELECT 
    column1, column2,...
FROM
    table
ORDER BY column1 DESC
LIMIT nth-1, count;
```

- offset첫 번째 행의 오프셋을 지정 반환한다. offset첫 번째 행의 0이 아닌 1이다.
  - count최대 행 수를 반환하도록 지정합니다.
  
![alt text](http://www.mysqltutorial.org/wp-content/uploads/2011/03/mysql-limit-offset.jpg)


### inner join

- using을 사용하면 같은 결과를 반환한다.(left, 마찬가지이다.)
```
SELECT 
    productCode, 
    productName, 
    textDescription
FROM
    products t1
        INNER JOIN
    productlines t2 ON t1.productline = t2.productline;
```
```
SELECT 
    productCode, 
    productName, 
    textDescription
FROM
    products
        INNER JOIN
    productlines USING (productline);
```

### left join

- join 에서 조건을 걸으면 결과가 다르게 나온다.
- where을 따로 거는 경우, left join에서 거는 경우


```
SELECT 
    o.orderNumber, 
    customerNumber, 
    productCode
FROM
    orders o
        LEFT JOIN
    orderDetails USING (orderNumber)
WHERE
    orderNumber = 10123;
```
![alt text](http://www.mysqltutorial.org/wp-content/uploads/2011/05/MySQL-LEFT-JOIN-Condition-in-WHERE-clause.png)

```
SELECT 
    o.orderNumber, 
    customerNumber, 
    productCode
FROM
    orders o
        LEFT JOIN
    orderdetails d ON o.orderNumber = d.orderNumber
        AND o.orderNumber = 10123;
```
![alt text](http://www.mysqltutorial.org/wp-content/uploads/2011/05/MySQL-LEFT-JOIN-Condition-in-ON-clause.png)

### group by, having

- 2018-03-03이라는 데이터가 있을 때 Year, Month, day를 쓰면 년, 월, 일을 추출할 수 있더라.

```
SELECT 
    YEAR(orderDate) AS year,
    SUM(quantityOrdered * priceEach) AS total
FROM
    orders
        INNER JOIN
    orderdetails USING (orderNumber)
WHERE
    status = 'Shipped'
GROUP BY YEAR(orderDate);
```

![alt text](http://www.mysqltutorial.org/wp-content/uploads/2011/05/MySQL-GROUP-BY-expression-example.png)

```
SELECT 
    YEAR(orderDate) AS year,
    SUM(quantityOrdered * priceEach) AS total
FROM
    orders
        INNER JOIN
    orderdetails USING (orderNumber)
WHERE
    status = 'Shipped'
GROUP BY YEAR
having year > 2003;
```

![alt text](http://www.mysqltutorial.org/wp-content/uploads/2011/05/MySQL-GROUP-BY-with-HAVING-example.png)


- group by 에서도 정렬을 할 수 있다.

```
SELECT 
    status, COUNT(*)
FROM
    orders
GROUP BY status DESC;
```

- having을 쓰는 예를 더 적어보았다.

```
SELECT 
    ordernumber,
    SUM(quantityOrdered) AS itemsCount,
    SUM(priceeach*quantityOrdered) AS total
FROM
    orderdetails
GROUP BY ordernumber
HAVING total > 1000 AND itemsCount > 600;
```

```
SELECT 
    a.ordernumber, status, SUM(priceeach*quantityOrdered) total
FROM
    orderdetails a
        INNER JOIN
    orders b ON b.ordernumber = a.ordernumber
GROUP BY ordernumber, status
HAVING status = 'Shipped' AND total > 1500;
```

### Rollup

- http://www.mysqltutorial.org/mysql-rollup/
- 기본서에 나오지 않은 내용이다.
- group by 결과가 있을 때 그 값의 총합을 구하는 것까지 하려면 어떻게 해야 할까?
  - 합산 결과가 나오는 table 만들어서 union all?
  - 그러면...
  - The query is quite lengthy.
  - The performance of the query may not be good since the database engine has to internally execute two separate queries and combine the result sets into one.
  
```
SELECT 
    select_list
FROM 
    table_name
GROUP BY
    c1, c2, c3 WITH ROLLUP;
```

```
SELECT 
    productLine, 
    SUM(orderValue) totalOrderValue
FROM
    sales
GROUP BY 
    productline WITH ROLLUP;
```

![alt text](http://www.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-ROLLUP-example.png)

- the ROLLUP clause generates not only the subtotals but also the grand total of the order values.
- ~~오옷....짱이다~~

```
SELECT 
    productLine, 
    orderYear,
    SUM(orderValue) totalOrderValue
FROM
    sales
GROUP BY 
    productline, 
    orderYear 
WITH ROLLUP;
```

![alt text](http://www.mysqltutorial.org/wp-content/uploads/2018/08/MySQL-ROLLUP-hierarchy.png)

- mysql 8.0.12 버전에서는 더 좋은 기능이 있으나 설치 안 해서 패스(rollup 링크 참조)


### DEFAULT CHARSET=utf8

- local DB에 csv 파일을 넣을 때 한글이 깨지는 경우가 발생한다.
- 그럴 때 생성하는 table에 명령어를 추가하면 쉽게 문제를 해결 할 수 있다.

```
create table 'DB name'.'table name' (
'table_name_1' INT(11) unsigned NOT NULL COMMENT 'hihihihi',
'table_name_2' INT(11) unsigned NOT NULL COMMENT 'hihihihi',
'table_name_3' VARCHAR(255) NOT NULL COMMENT 'hihihihi'
) DEFAULT CHARSET=utf8
```

- 한글이 잘 들어가는 것을 확인 할 수 있다.


### now(), sysdate(), current_date()

- 쿼리를 만들 때 현재 날짜와 관련하여 어떤 것을 사용해야 할까?
- 날짜만 필요하면 current_date(), 쿼리 날리는 시각의 변화가 불필요하면 now(), 같은 쿼리를 날리는 중에도 변화가 있으면 sysdate()

```
mysql> select now(), sysdate(), current_date(), sleep(5), now(), sysdate();
+---------------------+---------------------+----------------+----------+---------------------+---------------------+
| now()               | sysdate()           | current_date() | sleep(5) | now()               | sysdate()           |
+---------------------+---------------------+----------------+----------+---------------------+---------------------+
| 2019-05-21 03:11:57 | 2019-05-21 03:11:57 | 2019-05-21     |        0 | 2019-05-21 03:11:57 | 2019-05-21 03:12:02 |
+---------------------+---------------------+----------------+----------+---------------------+---------------------+
1 row in set (5.02 sec)
```

- [참고 페이지](https://stackoverflow.com/questions/24137752/difference-between-now-sysdate-current-date-in-mysql)



### concat_ws vs concat

- concat_ws와 concat의 차이를 아래와 같다.
- concat_ws에 문자열을 지정하면 두개의 문자열을 합치면서 그 사이에 지정된 문자열이 들어간다.

```
select concat_ws(', ', last_name, first_name) as full_name
from employees
limit 100;

#,full_name
1,"Facello, Georgi"
2,"Simmel, Bezalel"
3,"Bamford, Parto"
4,"Koblick, Chirstian"
5,"Maliniak, Kyoichi"
6,"Preusig, Anneke"
7,"Zielinski, Tzvetan"
8,"Kalloufi, Saniya"
9,"Peac, Sumant"
```
- 그렇다면 3개의 column이 지정되면...?
```
select concat_ws(', ', last_name, first_name, last_name) as full_name
from employees
limit 100;

#,full_name
1,"Facello, Georgi, Facello"
2,"Simmel, Bezalel, Simmel"
3,"Bamford, Parto, Bamford"
4,"Koblick, Chirstian, Koblick"
5,"Maliniak, Kyoichi, Maliniak"
6,"Preusig, Anneke, Preusig"
7,"Zielinski, Tzvetan, Zielinski"
8,"Kalloufi, Saniya, Kalloufi"
9,"Peac, Sumant, Peac"
```
- 위에 처럼 처음에 지정된 문자가 구분자가 된다.

- concat은 하나하나씩 다 지정을 해줘야 한다.
```
select concat(last_name, ', ',first_name) as full_name
from employees
limit 100

#,full_name
1,"Facello, Georgi"
2,"Simmel, Bezalel"
3,"Bamford, Parto"
4,"Koblick, Chirstian"
5,"Maliniak, Kyoichi"
6,"Preusig, Anneke"
7,"Zielinski, Tzvetan"
8,"Kalloufi, Saniya"
9,"Peac, Sumant"
```

### group_concat
- group by를 응용
- grouping 되는 항목과 관련된 다른 값들을 concat해서 출력할 수 있다.
- 구분자를 설정하지 않으면 기본적으로 ',' 가, 설정할 경우 예제에는 '-'가 표시되었다.

```
select productline, group_concat(distinct productScale)
from products
group by productLine


#,productline,group_concat(distinct productScale)
1,Classic Cars,"1:10,1:12,1:18,1:24"
2,Motorcycles,"1:10,1:12,1:18,1:24,1:32,1:50"
3,Planes,"1:18,1:72,1:24,1:700"
4,Ships,"1:18,1:24,1:700,1:72"
5,Trains,"1:18,1:32,1:50"
6,Trucks and Buses,"1:12,1:18,1:24,1:32,1:50"
7,Vintage Cars,"1:18,1:24,1:32,1:50"


select productline, group_concat(distinct productScale separator'-')
from products
group by productLine

#,productline,group_concat(distinct productScale separator'-')
1,Classic Cars,1:10-1:12-1:18-1:24
2,Motorcycles,1:10-1:12-1:18-1:24-1:32-1:50
3,Planes,1:18-1:72-1:24-1:700
4,Ships,1:18-1:24-1:700-1:72
5,Trains,1:18-1:32-1:50
6,Trucks and Buses,1:12-1:18-1:24-1:32-1:50
7,Vintage Cars,1:18-1:24-1:32-1:50
```


### IFNULL

- MySQL IFNULL function is one of the MySQL control flow functions that accepts two arguments and returns the first argument if it is not NULL. Otherwise, the IFNULL function returns the second argument.
```
IFNULL(expression_1,expression_2);
```
- The IFNULL function returns expression_1 if expression_1 is not NULL ; otherwise, it returns expression_2.

```
select ifnull(1, 0);

result : 1
```

```
select ifnull('', 1);

result : 빈칸 나옴 = ''
```

```
select ifnull(null, 'IFNULL_FUNCTION');

result : 'IFNULL_FUNCTION'
```


### Derived table

- A derived table is a virtual table returned from a SELECT statement. A derived table is similar to a temporary table, but using a derived table in the SELECT statement is much simpler than a temporary table because it does not require steps of creating the temporary table.

- The term **derived table** and subquery is often used interchangeably. When a stand-alone subquery is used in the FROM clause of a SELECT statement, we call it a derived table.

- The following illustrates a query that uses a derived table:

![alt text](http://www.mysqltutorial.org/wp-content/uploads/2017/07/MySQL-Derived-Table.png)  

```
SELECT 
    customerNumber,
    ROUND(SUM(quantityOrdered * priceEach)) sales,
    (CASE
        WHEN SUM(quantityOrdered * priceEach) < 10000 THEN 'Silver'
        WHEN SUM(quantityOrdered * priceEach) BETWEEN 10000 AND 100000 THEN 'Gold'
        WHEN SUM(quantityOrdered * priceEach) > 100000 THEN 'Platinum'
    END) customerGroup
FROM
    orderdetails
        INNER JOIN
    orders USING (orderNumber)
WHERE
    YEAR(shippedDate) = 2003
GROUP BY customerNumber;
```

```
SELECT 
    customerGroup, 
    COUNT(cg.customerGroup) AS groupCount
FROM
    (SELECT 
        customerNumber,
            ROUND(SUM(quantityOrdered * priceEach)) sales,
            (CASE
                WHEN SUM(quantityOrdered * priceEach) < 10000 THEN 'Silver'
                WHEN SUM(quantityOrdered * priceEach) BETWEEN 10000 AND 100000 THEN 'Gold'
                WHEN SUM(quantityOrdered * priceEach) > 100000 THEN 'Platinum'
            END) customerGroup
    FROM
        orderdetails
    INNER JOIN orders USING (orderNumber)
    WHERE
        YEAR(shippedDate) = 2003
    GROUP BY customerNumber) cg
GROUP BY cg.customerGroup;   
```

- result
![alt text](http://www.mysqltutorial.org/wp-content/uploads/2017/07/MySQL-Derived-Table-Customer-Group-Counts.png)


### MySQL CTE
mysql 8.0부터 지원 가능한 기능이다. 그러나 이 기능은 redshift를 사용할 때 종종 사용했던 기법으로 알면 참 좋은 기능이다. 현재 5.6으로 학습을 하고 있는데 그럼에도 불구하고 좋아서 남긴다.
```
MySQL introduced the common table expression feature or CTE in short since version 8.0 so you should have MySQL 8.0 installed on your computer in order to practice with the statements in this tutorial.
```

```
WITH customers_in_usa AS (
    SELECT 
        customerName, state
    FROM
        customers
    WHERE
        country = 'USA'
) SELECT 
    customerName
 FROM
    customers_in_usa
 WHERE
    state = 'CA'
 ORDER BY customerName;
```

![alt text](http://www.mysqltutorial.org/wp-content/uploads/2017/07/MySQL-CTE-Example-1.png)


### recursive CTE
- 나중에 학습 할 내용
- http://www.mysqltutorial.org/mysql-recursive-cte/


###

```
WITH topsales2003 AS (
    SELECT 
        salesRepEmployeeNumber employeeNumber,
        SUM(quantityOrdered * priceEach) sales
    FROM
        orders
            INNER JOIN
        orderdetails USING (orderNumber)
            INNER JOIN
        customers USING (customerNumber)
    WHERE
        YEAR(shippedDate) = 2003
            AND status = 'Shipped'
    GROUP BY salesRepEmployeeNumber
    ORDER BY sales DESC
    LIMIT 5
)
SELECT 
    employeeNumber, firstName, lastName, sales
FROM
    employees
        JOIN
    topsales2003 USING (employeeNumber);
```

![alt text](http://www.mysqltutorial.org/wp-content/uploads/2017/07/MySQL-CTE-Example-2.png)


### Union vs Union All

**Union**

Union을 하게 되면 컬럼이 같은 두 개의 테이블을 결합 할 수 있다. 단 중복되는 항목은 지워진다.

**Union All**

Union All을 하게 되면 마찬가지로 컬럼이 같은 두 개의 테이블을 결합 할 수 있다. 단 중복되는 항목은 지워지지 않는다.
-  the duplicates appear in the combined result set because of the UNION ALL operation.

```
SELECT id
FROM t1
UNION
SELECT id
FROM t2;

+----+
| id |
+----+
|  1 |
|  2 |
|  3 |
|  4 |
+----+


SELECT id
FROM t1
UNION ALL
SELECT id
FROM t2;

+----+
| id |
+----+
|  1 |
|  2 |
|  3 |
|  2 |
|  3 |
|  4 |
+----+
```

![alt text](http://www.mysqltutorial.org/wp-content/uploads/2009/12/MySQL-UNION.png)
![alt text](http://www.mysqltutorial.org/wp-content/uploads/2009/12/MySQL-UNION-vs-JOIN.png)

그렇다면 다른 join키가 없는 다른 테이블은 어떻게 결합 할 수 있을까?

![alt text](http://www.mysqltutorial.org/wp-content/uploads/2013/02/employees_table.png)

```
SELECT 
    firstName, 
    lastName
FROM
    employees 
UNION 
SELECT 
    contactFirstName, 
    contactLastName
FROM
    customers;
```

![alt text](http://www.mysqltutorial.org/wp-content/uploads/2009/12/MySQL-UNION-example.png)


### INTERSECT

basic concept
```
(SELECT column_list 
FROM table_1)
INTERSECT
(SELECT column_list
FROM table_2);
```

To use the INTERSECT operator for two queries, the following rules are applied:
- The order and the number of columns must be the same.
- The data types of the corresponding columns must be compatible.

![alt text](http://www.mysqltutorial.org/wp-content/uploads/2014/05/MySQL-INTERSECT.png)

**but** Unfortunately, MySQL does not support the INTERSECT operator. However, you can simulate the INTERSECT operator.

```
SELECT id
FROM t1;

id
----
1
2
3

SELECT id
FROM t2;

id
---
2
3
4
```

Simulate MySQL INTERSECT operator using DISTINCT operator and INNER JOIN clause.
- The following statement uses DISTINCT operator and INNER JOIN clause to return the distinct rows in both tables:

```
SELECT DISTINCT 
   id 
FROM t1
   INNER JOIN t2 USING(id);
   
id
----
2
3


ELECT DISTINCT
    id
FROM
    t1
WHERE
    id IN (SELECT 
            id
        FROM
            t2);
	    
id
----
2
3
```


### Minus
basic concept
```
SELECT column_list_1 FROM table_1
MINUS 
SELECT columns_list_2 FROM table_2;
```
- MYSQL에서는 Minus 기능을 지원하지 않는다.
- 그렇다면 어떻게...? LEFT join을 응용하면 됩니다.

![alt text](http://www.mysqltutorial.org/wp-content/uploads/2017/07/MySQL-MINUS.png)


```
SELECT 
    column_list 
FROM 
    table_1
    LEFT JOIN table_2 ON join_predicate
WHERE 
    table_2.id IS NULL; 
```

```
SELECT 
    id
FROM
    t1
        LEFT JOIN
    t2 USING (id)
WHERE
    t2.id IS NULL;
```

### Insert
The INSERT statement allows you to insert one or more rows into a table. The following illustrates the syntax of the INSERT statement

basice concept
```
INSERT INTO table(c1,c2,...)
VALUES (v1,v2,...);


INSERT INTO table(c1,c2,...)
VALUES 
   (v11,v12,...),
   (v21,v22,...),
    ...
   (vnn,vn2,...);
```

ref : http://www.mysqltutorial.org/mysql-insert-statement.aspx


### Insert into select

basic concept

```
INSERT INTO table_name(column_list)
SELECT 
   select_list 
FROM 
   another_table;
```

아래와 같은 테이블을 만든다고 하자.

```
CREATE TABLE suppliers (
    supplierNumber INT AUTO_INCREMENT,
    supplierName VARCHAR(50) NOT NULL,
    phone VARCHAR(50),
    addressLine1 VARCHAR(50),
    addressLine2 VARCHAR(50),
    city VARCHAR(50),
    state VARCHAR(50),
    postalCode VARCHAR(50),
    country VARCHAR(50),
    customerNumber INT
    PRIMARY KEY (supplierNumber)
);
```


```
SELECT 
    customerNumber,
    customerName,
    phone,
    addressLine1,
    addressLine2,
    city,
    state,
    postalCode,
    country
FROM
    customers
WHERE
    country = 'USA' AND 
    state = 'CA';
```

![alt text](http://www.mysqltutorial.org/wp-content/uploads/2018/09/MySQL-INSERT-INTO-SELECT-customers-data-to-be-inserted.png)

새롭게 생성한 테이블에 아래의 쿼리문을 넣으면...!!
```
INSERT INTO suppliers (
    supplierName, 
    phone, 
    addressLine1,
    addressLine2,
    city,
    state,
    postalCode,
    country,
    customerNumber
)
SELECT 
    customerName,
    phone,
    addressLine1,
    addressLine2,
    city,
    state ,
    postalCode,
    country,
    customerNumber
FROM 
    customers
WHERE 
    country = 'USA' AND 
    state = 'CA';
    
SELECT 
    *
FROM
    suppliers;
```

이렇게 된다.
![alt text](http://www.mysqltutorial.org/wp-content/uploads/2018/09/MySQL-INSERT-INTO-SELECT-Example.png)

### IF
- MySQL IF function is one of the MySQL control flow functions that returns a value based on a condition. The IF function is sometimes referred to as IF ELSE or IF THEN ELSE function.

```
IF(expr,if_true_expr,if_false_expr)
```

- If the expr evaluates to TRUE i.e., expr is not NULL and expr is not 0, the IF function returns the if_true_expr , otherwise, it returns if_false_expr The IF function returns a numeric or a string, depending on how it is used.

```
SELECT IF(1 = 2,'true','false'); -- false

SELECT IF(1 = 1,' true','false'); -- true
```

```
SELECT
    customerNumber, customerName, state, country
FROM
    customers;
    
#,customerNumber,customerName,state,country
1,103,Atelier graphique,,France
2,112,Signal Gift Stores,NV,USA
3,114,"Australian Collectors, Co.",Victoria,Australia
4,119,La Rochelle Gifts,,France
5,121,Baane Mini Imports,,Norway
6,124,Mini Gifts Distributors Ltd.,CA,USA
7,125,Havel & Zbyszek Co,,Poland



SELECT
    customerNumber,
    customerName,
    if(state is Null,'N/A', state),
    country
FROM
    customers;

#,customerNumber,customerName,"if(state is Null,'N/A', state)",country
1,103,Atelier graphique,N/A,France
2,112,Signal Gift Stores,NV,USA
3,114,"Australian Collectors, Co.",Victoria,Australia
4,119,La Rochelle Gifts,N/A,France
5,121,Baane Mini Imports,N/A,Norway
6,124,Mini Gifts Distributors Ltd.,CA,USA
7,125,Havel & Zbyszek Co,N/A,Poland
```

MySQL IF function with aggregate functions
- MySQL SUM IF – Combining the IF function with the SUM function
- The IF function is useful when it combines with an aggregate function. Suppose if you want to know how many orders have been shipped and cancelled, you can use the IF function with the SUM aggregate function as the following query:

```
select
    sum(if(status = 'Shipped', 1, 0)) AS shipped,
    sum(if(status = 'Cancelled', 1, 0)) AS Cancelled
from
    orders;

#,shipped,Cancelled
1,303,6
```

MySQL COUNT IF – Combining the IF function with the COUNT function

```
SELECT DISTINCT
    status
FROM
    orders
ORDER BY status;

#,status
1,Cancelled
2,Disputed
3,In Process
4,On Hold
5,Resolved
6,Shipped


SELECT 
    COUNT(IF(status = 'Cancelled', 1, NULL)) Cancelled,
    COUNT(IF(status = 'Disputed', 1, NULL)) Disputed,
    COUNT(IF(status = 'In Process', 1, NULL)) 'In Process',
    COUNT(IF(status = 'On Hold', 1, NULL)) 'On Hold',
    COUNT(IF(status = 'Resolved', 1, NULL)) 'Resolved',
    COUNT(IF(status = 'Shipped', 1, NULL)) 'Shipped'
FROM
    orders;
    
#,Cancelled,Disputed,In Process,On Hold,Resolved,Shipped
1,6,3,6,4,4,303 
```

같은 결과로 아래와 같이 사용 할 수도 있다.
```
SELECT status, COUNT(STATUS)
FROM orders
GROUP BY status
```
- 가로로 보여주느냐,...세로로 보여주느냐...



### Update
- use the UPDATE statement to update existing data in a table. you can also use the UPDATE statement to change column values of a single row, a group of rows, or all rows in a table.

```
UPDATE [LOW_PRIORITY] [IGNORE] table_name 
SET 
    column_name1 = expr1,
    column_name2 = expr2,
    ...
[WHERE
    condition];
```

- First, specify the table name that you want to update data after the UPDATE keyword.
- Second, the SET clause specifies which column that you want to modify and the new values. To update multiple columns, you use a list of comma-separated assignments. You supply a value in each column’s assignment in the form of a literal value, an expression, or a subquery.
- Third, specify which rows to be updated using a condition in the WHERE clause. The WHERE clause is optional. If you omit the WHERE clause, the UPDATE statement will update all rows in the table.


**LOW_PRIORITY**, **IGNORE**
- The LOW_PRIORITY modifier instructs the UPDATE statement to delay the update until there is no connection reading data from the table. The LOW_PRIORITY takes effect for the storage engines that use table-level locking only, for example, MyISAM, MERGE, MEMORY.
- The IGNORE modifier enables the UPDATE statement to continue updating rows even if errors occurred. The rows that cause errors such as duplicate-key conflicts are not updated.


```
SELECT 
    firstname, lastname, email
FROM
    employees
WHERE
    employeeNumber = 1056;
    
#	firstname	lastname	email
1	Mary	Patterson	mpatterso@classicmodelcars.com


update employees
set email = 'mary.patterson@classicmodelcars.com'
where employeeNumber = 1056;


SELECT 
    firstname, lastname, email
FROM
    employees
WHERE
    employeeNumber = 1056;

#	firstname	lastname	email
1	Mary	Patterson	mary.patterson@classicmodelcars.com
```

```
UPDATE employees 
SET 
    lastname = 'Hill',
    email = 'mary.hill@classicmodelcars.com'
WHERE
    employeeNumber = 1056;
    

SELECT
    firstname, lastname, email
FROM
    employees
WHERE
    employeeNumber = 1056;


#	firstname	lastname	email
1	Mary	Hill	mary.hill@classicmodelcars.com
```

```
SELECT 
    customername, salesRepEmployeeNumber
FROM
    customers
WHERE
    salesRepEmployeeNumber IS NULL;


#	customername	salesRepEmployeeNumber
1	Havel & Zbyszek Co	
2	Porto Imports Co.	
3	Asian Shopping Network, Co	
4	Natürlich Autos	
5	ANG Resellers	
6	Messner Shopping Network



UPDATE customers 
SET 
    salesRepEmployeeNumber = (SELECT 
            employeeNumber
        FROM
            employees
        WHERE
            jobtitle = 'Sales Rep'
        LIMIT 1)
WHERE
    salesRepEmployeeNumber IS NULL;
    
    
SELECT
     salesRepEmployeeNumber
FROM
    customers
WHERE
    salesRepEmployeeNumber IS NOT NULL;
    
    #	salesRepEmployeeNumber
1	1165
2	1165
3	1165
4	1165
5	1165
6	1165
7	1165
8	1165
9	1165
10	1165
11	1165
.....
```

---------------------------------------------------------------
### tips..
- [MySQL IN() for two value/array?](https://stackoverflow.com/questions/793784/mysql-in-for-two-value-array)
- mysql에서 한 쌍의 값을 검색하는 방법을 알아보자.


우선 질문 올라온 건 아래와 같다.
```
foo,1
boo,2
goo,3

SELECT * FROM [table] WHERE 
(column1 = 'foo' AND column2 = 1) OR
(column1 = 'boo' AND column2 = 2) OR
(column1 = 'goo' AND column2 = 3);
```

답변은 아래와 같다.
```
SELECT  *
FROM    foo
WHERE   ROW(column1, column2) IN (ROW('foo', 1), ROW('bar', 2))


SELECT *
FROM `foo`
WHERE (`id_obj` , `Foo_obj`)
IN (
  SELECT `id_obj` , `Foo_obj`
  FROM `foo`
  GROUP BY `id_obj` , `Foo_obj`
  HAVING count(*) > 1
)
```

테스트 해보니까 첫 번째 답변이 가장 좋은 것 같다. 직관적이고, 깔끔하다.
단,왜 작동 하는지 모르겠다.....

---------------------------------------------------------------


### Date Function

#### CURDATE
- The CURDATE() function returns the current date as a value in the 'YYYY-MM-DD' format if it is used in a string context or YYYMMDD format if it is used in a numeric context.

shows how the CURDATE() function is used in the string context.
```
select curdate();

2019-08-16
```

illustrates how the CURDATE() function is used in a numeric context
```
select curdate() + 0;

20190816
```

The CURRENT_DATE and CURRENT_DATE() are synonyms for CURDATE().
```
select current_date(), current_date, curdate()

#	current_date()	current_date	curdate()
1	2019-08-16	2019-08-16	2019-08-16
```

CURDATE vs. NOW
- The CURDATE() function returns the current date with date part only while the NOW() function returns both date and time parts of the current time.

```
select current_date, now()

#	current_date	now()
1	2019-08-16	2019-08-16 17:14:14


select date(now());

2019-08-16
```


#### DATEDIFF
- The MySQL DATEDIFF function calculates the number of days between two  DATE,  DATETIME, or  TIMESTAMP values.
- http://www.mysqltutorial.org/mysql-datediff.aspx

basic concept
```
DATEDIFF(date_expression_1,date_expression_2);
```

```
SELECT DATEDIFF('2011-08-17','2011-08-17'); --  0 day
0

SELECT DATEDIFF('2011-08-17','2011-08-08'); --  9 days
9

SELECT DATEDIFF('2011-08-08','2011-08-17'); -- -9 days
-9
```

```
SELECT 
    orderNumber, 
    DATEDIFF(requiredDate, shippedDate) daysLeft
FROM
    orders
ORDER BY daysLeft DESC;

#	orderNumber	daysleft
1	10409	11
2	10410	10
3	10105	9
4	10135	9
5	10190	9
6	10201	9
7	10254	9
8	10257	9
9	10289	9
10	10299	9
11	10302	9


SELECT
    orderNumber,
    DATEDIFF(requiredDate, orderDate) remaining_days
FROM
    orders
WHERE
    status = 'In Process'
ORDER BY remaining_days;

#	orderNumber	remaining_days
1	10423	6
2	10425	7
3	10421	8
4	10424	8
5	10420	9
6	10422	12


SELECT 
    orderNumber,
    ROUND(DATEDIFF(requiredDate, orderDate) / 7, 2),
    ROUND(DATEDIFF(requiredDate, orderDate) / 30,2)
FROM
    orders
WHERE
    status = 'In Process';
    
#	orderNumber	ROUND(DATEDIFF(requiredDate, orderDate) / 7, 2)	ROUND(DATEDIFF(requiredDate, orderDate) / 30,2)
1	10420	1.29	0.30
2	10421	1.14	0.27
3	10422	1.71	0.40
4	10423	0.86	0.20
5	10424	1.14	0.27
6	10425	1.00	0.23    
```

#### DAY

```
DAY(date)

SELECT DAY('2010-01-15');

15

응용하면....


SELECT 
    DAY(orderdate) dayofmonth, 
    COUNT(*)
FROM
    orders
WHERE
    YEAR(orderdate) = 2004
GROUP BY dayofmonth
ORDER BY dayofmonth;

#	dayofmonth	COUNT(*)
1	1	5
2	2	9
3	3	7
4	4	8
5	5	6
6	6	3
7	7	4
8	8	4
9	9	7
10	10	7
11	11	3
12	12	5
13	13	3
...
```

#### DATE_ADD
- http://www.mysqltutorial.org/mysql-date_add/
- The DATE_ADD function adds an interval to a DATE or DATETIME value. The following illustrates the syntax of the DATE_ADD function

```
DATE_ADD(start_date, INTERVAL expr unit);
```

The DATE_ADD function may return a DATETIME value or a string, depending on the arguments:
- 처음에 입력되는 형에 따라 달라진다.
- DATETIME if the first argument is a DATETIME value or if the interval value has time element such as hour, minute or second, etc.
- String otherwise.

```
SELECT 
    DATE_ADD('1999-12-31 23:59:59',
        INTERVAL 1 SECOND) result;
 
+---------------------+
| result              |
+---------------------+
| 2000-01-01 00:00:00 |
+---------------------+
1 row in set (0.00 sec)
```

```
SELECT 
    DATE_ADD('1999-12-31 00:00:01',
        INTERVAL 1 DAY) result;        
 
+---------------------+
| result              |
+---------------------+
| 2000-01-01 00:00:01 |
+---------------------+
1 row in set (0.00 sec)
```

- 멀티타입으로 입력도 되는 듯 minute_second
```
SELECT 
    DATE_ADD('1999-12-31 23:59:59',
        INTERVAL '1:1' MINUTE_SECOND) result;
 
+---------------------+
| result              |
+---------------------+
| 2000-01-01 00:01:00 |
+---------------------+
1 row in set (0.00 sec)
```

```
SELECT DATE_ADD('2000-01-01 00:00:00',
     INTERVAL '-1 5' DAY_HOUR) result;
 
+---------------------+
| result              |
+---------------------+
| 1999-12-30 19:00:00 |
+---------------------+
1 row in set (0.00 sec)
```

**interval handling**

```
SELECT 
    DATE_ADD('2000-01-01',
        INTERVAL 5 / 2 HOUR_MINUTE) result;
 
 
+---------------------+
| result              |
+---------------------+
| 2000-01-04 13:20:00 |
+---------------------+
1 row in set (0.00 sec)
```

```
SELECT 
    DATE_ADD('2010-01-30', 
              INTERVAL 1 MONTH) result;
 
+------------+
| result     |
+------------+
| 2010-02-28 |
+------------+
1 row in set (0.00 sec)
```
#### DATE_SUB
- http://www.mysqltutorial.org/mysql-date_sub/
- 결론적으로는 사용법 자체는 DATE_ADD랑 다를게 없는듯
- The DATE_SUB() function subtracts a time value (or an interval) from a DATE or DATETIME value. The following illustrates the DATE_SUB() function:

```
DATE_SUB(start_date,INTERVAL expr unit)
```

The DATE_SUB() function accepts two arguments:
- start_date is the starting DATE or DATETIME value.
- expr is a string that determines an interval value to be subtracted from the starting date. The unit is the interval unit that expr should be interpreted e.g., DAY, HOUR, etc.

```
SELECT DATE_SUB('2017-07-04',INTERVAL 1 DAY) result;
+------------+
| result     |
+------------+
| 2017-07-03 |
+------------+
1 row in set (0.00 sec) 
```

#### DATE_FORMAT
- To format a date value to a specific format, you use the DATE_FORMAT function. The syntax of the DATE_FORMAT function is as follows:

```
DATE_FORMAT(date,format)
```

The DATE_FORMAT function accepts two arguments:
- date : is a valid date value that you want to format
- format : is a format string that consists of predefined specifiers. Each specifier is preceded by a percentage character ( % ). See the table below for a list of predefined specifiers.
- 결국 %가 있는 부분을 잘 처리해야 하는데....너무 많아서 링크를 걸어놓고 자주 보자.
- http://www.mysqltutorial.org/mysql-date_format/

```
SELECT 
    orderNumber,
    DATE_FORMAT(orderdate, '%Y-%m-%d') orderDate,
    DATE_FORMAT(requireddate, '%a %D %b %Y') requireddate,
    DATE_FORMAT(shippedDate, '%W %D %M %Y') shippedDate
FROM
    orders;
    
    #	orderNumber	orderDate	requireddate	shippedDate
1	10100	2003-01-06	Mon 13th Jan 2003	Friday 10th January 2003
2	10101	2003-01-09	Sat 18th Jan 2003	Saturday 11th January 2003
3	10102	2003-01-10	Sat 18th Jan 2003	Tuesday 14th January 2003
4	10103	2003-01-29	Fri 7th Feb 2003	Sunday 2nd February 2003
5	10104	2003-01-31	Sun 9th Feb 2003	Saturday 1st February 2003
6	10105	2003-02-11	Fri 21st Feb 2003	Wednesday 12th February 2003
7	10106	2003-02-17	Mon 24th Feb 2003	Friday 21st February 2003
....
```

#### STR_TO_DATE()
- http://www.mysqltutorial.org/mysql-str_to_date/
- The STR_TO_DATE() converts the str string into a date value based on the fmt format string. The STR_TO_DATE() function may return a DATE , TIME, or DATETIME value based on the input and format strings. If the input string is illegal, the STR_TO_DATE() function returns NULL.
- 쿼리 할 때 종종 필요하더라...
```
STR_TO_DATE(str,fmt);
```

```
SELECT STR_TO_DATE('21,5,2013','%d,%m,%Y');

2013-05-21
```

Based on the format string ‘%d, %m, %Y’, the STR_TO_DATE() function scans the ‘21,5,2013’ input string.
- First, it attempts to find a match for the %d format specifier, which is a day of the month (01…31), in the input string. Because the number 21 matches with the %d specifier, the function takes 21 as the day value.
- Second, because the comma (,) literal character in the format string matches with the comma in the input string, the function continues to check the second format specifier %m , which is a month (01…12), and finds that the number 5 matches with the %m format specifier. It takes the number 5 as the month value.
- Third, after matching the second comma (,), the STR_TO_DATE() function keeps finding a match for the third format specifier %Y , which is four-digit year e.g., 2012,2013, etc., and it takes the number 2013 as the year value.

Point!!
- The STR_TO_DATE() function ignores extra characters at the end of the input string when it parses the input string based on the format string. See the following example:

```
SELECT STR_TO_DATE('21,5,2013 extra characters','%d,%m,%Y');

2013-05-21
```

불완전한 값이 들어오면...
- The STR_TO_DATE() sets all incomplete date values, which are not provided by the input string, to zero. See the following example:
```
SELECT STR_TO_DATE('2013','%Y');

2012-11-30


SELECT STR_TO_DATE('113005','%h%i%s');

11:30:05
```

#### extract
- The EXTRACT() function extracts part of a date.
- The EXTRACT() function requires two arguments unit and date.
- unit 지정하고, datetime을 넣어서 추출하면 됨!

```sql
extract (unit from date)
```

example query
````sql
SELECT EXTRACT(DAY FROM '2017-07-14 09:04:44') DAY;
+------+
| DAY  |
+------+
|   14 |
+------+
````

````sql
SELECT EXTRACT(DAY_HOUR FROM '2017-07-14 09:04:44') DAYHOUR;
+---------+
| DAYHOUR |
+---------+
|    1409 |
+---------+
````

````sql
SELECT EXTRACT(YEAR FROM '2017-07-14 09:04:44') YEAR;
+------+
| YEAR |
+------+
| 2017 |
+------+
````

````sql
SELECT EXTRACT(YEAR_MONTH FROM '2017-07-14 09:04:44') YEARMONTH;
+-----------+
| YEARMONTH |
+-----------+
|    201707 |
+-----------+
````

------------------------------
