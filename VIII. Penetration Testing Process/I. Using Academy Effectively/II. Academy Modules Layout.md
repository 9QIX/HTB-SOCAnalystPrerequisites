# Academy Modules Layout

Hack The Box was initially created to give technical professionals a safe place to practice and develop hacking skills and was not ideally suited for beginners starting their IT/Security journeys. Hack The Box began as solely a competitive CTF platform with a mix of machines and challenges, each awarding varying amounts of points depending on the difficulty, to be solved from a "black box" approach, with no walkthrough, guidance, or even hints. As the platform evolved, we saw the need for more beginner-friendly content and a guided learning approach to supplement the competitive side of the platform. With that goal in mind, HTB Academy was born. We aim to provide beginner-friendly content while helping mid-level and advanced practitioners upskill in various areas. We also offer Starting Point on the main HTB platform, which aims to help users become more comfortable attacking individual targets using a guided approach and eventually transitioning to solving boxes independently and even playing the competitive boxes. Each person likely has their personal opinion of HTB, and it may not be for everyone. However, we would like to take the time to explain our point of view as experienced IT specialists from various fields, with many years of combined experience and different journeys from beginners to where we are today.

IT (Information Technology) is a major business function of most organizations that focuses on building, administering, and supporting the computer technology used by organizations to achieve their mission. IT is a term often used to encompass many specialized sub-disciplines like Cybersecurity, Information Security, Software Development, Database Administration, Network Administration, and more. To become "good" in this field requires considerable practice and effort. Cyber security can be a very challenging discipline because it requires the basic knowledge necessary for a typical IT specialist and a much deeper understanding of all areas (networking, Linux and Windows systems administration, scripting, databases, etc.). We don't need to be experts in every single area of IT. However, the more experience and knowledge we have, the easier our job as an IT security specialist or penetration tester will become. We cannot work confidently as penetration testers if we don't have a deep understanding of the technologies we are assessing. For example, a web developer focuses only on developing web applications and websites. This generally requires knowledge of HTML, JavaScript, CSS, SQL, and server-side programming languages, such as PHP. Even if the developer has over ten years of experience in his field, it only takes one mistake for the entire web server to be unusable or for data to be stolen. As an attacker, the trick is to find a way to identify and exploit these errors.

With this in mind, we have laid a foundation for our students because, in our experience, it is hard to know where to start. We have structured and built our learning material so that it may seem difficult at first, but with time you will realize that this is the easiest and most efficient way to teach such complex material efficiently. We want to make the learning process as easy and efficient as possible while emphasizing the core fundamentals and returning to them repeatedly. For example, many of our tasks are set up to get you to think in a certain way. We do this to help you develop the essential analytical skills that are imperative to be successful in a field that can have so much uncertainty. We want to help craft professionals who see things differently and question everything, which ultimately can help deliver more value to clients if you're able to find nuanced issues that other testers miss. We can't teach analytical skills and the ability to dig deeper and "question everything" in one single module or path. This can be compared to playing a musical instrument. We can't learn to play the guitar well without considerable practice. We can learn everything about a guitar, the history of guitars, the name of every component, etc., but if we pick one up without practice, we will not be able to produce music that is equivalent to our knowledge of guitars. This is the same in the field of penetration testing. We may know everything about the history of computers and be able to describe every component, but without deep hands-on experience, we won't be able to perform penetration testing at a high level.

The remainder of this section will explain how we have structured the modules in the way that we did to give you insight into our thought process and teaching philosophy. Our primary focus is creating engaging and empowering training resources that benefit individuals at ALL skill levels.

![alt text](/Images/image-127.png)

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

Exploitation is the attack performed against a system or application based on the potential vulnerability discovered during our information gathering and enumeration. We use the information from the Information Gathering stage, analyze it in the Vulnerability Assessment stage, and prepare the potential attacks. Often many companies and systems use the same applications but make different decisions about their configuration. This is because the same application can often be used for various purposes, and each organization will have different objectives.

