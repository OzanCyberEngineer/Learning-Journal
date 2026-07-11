# Learning Report: Linux Permission Logic and Privilege Escalation Foundations

This report examines the file permission mechanism within the Linux operating system, the calculation of numerical access values, and the analysis of these configurations from a cybersecurity (privilege escalation) perspective.

---

## 🔐 Part 1: Linux Permission Architecture (Understanding Identities)
Linux implements a strict isolation model compared to alternative operating systems. Access control begins directly at the file system level. When a file is created, Linux assigns permissions across 3 distinct scopes:
* **User (u):** The specific individual who owns the file (e.g., your account: okeanos).
* **Group (g):** A collection of users sharing identical operational profiles (e.g., siber_ekip).
* **Others (o):** Every other identity within the local system environment.

Each of these 3 scopes can be configured with 3 foundational permission flags:
* **r (Read):** Permits viewing file contents using tools like cat. (Numerical value: 4)
* **w (Write):** Permits modifying or deleting the file structural data. (Numerical value: 2)
* **x (Execute):** Permits running the file if it contains a binary executable or script payload. (Numerical value: 1)

---

## 🔢 Part 2: The Logic of "755" or "644" Absolute Modes
Security documentation frequently mandates operations like "apply 777 rights" or "set an exploit payload to 755". These parameters represent a basic additive scoring system:
* **7** = $4 \text{ (Read)} + 2 \text{ (Write)} + 1 \text{ (Execute)}$ ➡️ (Full control, absolute access)
* **5** = $4 \text{ (Read)} + 0 + 1 \text{ (Execute)}$ ➡️ (Read and execute capabilities without write access)
* **4** = $4 \text{ (Read)} + 0 + 0$ ➡️ (Strictly read-only access)

When executing permission changes, supplying 3 consecutive digits systematically updates permissions in the order of User | Group | Others.

> **💡 Example - chmod 755 siber.sh:** The file owner (7) retains full management control; group associates (5) and external users (5) can strictly read and execute the script.

---

## 🏴‍☠️ Part 3: Cybersecurity Perspective (Privilege Escalation)
Upon obtaining initial access, an attacker typically operates with low-privileged, standard user constraints (such as okeanos). Standard users are blocked from altering system configurations or wiping forensic event trails. The strategic goal is to pivot to the absolute system authority: Root. This methodology is known as **Privilege Escalation**.

* **The Insecure 777 Risk:** Administrators occasionally over-assign privileges during configuration debugging by deploying `chmod 777`. This allows any local user context to erase critical scripts or inject malicious logic. Attackers actively audit filesystems to isolate objects misconfigured with 777 permissions.

---

## 🧪 Part 4: Terminal Command Representation
Executing `ls -l` (detailed listing) within a directory exposes the permission metadata on the far left column:

```bash
-rwxr-xr-x 1 okeanos siber_ekip 1024 Jul 11 hack_araci.sh
Deconstructing the characters into triplets reveals the access configuration:

rwx ➡️ Owner (okeanos) holds read, write, and execute permissions (7).

r-x ➡️ Group members (siber_ekip) hold read and execute permissions (5).

r-x ➡️ All other system users hold read and execute permissions (5).

To rapidly flag a target file as an executable entity, the following native utility is executed:

Bash
chmod +x dosya_adi.sh
This directly appends execution rights to the target file.
