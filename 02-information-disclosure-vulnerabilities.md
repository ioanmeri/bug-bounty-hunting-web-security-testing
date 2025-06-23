# Section 2: Information Disclosure Vulnerabilities

second most common secuirty thread in OWASP - cryptographic failures

These vulnerabilities allow you to access information that should not be attainable or visible to you

lab environment: portswigger

## Discovering Database Login Credentials

- visit normal shop website
- append to URL `/robots.txt`

**Result**
- User-agent: *
- Disallow: /backup

  is telling google and other search engines to not index whatever data included in the following path `/backup`

- append `/backup` to URL
- contains a file `ProductTemplate.java.bak`
- when loading the file we see that is leaking Java code which contains db login credentials

---

## Discovering Endpoints & Sensitive Data

- FeroxBuster: tool to discover endpoints
- Download and run with a wordlist
- example wordlist: SecLists.common.txt on github
- monitor if it gets a positive responds

```
./feroxbuster --url https://bitdiscovery.com --depth 2 --wordlist words
```

**Result**

Match for `/cgi-bin` path with `phpinfo.php` file with verbose information about php server with `SECRET_KEY`

Another example for information disclosure vulnerability
- `.git` folder discovered with `config` file that contains developer's emails and names
- also can show git diff merge changes to view previous hardcoded ADMIN_PASSWORD removed from source code

---



