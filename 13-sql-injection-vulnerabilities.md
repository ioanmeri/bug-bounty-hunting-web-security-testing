Section 13: SQL Injection Vulnerabilities

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
