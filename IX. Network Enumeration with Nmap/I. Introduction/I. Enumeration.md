# Enumeration

Enumeration is the most critical part of all. The art, the difficulty, and the goal are not to gain access to our target computer. Instead, it is identifying all of the ways we could attack a target we must find.

It is not just based on the tools we use. They will only do much good if we know what to do with the information we get from them. The tools are just tools, and tools alone should never replace our knowledge and our attention to detail. Here it is much more about actively interacting with the individual services to see what information they provide us and what possibilities they offer us.

It is essential to understand how these services work and what syntax they use for effective communication and interaction with the different services.

This phase aims to improve our knowledge and understanding of the technologies, protocols, and how they work and learn to deal with new information and adapt to our already acquired knowledge. Enumeration is collecting as much information as possible. The more information we have, the easier it will be for us to find vectors of attack.

Imagine the following situation:

Our partner is not at home and has misplaced our car keys. We call our partner and ask where the keys are. If we get an answer like "in the living room," it is entirely unclear and can take much time to find them there. However, what if our partner tells us something like "in the living room on the white shelf, next to the TV, in the third drawer"? As a result, it will be much easier to find them.

It's not hard to get access to the target system once we know how to do it. Most of the ways we can get access we can narrow down to the following two points:

- Functions and/or resources that allow us to interact with the target and/or provide additional information.
- Information that provides us with even more important information to access our target.

When scanning and inspecting, we look exactly for these two possibilities. Most of the information we get comes from misconfigurations or neglect of security for the respective services. Misconfigurations are either the result of ignorance or a wrong security mindset. For example, if the administrator only relies on the firewall, Group Policy Objects (GPOs), and continuous updates, it is often not enough to secure the network.

## Enumeration is the key

That's what most people say, and they are right. However, it is too often misunderstood. Most people understand that they haven't tried all the tools to get the information they need. Most of the time, however, it's not the tools we haven't tried, but rather the fact that we don't know how to interact with the service and what's relevant.

That's precisely the reason why so many people stay stuck in one spot and don't get ahead. Had these people invested a couple of hours learning more about the service, how it works, and what it is meant for, they would save a few hours or even days from reaching their goal and get access to the system.

## Manual enumeration

Manual enumeration is a critical component. Many scanning tools simplify and accelerate the process. However, these cannot always bypass the security measures of the services. The easiest way to illustrate this is to use the following example:

Most scanning tools have a timeout set until they receive a response from the service. If this tool does not respond within a specific time, this service/port will be marked as closed, filtered, or unknown. In the last two cases, we will still be able to work with it. However, if a port is marked as closed and Nmap doesn't show it to us, we will be in a bad situation. This service/port may provide us with the opportunity to find a way to access the system. Therefore, this result can take much unnecessary time until we find it.
