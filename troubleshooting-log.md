# Troubleshooting Log — Lab 10: Azure MFA Configuration

## Overview

This document records issues encountered during the MFA configuration lab and the steps taken to resolve them.

---

## Issue 1 — Wrong MFA Page Loaded (Premium Feature Wall)

### What Happened
When searching for "Multi-Factor Authentication" in the Azure search bar, the portal loaded the **Multifactor Authentication Overview** page which displayed a "Get a free Premium trial to use this feature" banner. This page required an Azure AD P1/P2 premium license to access Conditional Access-based MFA.

### Impact
Potential risk of accidentally starting a paid trial close to credit expiry (credits expiring 17 June 2026).

### Resolution
- Did NOT click the "Get Free Premium Trial" button
- Instead, navigated to **Users → Per-user MFA** via the three-dot (ellipsis) menu on the Users page
- This accessed the **free per-user MFA** configuration which does not require a premium license
- Successfully enabled MFA for the test user using this free method

### Lesson Learned
Azure has two MFA systems:
- **Per-user MFA** = Free, found under Users → "..." → Per-user MFA
- **Conditional Access MFA** = Requires Azure AD P1/P2 premium license

For lab purposes and free-tier accounts, always use Per-user MFA.

---

## Issue 2 — Status Column Not Visible After Enabling MFA

### What Happened
After enabling MFA for the test user, the Status column in the Per-user MFA table was not clearly visible in the screenshot due to the column being cut off on the right side of the screen.

### Resolution
- Scrolled right to confirm the status column
- Navigated to the individual user's Authentication methods page instead
- This provided clearer evidence of MFA enrollment, showing the registered device (iPhone 11 Pro) and default sign-in method (Microsoft Authenticator notification)

### Lesson Learned
The user's individual **Authentication methods** page (under Users → [user] → Authentication methods) provides more detailed and clear evidence of MFA registration than the Per-user MFA status table.

---

## Issue 3 — Verification Options Moved to New Location

### What Happened
On the MFA Service Settings page, the "Verification options" section (where you choose SMS, phone call, app notification) showed a notice that these settings had been **migrated to the Authentication Methods policy** page.

### Resolution
- Followed the blue link "Go to Authentication methods" on the Service Settings page
- Found the full list of authentication methods on the dedicated Authentication Methods page
- Confirmed Microsoft Authenticator, SMS, Email OTP, and Software OATH tokens are all enabled

### Lesson Learned
Microsoft has been gradually migrating legacy MFA settings to the newer **Authentication Methods** policy framework. Always check the Authentication Methods page (Entra ID → Security → Authentication Methods) for the most current configuration options.

---

## Issue 4 — Test User Has No Azure Subscription

### What Happened
After the MFA test user successfully logged in and completed MFA enrollment, the Azure Portal showed "Welcome to Azure! Don't have a subscription?" — the test user had no subscription assigned.

### Impact
This is expected behavior and did not affect the lab objectives. The purpose of this user was purely to test MFA enrollment and authentication, not to access Azure resources.

### Resolution
No action required. The MFA enrollment was successfully completed, which was the sole purpose of the test user account.

### Lesson Learned
For MFA testing purposes, a user does not need an Azure subscription. MFA authentication happens at the identity layer (Entra ID), before any subscription or resource access is granted.

---

## Best Practices Identified During Lab

Based on the lab experience, the following best practices were identified for securing administrator accounts with MFA:

1. **Always enable MFA for Global Administrator accounts** — These have the highest privileges and are the most targeted by attackers

2. **Use Microsoft Authenticator over SMS** — Push notifications with number matching are more secure and phishing-resistant than SMS codes

3. **Enable Number Matching** — Prevents MFA fatigue attacks where attackers repeatedly send approval requests

4. **Do NOT use the same device for both login and MFA approval** — This defeats the purpose of a second factor

5. **Regularly review registered authentication methods** — Check Users → Authentication methods to ensure no unauthorized devices have been registered

6. **Use Per-user MFA for small tenants** — For small directories without premium licenses, Per-user MFA provides solid protection at no extra cost

7. **Test MFA with a test account first** — Never test MFA changes directly on your admin account, as a misconfiguration could lock you out

8. **Keep backup authentication methods** — Always have at least one backup method (e.g., SMS) in case the primary device is lost

---

## Summary

All issues encountered during this lab were successfully resolved. The MFA configuration was completed without incurring any additional costs, and the full enrollment flow was demonstrated and documented through screenshots.
