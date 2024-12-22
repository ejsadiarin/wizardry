---
tags: []
date: 2024-08-05T21:11
---
<!-- 2024-08-05 (August 5, 2024 9:11 PM Monday) -->

#  


**Strategic Plane**

High level you need to think how do I manage lifecycle of identities and access approval for employees, service accounts/service principals, applications, endpoints, external identities, (computers, IOT, servers, etc.) while ensuring least privilege principle, in a centralized way and how do I monitor all of that effectively.

This is driven by the fact that compromized identities now are the most common attack vector during cyber security incidents, especially legacy identities used for legacy applications.

From my perspective, the north star for modern companies should be centralized identity management tool that  helps in managing all of that and provides single pane of glass view on your identity estate. There are many options, some to mention are Saviynt, Octa, etc.

This is easier said than done, while buying a tool would be extremely effective on greenfield environment, but the key is taking control of your legacy estate. You need to think how to integrate all you already have into it.

**Active Directory / Entra ID / External Identities**

Active Directory on-prem should be migrated to one global forest, instead of having lots of regional forests. Additionally, you would benefit from implementing [Enterprise Tiering Model](https://learn.microsoft.com/en-us/security/privileged-access-workstations/privileged-access-access-model), which would limit the blast radius for high privileged identities like service accounts for your monitoring solution, EDR, etc. You should plan computer accounts and service accounts migration strategy, starting from critical applications.

Secure that your domain controllers are backed up, and also there's immutable backup option as well. Domain controllers deployed in different regions generally have self-replication, so if one is corrupted, all others get corrupted (see not petya case for Maersk).

Active Directory on-prem needs to be synched with your Entra ID (Azure Active Directory), Active Directory DS needs to be on to enable legacy features for legacy applications.

The key is to handle as much as you can in Entra ID (Azure Active Directory), and slowly migrate there, enabling conditional access where possible.

External identities (your Vendors) should be done via Azure B2B and Azure Lighthouse, so when Vendor access is not required anymore, you just cut Lighthouse connection.

External identities (customer website) should be done via Azure B2C to minimize management overhead and let Microsoft handle that for you.

**Service Accounts / SPNs / GMSA**

Microsoft fairly recently introduced gMSA accounts, which should replace service accounts used for high privileged applications. Key benefit is password rotation, and password cannot be viewed in Active Directory. High privileged service accounts should be migrated to gMSA (for supported applications), and if legacy applications do not support it, then at least to turn off interactive logon and implement password rotation strategy.

SPNs should be leveraged for your Azure estate where possible with least privileged principle

**GPOs to Azure Policies**

Hardening is one of the key initiatives for all endpoints, and generally hardening is implement via GPOs. Managing GPOs at scale is challenging, while they are based on groups, nested groups, etc. On enterprise scale it becomes a mess fast. Consider moving to Azure Policies to harden your global forest, Entra ID identities, Windows/Linux Golden Images, etc.

**Converging all IAM tooling into Centralized Managemement Platform**

Essentially, you would need to handle your joiner-mover-leaver process ideally via centralized platform, where HR is initiating onboarding process of employees, and assigns a role (app developer), which then grants all required accesses. When HR offboards a person, all accesses are deleted automatically, and were not possible (external systems, legacy systems), then tickets are raised to IT to carry out these activities.

Application access also needs to be configured and integrated into the process.

HR plane can be in Success Factors, while IT plane can be in AD/AAD/CyberArk/Third Party Systems, but the common plane is Octa/Saviynt.

This, essentially, solves the issue of managing identity, but your organization needs to have a solid roadmap to implement this strategy, and support of the business as well.

https://www.reddit.com/r/sre/s/enFHP8KgB0
