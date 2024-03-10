# Package Management 📦

Managing packages is crucial for Linux administrators, whether maintaining systems, home machines, or penetration testing distributions. Packages include software binaries, configuration files, dependencies, and update information. Key features of package management systems include package downloading, dependency resolution, standard binary formats, common installation locations, system-related configurations, and quality control.

## Package Management Programs 🛠️

Here are examples of package management programs with their descriptions:

| Command    | Description                                                                                     |
| ---------- | ----------------------------------------------------------------------------------------------- |
| `dpkg`     | Installs, builds, removes Debian packages.                                                      |
| `apt`      | High-level command-line interface for package management.                                       |
| `aptitude` | High-level interface alternative to `apt`.                                                      |
| `snap`     | Installs, configures, refreshes, and removes snap packages.                                     |
| `gem`      | Front-end to RubyGems, the standard package manager for Ruby.                                   |
| `pip`      | Python package installer. Recommended for installing Python packages not in the Debian archive. |
| `git`      | Fast, scalable, distributed version control system.                                             |

Setting up a local virtual machine (VM) for experimentation is highly recommended. Now, let's delve into an example using the APT package manager.

### Advanced Package Manager (APT) 📥

APT is used in Debian-based Linux distributions. It simplifies program installation by handling dependencies efficiently. Repositories are queried for packages, which can be labeled as stable, testing, or unstable.

#### Checking Repository Information

```bash
z0x9n@htb[/htb]$ cat /etc/apt/sources.list.d/parrot.list
```

#### Searching for Impacket Packages

```bash
z0x9n@htb[/htb]$ apt-cache search impacket
```

#### Viewing Package Information

```bash
z0x9n@htb[/htb]$ apt-cache show impacket-scripts
```

#### Listing Installed Packages

```bash
z0x9n@htb[/htb]$ apt list --installed
```

#### Installing a Package

```bash
z0x9n@htb[/htb]$ sudo apt install impacket-scripts -y
```

### Git - Downloading Projects from GitHub 🐙

Now that Git is installed, we can clone projects from GitHub. For example, we clone the 'Nishang' project.

```bash
z0x9n@htb[/htb]$ mkdir ~/nishang/ && git clone https://github.com/samratashok/nishang.git ~/nishang
```

### Downloading with DPKG 📥

We can download programs separately and install them using `dpkg`. Here's an example with 'strace.'

```bash
z0x9n@htb[/htb]$ wget http://archive.ubuntu.com/ubuntu/pool/main/s/strace/strace_4.21-1ubuntu1_amd64.deb
z0x9n@htb[/htb]$ sudo dpkg -i strace_4.21-1ubuntu1_amd64.deb
```

With these examples, we've explored APT, Git for GitHub projects, and `dpkg` for direct package installation. These skills are valuable for effective Linux package management. 🚀
