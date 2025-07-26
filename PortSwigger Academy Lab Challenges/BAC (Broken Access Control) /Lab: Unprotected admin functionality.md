

# 🔓 Lab: Unprotected Admin Functionality

This writeup covers the **PortSwigger Web Security Academy** lab challenge on **Broken Access Control**, specifically the task titled **"Unprotected admin functionality"**.

---

## 🧠 Lab Objective

> This lab demonstrates a common **access control flaw** where admin functionalities are not properly protected, allowing unauthorized users to perform sensitive operations.

**Goal:**  
Delete the user `carlos` from the administrator panel without prior authentication.

---

## 🛠️ Exploitation Steps

1. **Recon via `robots.txt`:**  
   Append `/robots.txt` to the lab base URL to discover disallowed paths. One of the paths reveals access to the admin interface (`/administrator-panel`).

2. **Access Admin Interface:**  
   Replace `/robots.txt` in the URL bar with `/administrator-panel` to access the administration dashboard — despite not having elevated privileges.

3. **Delete the Target User:**  
   Use the interface to delete the target user `carlos`.

✅ **Lab Complete**

---

## 📽 Demonstration

A short video walkthrough of this exploitation process is provided below:

📁 `2025-07-26.07-47-41.mp4`
https://github.com/user-attachments/assets/d85507c4-190c-4083-9bda-2a319f2efc60 
*This video demonstrates accessing the admin panel and successfully completing the lab by deleting the user.*

> ⚠️ Note: Video may be embedded or downloadable, depending on your GitHub settings.

---

## 🧩 Vulnerability Type

- **Broken Access Control**
- **Security Misconfiguration**
- **Information Disclosure via `robots.txt`**

---

## 💡 Key Takeaways

- Never expose sensitive or internal endpoints via `robots.txt`.  
- All administrative routes must be protected with proper authentication and authorization checks — user roles must be enforced server-side.
- Security should not rely on obscurity.

---

## 🧾 References

- [PortSwigger Labs – Broken Access Control](https://portswigger.net/web-security/access-control)
- [OWASP – Broken Access Control](https://owasp.org/Top10/en/A01_2021-Broken_Access_Control/)
- [robots.txt file usage](https://developer.mozilla.org/en-US/docs/Web/Robots.txt)

---

## 👤 Author

**🔹 Ayan Ali** – *Web App Security Enthusiast & Writeup Author*  
GitHub: [@sudo-su-ayan-ali](https://github.com/sudo-su-ayan-ali)

---

> **Disclaimer:** This lab is solved for educational purposes only. Do not attempt to exploit systems unless you have legal permission.



