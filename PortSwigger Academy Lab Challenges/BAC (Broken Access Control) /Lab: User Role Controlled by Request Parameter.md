# 🛡️ Web Security Lab: User Role Controlled by Request Parameter

## 📘 Description

This lab demonstrates an **Insecure Direct Object Reference (IDOR)** or **Access Control Misconfiguration**, where access rights are determined by a client-side parameter or cookie. An attacker can escalate privileges by modifying the `Admin` cookie from `false` to `true`.

> 🔥 **Goal**: Access the admin panel and delete user `carlos`.

---

## 🧰 Tools Used

- 🖥️ Web Browser (e.g., Firefox or Chrome)
- 🐞 Burp Suite (Community or Professional Edition)
- 🐧 Linux Terminal (optional for cURL commands)

---

## 🚶 Steps to Reproduce

### 1. Visit `/admin`

- Navigate to the `/admin` endpoint.
- You'll see a message like:  
    **You are not authorized to access this page.**
- This confirms that we're currently logged in as a normal user.

---

### 2. Log In to the Application

- Go to the login page.
- Enter any default credentials (usually provided in the lab).
- Before submitting the login request, **enable interception** in Burp Proxy.

---

### 3. Use Burp Suite to Intercept and Modify the Response
**Setup:**

- Open Burp.
- Enable **Intercept Server Response**:  
    Proxy → Options → Intercept Server Responses → Check "Intercept responses based on the following rules".

**Intercept Login Response:**

- Submit your login form.
    
- In the intercepted response, you'll see something like:
    
    `Set-Cookie: Admin=false`
    

**Modify the Cookie:**

- Change that to:  
    `Set-Cookie: Admin=true`
- Forward the response.

---

### 4. Revisit `/admin`

- Now go back to `/admin` in your browser.
- You should have access to the **admin panel**.

---

### 5. Delete Carlos

- In the admin panel, look for user `carlos`.
- Click the **Delete** button next to that user.
- Wait for success message:  
    **Congratulations, you solved the lab!**
    
---

## 🧠 Root Cause Analysis

The application determines a user's role based on a client-modifiable cookie. This allows an attacker to escalate privileges by simply changing the value from `false` to `true`.

**Why it's bad:**  
Access control mechanisms should always be handled and verified server-side. Relying on user input (like URL parameters, cookies, or request headers) is insecure and can lead to privilege escalation.

**Fix recommendation:**  
Implement access control logic server-side using secure session controls. Store user roles in server-side session tokens or databases.

---

## ✅ Lab Status

|Task|Status|
|---|---|
|Access `/admin`|❌|
|Modify Admin cookie|✅|
|Access as admin|✅|
|Delete Carlos|✅|
|Lab Completed!|🎉|

---

## 📎 References

- OWASP: [Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control)
- PortSwigger:[Access control vulnerabilities](https://portswigger.net/web-security/access-control)
