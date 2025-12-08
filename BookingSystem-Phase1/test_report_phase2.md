# 🛡️ Penetration Test Report — Registration System (Phase 2 Retesting)

## 1️⃣ Introduction

**Tester**  
Name: **Prodip Saha**

**Purpose**  
The purpose of this penetration test was to re-evaluate the registration functionality of the Booking System in **Phase 2**, focusing specifically on the **five findings from Phase 1 / Part 2**.  
The goal was to verify which issues have been fixed, which remain vulnerable, and whether any regressions were introduced.

**Scope**

Tested Components:

- Phase 2 registration page  
- Email, password, and birthdate validation  
- Error-handling behavior  
- User creation workflow  
- Verification of fixes from previous phase (Findings F-01 to F-05)

**Test Approach**  
Gray-box / White-hat, manual functional + penetration-style testing.

**Test Environment & Dates**

- Start: 5th December 2025  
- End: 8th December 2025  
- OS: Windows 11  
- Tools: Docker Desktop, Browser, Manual Testing  
- Web Application: http://localhost:8002  
- Database: PostgreSQL (cybersec-db-phase2)

---

## 2️⃣ Executive Summary

The Phase 2 update demonstrates **mixed progress**.  
Some issues from Phase 1 / Part 2 have been **fully resolved**, while others have **regressed** or remain **partially unaddressed**.

The most critical regression is a **complete removal of password-length validation**, making password security worse than in earlier phases.

Birthdate validation has improved with rejection of future dates, but no minimum-age checks exist.  
Email validation is mostly correct but fails for edge-case formats using only special characters before "@", indicating incomplete sanitization.

### ✔️ Fix Status Summary (Top 5 Retest Findings)

| Finding | Description | Status |
|--------|-------------|--------|
| **F-01** | Weak / Missing Password Policy | ❌ Not fixed (regressed) |
| **F-02** | User Enumeration via Registration | ✅ Fixed |
| **F-03** | Invalid Email Format Accepted | ⚠️ Partially fixed |
| **F-04** | Inconsistent Long Password Behavior | ✅ Fixed |
| **F-05** | Birthdate Validation Missing | ⚠️ Partially fixed |

---

## 3️⃣ Severity Scale & Definitions

| Severity | Description | Recommended Action |
|---------|-------------|--------------------|
| 🔴 **High** | Critical vulnerability enabling compromise or data integrity issues | Fix immediately |
| 🟠 **Medium** | Significant weakness affecting reliability or privacy | Fix ASAP |
| 🟡 **Low** | Minor flaw or inconsistent behavior | Fix in development cycle |
| 🔵 **Info** | Informational | Monitor |

---

## 4️⃣ Findings

---

### **F-01 — Weak / Missing Password Policy**

**Severity:** 🔴 High  
**Status:** ❌ Not Fixed (Regression)

**Description:**  
Password validation has regressed compared to Phase 1 / Part 2.  
The system now:

- Accepts **extremely weak passwords** (e.g., `123456789`)  
- **Does not enforce minimum length**  
- **Does not require complexity**

This significantly reduces account security.

**Evidence:**  
Registration succeeded using weak passwords without warnings.

**Recommendation:**  
Enforce:

- Minimum password length  
- Mixed-character complexity  
- Deny common passwords  
- Provide feedback for weak passwords  

---

### **F-02 — User Enumeration via Registration**

**Severity:** 🔴 High  
**Status:** ✅ Fixed

**Description:**  
Phase 2 continues to prevent user enumeration by returning **identical messages** for both existing and new email addresses.  
No timing differences or UI hints were observed.

**Evidence:**  
Registration attempts with both existing and new emails produced indistinguishable responses.

**Recommendation:**  
Maintain generic error-handling behavior across all authentication functions.

---

### **F-03 — Invalid Email Format Accepted**

**Severity:** 🟠 Medium  
**Status:** ⚠️ Partially Fixed

**Description:**  
Typical invalid email formats are correctly rejected.  
However, the system **still accepts edge-case formats** composed entirely of special characters before “@”, such as:

```
******@example.com
```

This is not desirable and may introduce spoofing or input-sanitization issues.

**Evidence:**  
- `testgmail@com` → rejected  
- `missingatsign.com` → rejected  
- `******@example.com` → **accepted**  

**Recommendation:**  
Implement stricter local-part validation aligned with business rules, not full RFC permissiveness.

---

### **F-04 — Inconsistent Behavior with Long Passwords**

**Severity:** 🟡 Low  
**Status:** ✅ Fixed

**Description:**  
In Phase 1 / Part 2, long passwords caused inconsistent behavior depending on whether the email existed.  
Phase 2 resolves this entirely:

- Long passwords behave consistently  
- No errors or abnormal flow observed  

**Evidence:**  
Both new and existing email attempts produced stable and consistent results.

**Recommendation:**  
Maintain current approach for long-password handling.

---

### **F-05 — Birthdate Validation**

**Severity:** 🟠 Medium  
**Status:** ⚠️ Partially Fixed

**Description:**  
Phase 2 introduces **partial** birthdate validation:

- Future dates are now rejected  
- **Minimum age validation is still missing**

Users can register even if born recently, which may violate expected business rules for a booking system.

**Evidence:**  
- `01/01/2100` → rejected  
- Very recent dates → accepted  

**Recommendation:**  
Implement minimum age checks and enforce consistent frontend and backend validation rules.

---

## 5️⃣ Final Remarks

The Phase 2 retesting shows **clear improvements**, particularly with user enumeration protection and consistent password handling.  
However, the system still suffers from important weaknesses, especially:

- **Very weak password acceptance**  
- **Edge-case email validation gaps**  
- **Incomplete birthdate validation**

These issues should be addressed to strengthen the security posture and comply with secure development and privacy-by-design principles.
