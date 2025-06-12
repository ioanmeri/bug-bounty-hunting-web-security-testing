# Section 16: SSRF (Server-Side Request Forgery)

number 10 in OWASP

## Theory Behind SSRF Vulnerabilities & Their Impacts

Many times web servers communicate with other servers

Web Server -> Server 1

Web Server -> Server 2

can use domain or address, can be in the same network or in a completely different location in the cloud


Client request -> Web Server -> Server request -> Server 1

Make the server submit requests that it is not programmed to send and as a result give us useful information

**SSRF**

> Forged Request -> Web Server -> Server 3

the server 3 will see the requests coming from Web Server instead of the Client

or

Client can access restricted resources on Server 1 that only Web server could communicate

probably has a white/black list

> Forged Request -> Web Server -> server1.com/private

Another scenario is to send a Forged Request to the Web Server and cause it to send requests to itself, causing overload

Also could send forged requests to the web server to discover network topology, or open ports and running services

---
