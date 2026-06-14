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
| MFA Method Used | Microsoft Authenticator (Push Notification) |
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

### Step 6 — Reviewed Authentication Methods
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

### Step 9 — Verified MFA Registration from Admin Side
- Returned to admin account in the normal browser
- Navigated to MFA Test User → Authentication methods
- Confirmed:
  - Default sign-in method: Microsoft Authenticator notification
  - Registered device: iPhone 11 Pro
  - System preferred MFA method: PhoneAppNotification

---

## Security Principles Applied

### Zero Trust
Zero Trust means "never trust, always verify." Even a user inside the network must prove their identity every time. MFA enforces this by requiring a second factor regardless of where the user is logging in from.

### Defense in Depth
This principle means layering multiple security controls so that if one fails, others still protect the system. MFA is one layer — on top of passwords, firewalls, and other controls — creating a multi-layered security architecture.

---

## Screenshots

All screenshots are located in the `/screenshots` folder of this repository.

| Screenshot | Description |
|-----------|-------------|
| screenshot_01_azure_portal_home.png | Azure Portal home dashboard |
| screenshot_02_entra_id_home.png | Microsoft Entra ID overview page |
| screenshot_03_entra_users_page.png | Users list in Entra ID |
| screenshot_04_create_test_user.png | Create new user form |
| screenshot_05_per_user_mfa_option.png | Per-user MFA panel showing both users |
| screenshot_06_enable_mfa_popup.png | MFA enabled success notification |
| screenshot_07_service_settings_bottom.png | MFA Service Settings page |
| screenshot_08_authentication_methods.png | Authentication Methods policy list |
| screenshot_09_microsoft_authenticator_settings.png | Microsoft Authenticator settings |
| screenshot_09b_authenticator_configure.png | Authenticator Configure tab (number matching) |
| screenshot_10_mfa_enrollment_prompt.png | "Let's keep your account secure" prompt |
| screenshot_11_authenticator_added.png | "Authenticator Added" confirmation |
| screenshot_12_stay_signed_in.png | Successful login confirmation |
| screenshot_13_mfa_user_logged_in.png | Test user logged into Azure Portal |
| screenshot_14_user_auth_methods_registered.png | Admin view of registered MFA device |

---

## Conclusion

This lab successfully demonstrated the full lifecycle of Azure MFA configuration — from enabling per-user MFA as an administrator, to the end-user enrollment experience, to verifying the registration from the admin panel. The Microsoft Authenticator app with push notifications and number matching provides a strong, user-friendly second factor that significantly improves account security.
