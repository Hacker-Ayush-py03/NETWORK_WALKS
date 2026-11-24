
# Mediroza General Hospital — Web Application Penetration Testing

**Prepared by:** Ayush Shukla  
**Batch:** 8084B  
**Assessment Type:** Black-Box Web Application Penetration Testing  
**Target Referenced in Report:** `medirozahospital.com`  
**Report Classification:** Confidential — Client Use Only

---

## Project Overview

This project documents a black-box security assessment of the Mediroza General Hospital patient web portal.

The assessment focused on identifying security weaknesses in authentication mechanisms, protection of confidential patient documents, and exposure of sensitive files through publicly accessible web directories.

The primary objective was to understand potential security risks, evaluate their impact on confidentiality and organisational security, and provide recommendations for improving the overall security posture of the application.

This repository provides a theoretical and documentation-focused overview of the assessment. The detailed penetration testing report is provided separately in PDF format.

**Note:** This README does not contain exploit payloads, terminal commands, screenshots, recovered credentials, patient records, or detailed exploitation procedures.

## Objectives

- Understand the security posture of a public-facing web application.
- Review authentication mechanisms and input-handling security.
- Identify risks associated with SQL injection vulnerabilities.
- Understand the security implications of weak document encryption.
- Evaluate risks related to publicly accessible backup files.
- Map identified vulnerabilities to CWE and OWASP classifications.
- Assess vulnerability severity and potential business impact.
- Recommend appropriate security controls and remediation strategies.

## Assessment Scope

The source report describes a five-day black-box penetration testing engagement focused on the Mediroza General Hospital patient web portal.

| Parameter | Details |
|---|---|
| Organisation | Mediroza General Hospital |
| Target | https://medirozahospital.com |
| Assessment Type | Black-Box Web Application Penetration Testing |
| Testing Duration | 5 Days |
| Testing Approach | OWASP-Based Security Assessment |
| Primary Focus | Authentication, Document Security, Data Exposure |
| Methodology Reference | OWASP WSTG and OWASP Top 10 |

### Scope Limitations

The assessment was limited to the agreed target domain and testing window.

The following activities were outside the documented scope:

- Social engineering.
- Denial-of-service testing.
- Source-code review.
- Testing of external infrastructure outside the agreed scope.

The findings represent the security issues documented during this assessment and should not be interpreted as an exhaustive evaluation of the entire infrastructure.

## Methodology

The assessment followed a structured black-box web application testing approach.

### 1. Reconnaissance

The initial phase focused on understanding the publicly accessible application structure, identifying relevant web resources, and reviewing information exposed through public website configuration and navigation mechanisms.

### 2. Authentication Security Assessment

The patient portal authentication mechanism was reviewed to understand how user-supplied input was processed and how database queries influenced login decisions.

The assessment focused on identifying weaknesses in input handling and authentication validation.

### 3. Controlled Security Validation

Potential vulnerabilities were evaluated to understand whether they could affect application functionality or expose resources intended only for authenticated users.

### 4. Document Security Assessment

The protection mechanisms applied to patient-facing PDF reports were reviewed from a confidentiality perspective.

The assessment considered password strength, encryption practices, and the effectiveness of document-level access protection.

### 5. Backup and Data Exposure Assessment

Publicly accessible paths and legacy resources were examined to understand whether sensitive organisational information could be exposed through improper web-server configuration or storage practices.

## Summary of Security Findings

The penetration testing report documents three major findings.

| ID | Vulnerability | CWE | OWASP Category | Risk |
|---|---|---|---|---|
| M1 | SQL Injection – Authentication Bypass | CWE-89 | A03:2021 Injection | CRITICAL |
| M2 | Weak PDF Encryption Passwords | CWE-521 | A02:2021 Cryptographic Failures | HIGH |
| M3 | Sensitive Data Exposure via Backup File | CWE-538 | A01:2021 Broken Access Control / A05:2021 Security Misconfiguration | CRITICAL |

---

## Detailed Vulnerability Analysis

### M1 — Authentication Bypass via SQL Injection

**Severity:** CRITICAL  
**CWE:** CWE-89 — Improper Neutralisation of Special Elements Used in an SQL Command  
**OWASP:** A03:2021 — Injection

#### Description

SQL Injection is a web application vulnerability that occurs when user-controlled input is incorporated into database queries without appropriate validation and parameterisation.

The assessment report identified an SQL injection weakness in the patient portal login functionality.

This weakness affected the application's authentication process and demonstrated how improper database query construction could undermine access-control mechanisms.

#### Security Impact

- Unauthorised access to protected patient portal functionality.
- Potential exposure of confidential patient information.
- Compromise of authentication and access-control mechanisms.
- Increased risk of further data exposure.
- Potential legal, privacy, and reputational consequences.

#### Recommended Remediation

- Implement parameterised queries and prepared statements.
- Avoid dynamically constructing SQL queries through string concatenation.
- Apply server-side input validation.
- Follow the principle of least privilege for database accounts.
- Conduct security reviews of authentication-related functionality.
- Implement appropriate monitoring for suspicious database and login activity.
- Use Web Application Firewall protection as an additional security layer.

### M2 — Weak / Predictable PDF Encryption Passwords

**Severity:** HIGH  
**CWE:** CWE-521 — Weak Password Requirements  
**OWASP:** A02:2021 — Cryptographic Failures

#### Description

The assessment identified weak or predictable passwords protecting patient-facing PDF laboratory reports.

Document encryption provides an additional layer of confidentiality, but its effectiveness depends on the strength of the associated password and the encryption configuration.

