String Functions
--------

# String Function Basic

**String functions?**
- the most commonly used MySQL string functions that allow you to manipulate character string data effectively.
- https://www.mysqltutorial.org/mysql-string-functions/


**String functions 종류**
- CONCAT : Concatenate two or more strings into a single string
- INSTR : Return the position of the first occurrence of a substring in a string
- LENGTH : Get the length of a string in bytes and in characters
- LEFT : Get a specified number of leftmost characters from a string
- LOWER : Convert a string to lowercase
- LTRIM : Remove all leading spaces from a string
- REPLACE : Search and replace a substring in a string
- RIGHT : Get a specified number of rightmost characters from a string
- RTRIM : Remove all trailing spaces from a string
- SUBSTRING : Extract a substring starting from a position with a specific length.
- SUBSTRING_INDEX : Return a substring from a string before a specified number of occurrences of a delimiter
- TRIM : Remove unwanted characters from a string.
- FIND_IN_SET : Find a string within a comma-separated list of strings
- FORMAT : Format a number with a specific locale, rounded to the number of decimals
- UPPER : Convert a string to uppercase


## CONCAT and CONCAT_WS
To concatenate two or more quoted string values, you place the string next to each other as the following syntax

```sql
select 'MySQL ' 'String ' 'Concatenation';
```

```bash
MySQL String Concatenation
```

MySQL string concatenation is cleaner in comparison with other database management systems. 
- For example, if you use PostgreSQL or Oracle, you have to use the string concatenation operator ||. 
- In Microsoft SQL server, you use the addition arithmetic operator (+) to concatenate string values.
- 각 DBMS 마다 사용하는 방식이 약간씩 다르니 참고하자.

Besides using spaces for string concatenation, MySQL provides two other functions that concatenate string values
- CONCAT and CONCAT_WS.

### CONCAT

The MySQL CONCAT function takes one or more string arguments and concatenates them into a single string. 

The CONCAT function requires a minimum of one parameter otherwise it raises an error.

기본적인 사용은 아래와 같이 한다.
```sql
CONCAT('string1', 'string2')
```


```sql
SELECT CONCAT('MySQL','CONCAT');
```

```bash
MySQLCONCAT
```


If you add a NULL value, the CONCAT function returns a NULL value as follows:
- Null을 넣으면 Null이 튀어나오는 구조

```sql
SELECT CONCAT('MySQL',NULL,'CONCAT');
```


Sample database에 있는 데이터를 이용하여 조회해보기

```sql
SELECT 
    concat(contactFirstName,' ',contactLastName) Fullname
FROM
    customers;
```

```bash
#,Fullname
1,Carine  Schmitt
2,Jean King
3,Peter Ferguson
4,Janine  Labrune
5,Jonas  Bergulfsen
6,Susan Nelson
7,Zbyszek  Piestrzeniewicz
8,Roland Keitel
9,Julie Murphy
10,Kwai Lee
11,Diego  Freyre
12,Christina  Berglund
....
```

문자열 + 공백 + 문자열 구조로 출력되어서 나온다.

### CONCAT_WS

기본적인 구조는 아래와 같다.
- The first argument is the separator for other arguments: string1, string2, …
- The CONCAT_WS function adds the separator between string arguments and returns a single string with the separator inserted between string arguments.

```sql
CONCAT_WS(seperator,string1,string2, ... );
```

예시
```sql
SELECT CONCAT_WS(',','John','Doe');
```

```bash
John,Doe
```

Null이 들어갈 경우에는 null을 무시하고 나머지 항목만 연산 후 돌려준다.
- Unlike the CONCAT function, the CONCAT_WS function skips NULL values after the separator argument. In other words, it ignores NULL values.

```sql
SELECT CONCAT_WS(',','Jonathan', 'Smith',NULL);
```

```bash
Jonathan,Smith
```



```sql
SELECT
    CONCAT_WS(CHAR(13),
            CONCAT_WS(' ', contactLastname, contactFirstname),
            addressLine1,
            addressLine2,
            CONCAT_WS(' ', postalCode, city),
            country,
            CONCAT_WS(CHAR(13), '')) AS Customer_Address
FROM
    customers;
```

```bash
Customer_Address
"Schmitt Carine 
54, rue Royale
44000 Nantes
France
"
King Jean
8489 Strong St.
83030 Las Vegas
USA

....
```

## INSTR

