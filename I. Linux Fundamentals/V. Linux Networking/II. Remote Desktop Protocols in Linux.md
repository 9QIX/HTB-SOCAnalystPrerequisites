# Remote Desktop Protocols in Linux 💻

Remote desktop protocols are used in Windows, Linux, and macOS to provide graphical remote access to a system. Administrators can utilize remote desktop protocols in many scenarios like troubleshooting, software or system upgrading, and remote systems administration. The most common protocols for this usage are RDP (Windows) and VNC (Linux).

#### XServer

The XServer is the user-side part of the X Window System network protocol (X11 / X). The X11 is a fixed system that consists of a collection of protocols and applications that allow us to call application windows on displays in a graphical user interface. X11 is predominant on Unix systems, but X servers are also available for other operating systems.

When a desktop is started on a Linux computer, the communication of the graphical user interface with the operating system happens via an X server. The computer's internal network is used, even if the computer should not be in a network. The practical thing about the X protocol is network transparency. This protocol mainly uses TCP/IP as a transport base but can also be used on pure Unix sockets. The ports that are utilized for X server are typically located in the range of TCP/6001-6009, allowing communication between the client and server.

##### X11 Security 🔒

X11 is not a secure protocol without suitable security measures since X11 communication is entirely unencrypted. A completely open X server lets anyone on the network read the contents of its windows, for example, and this goes unnoticed by the user sitting in front of it. Therefore, it is not even necessary to sniff the network. This standard X11 functionality is realized with simple X11 tools like xwd and xgrabsc.

##### XDMCP

The X Display Manager Control Protocol (XDMCP) protocol is used by the X Display Manager for communication through UDP port 177 between X terminals and computers operating under Unix/Linux. It is used to manage remote X Window sessions on other machines and is often used by Linux system administrators to provide access to remote desktops. XDMCP is an insecure protocol and should not be used in any environment that requires high levels of security.

#### VNC

Virtual Network Computing (VNC) is a remote desktop sharing system based on the RFB protocol that allows users to control a computer remotely. It allows a user to view and interact with a desktop environment remotely over a network connection. The user can control the remote computer as if sitting in front of it. This is also one of the most common protocols for remote graphical connections for Linux hosts.

Traditionally, the VNC server listens on TCP port 5900. So it offers its display 0 there. Other displays can be offered via additional ports, mostly 590[x], where x is the display number.

For these VNC connections, many different tools are used. Among them are for example:

- TigerVNC
- TightVNC
- RealVNC
- UltraVNC

In this example, we set up a TigerVNC server, and for this, we need, among other things, also the XFCE4 desktop manager since VNC connections with GNOME are somewhat unstable.

TigerVNC Installation 🐯

```bash
sudo apt install xfce4 xfce4-goodies tigervnc-standalone-server -y
vncpasswd
```

Configuration 🔧

```bash
touch ~/.vnc/xstartup ~/.vnc/config
```

```bash
cat <<EOT >> ~/.vnc/xstartup
#!/bin/bash
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
/usr/bin/startxfce4
[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
x-window-manager &
EOT
```

```bash
cat <<EOT >> ~/.vnc/config
geometry=1920x1080
dpi=96
EOT
```

Start the VNC server 🚀

```bash
vncserver
```

List Sessions 📋

```bash
vncserver -list
```

Setting Up an SSH Tunnel 🛡️

```bash
ssh -L 5901:127.0.0.1:5901 -N -f -l htb-student 10.129.14.130
```

Connecting to the VNC Server 🖥️

```bash
xtightvncviewer localhost:5901
```

### Conclusion

Remote desktop protocols play a crucial role in enabling remote access and administration of systems. Whether it's for troubleshooting, software upgrades, or remote system administration, protocols like RDP and VNC provide valuable functionality for administrators and users alike. However, it's essential to ensure proper security measures are in place, especially when dealing with unencrypted protocols like X11 and XDMCP. By following best practices and using encryption and authentication mechanisms, administrators can secure their remote desktop
