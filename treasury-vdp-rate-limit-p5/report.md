# CDFI Fund - No Rate Limiting on Password Reset Endpoint

## Summary

The password reset endpoint:

https://amis.cdfifund.gov/secur/forgotpassword.jsp

appears to accept repeated password reset requests without rate limiting, CAPTCHA, or account lockout protections.

## Classification

- VRT: Server Security Misconfiguration > No Rate Limiting on Form > Change Password
- Severity: P5 (Informational/Low)

## Steps to Reproduce

1. Navigate to:

   https://amis.cdfifund.gov/secur/forgotpassword.jsp

2. Submit a password reset request using a valid email address.

3. Repeat the same request multiple times.

4. Observe that the server continues returning HTTP 200 responses and processes each request.

## Observed Behavior

- No CAPTCHA challenge.
- No account lockout.
- No rate limiting.
- No HTTP 429 responses.

## Potential Impact

An attacker could repeatedly trigger password reset emails, causing:

- Email flooding.
- User annoyance.
- Limited denial of service against the victim's mailbox.
- Assistance in user enumeration when combined with other weaknesses.

## Suggested Remediation

- Implement per-IP rate limiting.
- Implement per-account rate limiting.
- Return HTTP 429 when thresholds are exceeded.
- Introduce CAPTCHA after repeated requests.

## Evidence

images