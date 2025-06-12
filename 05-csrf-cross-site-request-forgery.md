# Section 5: CSRF - Cross-Site Request Forgery

**Broken Access Control** Vulnerability
- Requests are not validated at the server side
- Server does not properly check if the user generated the request
- Requests can be forged and sent to users to **make them take actions they don't intend to do** such as
  - Change email
  - Change password
  - Submit a payment
  - ...
 
---

## Steps to reproduce

- Create two accounts in target website
- Log in with one of the accounts
- Make a list of all requests that can submit with the logged in user
  - e.g. change name / email pass
  - submit comments / reviws
- See if you can forge them
- Log in as the other user
- Submit the forged requests
- See if they get accepted

---

## Example

- Copy HTML form locally and submit a POST request to change email
- Try even if a hidden csrf input exists because it might not be properly implemented 

---

## Ways to trick the victim's browser into submitting forged request

In a **CSRF attack**, the attacker must trick the victim’s browser into **automatically or unknowingly submitting a request** to a target site where the victim is authenticated. Here's **how attackers typically make that happen**:


### 🚨 1. **Auto-submitting HTML Forms**

The attacker can create a page (on their own site, or injected via XSS) with a hidden form that **submits automatically** when the page loads:

```html
<body onload="document.forms[0].submit()">
  <form action="https://bank.com/transfer" method="POST">
    <input type="hidden" name="amount" value="1000">
    <input type="hidden" name="to" value="attacker_account">
  </form>
</body>
```

* The victim **just visits the attacker's page** — no clicking needed.
* The browser **auto-submits** the form using the victim's session cookies.

---

### 🧠 2. **Using Social Engineering**

If auto-submission might fail (e.g., browser blocks it), the attacker can use **tricks to get the victim to click**:

* A fake "Win a Prize!" button.
* A disguised link that looks like something else.
* Embedding the form behind a CAPTCHA or a quiz to make it more believable.

Example:

```html
<form action="https://bank.com/delete-account" method="POST">
  <input type="hidden" name="confirm" value="yes">
  <button type="submit">Claim Your Reward!</button>
</form>
```

The victim thinks they're doing something harmless, but the form targets the real site.

---

### 🖼️ 3. **Using an Image or GET Request (for GET-only actions)**

If the target action is a **GET request**, it’s even easier:

```html
<img src="https://bank.com/transfer?amount=1000&to=attacker_account" style="display:none">
```

As soon as the page loads, the browser makes that request — again **with session cookies included**, if the victim is logged in.

---

