# Email OTP Verification Bypass Through HTTP Request/Response Manipulation

## 1. Vulnerability Overview

| Field                      | Details                                |
| :------------------------- | :------------------------------------- |
| **Vulnerability**          | Email OTP Verification Bypass          |
| **Target**                 | https://taskbear.com.au/               |
| **Affected Functionality** | User Registration / Email Verification |
| **Vulnerability Category** | Authentication Bypass                  |
| **Severity**               | Medium (Provisional)                   |
| **Testing Tool**           | Burp Suite                             |
| **Additional Tools**       | Temporary Email Service, Meld          |

---

## 2. Steps to Reproduce (PoC)

1. Open the target application and navigate to the sign-up page. Register using a temporary email address.
2. Submit an incorrect OTP and intercept the verification traffic using Burp Suite. Observe the unsuccessful response.
3. Repeat the registration process using another temporary email address and a valid OTP. Capture the successful verification exchange.
4. Compare both successful and unsuccessful verification responses using Burp Suite and Meld.
5. Register using a new temporary email address, submit an incorrect OTP, and manipulate the verification exchange to match the expected successful structure.
6. Forward the modified traffic and verify whether account registration succeeds without valid OTP verification.

**Result:** A new user account was successfully created without completing the intended OTP verification process.

---

## 3. Proof of Concept (PoC)

### 3.1 Registration Using Temporary Email

<img width="1919" height="1004" alt="1" src="https://github.com/user-attachments/assets/c93c4260-0f17-4d49-ada8-39abedda16ad" />


### 3.2 Incorrect OTP Submission

<img width="1919" height="1002" alt="2" src="https://github.com/user-attachments/assets/a92c1a19-258a-4c95-b2a0-8a76b4588083" />


### 3.3 Successful OTP Verification Traffic

<img width="1919" height="1006" alt="3" src="https://github.com/user-attachments/assets/41e996b1-96a4-42af-bc2f-ac2d49cc21f7" />


### 3.4 HTTP Response Comparison Using Meld

<img width="1919" height="1004" alt="4" src="https://github.com/user-attachments/assets/6665d1f5-9af7-4f7f-bbd1-931e6abfa076" />


### 3.5 Modified Verification Exchange in Burp Suite

<img width="1919" height="1011" alt="5" src="https://github.com/user-attachments/assets/dafff601-11bc-4b88-956c-35fe822c99ec" />


### 3.6 Successful Account Registration

<img width="1919" height="1005" alt="6" src="https://github.com/user-attachments/assets/e2b44391-f4e6-44cf-a571-d67c5dddd296" />


---

## 4. Impact

An attacker may potentially:

* Register accounts using unverified email addresses.
* Bypass the intended email ownership verification mechanism.
* Abuse registration functionality through automated account creation.
* Undermine the application's email-based verification process.
* Potentially impersonate another email address, depending on the application's account verification logic.

---

## 5. Remediation

1. **Server-Side OTP Validation:** Verify OTPs exclusively on the backend before marking an email as verified.
2. **Secure Verification State:** Maintain verification status securely on the server and bind it to the correct email and registration session.
3. **OTP Security Controls:** Implement OTP expiration, single-use validation and attempt limits.
4. **Registration Enforcement:** Prevent account creation until successful server-side email verification is confirmed.
