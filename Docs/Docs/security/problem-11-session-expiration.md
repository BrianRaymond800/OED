# **Research & Design Proposal**

**Issue 11 – Insufficient Session Expiration (CWE-613)**

## **1\. Introduction**

This document presents research findings and a proposed technical design to address **Problem Report 11 – Insufficient Session Expiration** in the Open Energy Dashboard (OED) system.

The vulnerability was identified during penetration testing and affects the Authentication API component. The issue arises from how session tokens (JWTs) are handled during logout and expiration, allowing previously issued tokens to remain valid server-side even after a user logs out.

Although the severity is classified as *Low*, the impact on confidentiality and integrity is *Medium*, and the weakness falls under **OWASP Top 10 2021 – A07: Identification and Authentication Failures**.

## **2\. Summary of Findings (from Penetration Test)**

OED currently uses **stateless JSON Web Tokens (JWTs)** as session tokens:

* Tokens are stored client-side (browser storage).

* Tokens expire after \~24 hours (exp claim).

* On logout:

  * The client deletes the token locally.

  * The server **does not invalidate the token**.

### **Resulting problem:**

If an attacker steals a valid JWT:

* They can continue using the token after the user logs out.

* The token remains valid until natural expiration.

* The attacker gains the same privileges as the victim user.

### **Confirmed by testing:**

Replaying a copied /api/verification request with the same token **after logout** still succeeds:

                { "success": true }

### **Classification**

* **CWE:** 613 – Insufficient Session Expiration

* **STRIDE:** Spoofing

* **OWASP 2021:** A07 – Identification & Authentication Failures

* **CVSS v3:** 4.8 (Low)

## **3\. Security Goal**

Ensure that **logging out invalidates session tokens server-side**, not only client-side.

After logout:

* Any previously issued token must be rejected by the server.

* A stolen token must become unusable immediately.

## **4\. Research of Possible Solutions**

### **Option A – Token Revocation List (jti blacklist)**

* Add jti (token ID) to JWT.

* Store revoked token IDs in DB or Redis.

* Check the blacklist on every request.

**Drawbacks:**

* Requires storing many revoked tokens.

* Cleanup needed after expiry.

* More operational complexity.  
* 

### **Option B – “Not Valid Before” Timestamp per User (Recommended)**

Recommended in the penetration test report:

“Each user can have a Not valid before value such that tokens issued before this time are invalid.”

**Advantages:**

* Only **one database field per user**

* No token storage required

* Invalidates **all previous sessions instantly**

* Simple to implement

* Scales well

## **5\. Proposed Design (Recommended Solution)**

### **Core idea**

Add a timestamp to each user account:

           token\_invalid\_before

* Any JWT issued **before** this time is rejected.

## **6\. Technical Design**

### **6.1 Database Change**

Add a new column to the users table:

       token\_invalid\_before TIMESTAMP NOT NULL DEFAULT NOW()

**Upgrade behavior:**  
During deployment, a database migration will set token\_invalid\_before \= NOW() for all existing users, forcing a one-time re-login. New users are unaffected since their tokens are issued after account creation.

### **6.2 JWT Validation Logic (Server Middleware)**

On every authenticated request:

1. Verify JWT signature

2. Verify expiration (exp)

3. Extract:

   * user ID

   * issued at time (iat)

4. Load user from DB

5. Compare:  
     
        if token.iat \< user.token\_invalid\_before → reject (401 Unauthorized)

### **6.3 Logout Endpoint Behavior**

Current logout:

* Removes token from browser only

New logout:

1. Client removes token

2. Client calls:

                     POST /api/logout

3. Server updates:  
     
          token\_invalid\_before \= NOW()

Result:

* All previous tokens become invalid immediately.

## **7\. Security Benefits**

| Threat |   Before | After |
| :---- | :---- | :---- |
| Token theft |   Works until expiry | Immediately invalid |
| Replay attacks |   Succeeds |  Fails |
| Account takeover |   Possible | Greatly reduced  |

## **8\. Testing Plan**

### **Reproduce original vulnerability**

1. Login

2. Capture /api/verification request

3. Replay using curl → success

4. Logout

5. Replay again

### **Expected new behavior**

Step 5 should return:

           401 Unauthorized

### **Additional tests**

* New login token works

* Expired token fails

* Multiple devices → all sessions invalidated after logout

## **9\. Acceptance Criteria**

* JWTs issued before logout are rejected server-side

* Replay attack no longer succeeds

* Logout endpoint updates server state

* Pen test steps no longer reproduce vulnerability

## **10\. Conclusion**

The proposed **Not-Valid-Before timestamp approach** provides a clean, scalable, and secure solution to OED’s insufficient session expiration vulnerability.

It directly mitigates CWE-613, aligns with OWASP recommendations, and significantly improves session security with minimal architectural change.

## **11\. References**

\[1\] MITRE, “CWE-613: Insufficient Session Expiration.” [https://cwe.mitre.org/data/definitions/613.html](https://cwe.mitre.org/data/definitions/613.html)

\[2\] OWASP Foundation, “OWASP Top 10 – A07: Identification and Authentication Failures [(2021).” https://owasp.org/Top10/A07\_2021-Identification\_and\_Authentication\_Failures/](https://owasp.org/Top10/A07_2021-Identification_and_Authentication_Failures/)

\[3\] OWASP Foundation, “Session Management Cheat Sheet.” [https://cheatsheetseries.owasp.org/cheatsheets/Session\_Management\_Cheat\_Sheet.html](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)

\[4\] OWASP Foundation, “JSON Web Token (JWT) Cheat Sheet.” [https://cheatsheetseries.owasp.org/cheatsheets/JSON\_Web\_Token\_for\_Java\_Cheat\_Sheet.html](https://cheatsheetseries.owasp.org/cheatsheets/JSON_Web_Token_for_Java_Cheat_Sheet.html)

\[5\] Jones, M., Bradley, J., & Sakimura, N., “JSON Web Token (JWT), RFC 7519,” IETF, 2015\. [https://datatracker.ietf.org/doc/html/rfc7519](https://datatracker.ietf.org/doc/html/rfc7519)

\[6\] NIST, “Digital Identity Guidelines – Authentication and Lifecycle Management (SP 800-63B).” [https://pages.nist.gov/800-63-3/sp800-63b.html](https://pages.nist.gov/800-63-3/sp800-63b.html)

