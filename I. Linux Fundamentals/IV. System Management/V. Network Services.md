# Network Services 🌐

When working with Linux, mastering various network services is crucial. This competence allows us to communicate with other computers, transfer files, analyze network traffic, and configure services for penetration tests. Understanding network security becomes paramount as we explore the intricacies of each service and its configurations.

## SSH - Secure Shell 🚀

**SSH** is a network protocol facilitating secure data and command transmission. Utilized widely for managing and accessing remote Linux systems securely, OpenSSH is the go-to server. Admins employ OpenSSH to execute commands, transfer files, and establish encrypted connections, safeguarding against interception.

### Installation

```bash
sudo apt install openssh-server -y
```

### Check Server Status

```bash
systemctl status ssh
```

### Connecting via SSH

```bash
ssh cry0l1t3@10.129.17.122
```

Configuration involves editing `/etc/ssh/sshd_config`, adjusting settings cautiously. SSH provides secure access and enables tunneling for encrypted data transmission.

## NFS - Network File System 📁

**NFS** allows remote file management as if stored locally, fostering efficient collaboration. Installing NFS on Linux:

### Installation

```bash
sudo apt install nfs-kernel-server -y
```

### Check Server Status

```bash
systemctl status nfs-kernel-server
```

| Permissions    | Description                                                                                                                  |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| rw             | Gives users and systems read and write permissions to the shared directory.                                                  |
| ro             | Gives users and systems read-only access to the shared directory.                                                            |
| no_root_squash | Prevents the root user on the client from being restricted to the rights of a normal user.                                   |
| root_squash    | Restricts the rights of the root user on the client to the rights of a normal user.                                          |
| sync           | Synchronizes the transfer of data to ensure that changes are only transferred after they have been saved on the file system. |
| async          | Transfers data asynchronously, which makes the transfer faster, but may cause inconsistencies in the file system.            |

Configuration lies in `/etc/exports`, specifying shared directories and access rights. Noteworthy access rights include `rw` (read/write), `ro` (read-only), and options like `no_root_squash`.

### Create NFS Share

```bash
mkdir nfs_sharing
echo '/home/cry0l1t3/nfs_sharing hostname(rw,sync,no_root_squash)' >> /etc/exports
```

### Mount NFS Share

```bash
mkdir ~/target_nfs
mount 10.129.12.17:/home/john/dev_scripts ~/target_nfs
tree ~/target_nfs
```

NFS shares facilitate file access as if on the local system, even offering privilege escalation possibilities.

## Web Server 🌐

Understanding web servers is crucial for penetration testers. Apache, Nginx, Lighttpd, and Caddy are popular choices. Apache, for instance, is feature-rich, supporting secure web applications. Installation:

### Install Apache Web Server

```bash
sudo apt install apache2 -y
```

Apache configuration involves specifying accessible folders in `/etc/apache2/apache2.conf`. Individual settings can be customized using `.htaccess`.

## Python Web Server 🐍

For simple, fast file transfers, Python's built-in web server is useful:

### Installation

```bash
sudo apt install python3 -y
```

### Start Python Web Server

```bash
python3 -m http.server
```

Custom port usage:

```bash
python3 -m http.server 443
```

## VPN - Virtual Private Network 🔒

VPNs provide secure network access, crucial for remote connectivity and traffic anonymization. OpenVPN, L2TP/IPsec, PPTP, SSTP, and SoftEther are popular VPN servers for Linux.

### Install OpenVPN

```bash
sudo apt install openvpn -y
```

OpenVPN customization involves editing `/etc/openvpn/server.conf`. Connecting to a VPN:

### Connect to VPN

```bash
sudo openvpn --config internal.ovpn
```

After establishing a connection, internal hosts on the network become accessible. VPNs are essential for secure remote access and privacy.
