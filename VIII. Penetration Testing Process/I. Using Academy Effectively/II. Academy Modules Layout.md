# Academy Modules Layout

Hack The Box was initially created to give technical professionals a safe place to practice and develop hacking skills and was not ideally suited for beginners starting their IT/Security journeys. Hack The Box began as solely a competitive CTF platform with a mix of machines and challenges, each awarding varying amounts of points depending on the difficulty, to be solved from a "black box" approach, with no walkthrough, guidance, or even hints. As the platform evolved, we saw the need for more beginner-friendly content and a guided learning approach to supplement the competitive side of the platform. With that goal in mind, HTB Academy was born. We aim to provide beginner-friendly content while helping mid-level and advanced practitioners upskill in various areas. We also offer Starting Point on the main HTB platform, which aims to help users become more comfortable attacking individual targets using a guided approach and eventually transitioning to solving boxes independently and even playing the competitive boxes. Each person likely has their personal opinion of HTB, and it may not be for everyone. However, we would like to take the time to explain our point of view as experienced IT specialists from various fields, with many years of combined experience and different journeys from beginners to where we are today.

IT (Information Technology) is a major business function of most organizations that focuses on building, administering, and supporting the computer technology used by organizations to achieve their mission. IT is a term often used to encompass many specialized sub-disciplines like Cybersecurity, Information Security, Software Development, Database Administration, Network Administration, and more. To become "good" in this field requires considerable practice and effort. Cyber security can be a very challenging discipline because it requires the basic knowledge necessary for a typical IT specialist and a much deeper understanding of all areas (networking, Linux and Windows systems administration, scripting, databases, etc.). We don't need to be experts in every single area of IT. However, the more experience and knowledge we have, the easier our job as an IT security specialist or penetration tester will become. We cannot work confidently as penetration testers if we don't have a deep understanding of the technologies we are assessing. For example, a web developer focuses only on developing web applications and websites. This generally requires knowledge of HTML, JavaScript, CSS, SQL, and server-side programming languages, such as PHP. Even if the developer has over ten years of experience in his field, it only takes one mistake for the entire web server to be unusable or for data to be stolen. As an attacker, the trick is to find a way to identify and exploit these errors.

With this in mind, we have laid a foundation for our students because, in our experience, it is hard to know where to start. We have structured and built our learning material so that it may seem difficult at first, but with time you will realize that this is the easiest and most efficient way to teach such complex material efficiently. We want to make the learning process as easy and efficient as possible while emphasizing the core fundamentals and returning to them repeatedly. For example, many of our tasks are set up to get you to think in a certain way. We do this to help you develop the essential analytical skills that are imperative to be successful in a field that can have so much uncertainty. We want to help craft professionals who see things differently and question everything, which ultimately can help deliver more value to clients if you're able to find nuanced issues that other testers miss. We can't teach analytical skills and the ability to dig deeper and "question everything" in one single module or path. This can be compared to playing a musical instrument. We can't learn to play the guitar well without considerable practice. We can learn everything about a guitar, the history of guitars, the name of every component, etc., but if we pick one up without practice, we will not be able to produce music that is equivalent to our knowledge of guitars. This is the same in the field of penetration testing. We may know everything about the history of computers and be able to describe every component, but without deep hands-on experience, we won't be able to perform penetration testing at a high level.

The remainder of this section will explain how we have structured the modules in the way that we did to give you insight into our thought process and teaching philosophy. Our primary focus is creating engaging and empowering training resources that benefit individuals at ALL skill levels.

## Pre-Engagement

The pre-engagement stage is where the main commitments, tasks, scope, limitations, and related agreements are documented in writing. During this stage, contractual documents are drawn up, and essential information is exchanged that is relevant for penetration testers and the client, depending on the type of assessment.

There is only one path we can take from here:

| Path                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Information Gathering | Next, we move towards the Information Gathering stage. Before any target systems can be examined and attacked, we must first identify them. It may well be that the customer will not give us any information about their network and components other than a domain name or just a listing of in-scope IP addresses/network ranges. Therefore, we need to get an overview of the target web application(s) or network before proceeding further. |

At this stage in the process, we should have a strong foundation that can be built through the following fundamental modules:

1. **Learning Process**
   To acquire this knowledge as quickly and as well as possible, we need to know how a human being's Learning Process works and how we can use this knowledge to increase our learning efficiency dramatically.
   Tier 0 | Fundamental | General | 12 Sections+ | 103 hours

In addition, we need fundamental knowledge about the world's most widely used operating systems. This includes Linux and Windows operating systems. Before we attack these systems, we first need to know how they work, so we can then learn how to best exploit them.

2. **Linux Fundamentals**
   Linux is one of the most stable operating systems today, and ubiquitous in corporate networks. Linux Fundamentals is essential so we learn its structure and can take the appropriate steps to achieve our goals.
   Tier 0 | Fundamental | General | 18 Sections+ | 106 hours

