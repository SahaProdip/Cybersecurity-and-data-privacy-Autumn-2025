# 🛡️ Penetration Test Report — Registration System (Phase 1 / Part 2)

## 1️⃣ Introduction

### Tester

**Name:** Prodip Saha

### Purpose  
The purpose of this penetration test was to reassess the registration functionality of the Booking System following updates introduced in **Phase 1 / Part 2**. The objective was to verify whether the vulnerabilities and anomalies identified during Part 1 had been resolved and to evaluate the system’s adherence to secure development and privacy principles.

### Scope

#### Tested Components
- Updated registration page (Phase 1 / Part 2)  
- Email, password, and birthdate validation  
- Error-handling behavior  
- User creation workflow  

### Test Approach  
**Gray-box / White-hat**

### Test Environment & Dates  
- **Start:** 1st November 2025  
- **End:** 2nd November 2025  
- OS: Windows 11  
- Tools: Docker Desktop, Browser, Manual Testing  
- Web Application: http://localhost:8001  
- Database: PostgreSQL container (`cybersec-db-phase1-part2`)  

### Assumptions & Constraints  
- No valid user credentials available  
- Manual testing only  
- Source code not reviewed  
- Time-boxed testing window  

---

## 2️⃣ Executive Summary

The updated application shows meaningful improvements over Phase 1 / Part 1.  
Several weaknesses have been corrected, notably email validation, user enumeration prevention, and long-password consistency.  
However, critical gaps remain, particularly regarding password strength enforcement and birthdate validation.

### Fix Status Summary (Top 5 Findings)

| Finding | Description | Status |
|--------|-------------|--------|
| F-01 | Weak / Missing Password Policy | ❌ Not fixed |
| F-02 | User Enumeration via Registration | ✅ Fixed |
| F-03 | Invalid Email Format Accepted | ✅ Fixed |
| F-04 | Inconsistent Long Password Behavior | ✅ Fixed |
| F-05 | No Birthdate Validation | ❌ Not fixed |

---

## 3️⃣ Severity Scale & Definitions

| Severity Level | Description | Recommended Action |
|----------------|-------------|--------------------|
| 🔴 **High** | Critical vulnerability enabling compromise or data integrity issues | Fix immediately |
| 🟠 **Medium** | Significant weakness affecting reliability or privacy | Fix as soon as possible |
| 🟡 **Low** | Minor flaw or inconsistent behavior | Fix in normal development cycle |
| 🔵 **Info** | Informational only | Monitor |

---

## 4️⃣ Findings

### F-01 — Weak / Missing Password Policy  
**Severity:** 🔴 High  
**Status:** ❌ Not Fixed  

**Description:**  
Although an 8-character minimum length has been implemented, the system still accepts trivially weak passwords such as `123456789`. No password complexity requirements exist, and users are not encouraged or required to use strong passwords.

**Evidence:**  
Registration with `123456789` was accepted without warnings.

**Recommendation:**  
Implement a comprehensive password policy (mixed-case characters, digits, special symbols, denylist for common passwords).

---

### F-02 — User Enumeration via Registration  
**Severity:** 🔴 High  
**Status:** ✅ Fixed  

**Description:**  
The user enumeration weakness identified in Phase 1 / Part 1 has been fully resolved. The application now returns consistent and generic error messages during registration, regardless of whether the email is new or already registered. This prevents attackers from determining whether an email address exists in the system.

**Evidence:**  
Attempts to register with both new and existing email addresses produced identical, non-revealing error messages. No behavioral differences or redirects were observed.

**Recommendation:**  
Maintain consistent error-handling practices to prevent reintroduction of user enumeration vulnerabilities.

---

### F-03 — Invalid Email Format Accepted  
**Severity:** 🟠 Medium  
**Status:** ✅ Fixed  

**Description:**  
Email validation has significantly improved. The system now detects and rejects invalid email formats, including incorrect domain structures.

**Evidence:**  
Formats such as `testgmail@com` or missing TLDs were correctly rejected.

**Recommendation:**  
None required at this stage.

---

### F-04 — Inconsistent Behavior with Long Passwords  
**Severity:** 🟡 Low  
**Status:** ✅ Fixed  

**Description:**  
The inconsistency observed in Part 1—where long passwords caused different results depending on email existence—has been resolved. In Phase 1 / Part 2, behavior is stable and consistent regardless of email state.

**Evidence:**  
Both new and existing email cases behaved consistently.

**Recommendation:**  
No further action required.

---

### F-05 — No Birthdate Validation  
**Severity:** 🟠 Medium  
**Status:** ❌ Not Fixed  

**Description:**  
The system still accepts future birthdates during registration, resulting in illogical or invalid user profiles and potentially interfering with age-based features expected in later phases of the application.

**Evidence:**  
Registration succeeded with a birthdate set in the year 2100.

**Recommendation:**  
Implement server-side date validation ensuring the birthdate is in the past and compliant with minimum age requirements.

---

## 5️⃣ Final Remarks

The Phase 1 / Part 2 update demonstrates substantial progress, particularly in addressing user enumeration, improving email validation, and stabilizing password handling behavior. Nevertheless, critical issues remain unresolved: password complexity requirements and birthdate validation still pose concerns regarding user account security and data integrity. Addressing these weaknesses should be prioritized to align the system with secure development practices and privacy-by-design principles.
