# Networking Models

Two networking models describe the communication and transfer of data from one host to another, called `ISO/OSI model` and the `TCP/IP model`. This is a simplified representation of the so-called `layers` representing transferred Bits in readable contents for us.

![alt text](/Images/image-57.png)

## The OSI Model

The `OSI` model, often referred to as `ISO/OSI` layer model, is a reference model that can be used to describe and define the communication between systems. The reference model has `seven` individual layers, each with clearly separated tasks.

The term `OSI` stands for `Open Systems Interconnection` model, published by the `International Telecommunication Union` (`ITU`) and the `International Organization for Standardization` (`ISO`). Therefore, the `OSI` model is often referred to as the `ISO/OSI` layer model.

## The TCP/IP Model

`TCP/IP` (`Transmission Control Protocol`/`Internet Protocol`) is a generic term for many network protocols. The protocols are responsible for the switching and transport of data packets on the Internet. The Internet is entirely based on the `TCP/IP` protocol family. However, `TCP/IP` does not only refer to these two protocols but is usually used as a generic term for an entire protocol family.

For example, `ICMP` (`Internet Control Message Protocol`) or `UDP` (`User Datagram Protocol`) belongs to the protocol family. The protocol family provides the necessary functions for transporting and switching data packets in a private or public network.

## ISO/OSI vs. TCP/IP

`TCP/IP` is a communication protocol that allows hosts to connect to the Internet. It refers to the `Transmission Control Protocol` used in and by applications on the Internet. In contrast to `OSI`, it allows a lightening of the rules that must be followed, provided that general guidelines are followed.

`OSI`, on the other hand, is a communication gateway between the network and end-users. The OSI model is usually referred to as the reference model because it is newer and more widely used. It is also known for its strict protocol and limitations.

![alt text](/Images/image-58.png)

## Packet Transfers

In a layered system, devices in a layer exchange data in a different format called a `protocol data unit` (`PDU`). For example, when we want to browse a website on the computer, the remote server software first passes the requested data to the application layer. It is processed layer by layer, each layer performing its assigned functions. The data is then transferred through the network's physical layer until the destination server or another device receives it. The data is routed through the layers again, with each layer performing its assigned operations until the receiving software uses the data.

During the transmission, each layer adds a `header` to the `PDU` from the upper layer, which controls and identifies the packet. This process is called `encapsulation`. The header and the data together form the PDU for the next layer. The process continues to the `Physical Layer` or `Network Layer`, where the data is transmitted to the receiver. The receiver reverses the process and unpacks the data on each layer with the header information. After that, the application finally uses the data. This process continues until all data has been sent and received.

For us, as penetration testers, both reference models are useful. With `TCP/IP`, we can quickly understand how the entire connection is established, and with `ISO`, we can take it apart piece by piece and analyze it in detail. This often happens when we can listen to and intercept specific network traffic. We then have to analyze this traffic accordingly, going into more detail in the `Network Traffic Analysis` module. Therefore, we should familiarize ourselves with both reference models and understand and internalize them in the best possible way.
