# ShopNest - Vulnerable Lab Design Task

## Overview

This project demonstrates a vulnerable password reset implementation in a Flutter-based mobile application called **ShopNest**.

The lab was created as part of an Offensive Security Engineering application task to demonstrate how insecure authentication logic can lead to account takeover vulnerabilities.

---

# Vulnerability

## Password Reset Token Reuse

The application generates password reset tokens and allows users to reset passwords using deep links.

However, previously used reset tokens are never invalidated after successful password reset.

An attacker with access to an old reset link can reuse the same token multiple times and reset the victim’s password again.

---

# CWE & OWASP Mapping

- CWE-640: Weak Password Recovery Mechanism
- OWASP Top 10 A07: Identification and Authentication Failures

---

# Vulnerable Flow

1. User requests password reset
2. Application generates reset token
3. Reset deep link is created
4. User resets password successfully
5. Token remains active in backend storage
6. Attacker reuses same token
7. Account takeover occurs again

---

# Example Deep Link

```text
shopnest://reset-password?token=euioyqpmve72
```

# Vulnerable Code
```
final email = FakeDatabase.resetTokens[token];

if (email != null) {

   FakeDatabase.demoUser.password =
       newPassword;

   return true;
}
```
# Issue
The token is never invalidated after successful password reset.

# Mitigation

Invalidate reset tokens immediately after successful password reset.

```
FakeDatabase.resetTokens.remove(token);
```
# Additional security improvements:

- allow one-time token usage
- add token expiration
- log repeated token reuse attempts
- notify users of suspicious reset activity

# Screenshots

<p align="center">
  <img src="https://github.com/ishan-walia/Vulnerable-Lab-Design-Task/blob/main/Photo/Screenshot%202026-05-25%20115342.png" width="820">

  <img src="https://github.com/ishan-walia/Vulnerable-Lab-Design-Task/blob/main/Photo/Screenshot%202026-05-25%20111715.png" width="220">

  <img src="https://github.com/ishan-walia/Vulnerable-Lab-Design-Task/blob/main/Photo/Screenshot%202026-05-25%20111725.png" width="220">

  <img src="https://github.com/ishan-walia/Vulnerable-Lab-Design-Task/blob/main/Photo/Screenshot%202026-05-25%20111753.png" width="220">
</p>
   
# Tech Stack
- Flutter
- Dart
- Android Studio
- Cyber Security

# Educational Purpose

This project was created for educational and security research purposes only.
The goal is to demonstrate insecure authentication logic and secure remediation practices in mobile applications.
