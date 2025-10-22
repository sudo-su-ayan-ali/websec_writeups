# The Philosophy of Recon
---
The process is generally split into two main types:

1. **Passive Recon:** Gathering information without directly interacting with the target's systems. This is stealthy and uses publicly available information.
2. **Active Recon:** Directly probing the target's systems to gather more detailed information. This is "noisier" and can be logged by the target.

---

### Phase 1: Passive Reconnaissance (Intelligence Gathering)

**Goal:** Understand the target's overall footprint from a distance.

#### Step 1: Defining the Scope

This is the most important first step. **Carefully read the bug bounty program's policy on their website (e.g., on HackerOne, Bugcrowd, or their self-hosted program).**

- **In-Scope:** What domains, subdomains, and IP ranges are you allowed to test? Look for wildcards like `*.example.com`.
- **Out-of-Scope:** What is explicitly forbidden? Testing out-of-scope assets can get you banned from the program or even lead to legal trouble.
- **Rules of Engagement:** Are there specific types of attacks that are forbidden (e.g., DDoS, social engineering)?

#### Step 2: Broad Information Harvesting

- **Company Acquisitions:** Use sources like **Crunchbase** and Wikipedia to find companies your target has acquired. Old, forgotten infrastructure from acquired companies is a goldmine for bugs.
- **ASN Discovery:** Find the target's Autonomous System Number (ASN). An ASN represents a block of IP addresses owned by an organization. This helps you find _all_ servers owned by the company, not just those tied to their main websites.
    - _Tools:_ `whois`, `bgp.he.net`

#### Step 3: Subdomain Enumeration (Passive)

The goal is to find as many subdomains as possible (`app.example.com`, `dev.example.com`, `api.example.com`).

- **Certificate Transparency (CT) Logs:** Every SSL/TLS certificate issued is publicly logged. You can search these logs for certificates issued for your target's domain, revealing subdomains.
    - _Tools:_ `crt.sh`, `censys.io`
- **Search Engine Dorking:** Use advanced search operators on Google, Bing, etc., to find indexed subdomains and sensitive information.
    - `site:*.example.com` - Shows all indexed subdomains.
    - `site:*.example.com -www` - Excludes the main `www` site.
    - `site:example.com intitle:"index of"` - Looks for open directories.
    - `site:example.com filetype:pdf "confidential"` - Looks for sensitive documents.
- **Public Datasets & Tools:** Use tools that aggregate data from multiple public sources.
    - _Tools:_ `subfinder`, `assetfinder`, `amass` (in passive mode)

#### Step 4: "Digital Archeology"

- **GitHub/GitLab Recon:** Search for the company's name, domain names, or unique internal project names on code-sharing platforms. Developers sometimes accidentally commit sensitive information.
    - _What to look for:_ API keys, passwords, secret tokens, internal hostnames, old code.
    - _Tools:_ GitHub's native search, `git-dorks`, `truffleHog`.
- **Wayback Machine:** Use the Internet Archive to look at old versions of websites. You might find old API endpoints that are still active, or information about technologies that have since been removed but are still running.

---

### Phase 2: Active Reconnaissance (Probing the Target)

**Goal:** Interact with the discovered assets to get detailed technical information. **Make sure you are only doing this on in-scope assets.**

#### Step 1: Subdomain Enumeration (Active)

- **DNS Brute-forcing:** Use a large wordlist of common subdomain names (like `dev`, `stage`, `api`, `vpn`) and try to see if they exist for the target domain.
    - _Tools:_ `ffuf`, `gobuster`, `dnsrecon`.

#### Step 2: Resolving and Validating

You now have a massive list of potential subdomains. You need to find out which ones are actually live and host a web service.

- **DNS Resolution:** Resolve all found subdomains to IP addresses.
- **HTTP Probing:** Check which of these live IPs are running a web server (on common ports like 80, 443, 8000, 8080, 8443).
    - _Tools:_ `httpx`, `httprobe`.

#### Step 3: Port Scanning

For the live IP addresses you found, check for other open ports and services. A web server might be running on a non-standard port, or you might find other interesting services like FTP, SSH, or a database.

- **Scanning:** Use a port scanner to check the top 1,000 or even all 65,535 ports on key servers.
    - _Tools:_ `nmap` (the industry standard), `masscan` (for very fast scanning of large IP ranges).
    - _Example Nmap Command:_ `nmap -sV -T4 -p- <IP_ADDRESS>` (This scans all ports, tries to identify service versions, and runs at a reasonably fast speed).

#### Step 4: Technology Fingerprinting

For every web service you've found, identify the technology stack. Knowing the framework, language, and server software helps you focus your vulnerability research.

- **What's it running?** Is it Apache or Nginx? PHP, Node.js, or Java? Is it a known CMS like WordPress or Drupal?
    - _Tools:_ `whatweb`, `wappalyzer` (browser extension), `nmap` version detection scripts (`-sV`).

#### Step 5: Content Discovery (Directory & File Brute-forcing)

This is a crucial step. You're looking for hidden files, folders, and API endpoints that aren't linked from the main site.

- **Brute-force:** Use a good wordlist to look for common paths.
    - _What to look for:_ `/admin`, `/login`, `/dashboard`, `/api`, `/v1`, `/backups`, `.git`, `.env`.
    - _Tools:_ `dirsearch`, `ffuf`, `gobuster`.

#### Step 6: Visual Reconnaissance

You might have hundreds or thousands of web pages. It's impossible to check them all manually. Use tools to take screenshots of all of them. This allows you to quickly scan visually for interesting pages like login portals, old-looking applications, or error pages.

- _Tools:_ `EyeWitness`, `aquatone`.

---

### Phase 3: Ongoing Reconnaissance (Monitoring for Change)

This is what separates the top bug hunters from the rest. The attack surface is not static; it's constantly changing.

- **Automation:** Set up scripts (e.g., cron jobs on a server) to automatically re-run your recon steps (especially subdomain discovery) on a daily or weekly basis.
- **Monitoring:** Use a `diff` tool to compare the new results with the old ones.
- **Alerts:** Set up alerts for when:
    - A new subdomain appears.
    - A new port is opened on a known server.
    - A new web technology is detected.

New applications or features are often deployed with bugs. Being the first to find a new subdomain gives you a massive advantage.

### Summary Workflow

1. **Scope:** Read the program policy.
2. **Passive:** Gather subdomains (crt.sh, subfinder), find IPs (ASN lookup), and look for leaks (GitHub).
3. **Active:** Brute-force more subdomains (ffuf), find live web servers (httpx), scan for open ports (nmap).
4. **Enumerate:** For each live web server:
    - Identify technology (whatweb).
    - Find hidden directories (dirsearch).
    - Take a screenshot (EyeWitness).
5. **Analyze:** Review all your data. Prioritize interesting targets (e.g., login pages, APIs, old-looking apps).
6. **Monitor:** Automate your recon to find new assets as soon as they appear.

By following this theoretical framework, you build a comprehensive and constantly updated map of your target, which is the foundation for successful bug hunting. Happy hacking
