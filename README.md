# 🖥️ Peer-to-Peer Network & Remote Access ## 📌 Overview This project demonstrates how to set up a **peer-to-peer network** between Windows 10 and Linux (Ubuntu) in VMware, create user accounts, manage processes, and automate software installation using scripts. --- ## 🚀 1. Setting Up a Peer-to-Peer Network To establish a stable connection between Windows and Linux, you need to **configure a static IP address**. ### 🔹 **VMware Network Configuration** 1. Open **VMware Workstation** 2. Select your virtual machine (Windows/Linux) → **Settings** → **Network Adapter** 3. Choose a network mode: - **Bridged** (recommended) for direct connection to the local network - **NAT** if you want to share the host’s connection 4. **Save and restart your virtual machines** ### 🔹 **Configuring Static IP on Windows 10** 1. Open **Network Connections** (`Win + R → ncpa.cpl → Enter`) 2. Select **Ethernet → Properties** 3. Choose **Internet Protocol Version 4 (TCP/IPv4) → Properties** 4. Enter the following details:
IP Address: 192.168.1.150
Subnet Mask: 255.255.255.0
Gateway: 192.168.1.1
DNS: 8.8.8.8
5. **Click OK and restart the adapter** ### 🔹 **Configuring Static IP on Ubuntu** Edit the **Netplan** configuration: ```bash sudo nano /etc/netplan/01-netcfg.yaml
Add the following configuration:
network: version: 2 renderer: networkd ethernets: ens33: dhcp4: no addresses: [192.168.1.101/24] gateway4: 192.168.1.1 nameservers: addresses: [8.8.8.8, 8.8.4.4]
Save (Ctrl + X → Y → Enter) and apply changes:
sudo netplan apply
👤 2. Creating Administrator & User Accounts
🔹 Windows (PowerShell)
# Create a standard user New-LocalUser -Name "user1" -Password (ConvertTo-SecureString "Pass123!" -AsPlainText -Force) -FullName "User One" -Description "Standard User" # Create an administrator New-LocalUser -Name "admin" -Password (ConvertTo-SecureString "AdminPass!" -AsPlainText -Force) -FullName "Administrator" -Description "System Administrator" # Add the admin user to the Administrators group Add-LocalGroupMember -Group "Administrators" -Member "admin"
🔹 Ubuntu (Terminal)
# Create a user sudo adduser user1 # Add a user to the sudo (admin) group sudo usermod -aG sudo user1
⚙️ 3. Managing Processes
🔹 Viewing Running Processes
• Windows (PowerShell):
Get-Process
• Ubuntu (Terminal):
ps aux
🔹 Killing a Process
• Windows (PowerShell):
Stop-Process -Name notepad -Force
• Ubuntu (Terminal):
sudo kill -9 <PID>
🔒 4. Setting Permissions
🔹 Windows (PowerShell)
• Set read-only permissions for a user on a folder:
icacls C:\SecureFolder /grant user1:R
• Grant full access to a group:
icacls C:\SharedFolder /grant "Users":F
🔹 Ubuntu (Terminal)
• Allow read-only access:
chmod 444 /home/user1/Documents
• Grant full access to a group:
sudo chown -R :developers /var/www sudo chmod -R 770 /var/www
📜 5. Automating Software Installation
🔹 Windows (PowerShell)
Install software via a script:
# Install Notepad++ using Chocolatey choco install notepadplusplus -y
🔹 Ubuntu (Bash)
Create a script to install software:
#!/bin/bash sudo apt update && sudo apt install -y vim
Make it executable and run it:
chmod +x install.sh ./install.sh
🛠️ Useful Commands
🔹 Network & Connectivity
• Check network interfaces:
ip a
• Ping a machine:
ping 192.168.1.150
• View routing table:
route -n
• Restart networking service:
sudo systemctl restart NetworkManager
🔹 System Information & Monitoring
• Check system resource usage:
top
• View disk space:
df -h
• Check memory usage:
free -m
🔹 User & Permission Management
• List all users:
cat /etc/passwd
• Check user groups:
groups user1
• Change file owner:
sudo chown user1:user1 filename
