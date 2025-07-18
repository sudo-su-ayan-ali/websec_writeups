## Enumerating for **XSS (Cross-Site Scripting)** on a domain in the context of **bug bounty** requires a systematic approach to discover and exploit input points, bypass filters, and
   escalate impact. Here's a **step-by-step enumeration and testing workflow** tailored for bug bounty hunting:


### 🔍 1. **Reconnaissance (Subdomain & Endpoint Discovery)**

#### 📌 Subdomain Enumeration

* Tools: `subfinder`, `assetfinder`, `amass`, `Sublist3r`
* Online: [crt.sh](https://crt.sh), [SecurityTrails](https://securitytrails.com), [Shodan](https://shodan.io)

```bash
subfinder -d example.com -o subs.txt
```

#### 📌 Endpoint Enumeration

* Tools:

  * `gau` (Get All URLs)
  * `waybackurls`
  * `hakrawler`
  * `katana` (for JS-heavy apps)
* Combine and sort:

```bash
cat subs.txt | gau | tee all_urls.txt
cat all_urls.txt | grep '=' > param_urls.txt
```

---

### 🔬 2. **Parameter Discovery & Analysis**

#### ✅ Tools:

* `ParamSpider`
* `Arjun`
* `kiterunner` for API param fuzzing
* Custom JS parser (to extract dynamic client-side params)

```bash
python3 paramspider.py -d example.com
arjun -u https://example.com/index.php -oT arjun_params.txt
```

#### ✅ Look for:

* Query params (`?search=abc`)
* Fragment-based params (`#name=abc`)
* POST/JSON/XML/WebSocket parameters
* DOM-based sinks (`document.write`, `location.href`, etc.)

---

### 🧪 3. **Test Each Input Point for Reflection**

#### 🔧 Tools:

* `XSStrike` (automated XSS detection)
* `Dalfox` (parameter fuzzing with XSS payloads)
* `KXSS` (detect reflected params)
* `Gxss` (XSS param reflector)
* Burp Suite with *Logger++* extension

```bash
cat param_urls.txt | dalfox pipe --deep-dive
```

#### ✍ Manual Reflection Check:

```bash
https://example.com/search?q=<script>alert(1)</script>
```

Check for:

* Raw reflection (`<script>`)
* Encoded reflection (`%3Cscript%3E`)
* Escaped reflection (`&lt;script&gt;`)
* Broken HTML/JS context

---

### 🕳️ 4. **Context Identification**

* Reflected in **HTML body**, **attribute**, **JS code**, or **DOM**
* Inject different payloads for each:

  * HTML: `"><svg/onload=alert(1)>`
  * Attribute: `" onmouseover="alert(1)`
  * JS: `';alert(1)//`
  * URL: `javascript:alert(1)`

Use [XSS Hunter](https://xsshunter.com) or your own **Burp Collaborator** to track blind XSS.

---

### 🛠️ 5. **Payload Encoding and Filter Bypass**

Apply techniques to bypass WAFs and filters:

* **Encoding**:

  * `&#x3C;script&#x3E;`
  * `%3Cscript%3E`
* **Bypass Tricks**:

  * Using UTF-7: `+ADw-script+AD4-alert(1)+ADw-/script+AD4-`
  * Breaking out of tags: `"><img src=x onerror=alert(1)>`
  * Using `srcdoc`, `iframe`, `data:` URLs

---

### ⚙️ 6. **Automated + Manual Testing in JS-heavy Apps**

Use **browser-based tools** and automation:

* **Burp Suite** (for intercept and modify)
* **XSStrike**: context-aware XSS scanner
* **Dalfox**: parameter-aware and DOM scanner
* **XSSHunter/XSS Radar**: for catching blind XSS
* **Playwright/Puppeteer**: automate headless browser testing

---

### 🧠 7. **DOM XSS Specific Enumeration**

DOM XSS won’t be caught by server logs. Analyze client-side JavaScript:

#### 🔧 Tools:

* `domdig`, `linkfinder`, `jsbeautifier`, `grep` for sinks

```bash
linkfinder -i index.js -o cli
grep -E "(document\.|window\.|location|eval|innerHTML|write)" *.js
```

Look for dangerous sinks like:

* `eval()`, `document.write()`, `innerHTML`, `location.href`

---

### 📄 8. **Proof of Concept + Reporting**

* Always demonstrate **impact**:

  * Session cookie theft
  * Account takeover
  * Redirection to attacker page
* Include:

  * Screenshot
  * HTTP request/response
  * Steps to reproduce
  * Payloads used
  * Context and severity

---

### 🛡️ Bonus: Tips

* Always test with a **collaborator endpoint** or **XSS hunter**.
* Check for **Content Security Policy (CSP)**; try CSP bypass.
* Try **postMessage**, **localStorage**, **JSONP**, **template injection**.
* Watch for **WAF** blocking patterns (try using headers like `X-Original-URL`, change User-Agent, or use `X-Forwarded-For` tricks).

---

### 📚 Practice Resources

* PortSwigger XSS Labs (Reflected, Stored, DOM)
* HackTheBox / TryHackMe XSS Rooms
* XSS Game: [https://xss-game.appspot.com/](https://xss-game.appspot.com/)
* JS Challenges: [https://prompt.ml/](https://prompt.ml/)

