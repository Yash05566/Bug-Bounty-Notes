# Email OTP Verification Bypass Through HTTP Request/Response Manipulation

## 1. Vulnerability Overview

| Field                  | Details                                             |
| ---------------------- | --------------------------------------------------- |
| Vulnerability          | Email OTP Verification Bypass                       |
| Target                 | https://taskbear.com.au/                            |
| Affected Functionality | User Registration / Email Verification              |
| Vulnerability Category | Authentication Bypass                               |
| Severity               | Medium (Provisional – subject to impact assessment) |
| Testing Tool           | Burp Suite                                          |
| Additional Tools       | Temporary Email Service                             |

## 2. Vulnerability Description

The application's email verification mechanism can potentially be bypassed through HTTP request/response manipulation.

During testing, it was observed that simply changing the HTTP response status to `200 OK` was insufficient to bypass verification.

However, by analyzing a legitimate successful OTP verification exchange and manipulating the verification flow to match its expected structure, registration was successfully completed using an email address that had not received a valid OTP.

This indicates a potential weakness in the application's OTP verification implementation.

## 3. Steps to Reproduce (PoC)

1. Open the target application and navigate to the sign-up page.
2. Register using a temporary email address.
3. When prompted for OTP verification, enter an incorrect OTP.
4. Intercept the verification traffic using Burp Suite and observe the unsuccessful response.
5. Repeat the registration process using another temporary email address and a valid OTP.
6. Capture and analyze the successful verification HTTP exchange.
7. Compare the successful and unsuccessful verification structures using Burp Suite and Meld.
8. Repeat the test with a new temporary email address and an incorrect OTP.
9. Manipulate the verification exchange to match the expected successful structure.
10. Forward the modified traffic and observe the registration result.

**Result:** The application allowed a new user account to be created without successfully completing the intended OTP verification process.

## 4. Proof of Concept (PoC)

### Screenshot 1 – Registration Using Temporary Email

<img width="1919" height="1004" alt="image" src="https://github.com/user-attachments/assets/084e135a-d565-4445-b7bc-1fa1cce912c0" />


### Screenshot 2 – Incorrect OTP Submission

<img width="1919" height="1002" alt="image" src="https://github.com/user-attachments/assets/cda33233-5cf9-4e22-a314-9a5980cd6fde" />


### Screenshot 3 – Successful OTP Verification Traffic

<img width="1919" height="1006" alt="image" src="https://github.com/user-attachments/assets/47b5d846-cbbc-4697-a404-a8623b20e91e" />


### Screenshot 4 – HTTP Request/Response Comparison

<img width="1919" height="1004" alt="image" src="https://github.com/user-attachments/assets/20b982c8-e7ca-407a-8613-c906fd5014d3" />


### Screenshot 5 – Modified Verification Exchange in Burp Suite

<img width="1919" height="1011" alt="image" src="https://github.com/user-attachments/assets/491c8dcd-328a-4bd1-9c4c-0b1912604b7b" />


### Screenshot 6 – Successful Account Registration

<img width="1919" height="1005" alt="image" src="https://github.com/user-attachments/assets/31c8f59a-e649-4e8e-a56f-abfd6263c8ad" />


## 5. Security Impact

An attacker may potentially:

* Register accounts using unverified email addresses.
* Bypass the intended email ownership verification mechanism.
* Abuse registration functionality through automated account creation.
* Undermine the application's email-based trust and verification process.
* Potentially impersonate another email address, depending on how the application uses email identities.

## 6. Root Cause

The potential root cause is improper server-side enforcement of the email OTP verification process.

The application may be relying on manipulable verification data or an insufficiently validated verification state instead of independently confirming OTP validity on the server.

## 7. Recommended Remediation

1. **Server-Side OTP Validation:** Verify OTPs exclusively on the backend before marking an email as verified.
2. **Secure Verification State:** Maintain verification status securely on the server and bind it to the correct email and registration session.
3. **OTP Security Controls:** Implement OTP expiration, single-use validation and attempt limits.
4. **Registration Enforcement:** Prevent account creation until successful server-side email verification is confirmed.

## 8. Conclusion

The testing identified a weakness in the application's email OTP verification process, allowing account registration without completing the intended verification.

Implementing strict server-side validation and secure verification controls will help prevent unauthorized account creation.
