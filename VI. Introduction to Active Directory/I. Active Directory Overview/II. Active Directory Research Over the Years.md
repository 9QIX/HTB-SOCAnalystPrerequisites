## Active Directory Research Over the Years

Active Directory has been a major area of focus for security researchers for the past decade or so. Starting in 2014, we began to see many tools and much research emerge, leading to the discovery of common attacks and techniques still used today. Below is a timeline of events highlighting the discovery of some of the most impactful attacks and flaws by incredible researchers, as well as the release of widely used tools by penetration testers to this day. While many techniques have been discovered over the years, AD (and now Azure AD) presents a vast attack surface, and new attacks are still being discovered. New tools emerge that both penetration testers and defenders must have a firm grasp of to assist organizations with the difficult but critical task of securing AD environments.

---

### AD Attacks & Tools Timeline

| **2021**                                                                                                                                                                                                                                                               |
| :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| - The PrintNightmare vulnerability was released. This was a remote code execution flaw in the Windows Print Spooler that could be used to take over hosts in an AD environment.                                                                                        |
| - The Shadow Credentials attack was released which allows for low privileged users to impersonate other user and computer accounts if conditions are right, and can be used to escalate privileges in a domain.                                                        |
| - The noPac attack was released in mid-December of 2021 when much of the security world was focused on the Log4j vulnerabilities. This attack allows an attacker to gain full control over a domain from a standard domain user account if the right conditions exist. |

| **2020**                                                                                                                                                    |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| - The ZeroLogon attack debuted late in 2020. This was a critical flaw that allowed an attacker to impersonate any unpatched domain controller in a network. |

| **2019**                                                                                                                                                        |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| - harmj0y delivered the talk "Kerberoasting Revisited" at DerbyCon which laid out new approaches to Kerberoasting.                                              |
| - Elad Shamir released a blog post outlining techniques for abusing resource-based constrained delegation (RBCD) in Active Directory.                           |
| - BC Security released Empire 3.0 (now version 4) which was a re-release of the PowerShell Empire framework written in Python3 with many additions and changes. |

| **2018**                                                                                                                                                                                                                                                                                         |
| :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| - The "Printer Bug" bug was discovered by Lee Christensen and the SpoolSample PoC tool was released which leverages this bug to coerce Windows hosts to authenticate to other machines via the MS-RPRN RPC interface.                                                                            |
| - harmj0y released the Rubeus toolkit for attacking Kerberos.                                                                                                                                                                                                                                    |
| - harmj0y also released the blog "Not A Security Boundary: Breaking Forest Trusts" which presented key research on performing attacks across forest trusts.                                                                                                                                      |
| - The DCShadow attack technique was also released by Vincent LE TOUX and Benjamin Delpy at the Bluehat IL 2018 conference.                                                                                                                                                                       |
| - The Ping Castle tool was released by Vincent LE TOUX for performing security audits of Active Directory by looking for misconfigurations and other flaws that can raise the risk level of a domain and producing a report that can be used to identify ways to further harden the environment. |

| **2017**                                                                                                                       |
| :----------------------------------------------------------------------------------------------------------------------------- |
| - The ASREPRoast technique was introduced for attacking user accounts that don't require Kerberos preauthentication.           |
| - \_wald0 and harmj0y delivered the pivotal talk on Active Directory ACL attacks "ACE Up the Sleeve" at Black Hat and DEF CON. |
| - harmj0y released his "A Guide to Attacking Domain Trusts" blog post on enumerating and attacking domain trusts.              |

| **2016**                                                                                            |
| :-------------------------------------------------------------------------------------------------- |
| - BloodHound was released as a game-changing tool for visualizing attack paths in AD at DEF CON 24. |

| **2015**                                                                                                                                             |
| :--------------------------------------------------------------------------------------------------------------------------------------------------- |
| - The PowerShell Empire framework was released.                                                                                                      |
| - PowerView 2.0 released as part of the (now deprecated) PowerTools repository.                                                                      |
| - The DCSync attack was first released by Benjamin Delpy and Vincent Le Toux as part of the mimikatz tool.                                           |
| - The first stable release of CrackMapExec (v1.0.0) was introduced.                                                                                  |
| - Sean Metcalf gave a talk at Black Hat USA about the dangers of Kerberos Unconstrained Delegation and released an excellent blog post on the topic. |
| - The Impacket toolkit was also released.                                                                                                            |

| **2014**                                                                           |
| :--------------------------------------------------------------------------------- |
| - Veil-PowerView was first released.                                               |
| - The Kerberoasting attack was first presented by Tim Medin at SANS Hackfest 2014. |

| **2013**                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| - The Responder tool was released by Laurent Gaffie. Responder is a tool used for poisoning LLMNR, NBT-NS, and MDNS on an Active Directory network. It can be used to obtain password hashes and also perform SMB Relay attacks (when combined with other tools) to move laterally and vertically in an AD environment. It has evolved considerably over the years and is still actively supported (with new features added) as of January 2022. |

---