From this stage, there are four paths we can take, depending on how far we have come:

| Path                  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Information Gathering | Once we have initial access to the target system, regardless of how high our privileges are at that moment, we need to gather information about the local system. Whether we use this new information for privilege escalation, lateral movement, or data exfiltration does not matter. Therefore, before we can take any further steps, we need to find out what we are dealing with. This inevitably takes us to the vulnerability assessment stage, where we analyze and evaluate the information we find.                                                                                                                  |
| Post-Exploitation     | Post-exploitation is mainly about escalating privileges if we have not yet attained the highest possible rights on the target host. As we know, more opportunities are open to us with higher privileges. This path actually includes the stages Information Gathering, Vulnerability Assessment, Exploitation, and Lateral Movement but from an internal perspective on the target system. The direct jump to post-exploitation is less frequent, but it does happen. Because through the exploitation stage, we may already have obtained the highest privileges, and from here on, we start again at Information Gathering. |
| Lateral Movement      | From here, we can also skip directly over to Lateral Movement. This can come under different conditions. If we have achieved the highest privileges on a dual-homed system used to connect two networks, we can likely use this host to start enumerating hosts that were not previously available to us.                                                                                                                                                                                                                                                                                                                      |
| Proof-of-Concept      | We can take the last path after gaining the highest privileges by exploiting an internal system. Of course, we do not necessarily have to have taken over all systems. However, if we have gained the Domain Admin privileges in an Active Directory environment, we can likely move freely across the entire network and perform any actions we can imagine. So we can create the Proof-of-Concept from our notes to detail and potentially automate the paths and activities and make them available to the technical department.                                                                                            |

This stage is so comprehensive that it has been divided into two distinct areas. The first category is general network protocols often used and present in almost every network. The actual exploitation of the potential and existing vulnerabilities is based on the adaptability and knowledge of the different network protocols we will be dealing with. In addition, we need to be able to create an overview of the existing network to understand its individual components' purposes. In most cases, web servers and applications contain a great deal of information that can be used against them. As stated previously, since web is a vast technical area in its own right, it will be treated separately. We are also interested in the remotely exposed services running on the target hosts, as these may have misconfigurations or known public vulnerabilities that we can leverage for initial access. Finally, existing users also play a significant role in the overall network.

18. **Password Attacks**
    If potential usernames or passwords were found during our information gathering, we may be able to use them specifically to perform Password Attacks on the systems and applications and authenticate ourselves. This module covers various methods to obtain credentials both remotely and locally on Windows and Linux systems.
    Tier I | Medium | Offensive | 18 Sections+ | 108 hours

19. **Attacking Common Services**
    Due to the variety of attacks that can be carried out, attacks on specific network services and web applications differ. Therefore, these are separated into different modules, as many specific attacks can only be carried out against web applications. However, there are many essential network services that can almost always be found in any corporate network. Therefore, knowing how to Attack Common Services is another major concept that needs to be covered in detail.
    Tier II | Medium | Offensive | 19 Sections+ | 208 hours

20. **Pivoting, Tunneling & Port Forwarding**
    When Pivoting, the exploited system is used as a node between the external and internal networks or between different internal networks. This is used to communicate with the internal systems to which we can usually not establish a direct connection from the Internet or another host in the internal network. It does not matter whether these are hosted on-premise or in the cloud. Network access and restrictions can be configured to and from specific hosts, even in the cloud. Tunnels must also be created to be able to transfer data securely. Port forwarding is often used to forward a local port to the port of an exploited system.
    Tier II | Medium | Offensive | 18 Sections+ | 202 days

21. **Active Directory Enumeration & Attacks**
    As we already know, most corporate networks are managed by administrators using Active Directory. Therefore, it is crucial to become familiar with this technology and how Active Directory Enumeration & Attacks can affect it. Its complexity can often lead to various vulnerabilities. Especially when administrators are careless or imprecise, vulnerabilities often arise that can lead to a complete domain takeover.
    Tier II | Medium | Offensive | 36 Sections+ | 207 days

