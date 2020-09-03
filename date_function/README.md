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
