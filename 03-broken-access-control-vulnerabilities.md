# Section 3: Broken Access Control Vulnerabilities

> Access / Modify data beyond limits or permissions

the most common security thread according to OWASP, 94% web apps

includes vulnerabilities like, Path Traversal, CSRF, IDOR

---

## Cookie Manipulation

- Access or modify user info **without** logging in

In the Target Website:

- Create a user in target website and login
- you have access to features like comments and reviews
  - increases your chances of finding bugs
- navigate to admin page
  - only admin has access message
- intercept request in Burp
- One of the Cookies is `Admin: false`
- Modify cookie to `Admin: true`
- Have access to admin page

In Burp in the request **Options** can add a global rule
- Match and Replace
- Type: Request header
- Match: `Admin=false`
- Replace: `Admin=true`
- tick rule
- can delete users and other admin functions
- untick rule

now is not needed to intercept through Burp and all requests will have the modified cookie value

---

## Accessing Private User Data

- Access or modify info that belongs to **another** user

In the Target Website:

- Create two accounts
- login as one of those accounts
- check if I can access other's account info

In a blog website, found that when visiting user's blog, user's UUID is appended to URL

```
/blogs?userId=2b1103e0-cb3-4a9b-b620-a108d748aaa20
```

When I am accessing my account, the URL path looks like this:

```
/my-account?id=dbe79c68-effa-42fc-aaa9-2748ae6c051e
```

- I can replace the id with the user's Id from the blog page

- also can guess certain usernames in URL's like

```
/my-account?id=administrator
```

---

## Discovering IDOR Vulnerabilities

Insecure **Direct** Object Reference
- Objects are accessed directly based on user input
  - Docs
  - Images
  - Database records

By manipulating the user input, we can retrieve data that does not belong to us

**Examples**
- Loading user data with a guessed id in the URL params, or profile image
- loading private image with a link in the URL
- Download text "view transcript" in live chat
  - intercept the post request
  - `GET /download-transcript/4.txt HTTP/1.1`
  - modify text file number `1.txt`
  - Download file that belongs to another user: IDOR
 
--- 

## Privelege Escalation with Burp Repeater

- update email page in web application
- intercept the POST request
- check body parameters that has an email
- send request to Repeater
- check the response
- modify payload in the next request

### Response

```
HTTP/1.1
{
  "username": "wiener",
  "email": "test2@test.com",
  "apiKey": "vsfsV309sfdfjaJfds2",
  "roleId": 1
}
```

### Modified Request

```
HTTP/1.1
{
  "email": "test2@test.com",
  "roleId": 2
}
```

and now the user has admin privileges

---

## HTTP TRACE

GET request in admin page, responses with a message of unauthorized access
- forward request
- try TRACE method instead of GET in the `/admin` request
- the server responses with the same request that sent to it

In-between nodes can modify the original request

`TRACE /admin` ➡️ Proxy ➡️ Target Web Server ➡️ `TRACE` response along with extra headers

- downloaded a text file with the TRACE response with extra header
- `X-CUSTOM-IP-Authorization: 89.101.123.254`
- modify IP to local host (educated guess) to make web server to think that we are admins and load admin page
- Request Options: Match and Replace
  - Add: `X-CUSTOM-IP-Authorization: 127.0.0.1`
  - Tick Rule
- have access to admin page
  - Untick Rule

---
