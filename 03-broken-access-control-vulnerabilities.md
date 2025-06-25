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