3. **Windows Fundamentals**
   On the other hand, Windows is one of the more user-friendly operating systems that most companies find in their IT infrastructure. It is essential to understand Windows Fundamentals to be able to handle the operating system in the best possible way and achieve the desired results.
   Tier 0 | Fundamental | General | 14 Sections+ | 106 hours

All connected systems communicate via different networks, routes, and protocols on the Internet or internal network. To understand how interconnected systems function and communicate, we must work through some theoretical components to understand key functionality and specific terms.

4. **Introduction to Networking**
   Most of the information world is interconnected, and understanding how hosts communicate and find each other on the Internet and within internal networks is another fundamental building block that we must master. Without deep understanding of Networking, we will not be effective in assessing interconnected systems.
   Tier 0 | Fundamental | General | 12 Sections+ | 103 hours

Web applications represent a separate category. We are comfortable using a web browser and browsing websites. But what happens behind the scenes when we interact with a web application? Before attacking web applications, we must focus on how they function and the processes that occur on the backend when using a web application.

5. **Introduction to Web Applications**
   Computer networking on the Internet is standardized over many layers and protocols. The most used type of applications are Web Applications. These are designed so that any user with a browser and internet connection can access the web pages on the Internet.
   Tier 0 | Fundamental | General | 17 Sections+ | 103 hours

6. **Web Requests**
   The communication takes place through different types of Web Requests, which the web application processes with specific functions. We will cover various types of web requests and how web browsers use them in the background. Some web server misconfigurations may even grant us access to the system without having to even exploit a web application directly.
   Tier 0 | Fundamental | General | 8 Sections+ | 104 hours

7. **JavaScript Deobfuscation**
   Most web applications nowadays are dynamic and include JavaScript, which we must also be familiar with to handle the dynamics of the web page correctly. JavaScript is a very popular programming language and is often obfuscated to make it difficult for attackers (and defenders) to understand the exact functionality of the code.
   Tier 0 | Fundamental | Defensive | 11 Sections+ | 104 hours

As we know, large IT networks need to be closely managed and secured. Most companies have a management structure, as managing hundreds or thousands of systems remotely or physically one by one would be unreasonable. For this reason, various technologies exist to facilitate and accelerate remote management of users, systems, and other resources.

8. **Introduction to Active Directory**
   Nowadays, most companies use a structured way of managing hundreds or thousands of computers and users. Active Directory is used to simplify and speed up management for administrators.
   Tier 0 | Fundamental | General | 16 Sections+ | 107 hours

9. **Getting Started**
   What causes the most significant difficulty for most when starting out? The answer to this is much easier than most may imagine because all we have to do is Get Started. This module includes many tips and tricks for those just starting out, examples of what technologies we will see and what attack methods we will use, and a guided walkthrough of solving a vulnerable box, culminating in solving (for some of us) our first box without assistance.
   Tier 0 | Fundamental | Offensive | 23 Sections+ | 108 hours

## Information Gathering

Information gathering is an essential part of any assessment. Because information, the knowledge gained from it, the conclusions we draw, and the steps we take are based on the information available. This information must be obtained from somewhere, so it is critical to know how to retrieve it and best leverage it based on our assessment goals.

From this stage, the next part of our path is clear:

| Path                     | Description                                                                                                                                                                                                                                                                                                                                        |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Vulnerability Assessment | The next stop on our journey is Vulnerability Assessment, where we use the information found to identify potential weaknesses. We can use vulnerability scanners that will scan the target systems for known vulnerabilities and manual analysis where we try to look behind the scenes to discover where the potential vulnerabilities might lie. |

The information we gather in advance will influence the results of the Exploitation stage. From this, we can see if we have collected enough or dived deep enough. Time, patience, and personal commitment all play a significant role in information gathering. This is when many penetration testers tend to jump straight into exploiting a potential vulnerability. This often fails and can lead, among other things, to a significant loss of time. Before attempting to exploit anything, we should have completed thorough information gathering, keeping detailed notes along the way, focusing on things to hone in on once we get to the exploitation stage. Most assessments are time-based, so we don't want to waste time bouncing around, which could lead to us missing something critical. Organization and patience are vital while being as thorough as possible.

10. **Network Enumeration with Nmap**
    Suppose we limit our scope to the corporate network infrastructure. In that case, we should know how to perform the Network Enumeration with Nmap, identify the potential targets, and bypass security measures like firewalls, intrusion prevention, and intrusion detection systems (IPS/IDS).
    Tier I | Easy | Offensive | 12 Sections+ | 107 hours

11. **Footprinting**
    Once we have identified the potential targets, we need to know how the individual services of these hosts can be examined. It is essential to understand what these services are used for, how they can be misconfigured, and how we, as attackers, can exploit them for our purposes. Because every service that communicates via the network leaves its own Footprint that we have to discover, knowing these footprints will give us a more accurate picture of what steps we can take next as we head into the exploitation phase.
    Tier II | Medium | Offensive | 20 Sections+ | 202 days

