# Password and Account Lockout Policy (GPO)

## Overview

Created a dedicated Group Policy Object to enforce domain-wide password and account lockout standards, rolled it out to multiple department OUs, and verified it worked with a live lockout test.

## Objectives


- Create a standalone GPO for password and lockout enforcement
- Configure password requirements (history, age, length, complexity, encryption)
- Configure account lockout thresholds and reset behavior
- Review GPO delegation and security filtering scope
- Link the GPO to multiple department OUs
- Force policy application and verify the lockout behavior works as configured


## Process

1. Creating a New GPO

Opened Group Policy Management, right-clicked Group Policy Objects, and created a new GPO named Password Policy with no Source Starter GPO — kept separate from the existing Mapped Drives GPO.

![image alt](https://github.com/Hamedadams01/Password-Policy-GPO-/blob/main/Screenshot%202026-06-27%20043547.png?raw=true)

2. Opening the GPO in the Policy Editor

Opened the Password Policy GPO in the Group Policy Management Editor, showing the standard Computer Configuration and User Configuration nodes.

![image alt](https://github.com/Hamedadams01/Password-Policy-GPO-/blob/main/Screenshot%202026-06-27%20043603.png?raw=true)

3. Reviewing Default Password Policy Settings

Navigated to Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Password Policy, where every setting (password history, max/min age, minimum length, complexity, reversible encryption) was showing as Not Defined.
![image alt](https://github.com/Hamedadams01/Password-Policy-GPO-/blob/main/Screenshot%202026-06-27%20043643.png?raw=true
)

4. Configuring Password Requirements

Set Enforce password history to 24 passwords remembered, Maximum password age to 90 days, Minimum password age to 1 day, Minimum password length to 14 characters, enabled Password must meet complexity requirements, and disabled Store passwords using reversible encryption.
![image alt](https://github.com/Hamedadams01/Password-Policy-GPO-/blob/main/Screenshot%202026-06-27%20044839.png?raw=true)

5. Configuring Account Lockout Policy

In the same Account Policies section, set Account lockout duration to 15 minutes, Account lockout threshold to 5 invalid logon attempts, and Reset account lockout counter after to 15 minutes.
![image alt](https://github.com/Hamedadams01/Password-Policy-GPO-/blob/main/Screenshot%202026-06-27%20044718.png?raw=true)

6. Reviewing GPO Delegation

Opened the Password Policy GPO's Delegation tab and reviewed permissions: #Finance_Department, #Marketing, and #Sales_Department with Read access; Authenticated Users with Read access via Security Filtering; and Domain Admins, Enterprise Admins, and SYSTEM with full edit rights.
![image alt](https://github.com/Hamedadams01/Password-Policy-GPO-/blob/main/Screenshot%202026-06-27%20045328.png?raw=true)

7. Confirming Security Filtering Scope

Re-checked the Delegation tab with Authenticated Users selected, confirming its permission was "Read from Security Filtering" — meaning the policy applies to any authenticated user in a linked OU.
![image alt](https://github.com/Hamedadams01/Password-Policy-GPO-/blob/main/Screenshot%202026-06-27%20045446.png?raw=true)

8. Linking the GPO to Finance Department

Linked the Password Policy GPO to the Finance Department OU and confirmed the link: Link Order 1, Enforced: Yes, Link Enabled: Yes, GPO Status: Enabled.
![image alt](https://github.com/Hamedadams01/Password-Policy-GPO-/blob/main/Screenshot%202026-06-27%20052758.png?raw=true)

9. Linking the GPO to Marketing Department

Repeated the process for the Marketing Department OU, selecting Password Policy from the Select GPO dialog alongside the other available GPOs.
![image alt](https://github.com/Hamedadams01/Password-Policy-GPO-/blob/main/Screenshot%202026-06-27%20052834.png?raw=true)

10. Verifying Links Across All Departments

Back on the GPO's Scope tab, confirmed the Links table showed the policy linked to Finance Department, Human Resources (HR), Marketing Department, and Sales Department, with Security Filtering still scoped to Authenticated Users.
![image alt](https://github.com/Hamedadams01/Password-Policy-GPO-/blob/main/Screenshot%202026-06-27%20084717.png?raw=true)

11. Applying the Policy Immediately

From an elevated command prompt on the server, ran gpupdate /force, confirming both "Computer Policy update has completed successfully" and "User Policy update has completed successfully".
![image alt](https://github.com/Hamedadams01/Password-Policy-GPO-/blob/main/Screenshot%202026-06-27%20053724.png?raw=true)

12. Testing the Lockout Policy

On the Windows 11 client, attempted to sign in as Adrian Ross with an incorrect password repeatedly. After repeated failed attempts, the account returned "The referenced account is currently locked out and may not be logged on to" — confirming the lockout threshold triggered exactly as configured.

![image alt](https://github.com/Hamedadams01/Password-Policy-GPO-/blob/main/Screenshot%202026-07-03%20175224.png?raw=true)
![image alt](https://github.com/Hamedadams01/Password-Policy-GPO-/blob/main/Screenshot%202026-07-03%20175231.png?raw=true)
