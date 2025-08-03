## Lab: Unprotected Admin Functionality with Unpredictable URL

**Summary:**  
An administrative panel is accessible to unauthenticated users via an obscure but unprotected URL. This allows any user to perform privileged actions, such as deleting other users, without proper authorization.

---

### Steps to Reproduce

1. **Review Source Code**
    
    - Open the lab’s main page.
    - Press `Ctrl+U` to view the page source.
    - Search for references to admin functionality or unusual URLs.
2. **Identify Hidden Admin Path**
    
    - In the JavaScript code, observe the following snippet:
        
        JavaScript
        
        ```
        var isAdmin = false;
        if (isAdmin) {
           var topLinksTag = document.getElementsByClassName("top-links")[0];
           var adminPanelTag = document.createElement('a');
           adminPanelTag.setAttribute('href', '/admin-uv2ylq');
           adminPanelTag.innerText = 'Admin panel';
           topLinksTag.append(adminPanelTag);
           var pTag = document.createElement('p');
           pTag.innerText = '|';
           topLinksTag.appendChild(pTag);
        }
        ```
        
    - The admin panel link (`/admin-uv2ylq`) is only rendered if `isAdmin` is true, but the path itself is hardcoded and not protected by authentication.
3. **Access the Admin Panel**
    
    - Navigate directly to `/admin-uv2ylq` in your browser.
4. **Exploit the Functionality**
    
    - On the admin panel, locate the user management section.
    - Find the user `carlos` and use the provided interface to delete this user.

---

### Impact

- **Privilege Escalation:** Any unauthenticated user can access the admin panel and perform sensitive actions.
- **Account Takeover/Deletion:** Attackers can delete arbitrary users, leading to denial of service for legitimate users.
- **Potential for Further Exploitation:** If other admin functionalities exist (e.g., changing passwords, viewing sensitive data), these are also exposed.

---

### Recommendation

- **Implement Proper Authentication and Authorization:**  
    Ensure that all administrative endpoints (including `/admin-uv2ylq`) are protected by robust authentication and authorization checks on the server side.
- **Do Not Rely on Obscurity:**  
    Obscure or unpredictable URLs should never be used as a security control.
- **Review All Sensitive Endpoints:**  
    Audit the application for other endpoints that may be exposed in a similar manner.

---

### Evidence

- **Source Code Reference:**  
    The admin panel link is only hidden in the UI, not protected on the backend.
- **Direct Access:**  
    Navigating to `/admin-uv2ylq` as an unauthenticated user grants full admin access.
- **User Deletion:**  
    Successfully deleted user `carlos` without authentication.

---

**Severity:** Critical

---

**References:**

- [OWASP A01:2021 - Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/)

---

**Solved by:** Ayan Ali
