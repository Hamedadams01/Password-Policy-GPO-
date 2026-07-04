# Password-Policy-GPO

# Overview
This project covers building a dedicated Group Policy Object to enforce password complexity and account lockout standards across the domain, then rolling it out department by department and confirming enforcement worked as expected.
# Objectives

- Create a dedicated GPO for password and account lockout policy
- Configure strong password requirements and lockout thresholds
- Verify permissions/delegation on the GPO before linking it
- Link the GPO across all department OUs
- Confirm the policy actually enforces lockouts on a client

# Step-by-Step

- I opened Group Policy Management, right-clicked Group Policy Objects, and created a new GPO named "Password Policy" with no Source Starter GPO.
- I opened the Password Policy GPO in the Group Policy Management Editor to navigate to Computer Configuration and User Configuration.
- I navigated to Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Password Policy, where every setting showed as "Not Defined."
- I set Enforce password history to 24 passwords remembered, Maximum password age to 90 days, Minimum password age to 1 day, Minimum password length to 14 characters, enabled "Password must meet complexity requirements," and disabled "Store passwords using reversible encryption."
- I configured Account Lockout Policy: lockout duration 15 minutes, lockout threshold 5 invalid attempts, and reset counter after 15 minutes.
- I opened the GPO's Delegation tab and reviewed permissions, confirming department groups had Read access and Domain Admins/Enterprise Admins/SYSTEM had full edit rights.
- I reviewed the Delegation tab again with Authenticated Users selected, confirming its permission was "Read from Security Filtering" so the policy would actually apply.
- I linked the Password Policy GPO to the Finance Department OU first and confirmed Link Order 1, Enforced Yes, Link Enabled Yes, GPO Status Enabled.
- I repeated the link process for the Marketing Department OU, selecting Password Policy from the Select GPO dialog.
- I confirmed on the GPO's Scope tab that the Links table showed the policy linked to Finance, HR, Marketing, and Sales Department OUs, with Security Filtering scoped to Authenticated Users.
- I ran gpupdate /force from an elevated prompt on the server to push the policy immediately.
- I tested the lockout by signing in as Adrian Ross on the Windows 11 client and repeatedly entering an incorrect password.
- I confirmed the account was locked out, receiving the message "The referenced account is currently locked out and may not be logged on to."

# Conclusion

- A dedicated Password Policy GPO was created with strong complexity, history, and lockout settings
- The policy was successfully linked across all department OUs (Finance, HR, Marketing, Sales)
- Delegation and security filtering were verified to ensure proper enforcement
- Real-world lockout behavior was tested and confirmed working on a domain client
