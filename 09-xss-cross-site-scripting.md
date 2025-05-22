# Section 9: XSS - Cross Site Scripting

- Allow an attacker to inject javascript code into the page
- Code is executed when the page loads
- Code is executed on the **client** machine not the server

Three main types:
1. **Reflected** XSS
2. Persistent/**Stored** XSS
3. **DOM** based XSS


Request with XSS Payload:

```
http://target.com/?search=test<script>alert('xss')</script>
```

Response with the XSS Payload **Embedded** within the page

----

## Reflected XSS

- None persistent, not stored
- Only works if the target visits a specially crafted URL

e.g.
```
http://target.com/page.php?something=<script>alert('xss')</script>
```

---

## Stored XSS

- Persistent, stored on the page or DB
- The injected code is executed everytime the page is loaded

---

## DOM Based XSS

- Similar to reflected and stored XSS
- Can be discovered and exploited similarly
- Main difference is that it occurs entirely on the client side
- **Payload is never sent to the server**
  - No logs, no filters, no server side protection

 ---

 ## HTML Injection

 - Allow an attacker to inject HTML code into the page
 - Code is executed when the page loads
 - Code is executed on the **client** machine not the server

 - Similar to XSS but **simpler**
 - **Hints** at the existence of an XSS


Example: in a search box search for `<b>digital</b>`

---

## Discovering Reflected & Stored XSS Vulnerabilities

### Reflected

Example: in a search box search for `<script>alert(2)</script)`

Reflected because the only way for it to work is to copy the link and send it to the target

### Stored XSS

Example: Post a comment in a blog with content `<script>alert(2)</script)`

Stored because, whatever is injected in the comment is saved in the DB and all users visiting will see the code

---
