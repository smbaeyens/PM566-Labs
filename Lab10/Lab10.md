Lab9HPC
================
Sylvia Baeyens
10/29/2021

# Setup

``` r
con <- dbConnect(SQLite(), ":memory:")

actor = read.csv("https://raw.githubusercontent.com/ivanceras/sakila/master/csv-sakila-db/actor.csv")
rental = read.csv("https://raw.githubusercontent.com/ivanceras/sakila/master/csv-sakila-db/rental.csv")
customer = read.csv("https://raw.githubusercontent.com/ivanceras/sakila/master/csv-sakila-db/customer.csv")
payment = read.csv("https://raw.githubusercontent.com/ivanceras/sakila/master/csv-sakila-db/payment_p2007_01.csv")

dbWriteTable(con, "actor", actor)
dbWriteTable(con, "rental", rental)
dbWriteTable(con, "customer", customer)
dbWriteTable(con, "payment", payment)

dbListTables(con)
```

    ## [1] "actor"    "customer" "payment"  "rental"

# 1.

``` sql
SELECT actor_id, first_name, last_name
FROM actor
ORDER by last_name, first_name
```

<div class="knitsql-table">

| actor\_id | first\_name | last\_name |
|----------:|:------------|:-----------|
|        58 | CHRISTIAN   | AKROYD     |
|       182 | DEBBIE      | AKROYD     |
|        92 | KIRSTEN     | AKROYD     |
|       118 | CUBA        | ALLEN      |
|       145 | KIM         | ALLEN      |
|       194 | MERYL       | ALLEN      |
|        76 | ANGELINA    | ASTAIRE    |
|       112 | RUSSELL     | BACALL     |
|       190 | AUDREY      | BAILEY     |
|        67 | JESSICA     | BAILEY     |

Displaying records 1 - 10

</div>

# 2.

``` sql
SELECT actor_id, first_name, last_name
FROM actor
WHERE last_name IN ('WILLIAMS', 'DAVIS')
```

<div class="knitsql-table">

| actor\_id | first\_name | last\_name |
|----------:|:------------|:-----------|
|         4 | JENNIFER    | DAVIS      |
|        72 | SEAN        | WILLIAMS   |
|       101 | SUSAN       | DAVIS      |
|       110 | SUSAN       | DAVIS      |
|       137 | MORGAN      | WILLIAMS   |
|       172 | GROUCHO     | WILLIAMS   |

6 records

</div>

# 3.

``` sql
SELECT DISTINCT rental_id
FROM rental
WHERE date(rental_date) = '2005-07-05'
```

<div class="knitsql-table">

| rental\_id |
|:-----------|
| 3470       |
| 3471       |
| 3472       |
| 3473       |
| 3474       |
| 3475       |
| 3476       |
| 3477       |
| 3478       |
| 3479       |

Displaying records 1 - 10

</div>

# 4.1

``` sql
SELECT *
FROM payment
WHERE amount IN (1.99, 7.99, 9.99)
```

<div class="knitsql-table">

| payment\_id | customer\_id | staff\_id | rental\_id | amount | payment\_date              |
|------------:|-------------:|----------:|-----------:|-------:|:---------------------------|
|       16050 |          269 |         2 |          7 |   1.99 | 2007-01-24 21:40:19.996577 |
|       16056 |          270 |         1 |        193 |   1.99 | 2007-01-26 05:10:14.996577 |
|       16081 |          282 |         2 |         48 |   1.99 | 2007-01-25 04:49:12.996577 |
|       16103 |          294 |         1 |        595 |   1.99 | 2007-01-28 12:28:20.996577 |
|       16133 |          307 |         1 |        614 |   1.99 | 2007-01-28 14:01:54.996577 |
|       16158 |          316 |         1 |       1065 |   1.99 | 2007-01-31 07:23:22.996577 |
|       16160 |          318 |         1 |        224 |   9.99 | 2007-01-26 08:46:53.996577 |
|       16161 |          319 |         1 |         15 |   9.99 | 2007-01-24 23:07:48.996577 |
|       16180 |          330 |         2 |        967 |   7.99 | 2007-01-30 17:40:32.996577 |
|       16206 |          351 |         1 |       1137 |   1.99 | 2007-01-31 17:48:40.996577 |

