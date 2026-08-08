# ⚙️ AD Design Automation with PowerShell — GreenEnergy Case

> Scripted Active Directory provisioning: departments, sub-OUs, security groups, and user/group membership — built entirely through PowerShell instead of the AD Users and Computers GUI.

A hands-on Active Directory automation lab built and verified on a **Windows Server 2022 domain controller**. Instead of building the organizational structure by hand through the GUI, the entire structure was scripted end-to-end using the `ActiveDirectory` PowerShell module.

---

## 🎯 Objectives

This lab was designed to demonstrate the ability to:

* Script a multi-level OU structure instead of building it manually through the GUI
* Apply a consistent organizational pattern across multiple departments
* Create security groups and provision users programmatically
* Chain OU creation, group creation, and group membership into a single repeatable script
* Debug distinguished-name path errors when a scripted AD structure doesn't match expectations

---

## 🏢 Environment

| Component | Configuration |
|---|---|
| Operating System | Windows Server 2022 |
| Directory Service | Active Directory Domain Services |
| Tooling | PowerShell ISE, `ActiveDirectory` module |
| Company scenario | GreenEnergy |
| Departments scripted | IT, Engineering, R&D, Business, Legal |

---

## 🧱 1. Scripted OU Structure

The script builds the structure top-down:

1. **Root OU** — `GreenEnergy_OU` at the domain root
2. **Department-level OUs** — IT, Engineering, R&D, Business, Legal
3. **Consistent sub-OU pattern inside every department** — Users, Groups, Computers, Resources, so all five departments follow the same layout instead of five inconsistent, ad-hoc structures

![PowerShell script creating the GreenEnergy OU structure, groups, and user](images/greenenergy-powershell-script.png)

---

## 👥 2. Security Groups and User Provisioning

The script also creates department-scoped security groups (`IT-HelpDesk`, `IT-Engineering`, `IT-R&D`, `IT-Business`, `IT-Legal`), creates a user (`MaryHelpdesk`), and adds that user directly to the `IT-HelpDesk` group — covering the full path from OU creation through group membership in one pass.

---

## ✅ 3. Verified Result

Confirmed directly in Active Directory Users and Computers: `GreenEnergy_OU` at the root, with all five department OUs (Business, Engineering, IT, Legal, RandD) created exactly as scripted.

![GreenEnergy_OU structure verified in Active Directory Users and Computers](images/greenenergy-ou-result.png)

---

## 🔎 A Real Challenge Working Through This

Scripting each individual OU, group, and user was straightforward. **Getting the paths right was not.** Every `New-ADOrganizationalUnit` and `New-ADGroup` call depends on an exact, correctly nested distinguished-name path — for example:

```text
"ou=Users,ou=IT,ou=GreenEnergy_OU,dc=eemile,dc=local"
```

A single wrong OU name or misordered path segment fails silently or creates the object in the wrong location. Verifying every department's path manually against the actual domain structure — rather than assuming the pattern held across all five departments — was the real work here. The scripting syntax itself was the easy part.

---

## 🧠 Key Concepts This Reflects

1. **Automation over repetition** — scripting a structure that would otherwise require dozens of repetitive manual GUI actions.
2. **Consistency at scale** — the same Users/Groups/Computers/Resources pattern applied identically across five departments.
3. **Path-level AD literacy** — understanding distinguished-name structure well enough to debug it when a script targets the wrong location.
4. **End-to-end provisioning** — OU creation, group creation, and actual group membership chained into one script rather than three disconnected steps.

---

## 🎓 Background

Developed while studying Active Directory Management as part of a B.S. in Information Technology, applied to a scripted multi-department AD provisioning scenario.
