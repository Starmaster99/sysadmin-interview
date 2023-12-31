* Explain the Linux boot process, from BIOS/UEFI to user space.
	First, there is a power on BIOS/UEFI. Then, the firmware runs POST to test the integrity of the machine. 
	After successful completion, BIOS/UEFI seeks for a boot device (typically a bootable USB, HDD or SSD). BIOS looks for the first sector of the boot device, and UEFI - for EFI partition, specifically for .efi file. 
	Next, the firmware launches a bootloader (typically GRUB2), which itself launches a kernel. If there are more than one, user can choose what kernel to load.
	After the kernel is loaded, it starts to decompress itself, perform hardware checks, gain access to vital hardware and run the init process - a parent of all the other processes, which means running a Systemd process, as systemd is the most widespread init system in corporate Linux environment. 
	Systemd probes all the hardware, mounts filesystems, initiates and terminates services, manages essential system processes like user login and runs a desktop environment.
	Basically,
	BIOS/UEFI -> boot device -> GRUB2 -> kernel -> systemd
* Describe the purpose and structure of the /etc/passwd, /etc/shadow and /etc/group files.
	~~~ /etc/passwd ~~~
	Basically, this file stores user account information. It determines who has what rights. 
	So, we have an entry in /etc/passwd:
	backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
	   1  :2: 3: 4:   5  :      6     :       7
	Here, we can see data sepated by colons. 
	1) Username: determines what access level user has upon logging in
	2) x: encrypted password, stored in /etc/shadow. You can change one's password by typing `passwd (user)`
	3) Used ID: a unique ID that must be signed to every user. UID 0 is reserved for root and UID 1-99 are used for system accounts. Further are used for normal users.
	4) Group ID: a primary group ID (stored in /etc/group)
	5) GECOS ID: a legacy user ID containing additional information about user
	6) Home directory: absolute path to the directory where user stores files
	7) Default shell: user's default shell that starts after user log in

	~~~ /etc/shadow ~~~
	Same as /etc/passwd, but stores secure things.
	mail:*:19478:0:99999:7:::
	  1 :2:  3  :4:  5  :6:7:8:
	1) Username: must be same as username in /etc/passwd, otherwise this is another user
	2) Password: stored in an encrypted format: $id$salt$hashed.
	Here, $id is hashing algorith.
	Asterisk means no password is needed.
	3) Last password change (day): the amount of days passed since 1 Jan 1970.
	0 means password change upon login.
	4) Minimum number of days between password change. 0 means no password age.
	5) Maximum number of days password being valid.
	6) Warn: number of days to warn before password is to expire

	~~~ /etc/group ~~~
	Defines the groups to which users belong.
	netdev:x:116:think
	   1  :2: 3 :  4
	1) Name of the group
	2) Password
	3) Group ID
	4) List of users assigned to the group
	To apply changes made in these files, log out and log in. You can also logout everyone forcefully, but don't forget to warn.

* What is /etc/shells?
	A file containing list of valid shells to use in shell scripts.
	Typically, you would use /usr/bin/env bash as you know for sure there will be a shell.
* How does the Linux kernel manage processes? Explain the role of fork, exec, and wait.
	
* What is the purpose of the /proc filesystem, and how is it used for process management?
	A pseudo-filesystem that gets mounted and populated every time computer launches. Used as an interface to kernel data structures.
	There are directories (filenames of whose correspond to their process IDs) storing information about process and other important files needed by kernel such as cpuinfo, vmstat and others.
	This filesystem is dymanic, meaning reading from them provides real-time data. Programs such as GNU top use the information in these files to get the current state of the system.
	It is also important to mention files in /proc don't take any space on hard drive since the whole directory is stored in RAM.
* Differentiate between ext3, ext4, XFS, and Btrfs filesystems. When would you choose one over the other?
	Basically, ext4 is a successor of ext3. It has many useful built-in features such as faster filesystem checks, 1EiB max filesystem size and 16TB max single file size, nanosecond timestamps and encryption support. Ext4 is also a default Linux filesystem as it is the most stable and widely supported one.
	XFS is a default filesystem for RHEL 7 and above. It's also not limited to RHEL distributions. It is optimized for high-performance machines and doesn't do that well for a general purpose usage. Works best with large filesizes and high IOPS disks.
	Btrfs is generally used for NAS and SAN environments as it can span across multiple disks, whereas ext4 and xfs can't.
