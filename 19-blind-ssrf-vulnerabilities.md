# Section 19: Blind SSRF Vulnerabilities

## Introduction to Blind SSRF Vulnerabilities

In blind SSRF, you send a forged request exactly the same way as the normal SSRF, but **there will be NO RESPONSE**

The impact is a bit lower, but is some scenarios you could gain:
  - code execution
  - exploit vulnerabilities within the devices that are connected to the same network

We can blindly send exploit payloads to the devices on a network. 
These services are only internal not exposed to the Internet.
Server admins don't keep up these devices always up to date.

---

## Discovering Blind SSRF

We could send a forged request to get the vulnerable web server, hosting the vulnerable web application, to:
- send request **to a server that I control** as the hacker
- I can look at the logs, of my server, then I can see if I hijacked the web server and get it to send requests where it wasn't supposed to send requests to


**Example**

turn on Interceptor in Burp Suite ➡️ check _Referer value_ (in any request) with a full link ➡️ (test for open redirect vulnerabilities and SSRF)

- There are not going to appear that is working, but it is working (Blind SSRF)
- We need to modify the server to a location that we own and put it in Referer value
- read the logs to see if it got any requests


---