Displaying records 1 - 10

</div>

# 4.2

``` sql
SELECT *
FROM payment
WHERE amount > 5
```

<div class="knitsql-table">

| payment\_id | customer\_id | staff\_id | rental\_id | amount | payment\_date              |
|------------:|-------------:|----------:|-----------:|-------:|:---------------------------|
|       16052 |          269 |         2 |        678 |   6.99 | 2007-01-28 21:44:14.996577 |
|       16058 |          271 |         1 |       1096 |   8.99 | 2007-01-31 11:59:15.996577 |
|       16060 |          272 |         1 |        405 |   6.99 | 2007-01-27 12:01:05.996577 |
|       16061 |          272 |         1 |       1041 |   6.99 | 2007-01-31 04:14:49.996577 |
|       16068 |          274 |         1 |        394 |   5.99 | 2007-01-27 09:54:37.996577 |
|       16073 |          276 |         1 |        860 |  10.99 | 2007-01-30 01:13:42.996577 |
|       16074 |          277 |         2 |        308 |   6.99 | 2007-01-26 20:30:05.996577 |
|       16082 |          282 |         2 |        282 |   6.99 | 2007-01-26 17:24:52.996577 |
|       16086 |          284 |         1 |       1145 |   6.99 | 2007-01-31 18:42:11.996577 |
|       16087 |          286 |         2 |         81 |   6.99 | 2007-01-25 10:43:45.996577 |

Displaying records 1 - 10

</div>

# 4.3

``` sql
SELECT *
FROM payment
WHERE amount > 5 AND amount < 8
```

<div class="knitsql-table">

| payment\_id | customer\_id | staff\_id | rental\_id | amount | payment\_date              |
|------------:|-------------:|----------:|-----------:|-------:|:---------------------------|
|       16052 |          269 |         2 |        678 |   6.99 | 2007-01-28 21:44:14.996577 |
|       16060 |          272 |         1 |        405 |   6.99 | 2007-01-27 12:01:05.996577 |
|       16061 |          272 |         1 |       1041 |   6.99 | 2007-01-31 04:14:49.996577 |
|       16068 |          274 |         1 |        394 |   5.99 | 2007-01-27 09:54:37.996577 |
|       16074 |          277 |         2 |        308 |   6.99 | 2007-01-26 20:30:05.996577 |
|       16082 |          282 |         2 |        282 |   6.99 | 2007-01-26 17:24:52.996577 |
|       16086 |          284 |         1 |       1145 |   6.99 | 2007-01-31 18:42:11.996577 |
|       16087 |          286 |         2 |         81 |   6.99 | 2007-01-25 10:43:45.996577 |
|       16092 |          288 |         2 |        427 |   6.99 | 2007-01-27 14:38:30.996577 |
|       16094 |          288 |         2 |        565 |   5.99 | 2007-01-28 07:54:57.996577 |

Displaying records 1 - 10

</div>

# 5.

``` sql
SELECT payment_id, amount
FROM payment AS a
  INNER JOIN customer AS b
  ON a.customer_id = b.customer_id
WHERE b.last_name = "DAVIS"
```

<div class="knitsql-table">

| payment\_id | amount |
|:------------|-------:|
| 16685       |   4.99 |
| 16686       |   2.99 |
| 16687       |   0.99 |

3 records

</div>

# 6.1

``` sql
SELECT 
  COUNT (*) as Number_Rows
FROM rental
```

<div class="knitsql-table">

| Number\_Rows |
|-------------:|
|        16044 |

1 records

</div>

# 6.2

``` sql
SELECT customer_id,
  COUNT (*) as Number_Rentals
FROM rental
GROUP BY customer_id
```

<div class="knitsql-table">

| customer\_id | Number\_Rentals |
|:-------------|----------------:|
| 1            |              32 |
| 2            |              27 |
| 3            |              26 |
| 4            |              22 |
| 5            |              38 |
| 6            |              28 |
| 7            |              33 |
| 8            |              24 |
| 9            |              23 |
| 10           |              25 |

Displaying records 1 - 10

</div>

# 6.3

