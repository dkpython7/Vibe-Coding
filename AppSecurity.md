# 🔐 Login Screen Security for AI Apps
### The Complete Vibe-Coding Security Guide

> A practical, copy-paste-ready security reference for developers building applications with AI coding agents (Cursor, Windsurf, Claude Code, GitHub Copilot, Bolt.new, etc.). This README explains **what** to secure, **why** it matters, **how** to implement it, and gives you **exact prompts** to hand to your AI coding agent so it ships secure authentication by default.

---

## 📖 Table of Contents

1. [Why This Guide Exists](#why-this-guide-exists)
2. [Validate and Sanitize Every User Input on the Server](#1-validate-and-sanitize-every-user-input-on-the-server)
3. [Implement Rate Limiting and Account Lockouts](#2-implement-rate-limiting-and-account-lockouts)
4. [Never Store Passwords in Plain Text](#3-never-store-passwords-in-plain-text)
5. [Use Generic Authentication Error Messages](#4-use-generic-authentication-error-messages)
6. [Use a Trusted Authentication Provider](#5-use-a-trusted-authentication-provider)
7. [Quick Security Checklist](#quick-security-checklist)
8. [Recommended Tools](#recommended-tools)
9. [Master Prompt (All 5 Rules in One Shot)](#master-prompt-all-5-rules-in-one-shot)
10. [References & Further Reading](#references--further-reading)

---

## Why This Guide Exists

AI coding agents have made it possible to ship a working application in days instead of months. The problem is that **speed of development has outpaced security awareness**. Most AI-generated login screens look polished on the surface — a nice form, validation messages, a "Forgot Password" link — but underneath, they often skip the security fundamentals entirely, because the agent was never explicitly told to include them.

The login screen is the **first point of contact** between a user and your application, and consequently the **first target** for attackers. A single oversight in authentication — an unhashed password, an endpoint with no rate limit, an error message that leaks whether an email exists — can lead to account takeovers, data breaches, and reputational damage.

This guide breaks the problem into five concrete pillars. For each one you get: an explanation of the risk, the best-practice fix, the tools that implement it, and a **prompt** you can paste directly into your AI coding agent.

---

## 1. Validate and Sanitize Every User Input on the Server

### The Problem
Client-side validation (e.g., a JavaScript check that an email field "looks right") only exists to improve user experience. It provides **zero security**, because any attacker can bypass your frontend entirely and send malformed or malicious requests straight to your API using tools like `curl`, Postman, or a simple script.

### What Needs Validation
| Field | Server-Side Checks |
|---|---|
| Email | Correct format, valid domain structure, max length |
| Password | Min/max length, complexity rules, never logged or stored in plain text |
| Username | Allowed character whitelist, max length, strip unwanted symbols |
| Free text (bio, display name, etc.) | Strip HTML tags, remove JavaScript, sanitize against script injection |

### Why It Matters
Without server-side validation, your application is exposed to:
- **SQL Injection** — malicious input alters database queries
- **Cross-Site Scripting (XSS)** — injected scripts execute in other users' browsers
- **Script Injection** — arbitrary code execution within your app's context
- **Malformed request attacks** — crashes or unexpected behavior from unexpected payloads

These can lead to full database exposure, session hijacking, or remote code execution inside your application.

### Best Practices
- Always validate on the server, regardless of frontend validation.
- **Whitelist** acceptable input rather than trying to blacklist "bad" input (blacklists are always incomplete).
- Sanitize data **before** storing it, and **again** before rendering it back to users.
- Use battle-tested validation libraries instead of writing your own regex from scratch:
  - **Zod** (TypeScript/JavaScript)
  - **Joi** (JavaScript/Node.js)
  - **Pydantic** (Python)

### 🧠 Prompt for Your AI Coding Agent
```
Add server-side input validation and sanitization to all authentication
endpoints (signup, login, password reset, profile update) in this project.

Requirements:
- Validate email format, domain structure, and max length.
- Enforce password rules: minimum 8 characters, at least one uppercase,
  one lowercase, one number, and one special character. Reject overly
  long passwords (DoS via bcrypt cost).
- Validate usernames against a strict whitelist of allowed characters
  (alphanumeric, underscore, hyphen) and enforce a max length.
- Strip HTML tags and sanitize any free-text fields (bio, display name)
  to prevent XSS and script injection.
- Use [Zod / Joi / Pydantic — pick based on my stack] for schema validation.
- Reject invalid requests with a 400 status and a generic error message
  (do not leak which specific field failed in a way that helps attackers
  enumerate valid accounts).
- Never trust any value coming from the client, even if it was already
  validated in the frontend form.
```

---

## 2. Implement Rate Limiting and Account Lockouts

### The Problem
Password-guessing attacks today are fully automated. Bots can attempt thousands or millions of password combinations against your login endpoint within hours (credential stuffing, brute force). A login endpoint with no limits is an open invitation.

### Recommended Protections

**Rate Limiting**
- Limit requests per IP to the login endpoint.
- Example: max 10 login requests per IP per minute.

**Account Lockout**
- Lock the account after repeated failed attempts.
- Recommended threshold: 5 failed attempts → 15-minute lockout.

**Progressive Delay**
- Increase the wait time after each failed attempt (e.g., 1s → 2s → 5s → 15s → 30s). This slows bots dramatically while barely affecting real users who mistype a password once or twice.

**CAPTCHA**
- Trigger a CAPTCHA challenge when suspicious behavior is detected (rapid repeated failures, unusual geographic patterns, etc.).

### Best Practices
- Store attempt counters in a fast, ephemeral store like **Redis** or **Upstash** (not your primary relational database).
- Send an email notification when an account is locked, so legitimate users are aware and can act.
- Never reveal to the requester whether the lockout is due to the account being locked vs. just a wrong password — keep responses generic (see Rule 4).

### 🧠 Prompt for Your AI Coding Agent
```
Add rate limiting and account lockout protection to the login endpoint.

Requirements:
- Use Redis (or Upstash if this is a serverless/edge deployment) to track
  login attempts per IP address and per account/email.
- Limit login attempts to 10 requests per IP per minute. Return HTTP 429
  with a generic "too many attempts, try again later" message when exceeded.
- After 5 consecutive failed login attempts for the same account, lock
  that account for 15 minutes.
- Implement progressive delay: increase response time after each failed
  attempt (1s, 2s, 5s, 15s, 30s) before returning the error.
- Send an email notification to the account owner when their account
  becomes locked, without revealing this fact to the requester in the
  HTTP response itself.
- Add CAPTCHA verification (e.g., hCaptcha or Cloudflare Turnstile) that
  triggers after 3 failed attempts from the same IP or account.
- Do not let any of these responses reveal whether the lockout is due to
  too many attempts vs. an invalid account — keep the message generic.
```

---

## 3. Never Store Passwords in Plain Text

### The Problem
If your database is ever leaked — and breaches happen even to well-funded companies — plaintext passwords mean instant, total compromise of every user account, and likely many accounts on *other* services too (since people reuse passwords).

### The Fix: Hashing
Hashing is a one-way transformation: it converts a password into an irreversible value. Even if attackers steal your database, they cannot directly recover the original passwords — they would need to brute-force each hash individually, which a strong algorithm makes computationally expensive.

### Recommended Algorithms
- **Argon2id** — the modern, recommended default for new applications (winner of the Password Hashing Competition)
- **bcrypt** — widely supported, battle-tested, still solid
- **scrypt** — memory-hard, also a good choice

### Never Use
- **MD5** — Designed for speed, not security; trivially brute-forced with modern GPUs.
- **SHA-1** — Same problem; fast hashing algorithms are *bad* for passwords because speed helps the attacker, not you.

### Password Hashing Best Practices
- Generate a unique, random salt for every password (most modern libraries do this automatically).
- Use an appropriate **work factor / cost parameter** that balances security and server performance — tune it so hashing takes roughly 200–500ms on your hardware.
- Hash passwords on signup and on every password change.
- Compare hashes using a **constant-time comparison** function to prevent timing attacks.
- Never log passwords, anywhere — not in application logs, error logs, or debugging output.
- If migrating from a legacy system using a weak hash (e.g., MD5), transparently rehash with Argon2id/bcrypt the next time the user logs in successfully.

### 🧠 Prompt for Your AI Coding Agent
```
Implement secure password hashing for this application.

Requirements:
- Use Argon2id (preferred) or bcrypt with a work factor appropriate for
  ~250-400ms hashing time on production hardware. Do not use MD5 or SHA-1.
- Generate a unique salt automatically per password using the library's
  built-in salt generation (do not implement custom salting).
- Hash the password on signup and again whenever the user changes it.
- Use the library's built-in constant-time comparison function when
  verifying a password at login — never compare hashes with `==` or `===`.
- Ensure passwords are never written to application logs, error logs,
  console output, or any debugging/telemetry tool.
- If this project is migrating from a legacy weak-hash system, add a
  rehashing step that detects old hashes and transparently upgrades them
  to Argon2id/bcrypt the next time that user logs in successfully.
```

---

## 4. Use Generic Authentication Error Messages

### The Problem
Many applications unintentionally tell attackers whether a given email address is registered, by returning different error messages for "wrong password" vs. "email not found." This is called **user/account enumeration**, and it's a goldmine for attackers building target lists for phishing or credential-stuffing campaigns.

### Bad vs. Good Examples

| ❌ Bad (Reveals Info) | ✅ Good (Generic) |
|---|---|
| "Email not found" | "Incorrect email or password." |
| "Wrong password" | "Incorrect email or password." |
| "Account doesn't exist" | "Incorrect email or password." |

The same principle applies to password reset flows:

| ❌ Bad | ✅ Good |
|---|---|
| "Email not found" | "If that email is registered, you'll receive a password reset link." |

### Why It Matters
Attackers routinely harvest lists of valid, registered email addresses before launching phishing campaigns or credential-stuffing attacks against other services. Generic responses remove their ability to confirm which addresses are "live."

### 🧠 Prompt for Your AI Coding Agent
```
Audit and fix all authentication-related error messages in this codebase
to prevent user/account enumeration.

Requirements:
- The login endpoint must return the exact same generic message for both
  "email not found" and "wrong password" cases: "Incorrect email or
  password." Do not differentiate the message, the HTTP status code, or
  the response timing between these two cases.
- The password reset endpoint must always respond with: "If that email
  is registered, you'll receive a password reset link." regardless of
  whether the email actually exists in the database.
- The signup endpoint should also avoid leaking whether an email is
  already registered where possible (e.g., consider "Check your inbox to
  complete signup" combined with a verification-email flow instead of
  an immediate "Email already exists" error).
- Make sure response times for "valid email, wrong password" and
  "invalid email" are similar, to prevent timing-based enumeration
  (e.g., perform a dummy hash comparison even when the email doesn't
  exist, so response time doesn't leak which branch executed).
```

---

## 5. Use a Trusted Authentication Provider

### The Problem
Authentication is one of the most security-critical parts of any application, and rolling your own means you are personally responsible for:
- Password hashing
- Session management
- JWT security
- OAuth implementation
- Multi-factor authentication
- Token rotation
- Ongoing security patches
- Regulatory compliance (GDPR, SOC 2, etc.)

This requires deep, continuously-updated security expertise that most teams — especially solo developers and small startups — simply don't have the bandwidth to maintain.

### The Fix
Use a trusted, dedicated authentication provider instead of building your own from scratch. Popular options include:

- **Clerk**
- **Supabase Auth**
- **Firebase Authentication**
- **Auth0**

These services provide out of the box:
- Secure authentication flows
- OAuth integrations (Google, GitHub, Apple, etc.)
- Session management
- Multi-factor authentication (MFA)
- Automatic security updates
- Compliance support
- User management dashboards

### What to Store Yourself
Keep your own database lean — store only application-specific data such as:
- User ID (reference to the auth provider's user record)
- Subscription plan
- Preferences
- Profile settings

**Never store passwords or raw authentication tokens yourself** unless there is an absolutely unavoidable reason to.

### 🧠 Prompt for Your AI Coding Agent
```
Integrate [Clerk / Supabase Auth / Firebase Auth / Auth0 — pick one] as
the authentication provider for this application instead of building
custom auth from scratch.

Requirements:
- Set up the provider's SDK for sign-up, login, logout, and session
  management.
- Enable OAuth login with Google and GitHub in addition to email/password.
- Enable multi-factor authentication (MFA) as an optional security setting
  users can turn on from their profile.
- In our own database, store only application-specific data: the
  provider's user ID, subscription plan, and user preferences. Do not
  store passwords, session tokens, or refresh tokens in our own database.
- Protect all backend routes by verifying the session/JWT issued by the
  auth provider before processing any request.
- Add middleware that redirects unauthenticated users away from
  protected routes/pages.
```

---

## Quick Security Checklist

Before launching your application, verify the following:

- [ ] Server-side validation is enabled for every authentication field.
- [ ] Inputs are sanitized before storage and before rendering.
- [ ] Rate limiting protects the login endpoint.
- [ ] Account lockouts activate after repeated failed attempts.
- [ ] Passwords are hashed using bcrypt, Argon2id, or scrypt.
- [ ] Passwords are never stored in plain text.
- [ ] Authentication error messages are generic.
- [ ] Password reset responses never reveal account existence.
- [ ] A trusted authentication provider handles authentication whenever possible.

---

## Recommended Tools

| Purpose | Recommended Tools |
|---|---|
| Input Validation | Zod, Joi, Pydantic |
| Rate Limiting | express-rate-limit, Redis, Upstash |
| Password Hashing | Argon2id, bcrypt, scrypt |
| Authentication | Clerk, Supabase Auth, Firebase Auth, Auth0 |

---

## Master Prompt (All 5 Rules in One Shot)

If you want your AI coding agent to apply **all five practices at once** to an existing or new project, paste this single combined prompt:

```
Review and harden the authentication system of this application according
to the following five security requirements. Apply all of them together
and explain what you changed in each file:

1. SERVER-SIDE VALIDATION: Validate and sanitize every authentication
   input (email, password, username, free text) on the server using
   [Zod/Joi/Pydantic]. Whitelist acceptable characters. Strip HTML/JS
   from free-text fields. Never rely on client-side validation alone.

2. RATE LIMITING & LOCKOUT: Add rate limiting (max 10 requests/IP/minute)
   to the login endpoint using Redis/Upstash. Lock accounts for 15
   minutes after 5 failed attempts. Add progressive delay on repeated
   failures and trigger CAPTCHA after 3 failures.

3. PASSWORD HASHING: Hash all passwords with Argon2id or bcrypt with an
   appropriate work factor and built-in salting. Never use MD5 or SHA-1.
   Use constant-time comparison for verification. Never log passwords.

4. GENERIC ERROR MESSAGES: Return the identical generic message
   "Incorrect email or password." for both invalid email and wrong
   password cases. Use "If that email is registered, you'll receive a
   password reset link." for password reset, regardless of whether the
   email exists. Equalize response timing to avoid leaking information.

5. TRUSTED AUTH PROVIDER: If this project is not already using a managed
   auth provider, recommend integrating Clerk, Supabase Auth, Firebase
   Auth, or Auth0, and migrate the custom auth logic to it. Store only
   non-sensitive application data (user ID, plan, preferences) in our
   own database.

After making changes, output the updated Quick Security Checklist with
each item marked as done or not yet done, and explain any item that
could not be fully implemented and why.
```

---

## References & Further Reading

These are foundational, well-established resources behind the practices in this guide:

1. **OWASP Top 10 (2021)** — the industry-standard list of the most critical web application security risks, including injection and broken authentication. https://owasp.org/Top10/
2. **OWASP Authentication Cheat Sheet** — detailed best practices for secure login systems. https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html
3. **OWASP Password Storage Cheat Sheet** — authoritative guidance on password hashing algorithms and work factors. https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html
4. **OWASP Input Validation Cheat Sheet** — guidance on whitelisting vs. blacklisting and sanitization strategy. https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html
5. **NIST Special Publication 800-63B** — *Digital Identity Guidelines: Authentication and Lifecycle Management* — the U.S. government's official guidance on credential strength, rate limiting, and authenticator security. https://pages.nist.gov/800-63-3/sp800-63b.html
6. **Password Hashing Competition (PHC)** — the academic competition that selected Argon2 as the recommended modern hashing algorithm (2013–2015). https://www.password-hashing.net/
7. Biryukov, A., Dinu, D., & Khovratovich, D. (2016). *"Argon2: New Generation of Memory-Hard Functions for Password Hashing and Other Applications."* IEEE European Symposium on Security and Privacy (EuroS&P). — the original research paper introducing Argon2.
8. **CWE-307: Improper Restriction of Excessive Authentication Attempts** — MITRE's formal weakness classification underlying the rate-limiting/lockout recommendation. https://cwe.mitre.org/data/definitions/307.html
9. **CWE-204: Observable Response Discrepancy** — MITRE's classification for the user-enumeration problem addressed in Rule 4. https://cwe.mitre.org/data/definitions/204.html
10. **CWE-89 (SQL Injection)** and **CWE-79 (Cross-Site Scripting)** — MITRE classifications for the vulnerability classes mitigated by Rule 1. https://cwe.mitre.org/data/definitions/89.html / https://cwe.mitre.org/data/definitions/79.html

> ⚠️ Note: This guide is meant as a practical starting checklist for vibe-coded AI applications, not a substitute for a professional security audit or penetration test before handling sensitive or regulated user data.

---

### 🎯 Final Thoughts

AI coding tools can generate a working application in minutes, but they cannot guarantee a *secure* implementation unless explicitly instructed to. Authentication should never be an afterthought. A few extra hours spent on validation, password hashing, rate limiting, generic error handling, and a trusted auth provider can prevent data breaches, account takeovers, and costly security incidents down the line.

**Ship fast — but ship securely.**