The INSTR function returns the position of the first occurrence of a substring in a string. 
If the substring is not found in the str, the INSTR function returns zero (0).
- Like처럼 문자열을 찾을 때 사용하는 함수 중 하나이다.
- Like와 비교하면 속도면에서는 같다.
  - 그러나 index가 걸린 컬럼이라면 Like가 월등히 빠른 속도를 보여준다.
  > The INSTR function performs a table scan even though the productname column has an index. This is because MySQL cannot make any assumption about the semantics of the INSTR function, whereby MySQL can utilize its understanding of the semantics of the LIKE operator.
  - 그렇기 때문에 굳이...써야하나..?
  - The fastest way to test if a substring exists in a string is to use a **full-text index**. 
  However, it is required a configure and maintain the index properly.
  
```sql
INSTR(str,substr);
```

- The str is the string that you want to search in.
- The substr is the substring that you want to search for.


```sql
SELECT INSTR('MySQL INSTR', 'MySQL');
```

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2013/12/MySQL-INSTR-example.jpg)


## LENGTH

LENGTH
- To get the length of a string measured in bytes, you use the LENGTH  function as follows:

```sql
LENGTH(str);
```

CHAR_LENGTH
- You use the CHAR_LENGTH  function to get the length of a string measured in characters as follows:

무슨 차이가 있을까?
- LENGTH는 문자의 Byte 길이를 가져온다. 한글은 정확한 길이를 알 수가 없다.
- CHAR_LENGTH는 순수한 길이만을 가져오기 때문에 한 글자의 한글이 작성되어도 문자열의 길이는 1이 된다.

나의 경우에는 CHAR_LENGTH를 사용하는 것이 더 많을 것 같다.

```sql
CHAR_LENGTH(str);
```


## LEFT
The LEFT function is a string function that returns the left part of a string with a specified length.

```sql
LEFT(str,length);
```

The LEFT function accepts two arguments:
- The str is the string that you want to extract the substring.
- The length is a positive integer that specifies the number of characters will be returned.


The LEFT function returns the leftmost length characters from the str string. 
- It returns a NULL value if either str or length argument is NULL.
- NULL을 넣으면 NULL을 리턴한다.

If the length is zero or negative, the LEFT function returns an empty string. 
- 0 또는 음수가 들어올 경우 empty string을 전달한다.

If the length is greater than the length of the str string, the LEFT function returns the entire str string.
- 주어진 string보다 긴 숫자가 들어올 경우 string 전체를 반환한다.

일반적인 경우

```sql
SELECT LEFT('MySQL LEFT', 5);
```

````bash
MySQL
````

전체 길이보다 큰 수가 들어왔을 경우

````sql
SELECT LEFT('MySQL LEFT', 9999);
````

````bash
MySQL LEFT
````

음의 값이 들어왔을 경우

````sql
SELECT LEFT('MySQL LEFT', -2);
````

````bash

````

NULL이 들어올 경우

```sql
SELECT LEFT('MySQL LEFT', NULL);
```

````bash
NULL
````


## LOWER

The LOWER() function accepts a string argument and returns the lowercase version of that string.
- 특별할 건 없다. 대문자로 되어 있는 것을 소문자로 처리해준다.

```sql
LOWER(str)
```

```sql
SELECT 
    LOWER('MySQL')
```


```bash
+----------------+
| LOWER('MySQL') |
+----------------+
| mysql          |
+----------------+
1 row in set (0.00 sec)
```


## LTRIM
The LTRIM() function takes a string argument and returns a new string with all the leading space characters removed.
- 왼쪽 공백만 제거 한다.

```sql
SELECT 
    LTRIM('   MySQL Tutorial') result;
```

## Replace
MySQL provides you with a useful string function called REPLACE that allows you to replace a string in a column of a table by a new string.
- 문자열을 원하는 형태로 바꾸는 것
- The REPLACE function is very handy to search and replace text in a table such as updating obsolete URL, correcting a spelling mistake, etc.
  - UPDATE, SET과 결합해서 사용하면 잘못 입력된 경우를 고칠 수 있다.

```sql
REPLACE(str,old_string,new_string);
```

```sql
UPDATE tbl_name 
SET 
    field_name = REPLACE(field_name,
        string_to_find,
        string_to_replace)
WHERE
    conditions;
```

```sql
UPDATE products 
SET 
    productDescription = REPLACE(productDescription,
        'abuot',
        'about');
```

### RIGHT
The MySQL RIGHT() function extracts a specified number of rightmost characters from a string.

````sql
RIGHT(str,length)
````

The RIGHT() function accepts two arguments:
- str is the string from which you want to extract the substring.
- length is the number the rightmost characters that you want to extract from the str.

The RIGHT() function will return NULL if any argument is NULL.


````sql
SELECT RIGHT('MySQL', 3);
````

```bash
+-------------------+
| RIGHT('MySQL', 3) |
+-------------------+
| SQL               |
+-------------------+
1 row in set (0.00 sec)
```

````sql
SET @str = '12/31/2019';
SELECT 
    RIGHT(@str, 4) year,
    LEFT(@str, 2) month,
    SUBSTRING(@str, 4, 2) day;
