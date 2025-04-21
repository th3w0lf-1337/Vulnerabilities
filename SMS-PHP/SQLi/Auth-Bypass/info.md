#### 1. **Vulnerability Overview**

- **Vulnerable Endpoint**: `/sms/classes/Login.php?f=login`
- **Vulnerable Parameter**: `username`
- **Issue Type**: SQL Injection - Authentication Bypass
- **Severity**: High (Critical) – Unauthorized access can be gained to the system, leading to potential unauthorized actions.
- **Software URL**: `https://www.sourcecodester.com/php/15023/stock-management-system-phpoop-source-code.html`

#### 2. **Vulnerability Description**

The Stock Management System software contains a SQL Injection vulnerability on the login page that leads to an authentication bypass. This vulnerability is triggered by the improper sanitization of user inputs, specifically the `username` parameter. Attackers can exploit this flaw to bypass the authentication mechanism and gain access to the system with admin privileges.

#### 3. **Steps to Reproduce**

To reproduce the vulnerability and achieve authentication bypass, follow these steps:

1. Access the login page located at `/sms/classes/Login.php?f=login`.
2. In the `username` input field, use the following payload:
    `admin' OR 1='1`
3. Input anything in the password field.
4. Submit the login form.

This will bypass the authentication and grant access as the `admin` user because the injected SQL statement results in a condition that is always true (`1='1'`), causing the system to authenticate the attacker without verifying the credentials.

#### 4. **SQL Injection Payload**

The following payload demonstrates the SQL Injection used to bypass authentication:
`admin' OR 1='1`

#### 5. **Proof of Concept (PoC)**

Below is a screenshot of the proof of concept, demonstrating the SQL Injection attack in action, which allows the attacker to bypass authentication.

![1](https://github.com/th3w0lf-1337/Vulnerabilities/blob/main/SMS-PHP/SQLi/Auth-Bypass/Auth-Bypass.png)

#### 6. **Impact**

- **Authentication Bypass**: Attackers can log in as an administrator or any other user without needing valid credentials.
- **Data Exposure**: Unauthorized users may access sensitive data stored in the database.
- **Privilege Escalation**: Depending on the system's role-based access control, an attacker could gain full administrative control over the application.
- **System Compromise**: If the attacker manages to gain administrative privileges, they could manipulate, steal, or delete sensitive information, leading to a potential system compromise.