``` sql
SELECT customer_id,
  COUNT (*) as Number_Rentals
FROM rental
GROUP BY customer_id
ORDER BY Number_Rentals DESC
```

<div class="knitsql-table">

| customer\_id | Number\_Rentals |
|-------------:|----------------:|
|          148 |              46 |
|          526 |              45 |
|          236 |              42 |
|          144 |              42 |
|           75 |              41 |
|          469 |              40 |
|          197 |              40 |
|          468 |              39 |
|          178 |              39 |
|          137 |              39 |

Displaying records 1 - 10

</div>

# 6.4

``` sql
SELECT customer_id,
  COUNT (*) as Number_Rentals
FROM rental
GROUP BY customer_id
HAVING Number_Rentals >= 40
ORDER BY Number_Rentals DESC
```

<div class="knitsql-table">

| customer\_id | Number\_Rentals |
|-------------:|----------------:|
|          148 |              46 |
|          526 |              45 |
|          236 |              42 |
|          144 |              42 |
|           75 |              41 |
|          469 |              40 |
|          197 |              40 |

7 records

</div>

# 7.

``` sql
SELECT MAX(amount) as max_amount,
  MIN(amount) as min_amount,
  AVG(amount) as avg_amount,
  SUM(amount) as sum_amount
  FROM payment
```

<div class="knitsql-table">

| max\_amount | min\_amount | avg\_amount | sum\_amount |
|------------:|------------:|------------:|------------:|
|       11.99 |        0.99 |    4.169775 |     4824.43 |

1 records

</div>

# 7.1

``` sql
SELECT customer_id,
  MAX(amount) as max_amount,
  MIN(amount) as min_amount,
  AVG(amount) as avg_amount,
  SUM(amount) as sum_amount
  FROM payment
  GROUP BY customer_id
```

<div class="knitsql-table">

| customer\_id | max\_amount | min\_amount | avg\_amount | sum\_amount |
|-------------:|------------:|------------:|------------:|------------:|
|            1 |        2.99 |        0.99 |    1.990000 |        3.98 |
|            2 |        4.99 |        4.99 |    4.990000 |        4.99 |
|            3 |        2.99 |        1.99 |    2.490000 |        4.98 |
|            5 |        6.99 |        0.99 |    3.323333 |        9.97 |
|            6 |        4.99 |        0.99 |    2.990000 |        8.97 |
|            7 |        5.99 |        0.99 |    4.190000 |       20.95 |
|            8 |        6.99 |        6.99 |    6.990000 |        6.99 |
|            9 |        4.99 |        0.99 |    3.656667 |       10.97 |
|           10 |        4.99 |        4.99 |    4.990000 |        4.99 |
|           11 |        6.99 |        6.99 |    6.990000 |        6.99 |

Displaying records 1 - 10

</div>

# 7.2

``` sql
SELECT customer_id,
  MAX(amount) as max_amount,
  MIN(amount) as min_amount,
  AVG(amount) as avg_amount,
  SUM(amount) as sum_amount
  FROM payment
  GROUP BY customer_id
  HAVING COUNT(payment_id) > 5
```

<div class="knitsql-table">

| customer\_id | max\_amount | min\_amount | avg\_amount | sum\_amount |
|-------------:|------------:|------------:|------------:|------------:|
|           19 |        9.99 |        0.99 |    4.490000 |       26.94 |
|           53 |        9.99 |        0.99 |    4.490000 |       26.94 |
|          109 |        7.99 |        0.99 |    3.990000 |       27.93 |
|          161 |        5.99 |        0.99 |    2.990000 |       17.94 |
|          197 |        3.99 |        0.99 |    2.615000 |       20.92 |
|          207 |        6.99 |        0.99 |    2.990000 |       17.94 |
|          239 |        7.99 |        2.99 |    5.656667 |       33.94 |
|          245 |        8.99 |        0.99 |    4.823333 |       28.94 |
|          251 |        4.99 |        1.99 |    3.323333 |       19.94 |
|          269 |        6.99 |        0.99 |    3.156667 |       18.94 |

Displaying records 1 - 10

</div>

# Clean up

``` r
# clean up
dbDisconnect(con)
```
