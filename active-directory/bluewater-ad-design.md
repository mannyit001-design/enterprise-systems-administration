# Active Directory Design — BlueWater Company Case

A small-business Active Directory environment designed and implemented from a
company case study: organizational unit structure, naming conventions, security
groups, shared directories, printer permissions, and logon restrictions — built
and verified on a live Windows Server 2022 domain controller.

## Why this exists

Most AD tutorials show you how to click through the wizards. They don't force you
to make actual naming and structure decisions for a real organization with
real edge cases (duplicate names, department-specific access needs, shared
resources). This project started as a coursework case study but the decisions
below reflect real design tradeoffs, not just "how to create a user in AD."

## The scenario

BlueWater is a small company needing a single, centrally-managed AD environment
covering sales, service technicians, accounting, and store management — with
shared file access, a shared printer, and department-appropriate login
restrictions.

## Design decisions

**Single OU for a small company.** Rather than a deep OU hierarchy, everything
lives under one `BlueWater_OU`. For an organization this size, a single OU keeps
administration simple without sacrificing the ability to apply group policy or
delegate permissions later if the company grows.

![BlueWater OU structure in Active Directory Users and Computers](images/bluewater-ou-structure.png)

**Naming convention: first initial + last name, with a fallback for collisions.**
Accounts follow `flastname` (e.g., Steve Brasil -> `sbrasil`). Two employees with
colliding names — Deb Dugeon and Dave Dugeon — would both resolve to `ddugeon`
under that pattern. The fix: append a numeric suffix to the second account
(`ddugeon`, `ddugeon1`). This is a real naming-convention edge case that a lot of
first-pass AD designs miss until it actually happens.

**Security groups named by function, with a consistent `_SG` suffix.**
`Sales_SG`, `Service_Tech_SG`, `Accountant_SG`, `Store_Manager_SG`, and others —
the suffix makes it immediately clear in any permissions dialog which entries are
access-control groups versus individual accounts.

![Security groups and user accounts in the BlueWater_OU](images/bluewater-security-groups-users.png)

**Shared directories named by audience, with permissions matched to that audience.**
`Public_SHR` is accessible company-wide; `Service_Techs_SHR` and `Accountants_SHR`
are scoped to their respective groups with Read/Write, while broader groups get
Read-only where appropriate. Every user also received a home directory.

![Per-user home directories under BlueWater_SHR](images/bluewater-home-directories.png)

**Logon time restrictions scoped by role.** The Sales security group has logon
hours restricted to 9 AM-9 PM, matching actual working hours — a small detail,
but one that reflects real operational security thinking rather than leaving
every account able to log in 24/7 by default.

**Delegated Domain Admin rights only where actually needed.** Rather than
over-provisioning access, specific individuals (e.g., `sbrasil`, `rjefferson`)
were added to Domain Admins based on an actual need for that level of access,
and Print Operator rights were scoped to a small set of users responsible for
managing the shared printer — not granted broadly.

## What this demonstrates

- Translating a business scenario into an AD structure, not just following a
  checklist
- Recognizing and resolving a real naming collision before it becomes a support
  ticket
- Scoping permissions and logon restrictions to actual roles instead of applying
  one policy to everyone
- Working directly in Active Directory Users and Computers on a live Windows
  Server 2022 domain controller

## Background

Built as a company-case lab for IFT 220 (Managing Configuration & Active
Directory) as part of a B.S. in Information Technology (Cybersecurity focus)
at Arizona State University.