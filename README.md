# OWASP Juice Simulated Attack Report

## Web Penetration Testing

### Executive Summary

#### Scope and Methodology
- **Scope**: Website (OWASP Juice Shop)
- **Approach**: Gray-box testing
- **Tools Used**: dirb, SQL Query, hydra, XSS testing tools

### Key Findings

#### 1. Enumeration to Find Admin Path
- **Critical Vulnerabilities**:
  - Attackers can locate the admin panel via predictable paths or enumeration techniques.
  - Unauthorized access to the admin panel may result in full control of the application.
- **Risk**:
  - Increased attack surface
  - Potential compromise of the entire application or database
  - Unauthorized administrative actions, such as data manipulation or deletion
  - Leakage of sensitive configuration or system information
- **Recommendations**:
  - Use unpredictable admin URLs and enforce IP whitelisting or VPN access.
  - Implement multi-factor authentication (MFA) for admin accounts.

#### Attack Simulation
- **Tools Used**: dirb
- **Steps**:
  1. Enumerate directories using the `dirb` tool.
  2. Command: `dirb http://localhost:3000 /usr/share/wordlists/dirb/common.txt -v | grep admin`
  3. Discover hidden admin paths.
  4. Verify paths and successfully access the admin panel.
- **Conclusion**:
  - Admin paths are easily discoverable.
  - Unauthorized users can locate the admin panel, increasing the likelihood of brute-force attempts and exploitation.

![Admin Path Enumeration](https://github.com/MostafaaaHussein/WebPenetration-Testing/blob/main/IMG-20241227-WA0077.jpg?raw=true)

#### 2. SQL Injection
- **Critical Vulnerabilities**:
  - User input is not sanitized, making the database susceptible to unauthorized access or manipulation.
- **Risk**:
  - Access to sensitive user data or credentials
  - Database corruption or data loss
  - Potential for remote code execution in severe cases
- **Recommendations**:
  - Use parameterized queries and strict input validation.
  - Implement least privilege principles for database accounts.

#### Attack Simulation
- **Steps**:
  1. Exploit improperly sanitized user inputs in SQL queries.
  2. Payload: `Email: ' OR 1=1--`
  3. Manipulate SQL query to always return true, bypassing authentication.
  4. Gain unauthorized access to the application.
- **Conclusion**:
  - Improper input validation allows attackers to manipulate SQL queries.
  - Potential compromise of sensitive data and database integrity.

![SQL Injection Attack](https://github.com/MostafaaaHussein/WebPenetration-Testing/blob/main/IMG-20241227-WA0078.jpg?raw=true)

#### 3. Brute Force on Admin Credentials
- **Critical Vulnerabilities**:
  - The login mechanism lacks protections against brute-force attacks.
- **Risk**:
  - High likelihood of success if weak or default credentials are used
  - Denial-of-service (DoS) due to excessive server resource usage
- **Recommendations**:
  - Enforce strong password policies, rate limiting, and account lockout mechanisms.
  - Integrate CAPTCHA and MFA for admin logins.

#### Attack Simulation
- **Tools Used**: hydra
- **Steps**:
  1. Use hydra to attempt brute-force attacks on known admin emails and IPs.
  2. Command: `hydra -l admin -P passwords.txt <IP>`
  3. Successfully brute-force credentials and log in.
- **Conclusion**:
  - Weak protections on the admin login page allow brute-force attempts.
  - Risk of full application compromise through admin credential theft.

#### 4. XSS in Product Search
- **Critical Vulnerabilities**:
  - Improperly sanitized user inputs allow malicious scripts to execute in users' browsers.
- **Risk**:
  - Theft of user session tokens, leading to account hijacking
  - Execution of malicious actions on behalf of affected users
- **Recommendations**:
  - Sanitize and validate all user inputs and outputs.
  - Implement a Content Security Policy (CSP).

#### Attack Simulation
- **Steps**:
  1. Inject payload in the product search field: `<iframe src='javascript:alert("XSS")'>`
  2. Observe JavaScript alert confirming vulnerability.
- **Conclusion**:
  - Lack of proper input sanitization allows XSS attacks.
  - Risk of malicious scripts executing in users' browsers.

![XSS Attack](https://github.com/MostafaaaHussein/WebPenetration-Testing/blob/main/IMG-20241227-WA0087.jpg?raw=true)

---

## Recommendations

### General Security Improvements
1. **Access Control**
   - Restrict admin panel access through IP whitelisting or VPN.
   - Implement role-based access control (RBAC).
2. **Authentication Hardening**
   - Enforce strong passwords.
   - Use multi-factor authentication (MFA).
3. **Web Server Configuration**
   - Disable directory listing.
   - Remove unused files and directories.
4. **Input Validation**
   - Sanitize all user inputs and outputs.
   - Use parameterized queries.
5. **Monitoring and Logging**
   - Monitor login attempts and unauthorized URL access.
   - Log all suspicious activities for review.

### Testing and Maintenance
- Regularly test the application for vulnerabilities using automated tools.
- Apply patches and updates to the database and application framework.

---

## Names and IDs
- **عبدالرحمن سامح كمال**: 2305039
- **احمد رؤوف احمد**: 2305067
- **مصطفي حسين عبدالعال**: 2305017

---

## Additional Resources
[Attack Simulation Video](https://drive.google.com/file/d/1l_n58hjuBKMmKer19l7Uqr_U4uJZy1nj/view?usp=sharing)