12. **Information Gathering - Web Edition**
    In most cases, web servers and web applications contain a great deal of information that can be used against them. Since web is a vast technical area in its own right, it will be treated separately. A web server can run many web applications, and some of these applications may be only intended for the developers and administrators. Therefore, finding these is an essential part of our Information Gathering - Web Edition. We also want to discover as many web applications as possible and gather detailed information on their structure and function which will help inform our attacks.
    Tier II | Easy | Offensive | 10 Sections+ | 207 hours

Things can become quite complex when we want to find information about a target company on the Internet. After all, sifting through various sources and social media platforms is time-consuming and requires a great deal of attention and patience.

13. **OSINT: Corporate Recon**
    This type of research is called open-source intelligence (OSINT) and has many subcategories. In summary, this process involves gathering information from all publicly available sources. OSINT: Corporate Recon, gives us a clear and structured approach that will allow us to work through many different types of data and information sources. A simple example would be finding a private SSH key that allows us to log into the corporate network as an administrator.
    Tier IV | Hard | Offensive | 23 Sections+ | 2002 days

## Vulnerability Assessment

The vulnerability assessment stage is divided into two areas. On the one hand, it is an approach to scan for known vulnerabilities using automated tools. On the other hand, it is analyzing for potential vulnerabilities through the information found. Many companies conduct regular vulnerability assessment audits to check their infrastructure for new known vulnerabilities and compare them with the latest entries in these tools' databases.

An analysis is more about thinking outside the box. We try to discover gaps and opportunities to trick the systems and applications to our advantage and gain unintended access or privileges. This requires creativity and a deep technical understanding. We must connect the various information points we obtain and understand its processes.

From this stage, there are four paths we can take, depending on how far we have come:

| Path                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Exploitation          | The first we can jump into is the Exploitation stage. This happens when we do not yet have access to a system or application. Of course, this assumes that we have already identified at least one gap and prepared everything necessary to attempt to exploit it.                                                                                                                                                                                                                                                   |
| Post-Exploitation     | The second way leads to the Post-Exploitation stage, where we escalate privileges on the target system. This assumes that we are already on the target system and can interact with it.                                                                                                                                                                                                                                                                                                                              |
| Lateral Movement      | Our third option is the Lateral Movement stage, where we move from the already exploited system through the network and attack other systems. Again, this assumes that we are already on a target system and can interact with it. However, privilege escalation is not strictly necessary because interacting with the system already allows us to move further in the network under certain circumstances. Other times we will need to escalate privileges before moving laterally. Every assessment is different. |
| Information Gathering | The last option is returning to the Information Gathering stage when we do not have enough information on hand. Here we can dig deeper to find more information that will give us a more accurate view.                                                                                                                                                                                                                                                                                                              |

The ability to analyze comes with time and experience. However, it also needs to be trained because proper analysis makes connections between different points and information. Connecting this information about the target network or target system and our experience will often allow us to recognize specific patterns. We can compare this to reading. Once we have read certain words often enough, we will know that word at some point and understand what it means just by looking at the letters.

14. **Vulnerability Assessment**
    After summarizing the information, we can use automated tools to scan the defined targets to detect known vulnerabilities in the systems. First, however, we need to know the scoring systems and learn how to configure and use these tools efficiently. The Vulnerability Assessment performed by these tools can give us a better overview of the potential vulnerabilities and the configuration of the target system. From this, new paths and opportunities can be revealed to us to help us find another way into the system.
    Tier 0 | Easy | Offensive | 17 Sections+ | 102 hours

15. **File Transfers**
    Before we can efficiently exploit the potential vulnerabilities, we need to be familiar with techniques and methods to transfer the required data to the target systems. This is because manual adjustments are often necessary to circumvent specific restrictions. Knowing the ways and means to perform File Transfers is an essential component that we must master and there are many ways to transfer files both to and from Windows and Linux hosts. If we have found a potential gap and do not know how to transfer the corresponding data to the target system, it will lead us to a dead end.
    Tier 0 | Medium | Offensive | 8 Sections+ | 103 hours

16. **Shells & Payloads**
    We also need to know what files we need to transfer to gain initial or further access to the systems. For this, it is necessary to know what Shell & Payloads are. With the help of the transmitted payloads, we get access to the command line of the target system. Many things have to be taken into consideration because these shells and payloads must be adapted to the environment and the targeted system.
    Tier I | Medium | Offensive | 17 Sections+ | 102 days

17. **Using the Metasploit-Framework**
    In addition, there is a handy framework called Metasploit-Framework that covers many attacks, enumeration, and privilege escalation methods and makes it faster for us to configure and execute. It can help us speed up our processes and get into the target systems in a semi-automated way. However, before we can do this, we need to understand what this tool is capable of and its limitations.
    Tier 0 | Easy | Offensive | 15 Sections+ | 105 hours

## Exploitation

Exploitation is the attack performed against a system or application based on the potential vulnerability discovered during our information gathering and enum
