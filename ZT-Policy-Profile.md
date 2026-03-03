## Project: Lab 3 - Zero Trust Policy 

Student Name: Pauline Kabambi

Commit Message: Completed

Due Date: March 2, 2026


1. ZTA Component Definitions

Zero Trust Architecture (ZTA) is a cybersecurity framework that operates on the principle of "never trust, always verify." Its main focus is to safeguard network resources rather than relying on static perimeters. ZTA acknowledges that threats can emerge from both inside and outside the network, requiring continuous authentication and authorization for every user, device, and application. 

There are three essential components of the Zero Trust Architecture: 

i.The Policy Engine (PE) is considered the brain of the ZTA. It does not enforce access directly; instead, it makes decisions based on various factors and the context of the request. The PE verifies multiple security layers, including risk level and location, among others, to establish policies. It then determines whether access should be approved, denied, or require additional verification.

ii. The Policy Administrator (PA) is the "rule setter." Its role is to transform decisions from the PE into actionable instructions for enforcement systems. The PA manages session configurations, like issuing tokens or credentials when access is granted, and ensures that policy rules are properly applied.

iii. The Policy Enforcement Point (PEP) acts as a vigilant "gatekeeper" for protected resources. It implements access decisions set by the PE. While it authoritatively allows or denies connections, the PEP does not create policies; it simply executes the directives to maintain the integrity and security of the resources.


2. Core Principle Application: Use Least Privilege

The principle of "Least Privilege" means that users are granted only the minimum level of access required to perform their job duties, nothing more. This limitation on access is determined by scope, duration, and specific permissions, aiming to reduce the potential impact of insider threats.
At the Golden State Water Treatment Facility, the HR Employee PII Database holds sensitive background check records and certification documentation. Not all HR staff need full administrative access to this data. 

When an HR employee requests access, the Policy Engine (PE) assesses their role and job functions to determine the appropriate permissions. For example: 

- An HR Assistant might only need read-only access to certification status. 
- An HR Manager may need permission to update employee records. 
- A Payroll Specialist may not require access to background checks at all.


The PE checks the user’s role attributes against policy rules. If a role doesn’t require access to certain PII fields, like background investigation results, the PE restricts access accordingly. For instance, if an HR Assistant tries to download the entire database, the PE will deny the request as it exceeds their permissions. 
This approach follows the principle of Least Privilege, minimizing unnecessary exposure to sensitive information and reducing the risk of data misuse or insider threats.

3.  Simplified Policy Table 

<img width="788" height="332" alt="image" src="https://github.com/user-attachments/assets/20580a03-efb0-44f9-943d-9f25edccfd69" />



# If any of these conditions are not met, access is denied by the Policy Enforcement Point based on the Policy Engine’s decision.



