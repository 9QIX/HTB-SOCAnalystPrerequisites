## Working with Web Services 🌐

Communication with web servers is crucial, and Linux offers various methods for setting up web servers. Apache, alongside IIS and Nginx, is widely used. Apache modules like mod_ssl for encryption, mod_proxy for proxy serving, and mod_headers for HTTP header manipulations enhance its functionality.

### Apache Installation

```bash
apt install apache2 -y
```

After installation, navigate to the default page using your browser (`http://localhost`) to confirm the server's functionality.

![alt text](/Images/image-12.png)

### cURL

**cURL** is a powerful tool for transferring files over various protocols. It's commonly pre-installed on Linux systems, aiding in remote website testing and analysis.

```bash
curl http://localhost
```

### wget

**wget** is another tool for downloading files from FTP or HTTP servers directly from the terminal, functioning as a reliable download manager.

```bash
wget http://localhost
```

### Python 3 Web Server

Python 3 provides a simple way to serve files locally, useful for testing web pages or transferring files.

```bash
python3 -m http.server
```

Accessing `http://0.0.0.0:8000/` allows you to view and interact with the served files. Requests made to the server are logged, providing insight into the communication between the client and server.

![alt text](/Images/image-13.png)

Understanding these tools and methods is essential for efficient web server management and troubleshooting.
