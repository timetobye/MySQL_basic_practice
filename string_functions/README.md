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


