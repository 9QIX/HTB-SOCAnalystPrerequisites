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
   What
