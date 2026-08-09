# 💿 OS Deployment — Bootable Task Sequence Media

> Creating certificate-secured bootable media for offline OS deployment with SCCM.

A hands-on SCCM lab building bootable Task Sequence media for offline Windows Server deployment, built and verified in a live Configuration Manager console.

---

## 🎯 Objective

Build bootable media for a Fresh Install Task Sequence, allowing that OS deployment to run on a target machine without requiring an existing network-based PXE boot — useful in environments where offline or removable-media deployment is the only practical option.

---

## 🛠️ What I Built

Using the SCCM console, I navigated to the Task Sequences folder and used **Create Task Sequence Media → Bootable Media** to generate a bootable ISO tied to the Fresh Install sequence.

**Key configuration decisions:**

* **Saved the ISO to a local path first**, rather than directly to the network share. Generating media directly onto network storage caused MECM to crash during creation — writing locally first and then moving the completed file to the shared network folder avoided that failure entirely.
* **Attached the Boot Media Certificate** from the designated certificate share, using the provided password, so the bootable media would be trusted during deployment rather than rejected by the target system's boot security checks.
* **Selected the Fresh Install Task Sequence** as the sequence the media would execute on boot.

After completing the wizard, the bootable media was confirmed successfully created and the ISO was moved to the designated network share.

---

## 🔎 A Real Problem I Solved

The first attempt at generating the bootable ISO directly to the network path (`\\asu-sccm\course`$...`) caused the SCCM console to crash partway through. Rather than repeating the same failing approach, I generated the ISO to a **local path** instead, let the wizard complete successfully, and then moved the finished file to the network share afterward. This is a small but real example of isolating *where* a failure happens (network write, not the media generation itself) instead of just retrying the same action and hoping for a different result.

---

## 🧠 Key Concepts This Reflects

1. **Certificate-based trust in deployment media** — bootable media isn't just a bootable file; it has to be signed with a trusted certificate or the target system won't accept it.
2. **Isolating the actual point of failure** — recognizing that the network write, not the media generation process itself, was the source of the crash, and working around it rather than repeating the same failing step.
3. **Enterprise OS deployment workflows** — building and configuring bootable deployment media that can be used to initiate a standardized Windows installation without relying on PXE boot.

---

## 🎓 Background

Developed while studying Advanced Systems Configuration Management as part of a B.S. in Information Technology.
