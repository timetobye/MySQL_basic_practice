Hackerrank SQL Quiz
--------------------

# Introduction
Hackerrank 사이트에서 제공하는 SQL 문제 풀이 기록입니다.
- https://www.hackerrank.com/domains/sql?badge_type=sql
- 사이트에서 풀었던 문제의 해답만 기록합니다. ~~사이트도 먹고 살아야죠~~
- 사이트에서 제공하는 순서대로 풀었습니다.
- MySQL을 선택하였습니다.

---------------

## Select All

```sql
select *
from city
```

## Select By ID

```sql
select *
from city
where id = 1661
```

## Revising the Select Query I

```sql
select *
from city
where countrycode='USA' and population > 100000
```

## Revising the Select Query II

```sql
select name
from city
where countrycode = 'USA' and population > 120000
```

## Japanese Cities' Attributes

```sql
select *
from city
where countrycode = 'JPN'
```

## Japanese Cities' Names

```sql
select Name
from city
where countrycode = 'JPN'
```

## Weather Observation Station 1

```sql
select city, state
from station
```

## Weather Observation Station 3

```sql
select distinct city
from station
where (id % 2) = 0
```

## Weather Observation Station 4

```sql
select count(*) - count(distinct city)
from station
```

## Weather Observation Station 5

```sql
select city, length(city)
from station
order by 2 asc, 1
limit 1;

select city, length(city)
from station
order by 2 desc, 1
limit 1;
```

## Weather Observation Station 6

```sql
select distinct city
from station
where left(city, 1) in ('a', 'e', 'i', 'o', 'u')
```

## Weather Observation Station 7

```sql
select distinct city
from station
where right(city, 1) in ('a', 'e', 'i', 'o', 'u')
```

## Weather Observation Station 8

```sql
select distinct city
from station
where right(city, 1) in ('a', 'e', 'i', 'o', 'u') 
and left(city, 1) in ('a', 'e', 'i', 'o', 'u')
```

## Weather Observation Station 9

```sql
select distinct city
from station
where left(city, 1) not in ('a', 'e', 'i', 'o', 'u')
```

## Weather Observation Station 10

```sql
select distinct city
from station
where right(city, 1) not in ('a', 'e', 'i', 'o', 'u')
```

## Weather Observation Station 11

```sql
select distinct city
from station
where right(city, 1) not in ('a', 'e', 'i', 'o', 'u') 
or left(city, 1) not in ('a', 'e', 'i', 'o', 'u')
```

## Weather Observation Station 12

```sql
select distinct city
from station
where right(city, 1) not in ('a', 'e', 'i', 'o', 'u') 
and left(city, 1) not in ('a', 'e', 'i', 'o', 'u')
```

## Higher Than 75 Marks

```sql
select name
from students
where marks > 75
order by right(name, 3) asc, id
```

## Employee Names

```sql
select name
from Employee
order by 1
```

## Employee Salaries

```sql
select
    name
from Employee
where months < 10 and salary > 2000
order by employee_id asc
```

## Type of Triangle
```sql
select
    case 
        when A + B > C and B + C > A and C + A > B then
            case 
                when A = B and B = C then "Equilateral"
                when A = B or B = C or C = A then "Isosceles"
                else "Scalene"
            end
        else "Not A Triangle"
    end
from TRIANGLES
```