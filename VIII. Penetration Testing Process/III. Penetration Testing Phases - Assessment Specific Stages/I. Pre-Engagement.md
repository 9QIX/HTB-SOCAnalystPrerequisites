# Pre-Engagement

Pre-engagement is the stage of preparation for the actual penetration test. During this stage, many questions are asked, and some contractual agreements are made. The client informs us about what they want to be tested, and we explain in detail how to make the test as efficient as possible.

The entire pre-engagement process consists of three essential components:

1. Scoping questionnaire
2. Pre-engagement meeting
3. Kick-off meeting

Before any of these can be discussed in detail, a Non-Disclosure Agreement (NDA) must be signed by all parties. There are several types of NDAs:

| Type             | Description                                                                                                                                                                                             |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Unilateral NDA   | This type of NDA obligates only one party to maintain confidentiality and allows the other party to share the information received with third parties.                                                  |
| Bilateral NDA    | In this type, both parties are obligated to keep the resulting and acquired information confidential. This is the most common type of NDA that protects the work of penetration testers.                |
| Multilateral NDA | Multilateral NDA is a commitment to confidentiality by more than two parties. If we conduct a penetration test for a cooperative network, all parties responsible and involved must sign this document. |

Exceptions can also be made in urgent cases, where we jump into the kick-off meeting, which can also occur via an online conference. It is essential to know who in the company is permitted to contract us for a penetration test. Because we cannot accept such an order from everyone. Imagine, for example, that a company employee hires us with the pretext of checking the corporate network's security. However, after we finished the assessment, it turned out that this employee wanted to harm their own company and had no authorization to have the company tested. This would put us in a critical situation from a legal point of view.

Below is a sample (not exhaustive) list of company members who may be authorized to hire us for penetration testing. This can vary from company to company, with larger organizations not involving the C-level staff directly and the responsibility falling on IT, Audit, or IT Security senior management or the like.

- Chief Executive Officer (CEO)
- Chief Technical Officer (CTO)
- Chief Information Security Officer (CISO)
- Chief Security Officer (CSO)
- Chief Risk Officer (CRO)
- Chief Information Officer (CIO)
- VP of Internal Audit
- Audit Manager
- VP or Director of IT/Information Security

It is vital to determine early on in the process who has signatory authority for the contract, Rules of Engagement documents, and who will be the primary and secondary points of contact, technical support, and contact for escalating any issues.

This stage also requires the preparation of several documents before a penetration test can be conducted that must be signed by our client and us so that the declaration of consent can also be presented in written form if required. Otherwise the penetration test could breach the Computer Misuse Act. These documents include, but are not limited to:

| Document                                                       | Timing for Creation                             |
| -------------------------------------------------------------- | ----------------------------------------------- |
| 1. Non-Disclosure Agreement (NDA)                              | After Initial Contact                           |
| 2. Scoping Questionnaire                                       | Before the Pre-Engagement Meeting               |
| 3. Scoping Document                                            | During the Pre-Engagement Meeting               |
| 4. Penetration Testing Proposal (Contract/Scope of Work (SoW)) | During the Pre-engagement Meeting               |
| 5. Rules of Engagement (RoE)                                   | Before the Kick-Off Meeting                     |
| 6. Contractors Agreement (Physical Assessments)                | Before the Kick-Off Meeting                     |
| 7. Reports                                                     | During and after the conducted Penetration Test |

Note: Our client may provide a separate scoping document listing in-scope IP addresses/ranges/URLs and any necessary credentials but this information should also be documented as an appendix in the RoE document.

**Important Note:**

These documents should be reviewed and adapted by a lawyer after they have been prepared.

## Scoping Questionnaire

After initial contact is made with the client, we typically send them a Scoping Questionnaire to better understand the services they are seeking. This scoping questionnaire should clearly explain our services and may typically ask them to choose one or more from the following list:

- ☐ Internal Vulnerability Assessment
- ☐ External Vulnerability Assessment
- ☐ Internal Penetration Test
- ☐ External Penetration Test
- ☐ Wireless Security Assessment
- ☐ Application Security Assessment
- ☐ Physical Security Assessment
- ☐ Social Engineering Assessment
- ☐ Red Team Assessment
- ☐ Web Application Security Assessment