## Web Exploitation

Web exploitation is the second part of the exploitation stage. Many different technologies, improvements, features, and enhancements have been developed in this area over the last few years, and things are constantly evolving. As a result, many different components come into play when dealing with web applications. This includes many kinds of databases that require differing command syntax to interact with. Due to the diversity of web applications available to companies and their prevalence worldwide, we must deal with this area separately and focus intently on it. Web applications present a vast attack surface and are often the main accessible targets during external penetration testing engagements, so strong web enumeration and exploitation skills are paramount.

22. **Using Web Proxies**
    Web servers and web applications work based on the HTTP/HTTPS protocol. Like other protocols, this protocol has a fixed structure for requests and responses. We will focus on Using Web Proxies to analyze and manipulate these requests. The way these requests and their HTTP headers can be manipulated plays a significant role in the results we can get from them. Even the absence of specific HTTP headers or too many allowed HTTP methods can be very dangerous for the webserver or web application quickly and easily.
    Tier II | Easy | Offensive | 15 Sections+ | 208 hours

23. **Attacking Web Applications with Ffuf**
    After learning which attack methods these web applications can be subject to, we can use many of these attack methods and start Attacking Web Applications with Ffuf. Since every web server and application works with many different parameters due to its link with the database, these parameters can be discovered manually and automatically. For this purpose, there are procedures and different possibilities that allow us to find these parameters to exploit further possible vulnerabilities.
    Tier 0 | Easy | Offensive | 13 Sections+ | 105 hours

24. **Login Brute Forcing**
    Authentication mechanisms are a vital target. Using these, we can gain access to different user accounts with the help of specific vulnerabilities. One of the most effective ways of gaining access is through Login Brute Forcing. Almost all web applications that offer any kind of user-specific functions work with the help of some sort of authentication mechanisms.
    Tier II | Easy | Offensive | 11 Sections+ | 206 hours

25. **SQL Injection Fundamentals**
    Whether it manages products or users, most every web application works with at least one database. This database is linked to the web application in some way and may open up another attack category called SQL Injection. With an understanding of SQL Injection Fundamentals, we can manipulate or exploit the database for our purposes by abusing functionality contained within the web application.
    Tier 0 | Medium | Offensive | 17 Sections+ | 108 hours

26. **SQLMap Essentials**
    Many of the attacks against web application database are summarized in a tool called SQLMap and should therefore also be learned to speed up our process after manual inspection. SQLMap Essentials should be learned to apply the tool appropriately and adapt it to the web application.
    Tier II | Easy | Offensive | 11 Sections+ | 208 hours

27. **Cross-Site Scripting (XSS)**
    Cross-site Scripting (XSS) is another of the most common attack categories. These vulnerabilities can be leveraged to launch various attacks, such as phishing, session hijacking, and others. Among other things, we can also potentially take over web sessions from other users or even administrators.
    Tier II | Easy | Offensive | 10 Sections+ | 206 hours

28. **File Inclusion**
    Depending on the webserver configuration and the web application, some vulnerabilities allow us some type of File Inclusion. For example, we may be able to acess files on the target system or use our own to execute code without being provided access by the developers or administrators.
    Tier 0 | Medium | Offensive

11 Sections+ | 108 hours

29. **Command Injections**
    We do not always need to attack the database using SQL Injections or XSS. Often direct Command Injections can be used to execute system commands. Some command injections are easier to spot than others, which may require advanced knowledge of identifying and bypassing filters in place.
    Tier II | Medium | Offensive | 12 Sections+ | 206 hours

30. **Web Attacks**
    The other top 10 most critical vulnerabilities include HTTP Verb Tampering, IDOR, and XXE. These are more advanced Web Attacks, as they require some security filters and encodings to be bypassed.
    Tier II | Medium | Offensive | 18 Sections+ | 202 days