````

```bash
+------+-------+------+
| year | month | day  |
+------+-------+------+
| 2019 | 12    | 31   |
+------+-------+------+
1 row in set (0.00 sec)
```


## RTRIM
The RTRIM() function takes a string argument and returns a new string with all the trailing space characters removed.
- 오른쪽 공백을 지우는 함수이다.

```sql
SELECT 
    RTRIM('MySQL   ') result;
```

```bash
+--------+
| result |
+--------+
| MySQL  |
+--------+
1 row in set (0.00 sec)
```

## SUBSTRING
The SUBSTRING function returns a substring with a given length from a string starting at a specific position. MySQL provides various forms of the substring function.

```sql
SUBSTRING(string,position);
SUBSTRING(string FROM position);
```

### MySQL SUBSTRING with position parameter
substring에 양수 값을 position으로 지정할 때

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2011/05/mysql-substring-string-demo.png)


```sql
SELECT SUBSTRING('MYSQL SUBSTRING', 7);
```

문자의 7번째 부터 출력을 해준다.

```bash
SUBSTRING
``` 

substring에 음수 값을 position으로 지정할 때

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2011/05/MySQLsubstring-with-negative-position.png)


````sql
SELECT SUBSTRING('MySQL SUBSTRING',-10);
````

문자열의 -10번 위치를 시작 값으로 하여 문자열을 출력한다.


if the position is zero, the SUBSTRING function returns an empty string
- 0을 넣을 경우 빈 문자열을 돌려준다.

```sql
SELECT SUBSTRING('MYSQL SUBSTRING', 0);
```

From을 써서 표현을 할 수 있다.
- 이럴 경우 ','는 빠지게 된다.

```sql
SELECT SUBSTRING('MySQL SUBSTRING' FROM -10);
```

### MySQL SUBSTRING with position and length
If you want to specify the length of the substring that you want to extract from a string, you can use the following form of the SUBSTRING function:
- 시작지점과 끝점을 지정하고 싶을 때는 아래와 같이 한다.

```sql
SUBSTRING(string,position,length);
```

```sql
-- SQL-standard version
SUBSTRING(string FROM position FOR length);
```

아래의 문자열 구성에서 MySQL만 뽑아낸다고 하자.

![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2011/05/mysql-substring-string-demo.png)

```sql
SELECT SUBSTRING('MySQL SUBSTRING',1,5);
```

```sql
SELECT SUBSTRING('MySQL SUBSTRING' FROM 1 FOR 5);
```

