# Group Policy Implementation — Sales Department

Domain-wide security policy configuration plus department-scoped Group Policy
Objects for a Sales OU, including delegated administrative control — built and
verified on a live Windows Server 2022 domain controller.

## Why this exists

Group Policy is easy to demo with a single toggle in a tutorial. It's a
different skill to actually decide what belongs at the domain level versus
what should be scoped to one department, and to hand off limited administrative
control without giving away the whole domain. This lab covers both.

## Domain-wide security policy

Configured on the Default Domain Policy, affecting every account in the domain:

**Password policy** — minimum password length set to 8 characters.

![Minimum password length policy set to 8 characters](images/gpo-password-policy.png)

**Account lockout policy** — a specific, deliberate set of values rather than
defaults:
- Lockout threshold: 3 invalid logon attempts
- Lockout duration: 10 minutes
- Reset lockout counter after: 10 minutes
- Administrator account lockout: enabled

Enabling lockout on the Administrator account specifically is easy to overlook —
by default Windows exempts it, which leaves the highest-privilege account as the
one account with no brute-force protection. Turning it on closes that gap.

![Account lockout threshold, duration, and administrator lockout settings](images/gpo-lockout-policy.png)

## Sales OU and department-scoped GPOs

Created a dedicated **Sales** OU and moved all Sales user accounts into it, so
department-specific policy could be applied without affecting the rest of the
domain.

Two GPOs were linked to the Sales OU:

**1. Restrict Control Panel access.** The "Prohibit access to Control Panel and
PC settings" policy was enabled, disabling Control Panel and the Settings app
entirely for Sales users. For a role that doesn't need to change system
configuration, this reduces the chance of accidental (or intentional)
misconfiguration on shared sales floor machines.

![Control Panel access restriction policy enabled](images/gpo-control-panel-restriction.png)

**2. Screen saver timeout.** Enabled with a 1500-second (25-minute) idle
timeout, so unattended sales terminals lock automatically rather than staying
open indefinitely.

![Screen saver timeout policy enabled at 1500 seconds](images/gpo-screensaver-timeout.png)

## Delegated administrative control

Rather than granting a non-IT staff member domain-wide administrative rights,
control over the Sales OU specifically was delegated using the Delegation of
Control Wizard. The task delegated was scoped narrowly: **create, delete, and
manage user accounts** — enough for day-to-day Sales account management,
nothing beyond it.

![Delegation of Control Wizard scoped to user account management](images/gpo-delegation-wizard.png)

This is the same principle as least-privilege access applied to administrative
delegation: give someone exactly the rights their role requires, not broader
access "to be safe."

## What this demonstrates

- Distinguishing domain-wide security baseline settings from department-specific
  policy, and applying each at the right scope
- Making a deliberate choice most people miss: enabling lockout on the built-in
  Administrator account
- Configuring GPOs that solve an actual operational problem (unattended
  terminals, unnecessary Control Panel access) rather than arbitrary settings
- Delegating a narrowly scoped administrative task instead of over-provisioning
  access

## Background

Built as part of IFT 220 (Managing Configuration & Active Directory), B.S.
Information Technology (Cybersecurity focus), Arizona State University.
