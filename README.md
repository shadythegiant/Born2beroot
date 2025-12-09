_This project has been created as part of the 42 curriculum by azahidi._

# Born2beRoot

## Description

**Born2beRoot** is a system administration project that aims to introduce the fundamental concepts of virtualization. The goal is to create a secure, minimal server environment using a Virtual Machine (VM). This exercise involves setting up an operating system with strict adherence to security rules, partition management, and script automation.

This project includes the **Bonus** part, which involves setting up a functional WordPress website, implementing a specific partition structure, and configuring an additional service (`Fail2Ban`) to enhance server security.

## Project Description & Design Choices

### Operating System Choice: Debian

For this project, **Debian** (stable) was chosen over Rocky Linux[cite: 78].

- **Reasoning:** Debian is widely recognized for its stability and is highly recommended for those new to system administration[cite: 79]. It utilizes the `apt` package manager and `AppArmor` for security, providing a robust environment for learning server management.

### Technical Comparisons

#### Debian vs. Rocky Linux

- **Debian:** Uses the `.deb` package format and the `apt` package manager. It is community-driven and uses **AppArmor** for mandatory access control[cite: 83].
- **Rocky Linux:** A downstream rebuild of RHEL (Red Hat Enterprise Linux). It uses `.rpm` packages and `dnf`/`yum`. It defaults to **SELinux** and is often preferred in enterprise environments requiring RHEL compatibility[cite: 82].

#### AppArmor vs. SELinux

- **AppArmor (Application Armor):** Used in Debian. It is a path-based Mandatory Access Control (MAC) system that restricts programs to a specific set of files and capabilities. It is generally considered easier to configure than SELinux[cite: 83].
- **SELinux (Security-Enhanced Linux):** Used in Rocky/RHEL. It is a label-based MAC system where every file and process has a security context. It offers fine-grained control but requires complex configuration[cite: 82].

#### UFW vs. firewalld

- **UFW (Uncomplicated Firewall):** The default configuration tool for Debian. It simplifies `iptables` management, making it easy to open/close ports (e.g., allowing port 4242)[cite: 97].
- **firewalld:** The default for Rocky Linux. It uses "zones" and services to manage traffic dynamically without breaking existing connections[cite: 97].

#### VirtualBox vs. UTM

- **VirtualBox:** A mature, cross-platform Type-2 hypervisor developed by Oracle[cite: 21]. It was chosen for its broad compatibility and strict adherence to the project subject.
- **UTM:** A virtualization tool often used on macOS (Apple Silicon) leveraging QEMU, allowing for emulation of x86 architectures on ARM chips.

### Configuration & Security Setup

Per the mandatory and bonus requirements, the server includes:

- **Partitioning (Bonus):** A complex partition scheme using **LVM** (Logical Volume Manager) splitting `/boot`, `/`, `/home`, `/var`, `/srv`, `/tmp`, and `/var/log` to ensure stability and security[cite: 230].
- **SSH:** Service running on port **4242**, with `root` login disabled[cite: 92, 93].
- **User Management:** A user with the login `<login>` belonging to `sudo` and `user42` groups[cite: 107, 108].
- **Password Policy:** Strict rules (expires every 30 days, min 10 chars, uppercase/lowercase/number, etc.) enforced via `libpam-pwquality` [cite: 111-120].
- **Sudo Policy:** Limits authentication attempts, logs inputs/outputs to `/var/log/sudo/`, and uses a custom error message [cite: 124-128].
- **Monitoring:** A custom `bash` script broadcasting system stats (CPU, RAM, LVM, etc.) to all terminals every 10 minutes[cite: 134, 135].

### Bonus Services Implementation

In addition to the mandatory requirements, the following services were installed:

1.  **WordPress Stack:** A functional website set up using **lighttpd**, **MariaDB**, and **PHP**[cite: 231].
2.  **Additional Service (Fail2Ban):**
    - **Choice:** **Fail2Ban** was selected as the additional service[cite: 232].
    - **Reasoning:** Fail2Ban scans log files (e.g., `/var/log/auth.log`) and bans IPs that show malicious signs, such as too many password failures. This creates an essential layer of protection against brute-force attacks on the SSH port (4242) exposed by the server.

## Instructions

Since the virtual machine itself is not submitted to the git repository[cite: 268], the submission consists of the virtual disk signature.

### Signature Verification

To verify that the submitted signature matches the virtual machine's disk, navigate to the folder containing the `.vdi` file and run the following command depending on your OS [cite: 251-261]:

**Windows:**

```cmd
certUtil -hashfile <machine_name>.vdi sha1
```