* Explain the concept of inodes and how they are used in file systems.	
* Describe the purpose and configuration of LVM (Logical Volume Manager).
* How do you troubleshoot disk I/O performance issues on a Linux system?
* Explain the role of /etc/hosts and /etc/resolv.conf in DNS resolution.
* Describe the purpose of iptables and firewalld in Linux firewall management.
* How do you troubleshoot network connectivity issues on a Linux server?
* Explain the significance of /etc/nsswitch.conf in name resolution.
* Discuss the differences between symmetric and asymmetric encryption. When would you use each?
* How do you manage user access and permissions on a Linux system?
* Explain the concept of SELinux and AppArmor. How do they enhance system security?
* What steps would you take to secure an SSH server?
* Provide an example of a complex shell script you've written for system automation.
* How do you schedule tasks in Linux? Compare cron and systemd timers.
* Explain the purpose and use of awk and sed in text processing.
* How do you monitor system resource usage? What tools do you use?
* Discuss the significance of load average in the context of system performance.
* What methods do you use to optimize the performance of a Linux server?
* Compare yum and apt package managers. When would you choose one over the other?
* How do you manage dependencies when installing software on a Linux system?
* Explain the purpose and use of the rpm and dpkg commands.
* Describe a challenging system issue you've encountered and how you resolved it.
* What steps would you take to recover a Linux system after a critical failure?
* How do you analyze system logs and interpret error messages?
* Explain the concept of high availability in the context of Linux servers.
* How do you set up and manage a Pacemaker/Corosync cluster for high availability?
* Compare KVM and Docker virtualization technologies. When would you choose one over the other?
* Describe the components of a typical Docker container and their purposes.
* How do you implement backup strategies for critical system data?
* Explain the significance of incremental and differential backups. When would you use each?
* Explain the purpose of the /proc directory in Linux.
* Differentiate between hard links and soft links in Linux.
* How does the chroot command work, and what is its use case?
* Discuss the advantages and disadvantages of using a monolithic kernel versus a microkernel.
* What is an LTS kernel?
* Explain how the systemctl command is used to manage services in a systemd-based system.
* Describe the role of the at and batch commands for job scheduling.
* How do you configure and use sudo to grant elevated privileges to users?
* Explain the concept of cgroups and their role in resource management.
* What is the purpose of the /etc/fstab file, and how do you modify it?
* Discuss the differences between TCP and UDP protocols. Provide use cases for each.
* How do you secure a Linux server against common security threats? Mention specific measures.
* Explain the purpose of the tcpdump and Wireshark tools in network troubleshooting.
* Discuss the significance of the /etc/securetty file in enhancing security.
* What is the purpose of the umask command, and how does it impact file permissions?
* How do you manage and monitor swap space in a Linux system?
* Explain the role of the journalctl command in viewing system logs.
* Discuss the advantages and disadvantages of using RAID configurations. Differentiate RAID levels.
* How do you set up and configure a simple web server using Apache or Nginx?
* Describe the role of a package manager in Linux, and how does it resolve dependencies?
* What is the purpose of the ssh-keygen command, and how do you use it to manage SSH keys?
* Explain the differences between TCP wrappers and firewalls for network security.
* How do you configure and use the crontab command for job scheduling?
* Discuss the role of the GRUB bootloader in the Linux boot process.
* Explain how you would recover a forgotten root password on a Linux system.
* What is the purpose of the /etc/nsswitch.conf file, and how is it configured?
* How do you use the sar command to monitor system performance over time?
* Describe the steps involved in compiling and installing software from source code.
* Explain how you would implement and manage user quotas on a Linux file system.
* Discuss the role of the sysctl command in managing kernel parameters.
* How do you use scp and rsync to transfer files securely between Linux systems?
* Explain the purpose of the /etc/sysconfig directory and its common use cases.
* How do you use the netstat and ss commands for network monitoring?
* Discuss the differences between IPv4 and IPv6 addressing. How do you enable IPv6 on a Linux system?
* Explain the Linux boot process.
* What is the purpose of the /etc/passwd file?
* Differentiate between a process and a thread.
* Explain the significance of /var/log directory.
* Describe the purpose of the /etc/fstab file.
* How do you extend a file system in Linux?
* Explain the differences between ext3 and ext4 file systems.
* What is LVM, and how is it used in disk management?
* Explain the purpose of the /etc/hosts file.
* How do you troubleshoot network connectivity issues?
* What is a subnet mask, and how is it used?
* Differentiate between TCP and UDP.
* How do you secure SSH access to a server?
* Explain the role of SELinux or AppArmor in enhancing security.
* Discuss methods to protect against DDoS attacks.
* What is two-factor authentication, and how can it be implemented?
* How do you monitor system resource usage?
* Explain the output of the top command.
* What is the purpose of the vmstat command?
* Discuss the significance of system logs.
* Write a Bash script to find and print all files modified in the last 24 hours.
* How do you schedule a task to run at a specific time using cron?
* Explain the use of awk and sed in text processing.
* Discuss the purpose of the shebang (#!) in scripts.
* How do you install software on a Debian-based system?
* Explain the differences between apt-get and aptitude.
* What is the purpose of the rpm command in Red Hat-based systems?
* How do you backup and restore a MySQL database?
* Explain the differences between InnoDB and MyISAM storage engines.
* Discuss the role of indexes in a database.
* What is Docker, and how does it work?
* Explain the concept of container orchestration.
* How would you scale a Docker container horizontally?
* Describe the key components of a cloud architecture.
* How do you deploy a virtual machine on a cloud platform?
* Explain the differences between IaaS, PaaS, and SaaS.
* How does Git differ from other version control systems?
* Explain the Git workflow: commit, push, pull, merge.
* What is the purpose of the .gitignore file?
* How do you handle a server running out of disk space?
* What steps would you take to troubleshoot a slow-performing server?
* How do you collaborate with other team members using Git?
* What is the importance of documentation in system administration?
* Discuss the benefits of using a Wiki for documentation.
* Have you worked with container orchestration tools like Kubernetes?
* What is serverless computing, and how does it work?
* How do you ensure high availability in a distributed system?
* Explain the role of the Linux kernel in system operation.
* How does the kernel handle process scheduling and priority?
* Discuss the purpose and usage of iptables and nftables.
* Explain the Linux bridging and bonding mechanisms.
* How do you configure and troubleshoot VLANs in Linux?
* Describe the steps to perform Linux server hardening.
* Explain the Principle of Least Privilege (PoLP) and its application to Linux.
* How do you implement and monitor Mandatory Access Controls (MAC) on Linux?
* Explain the purpose and benefits of RAID in disk management.
* Discuss the concept of disk encryption on Linux.
* How do you optimize disk I/O performance on a Linux server?
* Discuss advanced techniques for optimizing system performance.
* Explain the usage of perf for Linux performance analysis.
* How do you identify and mitigate memory leaks in a running system?
* Describe the advantages of Infrastructure as Code (IaC).
* How do you implement and manage Ansible roles and playbooks?
* What is Ansible dynamic inventory?
* Discuss the use of Terraform in managing infrastructure.
* Write a script to automate the deployment of a complex service stack.
* Explain the usage of Python in system administration tasks.
* Discuss the benefits and challenges of writing system utilities in C.
* Explain the concept of reverse proxy servers and their use in web architecture.
* Discuss advanced configurations for Nginx and Apache.
* How do you implement and manage SSL/TLS certificates for a web server?
* Discuss advanced MySQL performance tuning techniques.
* Explain the steps to perform a hot backup of a PostgreSQL database.
* How do you handle replication and failover in a database cluster?
* Discuss the challenges of deploying microservices in a containerized environment.
* How do you ensure security and isolation between containers?
* Describe strategies for monitoring and scaling containers in Kubernetes.
* Explain the principles of serverless computing and its applications.
* How do you design a multi-region, highly available cloud architecture?
* Discuss strategies for cost optimization in cloud environments.
* Discuss branching strategies and Git flow in a collaborative environment.
* How do you resolve complex Git merge conflicts?
* Explain the use of Git hooks for automation in version control.
* Describe your approach to troubleshooting a kernel panic.
* How do you analyze and resolve network latency issues?
* Discuss strategies for debugging and fixing complex system crashes.
* How do you coordinate the deployment of infrastructure changes across teams?
* Discuss strategies for maintaining accurate and up-to-date documentation in a dynamic environment.
* How do you prepare for the integration of quantum-safe cryptography in a Linux environment?
* Discuss the potential impact of edge computing on Linux system administration.
* What are your thoughts on the adoption of artificial intelligence in managing Linux infrastructure?
* What are rogue processes?
* What are zombie processes?
* What are the differences between Linux and Windows OS?
* Explain the differences between a UNIX and LINUX system?
* What are the basic elements of LINUX?
* Can you explain what LILO is?
* What is D-Bus?
* Describe a service that you might disable on a LINUX server.
* How would you check memory and CPU statistics?
* Why would Logical Volume Manager (LVM) be required?
* Where are SAR logs stored?
* Describe how you would reduce the size of an LVM partition.
* Describe how you would increase the size of an LVM partition.
* Where would you locate kernel modules?
* Explain the different network bonding modes used in LINUX.
* What would you do to enhance the security of password files stored in LINUX?
* Describe which shell you would assign to POP3 mail-only account.
* Explain how you would create a partition from a raw disk.
* How is the umask command used in a LINUX system?
* Explain how you would assign the umask to a user permanently.
* What would you do to change the default run level?
* Describe your process for creating an ext4 file system.
* How would you use NFS to share a directory?
* What is a "/proc" file system?
* What are some differences between an ext2 and ext3 file system?
* Can you explain the differences between DOS and BASH?
* Explain the meaning of CLI.
* What is the meaning of GUI?
* What is LFS?
* How would you use Terminal to create a file in LINUX?
* What function does DNS play on a network?
* What is HTTP?
* What is an HTTP proxy and how does it work?
* Describe briefly how HTTPS works.
* What is SMTP? Give the basic scenario of how a mail message is delivered via SMTP.
* What is RAID? What is RAID0, RAID1, RAID5, RAID10?
* What is a level 0 backup? What is an incremental backup?
* Describe the general file system hierarchy of a Linux system.
* Which difference have between public and private SSH key?
* What is the name and the UID of the administrator user?
* How to list all files, including hidden ones, in a directory?
* What is the Unix/Linux command to remove a directory and its contents?
* Which command will show you free/used memory? Does free memory exist on Linux?
* How to search for the string "my konfu is the best" in files of a directory recursively?
* How to connect to a remote server or what is SSH?
* How to get all environment variables and how can you use them?
* I get "command not found" when I run ifconfig -a. What can be wrong?
* What happens if I type TAB-TAB?
* What command will show the available disk space on the Unix/Linux system?
* What commands do you know that can be used to check DNS records?
* What Unix/Linux commands will alter a files ownership, files permissions?
* What does chmod +x FILENAME do?
* What does the permission 0750 on a file mean?
* What does the permission 0750 on a directory mean?
* How to add a new system user without login permissions?
* How to add/remove a group from a user?
* What is a bash alias?
* How do you set the mail address of the root/a user?
* What does CTRL-c do?
* What does CTRL-d do?
* What does CTRL-z do?
* What is in /etc/services?
* How to redirect STDOUT and STDERR in bash? (> /dev/null 2>&1)
* What is the difference between UNIX and Linux.
* What is the difference between Telnet and SSH?
* Explain the three load averages and what do they indicate. What command can be used to view the load averages?
* Can you name a lower-case letter that is not a valid option for GNU ls?
* What is a Linux kernel module?
* Walk me through the steps in booting into single user mode to troubleshoot a problem.
* Walk me through the steps you'd take to troubleshoot a 404 error on a web application you administer.
* What is ICMP protocol? Why do you need to use?
* What are:
  tee
  awk
  tr
  cut
  tac
  curl
  wget
  watch
  head
  tail
  less
  cat
  touch
  sar
  netstat
  tcpdump
  lsof
* What does an & after a command do?
* What does & disown after a command do?
* What is a packet filter and how does it work?
* What is Virtual Memory?
* What is memory dump?
* What is swap and what is it used for?
* What is an A record, an NS record, a PTR record, a CNAME record, an MX record?
* Are there any other RRs and what are they used for?
* What is a Split-Horizon DNS?
* What is the sticky bit?
* What does the immutable bit do to a file?
* What is the difference between hardlinks and symlinks? What happens when you remove the source to a symlink/hardlink?
* What is an inode and what fields are stored in an inode?
* How to force/trigger a file system check on next reboot?
* What is SNMP and what is it used for?
* What is a runlevel and how to get the current runlevel?
* What is SSH port forwarding?
* What is the difference between local and remote port forwarding?
* What are the steps to add a user to a system without using useradd/adduser?
* What is MAJOR and MINOR numbers of special files?
* Describe the mknod command and when you'd use it.
* Describe a scenario when you get a "filesystem is full" error, but 'df' shows there is free space.
* Describe a scenario when deleting a file, but 'df' not showing the space being freed.
* Describe how 'ps' works.
* What happens to a child process that dies and has no parent process to wait for it and what’s bad about this?
* Explain briefly each one of the process states.
* How to know which process listens on a specific port?
* What is a zombie process and what could be the cause of it?
* You run a bash script and you want to see its output on your terminal and save it to a file at the same time. How could you do it?
* Explain what echo "1" > /proc/sys/net/ipv4/ip_forward does.
* Describe briefly the steps you need to take in order to create and install a valid certificate for the site https://foo.example.com.
* Can you have several HTTPS virtual hosts sharing the same IP?
* What is a wildcard certificate?
* Which Linux file types do you know?
* What is the difference between a process and a thread? And parent and child processes after a fork system call?
* What is the difference between exec and fork?
* What is "nohup" used for?
* What is the difference between these two commands?
myvar=hello
export myvar=hello
* How many NTP servers would you configure in your local ntp.conf?
* What does the column 'reach' mean in ntpq -p output?
* You need to upgrade kernel at 100-1000 servers, how you would do this?
* How can you get Host, Channel, ID, LUN of SCSI disk?
* How can you limit process memory usage?
* What is bash quick substitution/caret replace(^x^y)?
* Do you know of any alternative shells? If so, have you used any?
* What is a tarpipe (or, how would you go about copying everything, including hardlinks and special files, from one server to another)?
* How can you tell if the httpd package was already installed?
* How can you list the contents of a package?
* How can you determine which package is better: openssh-server-5.3p1-118.1.el6_8.x86_64 or openssh-server-6.6p1-1.el6.x86_64 ?
* Can you explain to me the difference between block based, and object based storage?
* What is a tunnel and how you can bypass a http proxy?
* What is the difference between IDS and IPS?
* What shortcuts do you use on a regular basis?
* What is the Linux Standard Base?
* What is an atomic operation?
* Your freshly configured http server is not running after a restart, what can you do?
* What kind of keys are in ~/.ssh/authorized_keys and what it is this file used for?
* I've added my public ssh key into authorized_keys but I'm still getting a password prompt, what can be wrong?
* Did you ever create RPM's, DEB's or solaris pkg's?
* What does :(){ :|:& };: do on your system?
* How do you catch a Linux signal on a script?
* Can you catch a SIGKILL?
* What's happening when the Linux kernel is starting the OOM killer and how does it choose which process to kill first?
* Describe the linux boot process with as much detail as possible, starting from when the system is powered on and ending when you get a prompt.
* What's a chroot jail?
* When trying to umount a directory it says it's busy, how to find out which PID holds the directory?
* What's LD_PRELOAD and when it's used?
* You ran a binary and nothing happened. How would you debug this?
* What are cgroups? Can you specify a scenario where you could use them?
* How can you remove/delete a file with file-name consisting of only non-printable/non-type-able characters?
* How can you increase or decrease the priority of a process in Linux?
* A running process gets EAGAIN: Resource temporarily unavailable on reading a socket. How can you close this bad socket/file descriptor without killing the process?
* What do you control with swapiness?
* How do you change TCP stack buffers? How do you calculate it?
* What is Huge Tables? Why isn't it enabled by default? Why and when use it?
* What is LUKS? How to use it?
* What is localhost and why would ping localhost fail?
* What is the similarity between "ping" & "traceroute" ? How is traceroute able to find the hops.
* What is the command used to show all open ports and/or socket connections on a machine?
* Is 300.168.0.123 a valid IPv4 address?
* Which IP ranges/subnets are "private" or "non-routable" (RFC 1918)?
* What is a VLAN?
* What is ARP and what is it used for?
* What is the difference between TCP and UDP?
* What is the purpose of a default gateway?
* What is command used to show the routing table on a Linux box?
* A TCP connection on a network can be uniquely defined by 4 things. What are those things?
* When a client running a web browser connects to a web server, what is the source port and what is the destination port of the connection?
* How do you add an IPv6 address to a specific interface?
* You have added an IPv4 and IPv6 address to interface eth0. A ping to the v4 address is working but a ping to the v6 address gives you the response sendmsg: operation not permitted. What could be wrong?
* What is SNAT and when should it be used?
* Explain how could you ssh login into a Linux system that DROPs all new incoming packets using a SSH tunnel.
* How do you stop a DDoS attack?
* How can you see content of an ip packet?
* What is IPoAC (RFC 1149)?
* What will happen when you bind port 0?
* Can you describe your workflow when you create a script?
* What is GIT?
* What is a dynamically/statically linked file?
* What does "./configure && make && make install" do?
* What is puppet/chef/ansible used for?
* What is Nagios/Zenoss/NewRelic used for?
* What is Jenkins/TeamCity/GoCI used for?
* What is the difference between Containers and VMs?
* How do you create a new postgres user?
* What is a virtual IP address? What is a cluster?
* How do you print all strings of printable characters present in a file?
* How do you find shared library dependencies?
* What is Automake and Autoconf?
* ./configure shows an error that libfoobar is missing on your system, how could you fix this, what could be wrong?
* What are the advantages/disadvantages of script vs compiled program?
* What's the relationship between continuous delivery and DevOps?
* What are the important aspects of a system of continuous integration and deployment?
* How would you enable network file sharing within AWS that would allow EC2 instances in multiple availability zones to share data?
* Explain the Linux kernel boot process in detail.
* How does the Linux kernel manage memory, and what is the role of swap space?
* Describe the purpose and functionality of the init process.
* Explain the purpose and use of iptables and nftables. When would you prefer one over the other?
* How does DNS resolution work in Linux, and how can you troubleshoot DNS-related issues?
* Discuss the differences between TCP and UDP. In what scenarios would you prefer one over the other?
* Describe the Linux Security Modules (LSM) framework and common modules like AppArmor and SELinux.
* How would you secure SSH access to a Linux server? Discuss best practices.
* Explain the concept of chroot jails and when you might use them.
* How does journaling work in file systems, and what are the benefits?
* Explain the purpose and use of LVM (Logical Volume Manager).
* Provide examples of how you would automate system tasks using shell scripts.
* How do you monitor system performance, and what tools or commands would you use for this purpose?
* Describe your experience with configuration management tools like Ansible or Puppet.
* A server is experiencing high load. Outline the steps you would take to identify and resolve the issue.
* Discuss common causes of a server becoming unresponsive and the steps to troubleshoot.
* How do you analyze system logs for security incidents or performance issues?
* Explain the concept of high availability. How would you design a highly available system architecture?
* Discuss the role of heartbeat and Pacemaker in Linux high availability clustering.
* How would you handle data replication and synchronization in a clustered environment?
* Describe your approach to creating and verifying backups. How do you ensure data integrity?
* Discuss strategies for disaster recovery and the steps involved in restoring a system from backups.
* Have you worked with snapshot-based backups, and what considerations are important?
* Explain the differences between virtualization and containerization.
* How would you secure containerized applications, and what tools or practices would you use?
* Share your experience with hypervisors like KVM or Xen.
* How do you optimize the performance of a Linux server for specific workloads?
* Discuss the role of the nice and renice commands in managing process priorities.
* Share your experience with optimizing disk I/O and network performance.
