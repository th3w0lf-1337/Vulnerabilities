#### 1. **Vulnerability Overview**

- **Vulnerable Endpoint**: `/sms/admin/?page=receiving/view_receiving&id=1`
- **Vulnerable Parameter**: `id`
- **Issue Type**: SQL Injection
- **Software URL**: `https://www.sourcecodester.com/php/15023/stock-management-system-phpoop-source-code.html`

---

#### 2. **Vulnerability Description**

A SQL injection vulnerability has been identified in the Receiving page of the Stock Management System, specifically in the `id` parameter of the following endpoint:`/sms/admin/?page=receiving/view_receiving&id=1`

This vulnerability allows an attacker to inject arbitrary SQL queries into the backend database by manipulating the `id` parameter. The vulnerability can be exploited using a UNION-based SQL injection payload.

By injecting the following payload into the `id` parameter:

``` SQL
10' UNION SELECT 1,2,3,4,5,6,7,8,9,(select group_concat(username,0x3a,password) from users),11,12-- -
```

An attacker can perform a **UNION SELECT** query that returns sensitive data from the `users` table, specifically usernames and MD5 hashed passwords. This information can be easily extracted and used for unauthorized access or other malicious activities.

This exposure of user credentials, even if hashed, poses a serious security risk as attackers could attempt to crack these hashes or use them to gain unauthorized access to the system.

---

#### 3. **Steps to Reproduce**

1. Login to a staff member account.
2. Access the vulnerable page: `/sms/admin/?page=receiving/view_receiving&id=1`.
3. Modify the `id` parameter to the following payload:  
    `10' UNION SELECT 1,2,3,4,5,6,7,8,9,(select group_concat(username,0x3a,password) from users),11,12-- -`
4. The response will contain the concatenated usernames and password hashes of users stored in the `users` table.

---

#### 4. **SQL Injection Payload**

`10' UNION SELECT 1,2,3,4,5,6,7,8,9,(select group_concat(username,0x3a,password) from users),11,12-- -`

---

#### 5. **Proof of Concept (PoC)**

Below is a screenshot of the proof of concept, demonstrating the SQL Injection attack in action, which allows the attacker to reveals sensitive user credentials.

![1](https://github.com/th3w0lf-1337/Vulnerabilities/blob/main/SMS-PHP/SQLi/Receiving/Receiving-1.png)
![2](https://github.com/th3w0lf-1337/Vulnerabilities/blob/main/SMS-PHP/SQLi/Receiving/Receiving-2.png)

---

#### 6. **Impact**

This vulnerability has a significant impact on the security of the application. An attacker with knowledge of this vulnerability can:

1. Extract usernames and password hashes from the `users` table.
2. Potentially use these hashes to crack the passwords.
3. Gain unauthorized access to sensitive areas of the application or escalate privileges.

---
