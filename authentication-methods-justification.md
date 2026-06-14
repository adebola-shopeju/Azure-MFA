# Authentication Methods — Selection & Justification

## Methods Reviewed in This Lab

During the lab, the following authentication methods were found enabled in the Microsoft Entra ID Authentication Methods policy:

| Method | Enabled | Target |
|--------|---------|--------|
| Microsoft Authenticator | ✅ Yes | All users |
| SMS | ✅ Yes | All users |
| Email OTP | ✅ Yes | All users |
| Software OATH tokens | ✅ Yes | All users |
| Passkey (FIDO2) | ✅ Yes | All users |
| Voice call | ❌ No | — |
| Hardware OATH tokens | ❌ No | — |

---

## Primary Method Selected: Microsoft Authenticator (Push Notification)

### What It Is
The Microsoft Authenticator app is a free mobile application available on iOS and Android. When a user logs in, the app receives a push notification asking them to approve or deny the sign-in. With **Number Matching** enabled, the user must also type in a specific number shown on the login screen, adding an extra layer of protection.

### Why We Chose It

#### Security Reasons
- **Number Matching** prevents MFA fatigue attacks — where hackers repeatedly send approval requests hoping the user accidentally taps "Approve"
- Push notifications are tied to a **specific registered device** (in our case, iPhone 11 Pro), making it nearly impossible to approve without physical access to that phone
- The app works **offline** — it can generate one-time codes (OTP) even without internet, making it more reliable than SMS
- It is **phishing-resistant** because the notification shows which app and location is requesting access

#### Usability Reasons
- The app is **free** to download and use
- Approving a login takes **one tap** (or typing a 2-digit number with number matching)
- Users are already familiar with push notifications from other apps
- Works on both **iOS and Android**
- Does not require a phone signal — only internet connection

---

## Secondary Method Available: SMS

### What It Is
A one-time passcode (OTP) sent via text message to the user's registered phone number.

### Justification for Keeping It Enabled
- Serves as a **backup method** if the user loses or replaces their phone
- Accessible to users who may not have smartphones capable of running the Authenticator app
- Familiar and easy to use for non-technical users

### Limitations
- **Less secure** than the Authenticator app — SIM swapping attacks can intercept SMS codes
- Requires mobile signal (does not work offline)
- Should be used as a **fallback only**, not as the primary method

---

## Method NOT Selected: Voice Call

Voice call authentication was left **disabled** for the following reasons:
- Slower and more disruptive than push notifications
- Vulnerable to social engineering attacks where attackers redirect calls
- Poor user experience in noisy environments or when phone is on silent
- Not suitable for international users due to call costs

---

## Method NOT Selected: Hardware OATH Tokens

Hardware tokens were left **disabled** because:
- They require physical devices to be purchased and distributed to users — adding cost
- Lost or damaged tokens create support overhead
- Software-based methods (Authenticator app) provide equivalent security with zero hardware cost

---

## Security vs Usability Balance

| Factor | Microsoft Authenticator | SMS | Voice Call |
|--------|------------------------|-----|------------|
| Security Level | ⭐⭐⭐⭐⭐ High | ⭐⭐⭐ Medium | ⭐⭐ Low |
| Ease of Use | ⭐⭐⭐⭐⭐ Very Easy | ⭐⭐⭐⭐ Easy | ⭐⭐⭐ Moderate |
| Works Offline | ✅ Yes (OTP mode) | ❌ No | ❌ No |
| Cost | Free | Free (depends on carrier) | May incur costs |
| Phishing Resistant | ✅ Yes | ❌ No | ❌ No |

**Conclusion:** Microsoft Authenticator with push notifications and number matching provides the best balance of high security and excellent user experience. It was the right choice for this lab and for real-world enterprise deployments.