아래처럼 음의 값을 넣을 때도 동일하게 MySQL이라는 단어를 추출할 수 있다.
![alt text](https://sp.mysqltutorial.org/wp-content/uploads/2011/05/MySQLsubstring-with-negative-position.png)

```sql
SELECT SUBSTRING('MySQL SUBSTRING',-15,5);
```

```sql
SELECT SUBSTRING('MySQL SUBSTRING' FROM -15 FOR 5);
```


## Substring_index
The SUBSTRING_INDEX() function returns a substring from a string before a specified number of occurrences of the delimiter.
- 지정한 delimiter 를 기준으로 지정된 index 까지만큼만 출력하는 함수

```sql
SUBSTRING_INDEX(str,delimiter,n)
```

- str is the string from which you want to extract a substring.
- delimiter is a string that acts as a delimiter. The function performs a case-sensitive match when searching for the delimiter.
- n is an integer that specifies the number of occurrences of the delimiter. 
The n can be negative or positive. If n is positive, the function returns every character from the left of the string up to n number of occurrences of the delimiter. 
If n is negative, the function returns every character from right up to n number of occurrences of the delimiter.


### Using MySQL SUBSTRING_INDEX() function with a positive number of occurrences of a delimiter

Hello world에서 'l'에 의해 다음과 같이 'He / l / lo wor / ld' 나누어 진다. 그리고 각 index를 기준으로 이전에 구성된 string을 다 포함하는 방식을 취한다.

```sql
SELECT 
    SUBSTRING_INDEX('Hello World', 'l', 1);
```

```bash
+----------------------------------------+
| SUBSTRING_INDEX('Hello World', 'l', 1) |
+----------------------------------------+
| He                                     |
+----------------------------------------+
1 row in set (0.00 sec)
```

```sql
SELECT 
    SUBSTRING_INDEX('Hello World', 'l', 2);
```

```bash
+----------------------------------------+
| SUBSTRING_INDEX('Hello World', 'l', 2) |
+----------------------------------------+
| Hel                                    |
+----------------------------------------+
1 row in set (0.00 sec)
```

```sql
SELECT 
    SUBSTRING_INDEX('Hello World', 'l', 3);
```

```bash
+----------------------------------------+
| SUBSTRING_INDEX('Hello World', 'l', 3) |
+----------------------------------------+
| Hello Wor                              |
+----------------------------------------+
1 row in set (0.00 sec)
```

### Using MySQL SUBSTRING_INDEX() function with a negative number of occurrences of a delimiter

양수가 아닌 음수 index가 들어가도 원리는 동일하다.

```sql
SELECT 
    SUBSTRING_INDEX('Hello World', 'l', -1);
```

```bash
+-----------------------------------------+
| SUBSTRING_INDEX('Hello World', 'l', -1) |
+-----------------------------------------+
| d                                       |
+-----------------------------------------+
1 row in set (0.00 sec)
```

```sql
SELECT
    SUBSTRING_INDEX('Hello World', 'l', - 2) result1,
    SUBSTRING_INDEX('Hello World', 'l', - 3) result2;
```

```bash
+---------+----------+
| result1 | result2  |
+---------+----------+
| o World | lo World |
+---------+----------+
1 row in set (0.00 sec)
```

### Using MySQL SUBSTRING_INDEX() function with the table data example
example을 살펴보면 공백(' ')으로 나눈 후에 필요한 문자열을 출력하는 방식을 취하고 있다.


```sql
SELECT
    customerName,
    addressLine1,
    SUBSTRING_INDEX(addressLine1, ' ', 1) house_no # 공백으로 split한 뒤 필요한 값만 취하기.
FROM
    customers
WHERE
    country = 'USA'
ORDER BY
    customerName;
```

```bash
+------------------------------+---------------------------+----------+
| customerName                 | addressLine1              | house_no |
+------------------------------+---------------------------+----------+
| American Souvenirs Inc       | 149 Spinnaker Dr.         | 149      |
| Auto-Moto Classics Inc.      | 16780 Pompton St.         | 16780    |
| Boards & Toys Co.            | 4097 Douglas Av.          | 4097     |
| Cambridge Collectables Co.   | 4658 Baden Av.            | 4658     |
| Classic Gift Ideas, Inc      | 782 First Street          | 782      |
| Classic Legends Inc.         | 5905 Pompton St.          | 5905     |
| Collectable Mini Designs Co. | 361 Furth Circle          | 361      |
| Collectables For Less Inc.   | 7825 Douglas Av.          | 7825     |
| Corporate Gift Ideas Co.     | 7734 Strong St.           | 7734     |
| Diecast Classics Inc.        | 7586 Pompton St.          | 7586     |
| Diecast Collectables         | 6251 Ingle Ln.            | 6251     |
| FunGiftIdeas.com             | 1785 First Street         | 1785     |
| Gift Depot Inc.              | 25593 South Bay Ln.       | 25593    |
| Gift Ideas Corp.             | 2440 Pompton St.          | 2440     |
| Gifts4AllAges.com            | 8616 Spinnaker Dr.        | 8616     |
| Land of Toys Inc.            | 897 Long Airport Avenue   | 897      |
| Marta's Replicas Co.         | 39323 Spinnaker Dr.       | 39323    |
| Men 'R' US Retailers, Ltd.   | 6047 Douglas Av.          | 6047     |
| Microscale Inc.              | 5290 North Pendale Street | 5290     |
| Mini Classics                | 3758 North Pendale Street | 3758     |
| Mini Creations Ltd.          | 4575 Hillside Dr.         | 4575     |
| Mini Gifts Distributors Ltd. | 5677 Strong St.           | 5677     |
| Mini Wheels Co.              | 5557 North Pendale Street | 5557     |
| Motor Mint Distributors Inc. | 11328 Douglas Av.         | 11328    |
| Muscle Machine Inc           | 4092 Furth Circle         | 4092     |
| Online Diecast Creations Co. | 2304 Long Airport Avenue  | 2304     |
| Online Mini Collectables     | 7635 Spinnaker Dr.        | 7635     |
| Signal Collectibles Ltd.     | 2793 Furth Circle         | 2793     |
| Signal Gift Stores           | 8489 Strong St.           | 8489     |
| Super Scale Inc.             | 567 North Pendale Street  | 567      |
| Technics Stores Inc.         | 9408 Furth Circle         | 9408     |
| Tekni Collectables Inc.      | 7476 Moss Rd.             | 7476     |
| The Sharp Gifts Warehouse    | 3086 Ingle Ln.            | 3086     |
| Toys4GrownUps.com            | 78934 Hillside Dr.        | 78934    |
| Vitachrome Inc.              | 2678 Kingston Rd.         | 2678     |
| West Coast Collectables Co.  | 3675 Furth Circle         | 3675     |
+------------------------------+---------------------------+----------+
36 rows in set (0.00 sec)

```