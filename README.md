*This project has been created as part of the 42 curriculum by soyamagu.*

# Description

### Project Overview
Born2beRoot is a system administration project focused on setting up a secure Linux server inside a virtual machine.

The main is to construct a virtualized OS environment and to understand how the designed system works internally.



### Operating System Choice

**Selected OS:** Debian

#### Pros and Cons

|        | Debian | Rocky |
|--------|--------|--------|
| pros   | - Large community support<br>- Simpler configuration<br>- Stable and beginner-friendly | - Enterprise-grade (RHEL-compatible)<br>- Strong SELinux integration<br>- Long-term stability |
| cons   | - Less enterprise-oriented<br>- Slower release cycle | - More complex configuration<br>- Steeper learning curve |


### Main Design Choices

**Partitioning**
- Encrypted partitions using LVM
- Logical volumes separated for security and flexibility

**Security Policies**
- Strong password policy (expiration, complexity rules)
- SSH running on port 4242
- Root login via SSH disabled
- Firewall active at boot

**User Management**
- One user with login name
- User added to [sudo] and [user42] groups

**Services Installed**
- SSH
- UFW (Debian)
- monitoring.sh (cron-based system monitoring script)


# Comparisons

### Debian vs Rocky Linux
- Debian: community-driven, simpler, flexible
- Rocky: enterprise-focused, RHEL-based, stronger default security

### AppArmor vs SELinux
- AppArmor: profile-based, simpler to configure (Debian)
- SELinux: label-based, more detailed and powerful (Rocky)

### UFW vs firewalld
- UFW: simplified frontend for iptables. A static tool that applies rules in sequential order. (Debian)
- firewalld: dynamic firewall management that organizes traffic into zones (Rocky)

### VirtualBox vs UTM
- VirtualBox: cross-platform virtualization software
- UTM: virtualization software optimized for macOS (especially Apple Silicon)



# Instructions

### Instruction

- How to Start
  1. Open Oracle VirtualBox.
  2. Select the desired virtual machine and click Start.
  3. Enter your password to unlock the disk encryption.
  4. Enter your username and password to log in.

- How to Shut Down
  1. Run `sudo poweroff`.
  2. Enter your password to shut down the virtual machine.

- How to Reboot
  1. Run `sudo reboot`.
  2. Enter your password to reboot the virtual machine.

- How to get signature
  1. Navigate to the directory containing the `.vdi` file in the terminal.
  2. Run the following command:
  `sha1sum <filename>.vdi`

- Installation/Setup Summary
  - Create a virtual machine using VirtualBox.
  - Install the selected OS (Debian).
  - Configure encrypted LVM partitions.
  - Apply mandatory security and system configurations.
  - Configure users, sudo, SSH, firewall, and password policies.
  - Implement and automate the monitoring script.
  - Generate the VM disk signature and store it in `signature.txt`.

## Command lists
The commands listed below is the useable for checking or modifying OS setting.
#### check os details & install and package management
- `head -n 2 /etc/os-release` : display basic operating system identification
- `dpkg -l` : list all installed packages managed by dpkg
- `/usr/sbin/aa-status` : show the current status of AppArmor profiles

#### strage and partition construction
- `lsblk` : display block devices and their partition hierarchy

#### network management
- `ss -tunlp` : list listening TCP/UDP sockets with process information
- `nano /etc/ssh/sshd_config` : edit the SSH server configuration file

#### hostname management
- `nano /etc/hosts` : file that links ipaddress and hostname
- `hostnamectl set-hostname <new hostname>` : modify hostname

#### user management
- `sudo useradd -m -s /bin/bash <new username>` : create a new user account / -m (create new user folder)/ -s (default shell setting)
- `sudo passwd <username>`  : assign a new user password
- `sudo addgroup <new groupname>` : create a new group
- `sudo adduser <user> <group>` : add an existing user to a group
- `getent group <groupname>` : display group information from the system database
- `getent passwd | awk -F: '$3 == 0 || $3 >= 1000'` : list system and regular user accounts by UID
- `sudo cat /etc/shadow`: display all user passwords

#### sudo configuration
- `nano /etc/sudoers.d/sudo_config` : edit custom sudo configuration rules
  - `sudo visudo` : edit custom sudo configuration rules (/etc/sudoers)
  - `cat /etc/sudoers` : display custom sudo configuration rules
- `sudo sudoreplay -d <file path> -l`: check input log with sudo command (-d: select file path where sudo-io is located)
- `sudo sudoreplay <TSID>`: check output log with particular sudo command

#### password configuration
- `cat /etc/login.defs` : show system-wide password and login policy settings
- `cat /etc/pam.d/common-password` : list PAM configuration files for authentication policies
  - `cat /etc/security/pwquality.conf` : list PAM configuration files for authentication policies

#### ssh management
- `sudo service ssh status` : check the current status of the SSH service
- `sudo nano /etc/ssh/sshd_config` : edit the SSH daemon configuration
  - `sudo nano /etc/ssh/ssh_config` : edit the SSH client configuration

#### ufw management
- `sudo service ufw status` : check the current status of the ufw service
- `sudo ufw status`: display current status and rules
- `sudo ufw allow <new port number>` : allow incoming traffic on a specific port
- `sudo ufw delete <current rule>|<number>` : allow incoming traffic on a specific port

#### cron management
- `sudo systemctl disable cron` : disable the cron service at boot
- `sudo systemctl enable --now cron` : enable and start the cron service immediately
- `sudo systemctl stop cron` : stop the cron service in the current session
- `sudo systemctl start cron` : start the cron service in the current session

# Resources

### AI Usage Disclosure
AI was used as a learning tool. Usage included:
- Verifying understanding of subject requirements
- Clarifying system administration concepts (LVM, AppArmor, sudo, SSH, Firewall)
- Structuring documentation (README organization)

### Documentation & References
- 42 Project Subject PDF (Born2beRoot, version 5.1)
- [Born2beRoot Correction Sheet](https://github.com/Vikingu-del/Born2beRoot/blob/main/README.md)
- [Differences between Debian and Rocky Linux](https://scrapbox.io/kenjiked/Debian%E3%81%A8Rocky%E3%81%AE%E9%81%95%E3%81%84)
- [SELinux vs AppArmor comparison](https://tuxcare.com/blog/selinux-vs-apparmor/)
- [Overview of virtualization types and environments](https://zenn.dev/pictogram/scraps/e19342e781ba2d)
- [Introduction to Linux security mechanisms](https://linux-jp.org/?p=16476)
- [Explanation of user and group management in Linux](https://qiita.com/i13ame/items/bf4b6a0e3485ddc838ce)
- [Host OS–based virtualization and hypervisor-based virtualization](https://www.nttpc.co.jp/column/network/vps.html#:~:text=%E3%83%9B%E3%82%B9%E3%83%88OS%E5%9E%8B%E3%81%A8%E3%81%AF,%E3%82%92%E4%BB%AE%E6%83%B3%E5%8C%96%E3%81%A7%E3%81%8D%E3%81%BE%E3%81%99%E3%80%82)
- [Full disk encryption fundamentals](https://www.purestorage.com/jp/knowledge/full-disk-encryption.html#:~:text=FDE%20%E3%81%A8%E3%81%AF,%E8%A6%8B%E3%81%A6%E3%81%BF%E3%81%BE%E3%81%97%E3%82%87%E3%81%86%E3%80%82)
- [Logical Volume Manager (LVM) concepts and usage](https://www.infra-manual.com/lvm/)
