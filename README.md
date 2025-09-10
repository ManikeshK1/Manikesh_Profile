# IdBhsiM.github.io

This tutorial is designed for a group of five students to create a small computer network cluster. The goal is to set up a network where you can perform basic but fundamental networking tasks: pinging, transferring data, and using SSH to execute commands remotely.
Group Roles (5 students)
To ensure a smooth process, assign the following roles:
 * Network Architect: Responsible for the overall network design, IP address scheme, and ensuring connectivity.
 * Server Administrator (2 students): One student for each of the two server machines. They will handle the installation and configuration of the operating system and necessary services.
 * Client Administrator: Responsible for setting up and configuring the client machine and testing connectivity.
 * Documentation & Testing: This person will record all the steps, IP addresses, and commands used. They will also be in charge of verifying that all the tasks (ping, data transfer, SSH) are working correctly.
Project Overview
You will be creating a simple star-topology network with:
 * 1x Switch/Router: The central point of your network.
 * 2x Server Machines: These will be the targets for your networking tasks.
 * 2x Client Machines: These will be used to initiate connections to the servers.
Materials Needed
 * 5x Computers: Each with an Ethernet port. It's recommended that they all run a Linux distribution (like Ubuntu Desktop or Fedora) for a consistent experience.
 * 1x Network Switch: A small 5-port or 8-port switch will be perfect.
 * 5x Ethernet Cables: One for each computer to connect to the switch.
Part 1: Network Design & IP Planning (Role: Network Architect)
Before you touch any hardware, you need a plan.
 * Choose a Private IP Address Range: A common choice is the 192.168.1.0/24 subnet. This means all your devices will have an IP address starting with 192.168.1.x.
 * Assign Static IP Addresses: To avoid confusion and ensure stability, assign a fixed IP address to each machine. Fill out the table below as a group.
| Machine Name | Role | IP Address | MAC Address (Optional) |
|---|---|---|---|
| Server1 | Server | 192.168.1.10 |  |
| Server2 | Server | 192.168.1.11 |  |
| Client1 | Client | 192.168.1.20 |  |
| Client2 | Client | 192.168.1.21 |  |
| Switch | N/A | 192.168.1.1 (Optional) |  |
 * Note: The subnet mask for all devices will be 255.255.255.0.
Part 2: Hardware Setup (All Roles)
 * Connect the Switch: Place the network switch in a central location.
 * Connect the Computers: Use the Ethernet cables to connect the Ethernet port of each of the five computers to a port on the switch.
 * Power On: Turn on all the computers and the network switch.
Part 3: Software & Network Configuration (Roles: Server & Client Admins)
This is the most critical part. You will configure the network settings on each machine.
 * Configure Static IP Addresses:
   * GUI Method (e.g., Ubuntu Desktop):
     * Go to "Settings" -> "Network."
     * Click the gear icon next to your wired connection.
     * Go to the "IPv4" tab.
     * Select "Manual" for the IPv4 method.
     * Enter the IP Address, Netmask (255.255.255.0), and Gateway (192.168.1.1 - assuming this is your router, or you can leave it blank for a simple LAN).
     * Click "Apply."
   * Command Line Method (e.g., Ubuntu Server):
     * The configuration file is typically /etc/netplan/01-netcfg.yaml or similar.
     * Edit the file using sudo nano /etc/netplan/01-netcfg.yaml.
     * Modify the file to look something like this (adjust for your machine):
       network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:  # Replace with your actual interface name (use ip a to find it)
      dhcp4: no
      addresses: [192.168.1.10/24]
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4] # Optional, for internet access

     * Apply the changes with sudo netplan apply.
 * Install SSH Server:
   * On both Server1 and Server2, install the OpenSSH server.
   * Open a terminal and run: sudo apt update && sudo apt install openssh-server -y
   * The service should start automatically.
 * Install FTP/SFTP Server (for data transfer):
   * SFTP is built into SSH, so you don't need a separate installation. You'll use the scp command.
   * If you want to use a classic FTP server, install vsftpd on both servers: sudo apt install vsftpd -y. Note: SFTP is more secure and is the recommended method.
Part 4: Testing & Verification (Role: Documentation & Testing)
Now, it's time to test your cluster. Use the following commands from the client machines.
Task 1: Pinging
Objective: Verify basic network connectivity.
 * From Client1, open a terminal.
 * Ping Server1: ping 192.168.1.10
 * Ping Server2: ping 192.168.1.11
 * Ping Client2: ping 192.168.1.21
 * Expected Result: You should see successful replies (e.g., 64 bytes from 192.168.1.10: icmp_seq=1 ttl=64 time=0.21 ms). If you get Destination Host Unreachable, re-check your IP configuration.
Task 2: SSH Commands
Objective: Remotely log in and execute commands on the servers.
 * From Client1, open a terminal.
 * Log in to Server1: ssh username@192.168.1.10 (Replace username with the username on Server1).
 * The first time, you will be asked to confirm the authenticity. Type yes.
 * Enter the password for the Server1 user.
 * Execute a command: Once logged in, try running a command, like ls -l or hostname.
 * Log out with exit.
 * Repeat the process to SSH into Server2 from Client2: ssh username@192.168.1.11
Task 3: Data Transfer (using SCP)
Objective: Securely transfer a file from a client to a server.
 * From Client1, create a test file: echo "Hello, Server1!" > testfile.txt
 * Copy the file to Server1:
   scp testfile.txt username@192.168.1.10:/home/username/
 * You will be prompted for the password for the user on Server1.
 * Verify the transfer:
   * SSH into Server1: ssh username@192.168.1.10
   * List the files: ls -l
   * You should see testfile.txt. You can also view its content with cat testfile.txt.
Troubleshooting
 * "ping: Destination Host Unreachable" or "ping: Network is unreachable": Check your Ethernet cable connections and ensure the IP address is configured correctly with the right subnet mask.
 * "ssh: connect to host 192.168.1.10 port 22: Connection refused": The SSH server is likely not running on the server machine. Run sudo systemctl status ssh on the server to check, and sudo systemctl start ssh to start it.
 * "ssh: Connection timed out": A firewall might be blocking the connection. If you are using ufw (Uncomplicated Firewall) on Ubuntu, you may need to allow SSH traffic: sudo ufw allow ssh
Conclusion
Congratulations! Your group has successfully set up a basic computer network. You have configured static IP addresses, installed necessary services, and verified network connectivity. This foundational project demonstrates the core concepts of IP addressing, client-server architecture, and remote access, which are essential building blocks for more complex network tasks.
