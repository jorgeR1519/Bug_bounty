# U.S. Department of the Treasury VDP - VPN Login Rate Limiting (P5)

## Summary

The SSL VPN portal login endpoint did not implement any protection against repeated authentication attempts.

Multiple consecutive login requests could be sent without triggering rate limiting, CAPTCHA challenges, temporary account lockouts, or progressive delays.

## Program

U.S. Department of the Treasury - Vulnerability Disclosure Program

## Severity

P5 (Informational)

## Target

```
bucvpn.occ.gov
```

Endpoint:

```
/+webvpn+/index.html
```

## Vulnerability Type

Authentication Weakness / Missing Rate Limiting

## Description

The SSL VPN portal allows unlimited authentication attempts.

Ten consecutive POST requests using invalid credentials were sent with a 0.5-second interval.

Every request returned:

- HTTP 200 OK
- Redirect to `logon.html?reason=1`
- No HTTP 429 responses
- No HTTP 403 responses
- No account lockout
- No CAPTCHA challenge

## Steps to Reproduce

1. Obtain a valid CSRF token from the login page.
2. Capture the authentication request.
3. Send the same POST request 10 consecutive times.
4. Observe that every response returns HTTP 200 OK.

## Security Impact

An attacker could automate password guessing attacks against VPN accounts without being blocked.

Although additional protections may exist elsewhere, the absence of rate limiting increases brute-force attack exposure.

## Remediation

Implement one or more of the following protections:

- Rate limiting
- Temporary account lockout
- Progressive delays
- CAPTCHA
- IP reputation controls

## Lessons Learned

- Always test authentication endpoints.
- VPN portals are often overlooked attack surfaces.
- Verify whether protections exist server-side instead of assuming they are enabled.

## Disclosure Status

Accepted by the U.S. Department of the Treasury VDP.

Classification: P5 (Informational)

Date: June 2026