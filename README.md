_This project has been created as part of the 42 curriculum by azahidi._

# Born2beRoot

## Description

**Born2beRoot** is a system administration project that aims to introduce the fundamental concepts of virtualization. The goal is to create a secure, minimal server environment using a Virtual Machine (VM). This exercise involves setting up an operating system with strict adherence to security rules, partition management, and script automation.

This project includes the **Bonus** part, which involves setting up a functional WordPress website, implementing a specific partition structure, and configuring an additional service (`Fail2Ban`) to enhance server security.

## Project Description & Design Choices

### Operating System Choice: Debian

For this project, **Debian** (stable) was chosen over Rocky Linux

- **Reasoning:** Debian is widely recognized for its stability and is highly recommended for those new to system administration It utilizes the `apt` package manager and `AppArmor` for security, providing a robust environment for learning server management.

### Technical Comparisons

#### Debian vs. Rocky Linux

- **Debian:** Uses the `.deb` package format and the `apt` package manager. It is community-driven and uses **AppArmor** for mandatory access control
- **Rocky Linux:** A downstream rebuild of RHEL (Red Hat Enterprise Linux). It uses `.rpm` packages and `dnf`/`yum`. It defaults to **SELinux** and is often preferred in enterprise environments requiring RHEL compatibility

#### AppArmor vs. SELinux

- **AppArmor (Application Armor):** Used in Debian. It is a path-based Mandatory Access Control (MAC) system that restricts programs to a specific set of files and capabilities. It is generally considered easier to configure than SELinux
- **SELinux (Security-Enhanced Linux):** Used in Rocky/RHEL. It is a label-based MAC system where every file and process has a security context. It offers fine-grained control but requires complex configuration

#### UFW vs. firewalld

- **UFW (Uncomplicated Firewall):** The default configuration tool for Debian. It simplifies `iptables` management, making it easy to open/close ports (e.g., allowing port 4242)
- **firewalld:** The default for Rocky Linux. It uses "zones" and services to manage traffic dynamically without breaking existing connections

#### VirtualBox vs. UTM

- **VirtualBox:** A mature, cross-platform Type-2 hypervisor developed by Oracle. It was chosen for its broad compatibility and strict adherence to the project subject.
- **UTM:** A virtualization tool often used on macOS (Apple Silicon) leveraging QEMU, allowing for emulation of x86 architectures on ARM chips.

### Configuration & Security Setup

Per the mandatory and bonus requirements, the server includes:

- **Partitioning (Bonus):** A complex partition scheme using **LVM** (Logical Volume Manager) splitting `/boot`, `/`, `/home`, `/var`, `/srv`, `/tmp`, and `/var/log` to ensure stability and security.
- **SSH:** Service running on port **4242**, with `root` login disabled.
- **User Management:** A user with the login `azahidi` belonging to `sudo` and `user42` groups.
- **Password Policy:** Strict rules (expires every 30 days, min 10 chars, uppercase/lowercase/number, etc.) enforced via `libpam-pwquality` .
- **Sudo Policy:** Limits authentication attempts, logs inputs/outputs to `/var/log/sudo/`, and uses a custom error message .
- **Monitoring:** A custom `bash` script broadcasting system stats (CPU, RAM, LVM, etc.) to all terminals every 10 minutes.

### Bonus Services Implementation

In addition to the mandatory requirements, the following services were installed:

1.  **WordPress Stack:** A functional website set up using **lighttpd**, **MariaDB**, and **PHP**.
2.  **Additional Service (Fail2Ban):**
    - **Choice:** **Fail2Ban** was selected as the additional service.
    - **Reasoning:** Fail2Ban scans log files (e.g., `/var/log/auth.log`) and bans IPs that show malicious signs, such as too many password failures. This creates an essential layer of protection against brute-force attacks on the SSH port (4242) exposed by the server.

## Instructions

Since the virtual machine itself is not submitted to the git repository, the submission consists of the virtual disk signature.

### Signature Verification

To verify that the submitted signature matches the virtual machine's disk, navigate to the folder containing the `.vdi` file and run the following command depending on your OS :

**Windows:**

```cmd
certUtil -hashfile <machine_name>.vdi sha1
```

**Linux**
``terminal
sha1sum rocky_serv.vdi

**Mac**
``terminal
shasum rocky_serv.vdi

## Resources

### Classical :

**Debian Administrator's Handbook**
https://debian-handbook.info/

**VirtualBox Documentation**
https://www.virtualbox.org/wiki/Documentation

**UFW (Uncomplicated Firewall) Guide**
https://wiki.ubuntu.com/UncomplicatedFirewall

**AppArmor Documentation**
https://wiki.debian.org/AppArmor

### Conventional :

**Setting up LVM**
https://www.youtube.com/watch?v=Ap-S2faYjbA&t=165s

**Apparmor commands**
https://www.youtube.com/watch?v=KYM-Dzivnjs

**AWK**
https://www.youtube.com/watch?v=9YOZmI-zWok&t=1008s

### Usage of AI

**Tasks**: AI was used to clarify the differences between SELinux and AppArmor, and to generate explanations for the "Technical Comparisons" section of this README.

**Parts**: Specifically used for drafting the comparison text between Debian/Rocky and VirtualBox/UTM to ensure technical accuracy regarding their architecture and package management systems.