31. **Attacking Common Applications**
    Common web applications might be customized by administrators but are nevertheless used worldwide. Therefore, it is also essential to know how to Attack Common Applications.
    Tier II | Medium | Offensive | 22 Sections+ | 202 days

## Post-Exploitation

In most cases, when we exploit certain services for our purposes to gain access to the system, we usually do not obtain the highest possible privileges. Because services are typically configured in a certain way "isolated" to stop potential attackers, bypassing these restrictions is the next step we take in this stage. However, it is not always easy to escalate the privileges. After gaining in-depth knowledge about how these operating systems function, we must adapt our techniques to the particular operating system and carefully study how Linux Privilege Escalation and Windows Privilege Escalation work.

From this stage, there are four paths we can take, depending on how far we have come:

| Path                              | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Information Gathering / Pillaging | Before we can begin escalating privileges, we must first get an overview of the inner workings of the exploited system. After all, we do not know which users are on the system and what options are available to us up to this point. This step is also known as Pillaging. This path is not optional, as with the others, but essential. Again, entering the Information Gathering stage puts us in this perspective. This inevitably takes us to the vulnerability assessment stage, where we analyze and evaluate the information we find. |
| Exploitation                      | Suppose we have found sensitive information about the system and its' contents. In that case, we can use it to exploit local applications or services with higher privileges to execute commands with those privileges.                                                                                                                                                                                                                                                                                                                        |
| Lateral Movement                  | From here, we can also skip directly over to Lateral Movement. This can come under different conditions. If we have achieved the highest privileges on a dual-homed system used to connect two networks, we can likely use this host to start enumerating hosts that were not previously available to us.                                                                                                                                                                                                                                      |
| Proof-of-Concept                  | We can take the last path after gaining the highest privileges by exploiting an internal system. Of course, we do not necessarily have to have taken over all systems. However, if we have gained the Domain Admin privileges in an Active Directory environment, we can likely move freely across the entire network and perform any actions we can imagine. So we can create the Proof-of-Concept from our notes to detail and potentially automate the paths and activities and make them available to the technical department.            |

After we have gained access to a system, we must be able to take further steps from within the system. During a penetration test, customers often want to find out how far an attacker could go in their network. There are many different versions of operating systems. For example, we may run into Windows XP, Windows 7, 8, 10, 11, and Windows Server 2008, 2012, 2016, and 2019. There are also different distributions for Linux-based operating systems, such as Ubuntu, Debian, Parrot OS, Arch, Deepin, Redhat, Pop!\_OS, and many others. No matter which of these systems we get into, we have to find our way around it and understand the individual weak points that a system can have from within.

32. **Linux Privilege Escalation**
    The vast majority of web servers that make up the World Wide Web run Linux. In addition, we will find many Linux-based servers hosting critical infrastructure services that individuals & organizations use to be more productive and efficient in their daily work. Because of this widespread use of Linux, we must understand the fundamentals. There are many ways to misconfigure Linux systems. Discovering these flaws and taking advantage of them to escalate privileges is covered in Linux Privilege Escalation.
    Tier II | Easy | Offensive | 15 Sections+ | 1008 hours

33. **Windows Privilege Escalation**  
    Modern Windows systems, now have stronger security precautions (if an organization is diligent with patching), but administrator errors are possible in any environment. There are many different ways to find the misconfigurations in Windows-based systems, and we need them for Windows Privilege Escalation.
    Tier II | Medium | Offensive | 30 Sections+ | 1004 days

## Lateral Movement

Lateral movement is one of the essential components for moving through a corporate network. We can use it to overlap with other internal hosts and further escalate our privileges within the current subnet or another part of the network. However, just like Pillaging, the Lateral Movement stage requires access to at least one of the systems in the corporate network. In the Exploitation stage, the privileges gained do not play a critical role in the first instance since we can also move through the network without administrator rights.

There are three paths we can take from this stage:

