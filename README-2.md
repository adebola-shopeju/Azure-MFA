# LAB 10 — Azure MFA Configuration Report

## Overview

This lab demonstrates the configuration and implementation of Multi-Factor Authentication (MFA) within Microsoft Azure using Microsoft Entra ID (formerly Azure Active Directory). The goal was to move beyond password-only security by adding a second layer of identity verification, following the principles of **Zero Trust** and **Defense in Depth**.

---

## What is MFA and Why Does It Matter?

Multi-Factor Authentication (MFA) requires users to verify their identity using two or more factors:

- **Something you know** — your password
- **Something you have** — your phone (Microsoft Authenticator app)
- **Something you are** — biometrics (fingerprint, face ID)

Without MFA, a stolen password gives an attacker full access to an account. With MFA enabled, stealing the password alone is not enough — the attacker would also need physical access to the registered device. This dramatically reduces the risk of unauthorized access.

---

## Environment Details

| Item | Detail |
|------|--------|
| Platform | Microsoft Azure |
| Identity Service | Microsoft Entra ID (Azure AD) |
| Admin Account | pelumishopeju@yahoo... (Global Administrator) |
| Test User | mfatestuser@pelumishopejuyahoo.onmicrosoft.com |
| Primary MFA Method | Microsoft Authenticator (Push Notification) |
| Backup MFA Method | Phone number (SMS) |
| Emergency Method | Temporary Access Pass (Backup Code) |
| Device Registered | iPhone 11 Pro |
| Lab Date | 14 June 2026 |

---

## Configuration Steps Taken

### Step 1 — Accessed the Azure Portal
- Navigated to https://portal.azure.com
- Logged in with Global Administrator credentials
- Confirmed access to the Default Directory

### Step 2 — Navigated to Microsoft Entra ID
- Located Microsoft Entra ID (formerly Azure Active Directory) from the left sidebar
- Confirmed Global Administrator role assignment
- Noted the Identity Secure Score of 54.17% (to be improved by enabling MFA)

### Step 3 — Navigated to Per-User MFA Settings
- From the Users section, accessed Per-user multifactor authentication
- Observed that both existing users had MFA status of "Disabled"
- Identified this as the free MFA configuration method (no premium license required)

### Step 4 — Created a Test User Account
- Created a new user: MFA Test User
- Username: mfatestuser@pelumishopejuyahoo.onmicrosoft.com
- Auto-generated a secure password
- Account enabled immediately upon creation

### Step 5 — Enabled MFA for the Test User
- Selected MFA Test User in the Per-user MFA panel
- Clicked "Enable MFA"
- Confirmed the success notification: "Multifactor authentication was enabled successfully"
- User MFA status changed from "Disabled" to "Enabled"

### Step 6 — Reviewed Authentication Methods Policy
- Navigated to Authentication Methods policy page
- Confirmed the following methods are enabled for all users:
  - Microsoft Authenticator ✅
  - SMS ✅
  - Email OTP ✅
  - Software OATH tokens ✅
  - Passkey (FIDO2) ✅

### Step 7 — Reviewed Microsoft Authenticator Settings
- Confirmed Enable toggle is ON for All users
- Confirmed Number Matching is ENABLED (protects against MFA fatigue attacks)
- Confirmed OTP via Authenticator app is allowed
- Authentication mode set to "Any"

### Step 8 — Tested the MFA Enrollment Flow
- Opened an incognito browser window
- Logged in as the test user (mfatestuser@pelumishopejuyahoo.onmicrosoft.com)
- Azure immediately prompted: "Let's keep your account secure"
- Followed the Microsoft Authenticator setup wizard
- Scanned QR code using the Microsoft Authenticator app on iPhone 11 Pro
- Completed number matching verification
- Received confirmation: "Authenticator Added — This is now your default sign-in method"
- Test user successfully logged into the Azure Portal

### Step 9 — Configured SMS as Backup MFA Method
- Returned to admin account
- Navigated to MFA Test User → Authentication methods
- Clicked "+ Add authentication method" → Selected "Phone"
- Registered a primary mobile phone number for SMS verification
- Phone number confirmed as "Primary mobile" in the authentication methods list
- This provides a backup method if the Authenticator app is unavailable

### Step 10 — Generated Temporary Access Pass (Backup/Emergency Code)
- From MFA Test User → Authentication methods
- Clicked "+ Add authentication method" → Selected "Temporary Access Pass"
- System generated a time-limited emergency access code with the following details:
  - Valid from: 14/06/2026, 23:53:13
  - Valid until: 15/06/2026, 00:53:13
  - Duration: 1 Hour
  - Is usable: Yes - Enabled by policy
- This serves as the emergency "backup code" equivalent for account recovery

