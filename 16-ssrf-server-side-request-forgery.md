# Section 16: SSRF (Server-Side Request Forgery)

number 10 in OWASP

## Theory Behind SSRF Vulnerabilities & Their Impacts

Many times web servers communicate with other servers
  - Web Server -> Server 1
  - Web Server -> Server 2

can use domain or address, can be in the same network or in a completely different location in the cloud


  - Client request -> Web Server -> Server request -> Server 1

### SSRF

Make the server submit requests that it is not programmed to send and as a result give us useful information


  - Forged Request -> Web Server -> Server 3

the server 3 will see the requests coming from Web Server instead of the Client

or

Client can access restricted resources on Server 1 that only Web server could communicate

probably has a white/black list

  - Forged Request -> Web Server -> server1.com/private

Another scenario is to send a Forged Request to the Web Server and cause it to send requests to itself, causing overload
  - Forged Request -> Web Server -> Web Server -> ... -> Web Server

Also could send forged requests to the web server to discover network topology, or open ports and running services
  - Forged Request -> Web Server -> Discover network insights 

---

## Discovering a Basic SSRF Vulnerability

Exampe: A dropdown menu is used to select a city, and a button "Check Stock" that when clicked returns e.g. "248 units"

In a Burp Interceptor we see that is a POST request with a "stockApi" value (URL-decode)

**which is a request hitting another server**

```
stockApi=stock.weliketoshop.net:8080/product/stock/check?productId=3&storeId=2
```

**We can modify the path with whatever server we want**

To test, replace the stockApi with e.g.:

```
stockApi=http://localhost/
```

default port: 80, also URL-encode the URL as it was initially

---

## Accessing Private (admin) Resources Using an SSRF Vulnerability

The previous SSRF loaded the webpage with an extra link "Admin Panel" but now getting an error

"Admin interface only available if logged in as an administrator, or if requested from loopback"

You should get the server submit this request of your behalf, exploit SSRF again to load links

Copy admin link and replace it in the stopApi payload


```
stockApi=http://localhost/admin
```

and URL encode it

could also delete users as an admin using the web server sending requests to itself

```
stockApi=http://localhost/admin/delete?username=carlos
```


---
