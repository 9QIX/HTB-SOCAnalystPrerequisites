# Containerization 📦

Containerization is a process of packaging and running applications in isolated environments, such as a container, virtual machine, or serverless environment. Technologies like Docker, Docker Compose, and Linux Containers make this process possible in Linux systems. These technologies allow users to create, deploy, and manage applications quickly, securely, and efficiently. With these tools, users can configure their applications in various ways, allowing them to tailor the application to their needs. Additionally, containers are incredibly lightweight, perfect for running multiple applications simultaneously and providing scalability and portability. Containerization is a great way to ensure that applications are managed and deployed efficiently and securely.

🔒 **Container Security:**
Container security is an important aspect of containerization. They provide users a secure environment for running their applications since they are isolated from the host system and other containers. This isolation helps protect the host system from any malicious activities in the container while providing an additional layer of security for the applications running on the containers. Additionally, containers have the advantage of being lightweight, which makes them more difficult to compromise than traditional virtual machines. Furthermore, containers are easy to configure, making them ideal for running applications securely.

🐳 **Dockers:**
Docker is an open-source platform for automating the deployment of applications as self-contained units called containers. It uses a layered filesystem and resource isolation features to provide flexibility and portability. Additionally, it provides a robust set of tools for creating, deploying, and managing applications, which helps streamline the containerization process.

### Install Docker-Engine 🛠️

```bash
#!/bin/bash

# Preparation
sudo apt update -y
sudo apt install ca-certificates curl gnupg lsb-release -y
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine
sudo apt update -y
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y

# Add user htb-student to the Docker group
sudo usermod -aG docker htb-student
echo '[!] You need to log out and log back in for the group changes to take effect.'

# Test Docker installation
docker run hello-world
```

## Creating a Docker Image 🖼️

Creating a Docker image is done by creating a Dockerfile, which contains all the instructions the Docker engine needs to create the container. We can use Docker containers as our "file hosting" server when transferring specific files to our target systems. Therefore, we must create a Dockerfile based on Ubuntu 22.04 with Apache and SSH server running. With this, we can use `scp` to transfer files to the Docker image, and Apache allows us to host files and use tools like `curl`, `wget`, and others on the target system to download the required files.

### Dockerfile 🐳

```bash
# Use the latest Ubuntu 22.04 LTS as the base image
FROM ubuntu:22.04

# Update the package repository and install the required packages
RUN apt-get update && \
    apt-get install -y \
        apache2 \
        openssh-server \
        && \
    rm -rf /var/lib/apt/lists/*

# Create a new user called "student"
RUN useradd -m docker-user && \
    echo "docker-user:password" | chpasswd

# Give the htb-student user full access to the Apache and SSH services
RUN chown -R docker-user:docker-user /var/www/html && \
    chown -R docker-user:docker-user /var/run/apache2 && \
    chown -R docker-user:docker-user /var/log/apache2 && \
    chown -R docker-user:docker-user /var/lock/apache2 && \
    usermod -aG sudo docker-user && \
    echo "docker-user ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Expose the required ports
EXPOSE 22 80

# Start the SSH and Apache services
CMD service ssh start && /usr/sbin/apache2ctl -D FOREGROUND
```

### Docker Build 🔨

```bash
docker build -t FS_docker .
```

## Docker Management 🛠️

When managing Docker containers, Docker provides a comprehensive suite of tools that enable us to easily create, deploy, and manage containers. Some of the most commonly used Docker management commands are:

| Command          | Description                  |
| ---------------- | ---------------------------- |
| `docker ps`      | List all running containers  |
| `docker stop`    | Stop a running container     |
| `docker start`   | Start a stopped container    |
| `docker restart` | Restart a running container  |
| `docker rm`      | Remove a container           |
| `docker rmi`     | Remove a Docker image        |
| `docker logs`    | View the logs of a container |

## Linux Containers (LXC) 🐧

Linux Containers (LXC) is a virtualization technology that allows multiple isolated Linux systems to run on a single host. It uses resource isolation features, such as cgroups and namespaces, to provide a lightweight virtualization solution. By combining the advantages of LXC with the power of Docker, users can achieve a fully-fledged containerization experience in Linux systems.

### Install LXC 🛠️

```bash
sudo apt-get install lxc lxc-utils -y
```

### Creating an LXC Container 📦

```bash
sudo lxc-create -n linuxcontainer -t ubuntu
```

### Managing LXC Containers 📊