Predictable passwords can significantly reduce the protection provided by encrypted documents.

#### Security Impact

- Unauthorised access to confidential laboratory reports.
- Exposure of sensitive personal and health-related information.
- Reduced effectiveness of document-level encryption.
- Increased privacy and regulatory risks.
- Potential damage to organisational reputation.

#### Recommended Remediation

- Enforce strong password policies for protected documents.
- Generate unique and unpredictable passwords.
- Avoid common passwords and predictable password patterns.
- Use secure channels for distributing document access credentials.
- Prefer authenticated portal-based document access wherever feasible.
- Adopt strong modern encryption configurations.
- Periodically review document-generation and encryption mechanisms.

### M3 — Sensitive Data Exposure via Unindexed Backup File

**Severity:** CRITICAL  
**CWE:** CWE-538 — Insertion of Sensitive Information into Externally-Accessible File  
**OWASP:** A01:2021 — Broken Access Control / A05:2021 — Security Misconfiguration

#### Description

The assessment report identified a publicly accessible legacy database backup file associated with the web application.

The report also highlighted the incorrect security assumptions that can arise from relying on `robots.txt` to protect sensitive directories.

The `robots.txt` file is designed to communicate crawler preferences. It does not provide authentication, authorisation, or access restrictions.

Sensitive backup files must therefore be protected through actual server-side access controls and secure storage practices.

#### Security Impact

- Potential exposure of internal organisational information.
- Disclosure of employee-related financial records.
- Exposure of confidential corporate ownership information.
- Increased risk of privacy and financial harm.
- Legal, regulatory, and reputational consequences.

#### Recommended Remediation

- Remove exposed database backups from public directories.
- Store backups outside the web server document root.
- Implement encryption and access restrictions for backup storage.
- Disable unnecessary directory listing.
- Audit web directories for legacy and temporary files.
- Apply proper server-side access-control policies.
- Never treat `robots.txt` as a security mechanism.
- Establish a secure backup retention and disposal policy.

---

## Risk Assessment

The report uses a qualitative Likelihood × Impact approach to determine the overall risk level of each finding.

This assessment considers the potential feasibility of exploitation, sensitivity of affected resources, and consequences for the organisation.

| Finding | Likelihood | Impact | Overall Risk |
|---|---|---|---|
| F1 — SQL Injection | High | Critical | CRITICAL |
| F2 — Weak PDF Encryption | High | High | HIGH |
| F3 — Backup Data Exposure | High | Critical | CRITICAL |

### Risk Prioritisation

**Critical Priority:**
- F1 — Authentication Bypass via SQL Injection.
- F3 — Sensitive Data Exposure via Backup File.

**High Priority:**
- F2 — Weak PDF Encryption Passwords.

Critical findings should be addressed first because of their potential to expose sensitive information and compromise important application security controls.

## General Security Recommendations

In addition to resolving the individual vulnerabilities, the following security practices should be incorporated into the organisation's development and infrastructure management processes.

1. **Secure Software Development Lifecycle (SSDLC):** Integrate security reviews and testing throughout application development.

2. **Regular Penetration Testing:** Conduct periodic security assessments and reassess applications after significant updates.

3. **Centralised Logging and Monitoring:** Monitor authentication anomalies, suspicious requests, and access to sensitive resources.

4. **Data Classification:** Establish policies for handling, storing, transmitting, and retaining confidential information.

5. **Backup Security:** Ensure that database backups are encrypted and stored in restricted locations.

6. **Access Control:** Enforce least privilege and server-side authorisation for protected resources.

7. **Incident Response:** Maintain procedures for investigating security incidents, evaluating exposure, and managing appropriate notifications.

## Key Learning Outcomes

This assessment demonstrates several important web application security concepts:

- Understanding SQL Injection and its impact on authentication mechanisms.
- Importance of secure database query construction.
- Understanding the relationship between password strength and document confidentiality.
- Importance of secure encryption and credential management.
- Identifying security risks associated with exposed backup files.
- Understanding the limitations of `robots.txt`.
- Mapping security vulnerabilities to CWE and OWASP classifications.
- Evaluating vulnerability severity using likelihood and impact.
- Developing practical remediation strategies for web application vulnerabilities.
- Understanding how multiple security weaknesses can combine to increase overall organisational risk.

## Conclusion

The Mediroza General Hospital penetration testing assessment documents three significant security weaknesses involving authentication security, document encryption, and sensitive backup data exposure.

These findings demonstrate how weaknesses in different application components can collectively create serious risks to confidentiality and organisational security.

The recommended approach is to prioritise the Critical findings, strengthen document protection mechanisms, secure sensitive backup storage, and introduce continuous security assessment practices.

A follow-up penetration test should be conducted after remediation to verify that the identified vulnerabilities have been resolved effectively.

## Project Deliverable

The detailed penetration testing report is provided separately in PDF format.

The report contains the complete assessment documentation, including:

- Executive Summary.
- Scope and Methodology.
- Detailed Vulnerability Findings.
- Proof of Exploitation Documentation.
- Risk Assessment.
- Recommendations and Remediation.

This README is intended to provide a theoretical overview of the project, while the separate PDF serves as the primary technical report.

## Disclaimer

This repository is intended for educational, documentation, and cybersecurity learning purposes.

All security testing must be performed only on systems for which explicit authorisation has been granted and within the approved scope.

The concepts discussed in this repository must not be used for unauthorised access, exploitation, or disclosure of confidential information.

---

**Prepared By:** Ayush Shukla  
**Batch:** 8084B  
**Project:** Mediroza General Hospital — Black-Box Web Application Penetration Testing

---
