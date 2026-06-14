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
- Navigated to the individual user's Authentication methods page instead
- This provided clearer evidence of MFA enrollment, showing the registered device (iPhone 11 Pro) and default sign-in method (Microsoft Authenticator notification)

### Lesson Learned
The user's individual **Authentication methods** page (under Users → [user] → Authentication methods) provides more detailed and clear evidence of MFA registration than the Per-user MFA status table.

---

## Issue 3 — Verification Options Moved to New Location

### What Happened
On the MFA Service Settings page, the "Verification options" section showed a notice that these settings had been **migrated to the Authentication Methods policy** page.

### Resolution
- Followed the blue link "Go to Authentication methods" on the Service Settings page
- Found the full list of authentication methods on the dedicated Authentication Methods page
- Confirmed Microsoft Authenticator, SMS, Email OTP, and Software OATH tokens are all enabled

### Lesson Learned
Microsoft has been gradually migrating legacy MFA settings to the newer **Authentication Methods** policy framework. Always check the Authentication Methods page for current configuration options.

---

## Issue 4 — Test User Has No Azure Subscription

### What Happened
After the MFA test user successfully logged in and completed MFA enrollment, the Azure Portal showed "Welcome to Azure! Don't have a subscription?" — the test user had no subscription assigned.

### Impact
Expected behavior — did not affect lab objectives. MFA authentication happens at the identity layer before subscription access.

### Resolution
No action required. The MFA enrollment was the sole purpose of the test user account.

---

## Issue 5 — SMS Sign-in State Shows "Not Configured"

### What Happened
After adding a phone number as a backup method, the Phone number details panel showed "SMS sign-in state: Not configured."

### Impact
Initially appeared to suggest SMS was not working as a backup MFA method.

### Resolution
Investigated and confirmed this is expected behaviour. "SMS sign-in state" refers to **passwordless SMS sign-in** (logging in with ONLY a phone number, no password at all) — which is a separate feature from SMS as an MFA backup method. The phone number was correctly registered as a backup MFA verification method regardless of this state.

### Lesson Learned
There are two different SMS features in Entra ID:
- **SMS as MFA backup** = phone number used to receive a verification code during MFA (this is what we configured ✅)
- **SMS sign-in** = passwordless login using only a phone number (separate feature, not needed for this lab)

---

## Account Lockout Recovery Procedures

### Scenario A — Lost Primary MFA Device (Phone)
1. User contacts the Global Administrator
2. Admin goes to Entra ID → Users → MFA Test User → Authentication methods
3. Admin clicks "+ Add authentication method" → "Temporary Access Pass"
4. Admin sets duration (1 hour recommended for security)
5. Admin shares the TAP code securely with the user (in person or verified phone call — NEVER email)
6. User logs in with: username + password + TAP code
7. Azure prompts user to register a new authentication method
8. User scans QR code with new device to re-register Microsoft Authenticator
9. Admin revokes the old device registration from the Authentication methods page

### Scenario B — Lost Access to Both Phone and SMS Number
1. Admin generates a Temporary Access Pass as above
2. Admin also verifies the user's identity before sharing the TAP (ID check, video call, etc.)
3. After login with TAP, user registers entirely new authentication methods
4. User should register at least TWO methods to prevent future lockouts

### Scenario C — Temporary Access Pass Has Expired
1. Admin generates a new TAP — there is no limit on how many can be generated
2. Previous expired TAPs are automatically removed from the authentication methods list
3. New TAP follows the same secure sharing procedure as above

---

## Best Practices Identified During Lab

1. **Always enable MFA for Global Administrator accounts** — highest privileges, most targeted
2. **Use Microsoft Authenticator over SMS** — push notifications with number matching are more secure
3. **Enable Number Matching** — prevents MFA fatigue attacks
4. **Configure at least TWO backup methods** — in this lab: SMS + Temporary Access Pass
5. **Store TAP codes securely** — password manager, physical safe, never plain text
6. **Do NOT use the same device for both login and MFA approval**
7. **Regularly review registered authentication methods** — check for unauthorised devices
8. **Use Per-user MFA for small tenants** — free, no premium license required
9. **Test MFA with a test account first** — never test on your admin account directly
10. **Document recovery procedures** — ensure admins know how to recover locked-out users

---

## Summary

All issues encountered during this lab were successfully resolved. The MFA configuration was completed without incurring any additional costs, and the full enrollment flow was demonstrated including primary method (Microsoft Authenticator), backup method (SMS), and emergency recovery method (Temporary Access Pass).
