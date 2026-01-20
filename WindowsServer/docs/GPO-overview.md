# Windows Server Active Directory Lab - GPO Setup

This document describes how I set up Group Policy Objects (GPOs) in my Windows Server lab environment.

---

## 1. Group Policy Objects Created

I created the following GPOs:

| GPO Name        | Applied To | Purpose / Policy |
|-----------------|------------|-----------------|
| **Disable CMD** | NY-IT      | Blocks users from opening Command Prompt for NY-IT group |
| **LocalUserNotAllowed** | NY-IT | Blocks the use of local accounts on domain computers |
| **Disable CMD** | HR     | Blocks users from opening Command Prompt for HR group |



![NY-IT gpo](screenshots/GPO-overview/NY-IT.png)


![HR gpo](screenshots/GPO-overview/HR.png)

- Each GPO was linked to the respective **Organizational Unit (OU)**:
  - `NY-IT` OU → `Disable CMD` & `LocalUserNotAllowed`
  - `HR` OU → `Disable CMD`
- Policies were **tested on computers joined to the domain** to make sure they work correctly.

![Cmd Blocked](screenshots/GPO-overview/CMDnotallowed.png)  

---

## 2. Steps to Create GPO

1. Open **Group Policy Management Console (GPMC)**.
2. Right-click the target OU → **Create a GPO in this domain, and Link it here**.
3. Name the GPO (`Disable CMD`, `LocalUserNotAllowed`).
4. Edit the GPO:
   - For CMD restriction: `User Configuration → Administrative Templates → System → Prevent access to the command prompt`.
   - For local account restriction: `Computer Configuration → Windows Settings → Security Settings → Local Policies → User Rights Assignment`.
5. Make sure the GPO is **linked** to the OU.
6. Run `gpupdate /force` on a client computer to apply the policy immediately.
7. Test the policy by logging in as a user in the OU.

---

## 3. Notes

- This lab was created to **practice creating and applying GPOs**.
- I focused on **blocking CMD and local accounts** to see how policies affect users and computers.
- Linking GPOs to OUs allowed me to control which users and computers the rules apply to.

---

*This lab setup is for learning purposes only and not intended for production.*
