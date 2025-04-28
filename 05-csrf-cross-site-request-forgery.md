# Section 5: CSRF - Cross-Site Request Forgery

**Broken Access Control** Vulnerability

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
