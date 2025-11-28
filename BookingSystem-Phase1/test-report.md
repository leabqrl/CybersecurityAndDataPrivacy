# 1️⃣ Introduction

**Tester(s):**  
- Léa Becquerel

**Purpose:**  
- Identify vulnerabilities in registration and authentication flows
- Assess the backend’s resistance to common attack vectors 

**Scope:**  
- Tested components:
   - Public root endpoint /
   - Registration page /register
   - Static assets under /static/*
- Exclusions:
   - Authenticated pages (none tested)
   - API endpoints other than registration
- Test approach: Gray-box

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

**Overall risk level:**  High

**Top 5 immediate actions:**  
1.  Fix SQL Injection vulnerability in the registration endpoint
2.  Implement CSRF protection tokens across all forms
3.  Add a strict Content-Security-Policy header
4.  Add X-Frame-Options or CSP frame-ancestors to prevent clickjacking
5.  Implement proper error handling to stop server error disclosure

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
| F-01 | 🔴 High | SQL Injection in registration | Input field allows `' OR '1'='1` injection | <img width="380" height="302" alt="Capture d&#39;écran 2025-11-28 195148" src="https://github.com/user-attachments/assets/770e2eeb-9798-431b-b59b-f954eb6ea2d0" /> |
| F-02 | 🟠 Medium | Missing Anti-clickjacking Header |The response does not protect against 'ClickJacking' attacks | <img width="611" height="314" alt="Capture d&#39;écran 2025-11-28 175135" src="https://github.com/user-attachments/assets/bf4b8524-2bbf-4abe-95ab-94dae632df0c" /> |
| F-03 | 🟠 Medium | Absence of Anti-CSRF Tokens | Registration form has no CSRF protection | <img width="529" height="293" alt="Capture d&#39;écran 2025-11-28 175120" src="https://github.com/user-attachments/assets/70d0d1cb-31dd-453d-96ba-614c6ca22023" /> |
| F-04 | 🟠 Medium | Content Security Policy Missing | CSP header not set on / and /register | <img width="611" height="314" alt="Capture d&#39;écran 2025-11-28 175135" src="https://github.com/user-attachments/assets/78298be7-9e80-49a8-b3e0-7f98322c3cb4" /> |
| F-05 | 🟡 Low | Weak password policy | Accepts passwords like "12345" | <img width="762" height="95" alt="Capture d&#39;écran 2025-11-28 172304" src="https://github.com/user-attachments/assets/ff2726c2-de2f-4f5c-a320-86b458cd20ed" /> |


---

# 5️⃣ OWASP ZAP Test Report (Attachment)

**Purpose:**  
- https://github.com/leabqrl/CybersecurityAndDataPrivacy/blob/main/BookingSystem-Phase1/zap_report_round1.md