Under each of these, the questionnaire should allow the client to be more specific about the required assessment. Do they need a web application or mobile application assessment? Secure code review? Should the Internal Penetration Test be black box and semi-evasive? Do they want just a phishing assessment as part of the Social Engineering Assessment or also vishing calls? This is our chance to explain the depth and breadth of our services, ensure that we understand our client's needs and expectations, and ensure that we can adequately deliver the assessment they require.

Aside from the assessment type, client name, address, and key personnel contact information, some other critical pieces of information include:

- How many expected live hosts?
- How many IPs/CIDR ranges in scope?
- How many Domains/Subdomains are in scope?
- How many wireless SSIDs in scope?
- How many web/mobile applications? If testing is authenticated, how many roles (standard user, admin, etc.)?
- For a phishing assessment, how many users will be targeted? Will the client provide a list, or we will be required to gather this list via OSINT?
- If the client is requesting a Physical Assessment, how many locations? If multiple sites are in-scope, are they geographically dispersed?
- What is the objective of the Red Team Assessment? Are any activities (such as phishing or physical security attacks) out of scope?
- Is a separate Active Directory Security Assessment desired?
- Will network testing be conducted from an anonymous user on the network or a standard domain user?
- Do we need to bypass Network Access Control (NAC)?

Finally, we will want to ask about information disclosure and evasiveness (if applicable to the assessment type):