| Path                              | Description                                                                                                                                                                                                                                                                                                             |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Vulnerability Assessment          | If the penetration test is not finished yet, we can jump from here to the Vulnerability Assessment stage. Here, the information already obtained from pillaging is used and analyzed to assess where the network services or applications using an authentication mechanism that we may be able to exploit are running. |
| Information Gathering / Pillaging | After a successful lateral movement, we can jump into Pillaging once again. This is local information gathering on the target system that we accessed.                                                                                                                                                                  |
| Proof-of-Concept                  | Once we have made the last possible lateral movement and completed our attack on the corporate network, we can summarize the information and steps we have collected and perhaps even automate certain sections that demonstrate vulnerability to the vulnerabilities we have found.                                    |

Since both Lateral Movement and Pillaging require access to an already exploited system, these techniques and methods are covered in different modules, such as Getting Started, Linux Privilege Escalation, and Windows Privilege Escalation, and many others.

## Proof-of-Concept

The Proof-Of-Concept (POC) is merely proof that a vulnerability found exists. As soon as the administrators receive our report, they will try to confirm the vulnerabilities found by reproducing them. After all, no administrator will change business-critical processes without confirming the existence of a given vulnerability. A large network may have many interoperating systems and dependencies that must be checked after making a change, which can take a considerable amount of time and money. Just because a pentester found a given flaw, it doesn't mean that the organization can easily remediate it by just changing one system, as this could negatively affect the business. Administrators must carefully test fixes to ensure no other system is negatively impacted when a change is introduced. PoCs are sent along with the documentation as part of a high-quality penetration test, allowing administrators to use them to confirm the issues themselves.

From this stage, there is only one path we can take:

| Path            | Description                                                                                                                                                        |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Post-Engagement | At this point, we can only go to the post-engagement stage, where we optimize and improve the documentation and send it to the customer after an intensive review. |

When we already have all the information we have collected and have used the vulnerability to our advantage, it does not take much effort to automate the individual steps for this.

34. **Introduction to Python 3**
    Python is one of the easiest programming languages to learn, and it is also quite powerful. This makes it easy to automate many steps and, with the help of comments in our code, to understand exactly how the vulnerability is exploited step-by-step. Therefore, Introduction to Python 3 will be sufficient for most aspects of automation once we understand the structure of this programming language.
    Tier I | Easy | General | 14 Sections+ | 105 hours

## Post-Engagement

The Post-Engagement stage also includes cleaning up the systems we exploit so that none of these systems can be exploited using our tools. For example, leaving a bind shell on a web server that does not require authentication and is easy to find will do the opposite of what we are trying to do. In this way, we endanger the network through our carelessness. Therefore, it is essential to remove all content that we have transferred to the systems during our penetration test so that the corporate network is left in the same state as before our penetration test. We also should note down any system changes, successful exploitation attempts, captured credentials, and uploaded files in the appendices of our report so our clients can cross-check this against any alerts they receive toprove that they were a result of our testing actions and not an actual attacker in the network.

In addition, we have to reconcile all our notes with the documentation we have written in the meantime to make sure we have not skipped any steps and can provide a comprehensive, well-formatted and neat report to our clients.

35. **Documentation & Reporting**
    We need to understand proper Documentation and Reporting, how to stay organized and take detailed notes, and how to write effectively and deliver high quality client deliverables. Practice in this area will simplify preparation of our reports and save us considerable time. This module also helps us optimize our notetaking and organization, which we must adapt to our needs to work as efficiently as possible.
    Tier II | Easy | General | 8 Sections+ | 202 days

36. **Attacking Enterprise Networks**
    It is essential to get and keep an overall view of all these stages, their contents, and possible challenges. Attacking Enterprise Networks can be a daunting task, and we can get lost in the diversity of our options and overlook some of the essentials. So instead, we need to familiarize ourselves with how to attack such large networks and what vulnerabilities may exist with a large number of systems in a network.
    Tier II | Medium | Offensive | 14 Sections+ | 202 days

Now that we've covered the general layout of Academy modules regarding the penetration testing process, we'll briefly discuss how exercises and questions are presented in HTB Academy.
