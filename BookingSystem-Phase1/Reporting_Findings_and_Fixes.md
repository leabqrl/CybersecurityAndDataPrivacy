# Reporting Findings and Fixes
Tester : Léa Becquerel

---
## Summary of findings verification 

| ID | Severity | Finding | Description | Statut |
|------|-----------|----------|--------------|------------------|
| F-01 | 🔴 High | SQL Injection in registration | Input field allows `' OR '1'='1` injection | Fixed |
| F-02 | 🟠 Medium | Missing Anti-clickjacking Header |The response does not protect against 'ClickJacking' attacks | Fixed |
| F-03 | 🟠 Medium | Absence of Anti-CSRF Tokens | Registration form has no CSRF protection | Not fixed |
| F-04 | 🟠 Medium | Content Security Policy (CSP) Header Not Set | CSP is an added layer of security that helps to detect and mitigate certain types of attacks, including Cross Site Scripting (XSS) and data injection attacks. | Fixed |
| F-05 | 🟡 Low | Weak password policy | Accepts passwords like "12345" | Fixed |

---
## Finding 1 – SQL Injection in registration
- ### Original issue :
The page results were successfully manipulated using the boolean conditions [foo-bar@example.com' AND '1'='1' -- ] and [foo-bar@example.com' AND '1'='2' -- ]

- ### Verification steps :
This vulnerability was identified by launching an active scan on ZAP.

- ### Status:
Fixed

- ### Evidence :
| The list of alerts before : | The list of alerts after : | 
|-----------------------------|----------------------------|
| <img width="453" height="225" alt="Capture d&#39;écran 2025-12-02 213822" src="https://github.com/user-attachments/assets/dfb9c7c3-82d1-4dae-ac4f-abed77e22910" /> | <img width="395" height="203" alt="Capture d&#39;écran 2025-12-02 213849" src="https://github.com/user-attachments/assets/0a4828d0-0de5-4b62-9e8b-7c442b53d705" /> |


---

## Finding 2 – Missing Anti-clickjacking Header

- ### Original issue :
The response does not protect against 'ClickJacking' attacks. It should include either Content-Security-Policy with 'frame-ancestors' directive or X-Frame-Options

- ### Verification steps :
This vulnerability was identified by launching an active scan on ZAP.

- ### Status:
Fixed

- ### Evidence :

| The list of alerts before : | The list of alerts after : | 
|-----------------------------|----------------------------|
| <img width="453" height="225" alt="Capture d&#39;écran 2025-12-02 213822" src="https://github.com/user-attachments/assets/dfb9c7c3-82d1-4dae-ac4f-abed77e22910" /> | <img width="395" height="203" alt="Capture d&#39;écran 2025-12-02 213849" src="https://github.com/user-attachments/assets/0a4828d0-0de5-4b62-9e8b-7c442b53d705" /> |


---

## Finding 3 – Absence of Anti-CSRF Tokens

- ### Original issue :
No Anti-CSRF tokens were found in a HTML submission form.

- ### Verification steps :
This vulnerability was identified by launching an active scan on ZAP.

- ### Status:
Not fixed

- ### Evidence :
<img width="395" height="203" alt="Capture d&#39;écran 2025-12-02 213849" src="https://github.com/user-attachments/assets/0a4828d0-0de5-4b62-9e8b-7c442b53d705" />

---

## Finding 4 – Content Security Policy (CSP) Header Not Set

- ### Original issue :
The CSP Header is not set.

- ### Verification steps :
This vulnerability was identified by launching an active scan on ZAP.

- ### Status:
Fixed

- ### Evidence :
| The list of alerts before : | The list of alerts after : | 
|-----------------------------|----------------------------|
| <img width="453" height="225" alt="Capture d&#39;écran 2025-12-02 213822" src="https://github.com/user-attachments/assets/dfb9c7c3-82d1-4dae-ac4f-abed77e22910" /> | <img width="395" height="203" alt="Capture d&#39;écran 2025-12-02 213849" src="https://github.com/user-attachments/assets/0a4828d0-0de5-4b62-9e8b-7c442b53d705" /> |


---

## Finding 5 – Weak password policy

- ### Original issue :
The site accepts passwords like "12345".

- ### Verification steps :
I tried to register with "12345" for the password. 

- ### Status:
Fixed

- ### Evidence :
<img width="664" height="319" alt="Capture d&#39;écran 2025-12-02 215953" src="https://github.com/user-attachments/assets/7eeaf9e6-e0a0-4b95-9678-961a6128b803" />

---

## Conclusion

- **Number of findings fixed** : 4/5
