# Wireless Networks

Wireless networks are computer networks that use wireless data connections between network nodes. These networks allow devices such as laptops, smartphones, and tablets to communicate with each other and the Internet without needing physical connections such as cables.

Wireless networks use radio frequency (RF) technology to transmit data between devices. Each device on a wireless network has a wireless adapter that converts data into RF signals and sends them over the air. Other devices on the network receive these signals with their own wireless adapters, and the data is then converted back into a usable form. Those can operate over various ranges, depending on the technology used. For example, a local area network (LAN) that covers a small area, such as a home or small office, might use a wireless technology called WiFi, which has a range of a few hundred feet. On the other hand, a wireless wide area network (WWAN) might use mobile telecommunication technology such as cellular data (3G, 4G LTE, 5G), which can cover a much larger area, such as an entire city or region.

Therefore, to connect to a wireless network, a device must be within range of the network and configured with the correct network settings, such as the network name and password. Once connected, devices can communicate with each other and the Internet, allowing users to access online resources and exchange data.

Communication between devices occurs over RF in the 2.4 GHz or 5 GHz bands in a WiFi network. When a device, like a laptop, wants to send data over the network, it first communicates with the Wireless Access Point (WAP) to request permission to transmit. The WAP is a central device, like a router, that connects the wireless network to a wired network and controls access to the network. Once the WAP grants permission, the transmitting device sends the data as RF signals, which are received by the wireless adapters of other devices on the network. The data is then converted back into a usable form and passed on to the appropriate application or system.

The strength of the RF signal and the distance it can travel are influenced by factors such as the transmitter's power, the presence of obstacles, and the density of RF noise in the environment. So, to ensure reliable communication, WiFi networks use techniques such as spread spectrum transmission and error correction to overcome these challenges.

## WiFi Connection

The device must also be configured with the correct network settings, such as the network name / Service Set Identifier (SSID) and password. So, to connect to the router, the laptop uses a wireless networking protocol called IEEE 802.11. This protocol defines the technical details of how wireless devices communicate with each other and with WAPs. When a device wants to join a WiFi network, it sends a request to the WAP to initiate the connection process. This request is known as a connection request frame or association request and is sent using the IEEE 802.11 wireless networking protocol. The connection request frame contains various fields of information, including the following but not limited to:

- MAC address: A unique identifier for the device's wireless adapter.
- SSID: The network name, also known as the Service Set Identifier of the WiFi network.
- Supported data rates: A list of the data rates the device can communicate.
- Supported channels: A list of the channels (frequencies) on which the device can communicate.
- Supported security protocols: A list of the security protocols that the device is capable of using, such as WPA2/WPA3.

The device then uses this information to configure its wireless adapter and connect to the WAP. Once the connection is established, the device can communicate with the WAP and other network devices. It can also access the Internet and other online resources through the WAP, which acts as a gateway to the wired network. However, the SSID can be hidden by disabling broadcasting. That means that devices that search for that specific WAP will not be able to identify its SSID. Nevertheless, the SSID can still be found in the authentication packet.

In addition to the IEEE 802.11 protocol, other networking protocols and technologies may also be used, like TCP/IP, DHCP, and WPA2, in a WiFi network to perform tasks such as assigning IP addresses to devices, routing traffic between devices, and providing security.

## WEP Challenge-Response Handshake

The challenge-response handshake is a process to establish a secure connection between a WAP and a client device in a wireless network that uses the WEP security protocol. This involves exchanging packets between the WAP and the client device to authenticate the device and establish a secure connection.

| Step | Who    | Description                                                                                                                                  |
| ---- | ------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | Client | Sends an association request packet to the WAP, requesting access.                                                                           |
| 2    | WAP    | Responds with an association response packet to the client, which includes a challenge string.                                               |
| 3    | Client | Calculates a response to the challenge string and a shared secret key and sends it back to the WAP.                                          |
| 4    | WAP    | Calculates the expected response to the challenge with the same shared secret key and sends an authentication response packet to the client. |

Nevertheless, some packets can get lost, so the so-called CRC checksum has been integrated. Cyclic Redundancy Check (CRC) is an error-detection mechanism used in the WEP protocol to protect against data corruption in wireless communications. A CRC value is calculated for each packet transmitted over the wireless network based on the packet's data. It is used to verify the integrity of the data. When the destination device receives the packet, the CRC value is recalculated and compared to the original value. If the values match, the data has been transmitted successfully without any errors. However, if the values do not match, the data has been corrupted and needs to be retransmitted.

The design of the CRC mechanism has a flaw that allows us to decrypt a single packet without knowing the encryption key. This is because the CRC value is calculated using the plaintext data in the packet rather than the encrypted data. In WEP, the CRC value is included in the packet header along with the encrypted data. When the destination device receives the packet, the CRC value is recalculated and compared to the original one to ensure that the data has been transmitted successfully without any errors. However, we can use the CRC to determine the plaintext data in the packet, even if the data is encrypted.
