# 🖥️ Enterprise Systems Administration

> Hands-on Windows infrastructure portfolio focused on Active Directory, Group Policy, PowerShell automation, and Microsoft Configuration Manager (SCCM).


This repository showcases practical experience designing, securing, automating, and managing Windows enterprise environments.

The work covers the full administration lifecycle. From building an organized Active Directory environment and enforcing security policies to automating configuration, managing endpoints, deploying software, and maintaining systems through centralized patch management.

Although these projects were originally completed as part of university coursework, each implementation is documented and presented around real-world enterprise administration practices, security principles, automation, and operational workflows.

---

## 📂 Contents

### 🔐 Active Directory

- **[BlueWater Company Case](active-directory/bluewater-ad-design.md)** — OU design, naming conventions, security groups, shared directories, and role-based access for a small-business AD environment.
- **[Group Policy Implementation — Sales Department](active-directory/group-policy-implementation.md)** — domain-wide security baseline, department-scoped GPOs, and least-privilege administrative delegation.
- **[AD Design Automation with PowerShell — GreenEnergy Case](active-directory/powershell-ad-automation.md)** — scripting a full department/OU/group structure instead of building it through the GUI.

### 🛠️ SCCM Endpoint Management

- **[Patch Management — Automatic Deployment Rules](sccm-endpoint-management/patch-management.md)** — automated Windows Server patching, update filtering, deployment packages, and maintenance windows.
- **[Software Metering Rule](sccm-endpoint-management/software-metering.md)** — tracking real-world application usage across a device collection.
- **[CMPivot — Live Device Queries](sccm-endpoint-management/cmpivot-live-queries.md)** — real-time device querying for security auditing and troubleshooting.
- **[OS Deployment — Bootable Task Sequence Media](sccm-endpoint-management/os-deployment-bootable-media.md)** — certificate-secured bootable media for offline OS deployment, built as part of a 4-person team project.

---

## 🎓 Background

Developed while studying Active Directory Management and Advanced Systems Configuration Management as part of a B.S. in Information Technology.
