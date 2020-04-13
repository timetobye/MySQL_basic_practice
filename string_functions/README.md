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