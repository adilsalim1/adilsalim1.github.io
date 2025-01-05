---
title: My Bug-Hunting Checklist
date: 2025-01-05 12:35:00 +0530
categories: [Web Security]
tags: [Checklist]  # TAG names should always be lowercase
layout: post
image:
  path: assets/Images/Bug-Bounty.png
  alt: Bugbounty
---

# Bug Bounty Checklist 🛠️

A comprehensive checklist to ensure systematic and thorough testing during bug bounty hunting.

---

## 1. **Reconnaissance** 🔍
- [ ] Identify subdomains (e.g., `subfinder`, `assetfinder`, `amass`)
- [ ] Perform DNS enumeration (`dnsrecon`, `dnsx`)
- [ ] Check for wildcard subdomains
- [ ] Scrape public sources for information (`github-dorks`, `shodan`, `censys`)
- [ ] Gather WHOIS information

---

## 2. **Scanning** 📡
- [ ] Port scanning (`nmap`, `masscan`)
- [ ] Service enumeration (`nmap -sV`, `whatweb`, `nikto`)
- [ ] Technology stack identification (`wappalyzer`, `builtwith`)
- [ ] Check for misconfigured services (`CVE-lookup`, `searchsploit`)

---

## 3. **Web Application Testing** 🌐
### a. **Authentication and Authorization**
- [ ] Test for IDOR vulnerabilities
- [ ] Test for session management issues
- [ ] Check for multi-factor authentication bypass
- [ ] Analyze forgot/reset password flows

### b. **Input Validation**
- [ ] Test for SQL Injection (`sqlmap`, manual payloads)
- [ ] Test for XSS (Reflected, Stored, DOM-based)
- [ ] Check for file upload vulnerabilities
- [ ] Check for command injection vulnerabilities

### c. **Business Logic**
- [ ] Analyze workflows for bypass opportunities
- [ ] Test for coupon code manipulation
- [ ] Check for payment gateway bypass
- [ ] Test for rate-limiting issues

---

## 4. **API Testing** 📡
- [ ] Discover endpoints (`postman`, `burpsuite`, `httpx`)
- [ ] Test for authentication flaws
- [ ] Test for insecure data storage or transmission
- [ ] Check for improper API request methods (e.g., `GET` instead of `POST`)
- [ ] Analyze rate-limiting for APIs

---

## 5. **Network-Level Testing** 🌐
- [ ] Scan for open ports and services
- [ ] Test for misconfigured network protocols
- [ ] Check for publicly accessible admin panels
- [ ] Assess for vulnerable services like SMB, RDP, etc.

---

## 6. **Cloud Security Testing** ☁️
- [ ] Check for exposed storage buckets (AWS S3, GCP, Azure Blob)
- [ ] Test IAM misconfigurations
- [ ] Analyze application secrets or keys in the source code
- [ ] Assess cloud-specific vulnerabilities (`ScoutSuite`, `Prowler`)

---

## 7. **Other Tests** 🛠️
- [ ] Look for outdated software and libraries
- [ ] Perform directory brute-forcing (`dirb`, `gobuster`, `ffuf`)
- [ ] Test for security headers (`httpx`, `curl`)
- [ ] Check for sensitive information exposure (e.g., `robots.txt`, `.git`, `.env`)

---

## 8. **Reporting** 📝
- [ ] Include clear steps to reproduce the issue
- [ ] Provide screenshots and payloads
- [ ] Suggest mitigation strategies
- [ ] Follow the program's report format

---

## Tools to Consider 🧰
- **Reconnaissance**: `amass`, `subfinder`
- **Web Testing**: `burpsuite`, `OWASP ZAP`
- **Network Testing**: `nmap`, `masscan`
- **API Testing**: `postman`, `insomnia`
- **Cloud Security**: `ScoutSuite`, `Prowler`
- **Automation**: `bash scripting`, `ansible`

---

Feel free to add or modify items to suit the scope of your bug bounty engagement!