| Command                                      | Description                        |
| -------------------------------------------- | ---------------------------------- |
| `lxc-ls`                                     | List all existing containers       |
| `lxc-stop -n <container>`                    | Stop a running container           |
| `lxc-start -n <container>`                   | Start a stopped container          |
| `lxc-restart -n <container>`                 | Restart a running container        |
| `lxc-config -n <container name> -s storage`  | Manage container storage           |
| `lxc-config -n <container name> -s network`  | Manage container network settings  |
| `lxc-config -n <container name> -s security` | Manage container security settings |

## Securing LXC 🔐

To secure LXC containers, we can limit resources and enforce access control:

- **Resource Limits:** Configure CPU and memory limits for containers.
- **Access Control:** Restrict access to containers, enforce secure protocols, and use strong authentication mechanisms.

### Limiting Resources

To configure resource limits for LXC containers, we can create a new configuration file in the `/usr/share/lxc/config/` directory for each container.

```bash
sudo vim /usr/share/lxc/config/linuxcontainer.conf
```

In the configuration file, add the following lines to limit CPU and memory usage:

```txt
lxc.cgroup.cpu.shares = 512
lxc.cgroup

.memory.limit_in_bytes = 512M
```

Save and close the file, then restart the LXC service:

```bash
sudo systemctl restart lxc.service
```

### Optional Practice

1. Install LXC on your machine and create your first container.
2. Configure the network settings for your LXC container.
3. Create a custom LXC image and use it to launch a new container.
4. Configure resource limits for your LXC containers (CPU, memory, disk space).
5. Explore the `lxc-*` commands for managing containers.
6. Use LXC to create a container running a specific version of a web server (e.g., Apache, Nginx).
7. Configure SSH access to your LXC containers and connect to them remotely.
8. Create a container with persistence, so changes made to the container are saved and can be reused.
9. Use LXC to test software in a controlled environment, such as a vulnerable web application or malware.

### Answer

1. **Install LXC:**
   Install LXC on your system. Here are the commands for Ubuntu:

   ```bash
   sudo apt update
   sudo apt install lxc
   ```

2. **Create your first container:**
   Create a new LXC container named "mycontainer" using the `lxc-create` command:

   ```bash
   sudo lxc-create -n mycontainer -t download
   ```

3. **Configure network settings:**
   Edit the container configuration file to configure network settings:

   ```bash
   sudo nano /var/lib/lxc/mycontainer/config
   ```

   Add or modify network settings as needed. For example:

   ```
   lxc.net.0.type = veth
   lxc.net.0.link = lxcbr0
   lxc.net.0.flags = up
   lxc.net.0.hwaddr = 00:16:3e:xx:xx:xx
   ```

4. **Create a custom LXC image:**
   After making changes to your container, create a snapshot of the container using `lxc-snapshot` command:

   ```bash
   sudo lxc-snapshot -n mycontainer
   ```

5. **Configure resource limits:**
   Edit the container configuration file to configure resource limits:

   ```bash
   sudo nano /var/lib/lxc/mycontainer/config
   ```

   Add or modify resource limits as needed. For example:

   ```
   lxc.cgroup.memory.limit_in_bytes = 1G
   lxc.cgroup.cpu.cfs_period_us = 100000
   lxc.cgroup.cpu.cfs_quota_us = 50000
   ```

6. **Manage containers using lxc-\* commands:**
   Explore various `lxc-*` commands to manage your containers:

   - Start container: `sudo lxc-start -n mycontainer`
   - Stop container: `sudo lxc-stop -n mycontainer`
   - Get container info: `sudo lxc-info -n mycontainer`
   - List containers: `sudo lxc-ls`

7. **Create a container running a specific version of a web server:**
   Install a web server inside the container. For example, to install Apache:

   ```bash
   sudo lxc-start -n mycontainer
   sudo lxc-attach -n mycontainer -- apt update
   sudo lxc-attach -n mycontainer -- apt install apache2
   ```

8. **Configure SSH access:**
   Install SSH server inside the container and configure SSH as needed:

   ```bash
   sudo lxc-start -n mycontainer
   sudo lxc-attach -n mycontainer -- apt update
   sudo lxc-attach -n mycontainer -- apt install openssh-server
   ```

   You may also need to configure port forwarding if your containers are behind NAT.

9. **Create a container with persistence:**
   Create a container with a persistent storage backend such as ZFS or LVM. For example, to create a container with ZFS:

   ```bash
   sudo lxc-create -n mycontainer -t download -- --storage zfs
   ```

## Conclusion 🎉

Containerization technologies like Docker and Linux Containers provide powerful tools for deploying, managing, and securing applications. By leveraging these technologies, users can achieve efficient and secure application deployment, enabling seamless execution across various environments. Whether it's Docker for automated deployment or LXC for lightweight virtualization, containerization offers versatility and scalability for modern software development and deployment practices.