- Is the Penetration Test black box (no information provided), grey box (only IP address/CIDR ranges/URLs provided), white box (detailed information provided)
- Would they like us to test from a non-evasive, hybrid-evasive (start quiet and gradually become "louder" to assess at what level the client's security personnel detect our activities), or fully evasive.

This information will help us ensure we assign the right resources and deliver the engagement based on the client's expectations. This information is also necessary for providing an accurate proposal with a project timeline (for example, a Vulnerability Assessment will take considerably less time than a Red Team Assessment) and cost (an External Penetration Test against 10 IPs will cost significantly less than an Internal Penetration Test with 30 /24 networks in-scope).

Based on the information we received from the scoping questionnaire, we create an overview and summarize all information in the Scoping Document.

## Pre-Engagement Meeting

Once we have an initial idea of the client's project requirements, we can move on to the pre-engagement meeting. This meeting discusses all relevant and essential components with the customer before the penetration test, explaining them to our customer. The information we gather during this phase, along with the data collected from the scoping questionnaire, will serve as inputs to the Penetration Testing Proposal, also known as the Contract or Scope of Work (SoW). We can think of the whole process as a visit to the doctor to inform ourselves regarding the planned examinations. This phase typically occurs via e-mail and during an online conference call or in-person meeting.

Note: We may encounter clients during our career that are undergoing their first ever penetration test, or the direct client PoC is not familar with the process. It is not uncommon to use part of the pre-engagement meeting to review the scoping questionnaire either in part or step-by-step.

### Contract - Checklist

| Checkpoint                         | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ☐ NDA                              | Non-Disclosure Agreement (NDA) refers to a secrecy contract between the client and the contractor regarding all written or verbal information concerning an order/project. The contractor agrees to treat all confidential information brought to its attention as strictly confidential, even after the order/project is completed. Furthermore, any exceptions to confidentiality, the transferability of rights and obligations, and contractual penalties shall be stipulated in the agreement. The NDA should be signed before the kick-off meeting or at the latest during the meeting before any information is discussed in detail. |
| ☐ Goals                            | Goals are milestones that must be achieved during the order/project. In this process, goal setting is started with the significant goals and continued with fine-grained and small ones.                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| ☐ Scope                            | The individual components to be tested are discussed and defined. These may include domains, IP ranges,                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ☐ Scope                            | The individual components to be tested are discussed and defined. These may include domains, IP ranges, individual hosts, specific accounts, security systems, etc. Our customers may expect us to find out one or the other point by ourselves. However, the legal basis for testing the individual components has the highest priority here.                                                                                                                                                                                                                                                                                              |
| ☐ Penetration Testing Type         | When choosing the type of penetration test, we present the individual options and explain the advantages and disadvantages. Since we already know the goals and scope of our customers, we can and should also make a recommendation on what we advise and justify our recommendation accordingly. Which type is used in the end is the client's decision.                                                                                                                                                                                                                                                                                  |
| ☐ Methodologies                    | Examples: OSSTMM, OWASP, automated and manual unauthenticated analysis of the internal and external network components, vulnerability assessments of network components and web applications, vulnerability threat vectorization, verification and exploitation, and exploit development to facilitate evasion techniques.                                                                                                                                                                                                                                                                                                                  |
| ☐ Penetration Testing Locations    | External: Remote (via secure VPN) and/or Internal: Internal or Remote (via secure VPN)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ☐ Time Estimation                  | For the time estimation, we need the start and the end date for the penetration test. This gives us a precise time window to perform the test and helps us plan our procedure. It is also vital to explicitly ask how time windows the individual attacks (Exploitation / Post-Exploitation / Lateral Movement) are to be carried out. These can be carried out during or outside regular working hours. When testing outside regular working hours, the focus is more on the security solutions and systems that should withstand our attacks.                                                                                             |
| ☐ Third Parties                    | For the third parties, it must be determined via which third-party providers our customer obtains services. These can be cloud providers, ISPs, and other hosting providers. Our client must obtain written consent from these providers describing that they agree and are aware that certain parts of their service will be subject to a simulated hacking attack. It is also highly advisable to require the contractor to forward the third-party permission sent to us so that we have actual confirmation that this permission has indeed been obtained.                                                                              |
| ☐ Evasive Testing                  | Evasive testing is the test of evading and passing security traffic and security systems in the customer's infrastructure. We look for techniques that allow us to find out information about the internal components and attack them. It depends on whether our contractor wants us to use such techniques or not.                                                                                                                                                                                                                                                                                                                         |
| ☐ Risks                            | We must also inform our client about the risks involved in the tests and the possible consequences. Based on the risks and their potential severity, we can then set the limitations together and take certain precautions.                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ☐ Scope Limitations & Restrictions | It is also essential to determine which servers, workstations, or other network components are essential for the client's proper functioning and its customers. We will have to avoid these and must not influence them any further, as this could lead to critical technical errors that could also affect our client's customers in production.                                                                                                                                                                                                                                                                                           |
| ☐ Information Handling             | HIPAA, PCI, HITRUST, FISMA/NIST, etc.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ☐ Contact Information              | For the contact information, we need to create a list of each person's name, title, job title, e-mail address, phone number, office phone number, and an escalation priority order.                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ☐ Lines of Communication           | It should also be documented which communication channels are used to exchange information between the customer and us. This may involve e-mail correspondence, telephone calls, or personal meetings.                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ☐ Reporting                        | Apart from the report's structure, any customer-specific requirements the report should contain are also discussed. In addition, we clarify how the reporting is to take place and whether a presentation of the results is desired.                                                                                                                                                                                                                                                                                                                                                                                                        |
| ☐ Payment Terms                    | Finally, prices and the terms of payment are explained.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

The most crucial element of this meeting is the detailed presentation of the penetration test to our client and its focus. As we already know, each piece of infrastructure is unique for the most part, and each client has particular preferences on which they place the most importance. Finding out these priorities is an essential part of this meeting.

We can think of it as ordering in a restaurant. If we want a medium-rare steak and the chef gives us a well-done steak because he believes it is better, it will not be what we were hoping for. Therefore, we should prioritize our client's wishes and serve the steak as they ordered.

Based on the Contract Checklist and the input information shared in scoping, the Penetration Testing Proposal (Contract) and the associated Rules of Engagement (RoE) are created.

## Rules of Engagement - Checklist

| Checkpoint                                | Contents                                                                                              |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| ☐ Introduction                            | Description of this document.                                                                         |
| ☐ Contractor                              | Company name, contractor full name, job title.                                                        |
| ☐ Penetration Testers                     | Company name, pentesters full name.                                                                   |
| ☐ Contact Information                     | Mailing addresses, e-mail addresses, and phone numbers of all client parties and penetration testers. |
| ☐ Purpose                                 | Description of the purpose for the conducted penetration test.                                        |
| ☐ Goals                                   | Description of the goals that should be achieved with the penetration test.                           |
| ☐ Scope                                   | All IPs, domain names, URLs, or CIDR ranges.                                                          |
| ☐ Lines of Communication                  | Online conferences or phone calls or face-to-face meetings, or via e-mail.                            |
| ☐ Time Estimation                         | Start and end dates.                                                                                  |
| ☐ Time of the Day to Test                 | Times of the day to test.                                                                             |
| ☐ Penetration Testing Type                | External/Internal Penetration Test/Vulnerability Assessments/Social Engineering.                      |
| ☐ Penetration Testing Locations           | Description of how the connection to the client network is established.                               |
| ☐ Methodologies                           | OSSTMM, PTES, OWASP, and others.                                                                      |
| ☐ Objectives / Flags                      | Users, specific files, specific information, and others.                                              |
| ☐ Evidence Handling                       | Encryption, secure protocols                                                                          |
| ☐ System Backups                          | Configuration files, databases, and others.                                                           |
| ☐ Information Handling                    | Strong data encryption                                                                                |
| ☐ Incident Handling and Reporting         | Cases for contact, pentest interruptions, type of reports                                             |
| ☐ Status Meetings                         | Frequency of meetings, dates, times, included parties                                                 |
| ☐ Reporting                               | Type, target readers, focus                                                                           |
| ☐ Retesting                               | Start and end dates                                                                                   |
| ☐ Disclaimers and Limitation of Liability | System damage, data loss                                                                              |
| ☐ Permission to Test                      | Signed contract, contractors agreement                                                                |

## Kick-Off Meeting

The kick-off meeting usually occurs at a scheduled time and in-person after signing all contractual documents. This meeting usually includes client POC(s) (from Internal Audit, Information Security, IT, Governance & Risk, etc., depending on the client), client technical support staff (developers, sysadmins, network engineers, etc.), and the penetration testing team (someone in a management role (such as the Practice Lead), the actual penetration tester(s), and sometimes a Project Manager or even the Sales Account Executive or similar). We will go over the nature of the penetration test and how it will take place. Usually, there is no Denial of Service (DoS) testing. We also explain that if a critical vulnerability is identified, penetration testing activities will be paused, a vulnerability notification report will be generated, and the emergency contacts will be contacted. Typically these are only generated during External Penetration Tests for critical flaws such as unauthenticated remote code execution (RCE), SQL injection, or another flaw that leads to sensitive data disclosure. The purpose of this notification is to allow the client to assess the risk internally and determine if the issue warrants an emergency fix. We would typically only stop an Internal Penetration Test and alert the client if a system becomes unresponsive, we find evidence of illegal activity (such as illegal content on a file share) or the presence of an external threat actor in the network or a prior breach.

We must also inform our customers about potential risks during a penetration test. For example, we should mention that a penetration test can leave many log entries and alarms in their security applications. In addition, if brute forcing or any similar attack is used, it is also worth mentioning that we may accidentally lock some users found during the penetration test. We also must inform our customers that they must contact us immediately if the penetration test performed negatively impacts their network.

Explaining the penetration testing process gives everyone involved a clear idea of our entire process. This demonstrates our professional approach and convinces our questioners that we know what we are doing. Because apart from the technical staff, CTO, and CISO, it will sound like a certain kind of magic that is very difficult for non-technical professionals to understand. So we must be mindful of our audience and target the most technically inexperienced questioner so our approach can be followed by everyone we talk to.

All points related to testing need to be discussed and clarified. It is crucial to respond precisely to the wishes and expectations of the customer/client. Every company structure and network is different and requires an adapted approach. Each client has different goals, and we should adjust our testing to their wishes. We can typically see how experienced our clients are in undergoing penetration tests early in the call, so we may have to shift our focus to explain things in more detail and be prepared to field more questions, or the kickoff call may be very quick and straightforward.

## Contractors Agreement

If the penetration test also includes physical testing, then an additional contractor's agreement is required. Since it is not only a virtual environment but also a physical intrusion, completely different laws apply here. It is also possible that many of the employees have not been informed about the test. Suppose we encounter employees with a very high-security awareness during the physical attack and social engineering attempts, and we get caught. In that case, the employees will, in most cases, contact the police. This additional contractor's agreement is our "get out of jail free card" in this case.

### Contractors Agreement - Checklist for Physical Assessments

Checkpoint

| Category              |
| --------------------- |
| ☐ Introduction        |
| ☐ Contractor          |
| ☐ Purpose             |
| ☐ Goal                |
| ☐ Penetration Testers |
| ☐ Contact Information |
| ☐ Physical Addresses  |
| ☐ Building Name       |
| ☐ Floors              |
| ☐ Physical Room IDs   |
| ☐ Physical Components |
| ☐ Timeline            |
| ☐ Notarization        |
| ☐ Permission to Test  |

## Setting Up

After all the above points have been worked through, and we have the necessary information, we plan our approach and prepare everything. We will find that the penetration test results are still unknown, but we can prepare our VMs, VPS, and other tools/systems for all scenarios and situations. More information and how to prepare these systems can be found in the Setting Up module.
