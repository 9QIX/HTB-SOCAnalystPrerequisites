# Networking Overview

A network enables two computers to communicate with each other. There is a wide array of topologies (mesh/tree/star), mediums (ethernet/fiber/coax/wireless), and protocols (TCP/UDP/IPX) that can be used to facilitate the network. It is important as security professionals to understand networking because when the network fails, the error may be silent, causing us to miss something.

Setting up a large, flat network is not extremely difficult, and it can be a reliable network, at least operationally. However, a flat network is like building a house on a land plot and considering it secure because it has a lock on the door. By creating lots of smaller networks and having them communicate, we can add defense layers. Pivoting around a network is not difficult, but doing it quickly and silently is tough and will slow attackers down. Back to the house scenario, let's walk through the following examples:

### Example No. 1

Building smaller networks and putting Access Control Lists around them is like putting a fence around the property's border that creates specific entry and exit points. Yes, an attacker could jump over the fence, but this looks suspicious and is not common, allowing it to be quickly detected as malicious activity. Why is the printer network talking to the servers over HTTP?

### Example No. 2

Taking the time to map out and document each network's purpose is like placing lights around the property, making sure all activity can be seen. Why is the printer network talking to the internet at all?

### Example No. 3

Having bushes around windows is a deterrent to people attempting to open the window. Just like Intrusion Detection Systems like Suricata or Snort are a deterrent to running network scans. Why did a port scan originate from the printer network?

These examples may sound silly, and it is common sense to block a printer from doing all of the above. However, if the printer is on a "flat /24 network" and gets a DHCP address, it can be challenging to place these types of restrictions on them.

## Story Time - A Pentesters Oversight

Most networks use a /24 subnet, so much so that many Penetration Testers will set this subnet mask (255.255.255.0) without checking. The /24 network allows computers to talk to each other as long as the first three octets of an IP Address are the same (ex: 192.168.1.xxx). Setting the subnet mask to /25 divides this range in half, and the computer will be able to talk to only the computers on "its half." We have seen Penetration Test reports where the assessor claimed a Domain Controller was offline when it was just on a different network in reality. The network structure was something like this:

- Server Gateway: 10.20.0.1/25
- Domain Controller: 10.20.0.10/25
- Client Gateway: 10.20.0.129/25
- Client Workstation: 10.20.0.200/25
- Pentester IP: 10.20.0.252/24 (Set Gateway to 10.20.0.1)

The Pentester communicated with the Client Workstations and thought they did an excellent job because they managed to steal a workstation password via Impacket. However, due to a failure to understand the network, they never managed to get off the Client Network and reach more "high value" targets such as database servers. Hopefully, if this sounds confusing to you, you can come back to this statement at the end of the module and understand it!

## Basic Information

Let us look at the following high-level diagram of how a Work From Home setup may work.

![image](https://user-images.githubusercontent.com/72584733/230755548-6794fc9e-ee74-4796-90f4-33f4c7a02aec.png)

The entire internet is based on many subdivided networks, as shown in the example and marked as "Home Network" and "Company Network." We can imagine networking as the delivery of mail or packages sent by one computer and received by the other.

Suppose we imagine as a scenario that we want to visit a company's website from our "Home Network." In that case, we exchange data with the company's website located in their "Company Network." As with sending mail or packets, we know the address where the packets should go. The website address or Uniform Resource Locator (URL) which we enter into our browser is also known as Fully Qualified Domain Name (FQDN).

The difference between URLs and FQDNs is that:

- an FQDN (www.hackthebox.eu) only specifies the address of the "building" and
- an URL (https://www.hackthebox.eu/example?floor=2&office=dev&employee=17) also specifies the "floor," "office," "mailbox" and the corresponding "employee" for whom the package is intended.

We will discuss the exact representations and definitions more clearly and precisely in other sections.

The fact is that we know the address, but not the exact geographical location of the address. In this situation, the post office can determine the exact location, which then forwards the packets to the desired location. Therefore, our post office forwards our packets to the main post office, representing our Internet Service Provider (ISP).

Our post office is our router which we utilize to connect to the "Internet" in networking.

As soon as we send our packet through our post office (router), the packet is forwarded to the main post office (ISP). This main post office looks in the address register/phonebook (Domain Name Service) where this address is located and returns the corresponding geographical coordinates (IP address). Now that we know the address's exact location, our packet is sent directly there by a direct flight via our main post office.

After the web server has received our packet with the request of what their website looks like, the webserver sends us back the packet with the data for the presentation of the website via the post office (router) of the "Company Network" to the specified return address (our IP address).
