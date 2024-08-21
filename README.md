# Bug Bounty Recon Workflow

This guide outlines a comprehensive workflow for performing reconnaissance in bug bounty programs. The steps below cover the essential tasks for discovering assets, enumerating subdomains, identifying vulnerabilities, and more. Use this checklist as a guide to ensure no step is missed during your recon process.

## 1. **Gather Target Information**
   - **Scope Analysis**: Review the program's scope carefully, noting allowed domains, subdomains, and exclusions.
   - **Target Enumeration**: Collect any available data about the target, such as:
     - IP ranges
     - Domain names
     - Known services
   - **WHOIS Information**: Extract WHOIS data to identify domain registrant information and network ranges.
     - Tools: `whois`, `Amass`, `Metasploit`
   - **SSL Certificates**: Extract subject alternative names (SANs) from SSL certificates to find additional domains.
     - Tools: `crt.sh`, `Censys`, `CertSpotter`

## 2. **Subdomain Enumeration**
   - **Passive Enumeration**:
     - Gather subdomains using public databases and certificate transparency logs.
     - Tools: `Sublist3r`, `Amass`, `crt.sh`, `Censys`, `CertSpotter`
   - **Active Enumeration**:
     - Use brute-force techniques to uncover subdomains.
     - Tools: `Amass`, `SubBrute`, `DNSRecon`, `Fierce`
   - **Permutation & Alteration**:
     - Generate permutations of discovered subdomains to uncover more assets.
     - Tools: `dnsgen`, `altdns`, `massdns`
   - **Validation**:
     - Verify the existence of subdomains by checking for active servers.
     - Tools: `Httpx`, `Masscan`, `nmap`

## 3. **Port Scanning & Service Enumeration**
   - **Initial Port Scan**:
     - Conduct a broad scan to identify open ports on discovered hosts.
     - Tools: `Nmap`, `Masscan`, `Rustscan`
   - **Service Fingerprinting**:
     - Identify services running on open ports, their versions, and any associated vulnerabilities.
     - Tools: `Nmap`, `WhatWeb`, `Nikto`
   - **Banner Grabbing**:
     - Collect service banners to gain more insights into the running services.
     - Tools: `Netcat`, `Telnet`, `Nmap`

## 4. **Web Application Recon**
   - **Content Discovery**:
     - Identify hidden files, directories, and endpoints.
     - Tools: `ffuf`, `Gobuster`, `Dirsearch`, `Burp Suite`
   - **Parameter Discovery**:
     - Look for hidden parameters that may not be exposed in the UI.
     - Tools: `ParamSpider`, `Arjun`, `Burp Suite`
   - **CMS Detection**:
     - Identify the CMS used and look for known vulnerabilities.
     - Tools: `WhatWeb`, `Wappalyzer`, `WPScan`

## 5. **Vulnerability Research**
   - **Common Vulnerabilities**:
     - Test for typical web vulnerabilities (XSS, SQLi, RCE, SSRF, etc.).
     - Tools: `Burp Suite`, `OWASP ZAP`, `SQLMap`, `XSStrike`
   - **Logic Flaws**:
     - Investigate the application’s logic for potential flaws.
   - **Authentication Issues**:
     - Test for weak authentication mechanisms, session management flaws, and more.
     - Tools: `Burp Suite`, `Postman`, `JWT Tool`
   - **Configuration Issues**:
     - Check for misconfigurations, exposed admin panels, default credentials, etc.
     - Tools: `Nikto`, `WPScan`, `Shodan`

## 6. **External Recon**
   - **Public Code Repositories**:
     - Search for exposed API keys, credentials, and sensitive information in public repositories.
     - Tools: `Gitrob`, `TruffleHog`, `Gitleaks`
   - **Google Dorking**:
     - Use advanced Google search queries to find exposed data.
     - Tools: `Google`, `Bishop Fox’s Dorking Guide`

## 7. **Automation & Scripting**
   - **Automation Tools**:
     - Use automation tools to speed up your recon process.
     - Tools: `ReconFTW`, `Nuclei`, `Hakrawler`
   - **Custom Scripts**:
     - Write custom scripts to automate repetitive tasks and customize scans.

## 8. **Reporting & Documentation**
   - **Documentation**:
     - Keep detailed notes on discovered vulnerabilities, steps to reproduce, and screenshots.
     - Tools: `CherryTree`, `Notion`, `Joplin`
   - **Report Submission**:
     - Craft a clear, concise, and actionable report for each finding.
     - Include:
       - Vulnerability description
       - Steps to reproduce
       - Impact analysis
       - Potential fixes
       - Attach relevant screenshots and PoCs

## 9. **Continuous Learning**
   - **Stay Updated**:
     - Follow the latest security trends, CVEs, and techniques.
     - Resources: `Twitter`, `HackerOne`, `Bugcrowd`, `Security Conferences`
   - **Practice Regularly**:
     - Continuously practice on platforms like:
       - `Hack The Box`
       - `TryHackMe`
       - `PortSwigger Academy`

## 10. **Community & Networking**
   - **Engage with the Community**:
     - Share your findings, tools, and techniques with others.
     - Join bug bounty forums, Slack groups, and attend meetups.
     - Platforms: `Twitter`, `Reddit`, `HackerOne Community`

---

This workflow is designed to be iterative; revisit previous steps as new information becomes available. Always ensure your methods adhere to the rules of engagement for each program and respect all legal boundaries.
