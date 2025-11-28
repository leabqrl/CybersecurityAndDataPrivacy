# 1️⃣ Introduction

**Tester(s):**  
- Léa Becquerel

**Purpose:**  
- Identify vulnerabilities in registration and authentication flows
- Assess the backend’s resistance to common attack vectors 

**Scope:**  
- Tested components:
   - Home page (/)
   - Registration page (/register)
   - Static resources (/static/.js, /static/.css) 
- Exclusions:  
- Test approach: Gray-box / Black-box / White-box

**Test environment & dates:**  
- Start:  27/11/2025
- End:  27/11/2025
- Test environment details (OS, runtime, DB, browsers):
   - OS : Windows
   - runtime : Docker Desktop
   - DB : PostgreSQL
   - browsers : Chrome + ZAP

**Assumptions & constraints:**  
- The application is accessible locally at http://localhost:8000/.
- No special network authentication (VPN) is required.
- The report contains preliminary results — replace evidence with ZAP exports.

---

# 2️⃣ Executive Summary

**Short summary (1-2 sentences):** 
The initial scan revealed several classic vulnerabilities affecting confidentiality and integrity (XSS, CSRF, insecure cookies), as well as issues with sensitive information exposed via headers and paths. These vulnerabilities can allow session hijacking, client-side script execution, and unauthorized access to booking data. 

**Overall risk level:**  Medium

**Top 5 immediate actions:**  
1.  Fix SQL Injection vulnerabilities
2.  Prevent Path Traversal
3.  Add CSRF protection
4.  Configure essential security headers
5.  Implement custom error

---

# 3️⃣ Severity scale & definitions

|  **Severity Level**  | **Description**                                                                                                              | **Recommended Action**           |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
|      🔴 **High**     | A serious vulnerability that can lead to full system compromise or data breach (e.g., SQL Injection, Remote Code Execution). | *Immediate fix required*         |
|     🟠 **Medium**    | A significant issue that may require specific conditions or user interaction (e.g., XSS, CSRF).                              | *Fix ASAP*                       |
|      🟡 **Low**      | A minor issue or configuration weakness (e.g., server version disclosure).                                                   | *Fix soon*                       |
|     🔵 **Info**      | No direct risk, but useful for system hardening (e.g., missing security headers).                                            | *Monitor and fix in maintenance* |


---

# 4️⃣ Findings (filled with examples → replace)

> Fill in one row per finding. Focus on clarity and the most important issues.

| ID | Severity | Finding | Description | Evidence / Proof |
|------|-----------|----------|--------------|------------------|
| F-01 | 🔴 High | SQL Injection in registration | Input field allows `' OR '1'='1` injection | <img width="502" height="269" alt="Capture d&#39;écran 2025-11-28 175150" src="https://github.com/user-attachments/assets/ba131874-9342-4edf-8748-b6316602d9f6" /> |
| F-02 | 🟠 Medium | Missing Anti-clickjacking Header |The response does not protect against 'ClickJacking' attacks | <img width="611" height="314" alt="Capture d&#39;écran 2025-11-28 175135" src="https://github.com/user-attachments/assets/bf4b8524-2bbf-4abe-95ab-94dae632df0c" /> |
| F-03 | 🟠 Medium | Absence of Anti-CSRF Tokens | Registration form has no CSRF protection | <img width="529" height="293" alt="Capture d&#39;écran 2025-11-28 175120" src="https://github.com/user-attachments/assets/70d0d1cb-31dd-453d-96ba-614c6ca22023" /> |
| F-04 | 🟠 Medium | Content Security Policy Missing | CSP header not set on / and /register | <img width="611" height="314" alt="Capture d&#39;écran 2025-11-28 175135" src="https://github.com/user-attachments/assets/78298be7-9e80-49a8-b3e0-7f98322c3cb4" /> |
| F-03 | 🟡 Low | Weak password policy | Accepts passwords like "12345" | <img width="762" height="95" alt="Capture d&#39;écran 2025-11-28 172304" src="https://github.com/user-attachments/assets/ff2726c2-de2f-4f5c-a320-86b458cd20ed" /> |


---

> [!NOTE]
> Include up to 5 findings total.   
> Keep each description short and clear.

---

# 5️⃣ OWASP ZAP Test Report (Attachment)

**Purpose:**  
- Attach or link your OWASP ZAP scan results (Markdown format preferred).

---

**Instructions (CMD version):**
1. Run OWASP ZAP baseline scan:  
   ```bash
   zap-baseline.py -t https://example.com -r zap_report_round1.html -J zap_report.json
   ```
2. Export results to markdown:  
   ```bash
   zap-cli report -o zap_report_round1.md -f markdown
   ```
3. Save the report as `zap_report_round1.md` and link it below.

---
> [!NOTE]
> 📁 **Attach full report:** → `check itslearning` → **Add a link here**

---
