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
