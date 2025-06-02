# Section 12: Bypassing Content Security Policy

## CSP & XSS

Content Security Policy CSP
- Browser feature that prevents XSS and other attacks
- To enable it, response headers would include `Content-Security-Policy`

Example: 
- HTML injection test (`<b>test</b>` in search)
- JS injection test (`<script>alert(1)</script)` in search)
  - script is injected, but it's not popping up. Also, the search term, payload, it is not showing on the page. Probably, the web app doesn't know if should be treated as HTML or JS)

## Bypassing Basic Filtering

JS code is injected in script but is enclosed in back ticks, which is used for literal templates, the payload is now:

```
${alert(222)}
```

---

## Discovering an XSS in a CSP Enabled Application

In Burp Suite Interceptor discovered that another POST request in an csp-report endpoint is called in every search request.

The csp-report POST request includes a token in the payload

Can use Burp Proxy to intercept responses (tick box intercept responses in the settings)

Used an example token in the search function

```
?search=<script>alert%281%29<%2Fscript>&token=lalala
```

fount the sample: lalala in the burp proxy response, being inject in `Content-Security-Policy` header

CSP has already:
```
script-src 'self';
```

which means that inline JS or inline script are prevented in this web application

DO NOT RUN INLINE JS

<b>BUT</b>

Can use the tag `script-src-elem` to override `script-src`, set the tag:

```
script-src-elem 'unsafe-inline'
```

and now inline JS is enabled

```
?search=<script>alert%281%29<%2Fscript>&token=;script-src-elem 'unsafe-inline'
```

and alert runs

---