### Step 11 — Verified All Three MFA Methods from Admin Side
- Confirmed all three methods visible in the Authentication methods panel:
  - Microsoft Authenticator → iPhone 11 Pro (Primary)
  - Temporary Access Pass → Expires 15/06/2026 (Emergency)
  - Phone number → Primary mobile (SMS Backup)

---

## Backup Codes — Temporary Access Pass (TAP)

### What Are Backup Codes?
Backup codes are emergency one-time passwords that allow a user to log in when their primary MFA device is unavailable — for example, if their phone is lost, stolen, broken, or has a dead battery.

### Azure's Equivalent: Temporary Access Pass (TAP)
In Microsoft Entra ID, the equivalent of backup codes is the **Temporary Access Pass**. It is a time-limited passcode generated by an administrator that allows a user to sign in and re-register new authentication methods.

### How to Generate a TAP
1. Navigate to Microsoft Entra ID → Users → [User] → Authentication methods
2. Click "+ Add authentication method"
3. Select "Temporary Access Pass"
4. Configure the duration (default: 1 hour)
5. Click Add — the code is displayed once on screen

### How to Store TAP Codes Securely
- Print and store in a **physical safe** or locked drawer
- Save in a **password manager** (e.g., Bitwarden, 1Password)
- Store in an **encrypted notes app**
- NEVER store in plain text on the same device used for login
- NEVER share via email or chat messages
- Treat it like a **physical house key** — secure, limited copies, know exactly where it is

### When to Use a TAP
- User lost their phone and cannot approve Authenticator notifications
- User got a new phone and needs to re-register the Authenticator app
- User is locked out after too many failed MFA attempts
- Emergency access needed during onboarding before primary method is set up

---

## Security Principles Applied

### Zero Trust
Zero Trust means "never trust, always verify." Even a user inside the network must prove their identity every time. MFA enforces this by requiring a second factor regardless of where the user is logging in from.

### Defense in Depth
This principle means layering multiple security controls so that if one fails, others still protect the system. In this lab, three layers of authentication were configured:
1. **Microsoft Authenticator** (primary — strongest)
2. **SMS Phone number** (backup — moderate)
3. **Temporary Access Pass** (emergency — time-limited)

---

## Account Recovery Procedures

### Scenario 1 — User Lost Their Phone
1. User contacts the administrator
2. Admin navigates to Entra ID → Users → [User] → Authentication methods
3. Admin generates a new Temporary Access Pass (TAP)
4. Admin securely shares the TAP with the user (in person or via verified channel)
5. User logs in with username + password + TAP code
6. User is prompted to register a new Authenticator app on their new device
7. Admin revokes the old iPhone 11 Pro Authenticator registration

### Scenario 2 — User Cannot Receive SMS
1. User attempts login and selects "Use a different verification option"
2. User switches to Microsoft Authenticator app notification instead
3. If app is also unavailable, user contacts admin for a TAP code

### Scenario 3 — User Locked Out Completely
1. Admin generates a Temporary Access Pass from the admin portal
2. Admin also checks if the account is still enabled (Users → [User] → Account status)
3. Admin shares TAP with user through a verified out-of-band channel (e.g., phone call)
4. User logs in with TAP and re-registers all authentication methods

---

## Screenshots

All screenshots are located in the `/screenshots` folder of this repository.

| Screenshot | Description |
|-----------|-------------|
| azure_portal_home.png | Azure Portal home dashboard |
| entra_id_home.png | Microsoft Entra ID overview page |
| entra_users_page.png | Users list in Entra ID |
| create_test_user.png | Create new user form |
| per_user_mfa_option.png | Per-user MFA panel showing both users |
| enable_mfa_popup.png | MFA enabled success notification |
| mfa_service_settings.png | MFA Service Settings page |
| authentication_methods.png | Authentication Methods policy list |
| microsoft_authenticator_settings.png | Microsoft Authenticator settings and Configure tab |
| test_user_login.png | Test user login screen in incognito window |
| mfa_setup_step1.png | MFA enrollment prompt — "Let's keep your account secure" |
| authenticator_added.png | "Authenticator Added" confirmation screen |
| stay_signed_in.png | Successful login confirmation |
| mfa_user_logged_in.png | Test user logged into Azure Portal with MFA complete |
| user_auth_methods_registered.png | Admin view showing iPhone 11 Pro registered as MFA device |
| tap_backup_code_generated_.png | Temporary Access Pass (backup code) generated with expiry details |
| sms_backup_method_add.png | Phone number registered as SMS backup method |
| all_auth_methods_registered.png | All three MFA methods configured: Authenticator + TAP + SMS |

---

## Conclusion

This lab successfully demonstrated the full lifecycle of Azure MFA configuration — from enabling per-user MFA as an administrator, to end-user enrollment, to configuring backup and emergency recovery methods. The three-layer authentication setup (Microsoft Authenticator + SMS backup + Temporary Access Pass) provides a robust, resilient security posture that balances strong protection with practical account recovery options.
