# Group Policy Implementation — Sales Department

Enterprise Active Directory security baseline plus department-scoped Group
Policy and least-privilege administrative delegation, built and verified on a
live Windows Server 2022 domain controller.

## Objectives

This lab was built to demonstrate the ability to:

- Configure domain-wide password and account lockout policies
- Design and scope a department-specific Organizational Unit (OU)
- Create and link Group Policy Objects (GPOs) to that OU without affecting the
  rest of the domain
- Delegate administrative privileges narrowly, using the Delegation of Control
  Wizard, instead of granting broad access

## Environment

| Component | Configuration |
|---|---|
| Operating System | Windows Server 2022 |
| Directory Service | Active Directory Domain Services |
| Policy Management | Group Policy Management Console (GPMC) |
| Department scoped | Sales |
| Administrative model | Delegated, least privilege |

## 1. Domain-wide security policy

Configured on the Default Domain Policy, so these controls apply across the
entire domain rather than being limited to one department.

**Password policy** — minimum password length set to 8 characters.

![Minimum password length policy set to 8 characters](images/gpo-password-policy.png)

**Account lockout policy** — deliberate values rather than defaults:

| Setting | Configuration |
|---|---|
| Lockout threshold | 3 invalid attempts |
| Lockout duration | 10 minutes |
| Reset lockout counter | 10 minutes |
| Administrator account lockout | Enabled |

**Why the Administrator account lockout matters:** Windows exempts the
built-in Administrator account from lockout policy by default. Left as-is,
that leaves the single highest-privilege account in the domain with no
brute-force protection at all. Enabling it explicitly closes that gap.

![Account lockout threshold, duration, and administrator lockout settings](images/gpo-lockout-policy.png)

## 2. Sales Organizational Unit

Created a dedicated **Sales** OU and moved Sales user accounts into it,
establishing a policy boundary so department-specific GPOs could be applied
without affecting users elsewhere in the domain.

## 3. Sales-scoped GPOs

Two GPOs were created and linked specifically to the Sales OU.

**GPO 1 — Restrict Control Panel access.** The "Prohibit access to Control
Panel and PC settings" policy was enabled. Sales staff don't need to change
workstation configuration, so removing that access reduces the chance of
accidental misconfiguration or a user disabling a security-relevant setting.

![Control Panel access restriction policy enabled](images/gpo-control-panel-restriction.png)

**GPO 2 — Screen saver timeout.** Set to 1,500 seconds (25 minutes). Sales
terminals are often left unattended between meetings or on the floor — an
enforced idle timeout limits how long an unattended, logged-in session stays
exposed.

![Screen saver timeout policy enabled at 1500 seconds](images/gpo-screensaver-timeout.png)

## 4. Delegated administrative control

Rather than granting a Sales staff member domain-wide administrative rights,
control over the Sales OU was delegated narrowly using the Delegation of
Control Wizard. The specific task delegated: **create, delete, and manage
user accounts** — scoped to the Sales OU only, not the domain.

![Delegation of Control Wizard scoped to user account management](images/gpo-delegation-wizard.png)

**The principle:** give someone exactly the access their role requires.
Domain Admin would have been far more access than necessary; scoping to the
Sales OU specifically keeps the delegated administrator able to do their job
without touching anything outside it.

## Key concepts this reflects

- **Policy scope** — not every control belongs at the domain level; department
  policy should stay scoped to the department
- **Least privilege** — administrative delegation limited to the exact OU and
  task required, not broader "to be safe" access
- **Defense against credential attacks** — deliberate lockout thresholds
  rather than accepting defaults
- **Unattended-session risk** — automatic screen locking as a basic physical
  security control

## Background

Built as part of IFT 220 (Managing Configuration & Active Directory), B.S.
Information Technology (Cybersecurity focus), Arizona State University.