<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this project, I observed various network traffic transmitted to and from two Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
- Remote Desktop
- Several Command-Line Tools (Powershell, Command Prompts)
- Different Network Protocols (SSH, RDP, DNS, DHCP, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04 (Linux)

<h2>High-Level Steps</h2>

- Create Virtual Machines
- Remote Desktop into the VMs
- Install and Run Wireshark
- Test Connectivity Between VMs
- Alter Network Security Group Settings
- SSH into VM
- Observe DHCP Traffic
- Observe RDP Traffic


<h2>Actions and Observations</h2>

<p>
First, I created two VMs on Azure. One machine was a Linux machine and the other was a Windows 10 machine. Both has two CPUs and were on the same VNET. After that, I downloaded Wireshark on Windows Machine. Here's a link to download Wireshark: https://www.wireshark.org/download.html. Once, it was installed, I opened Wireshark and filtered for ICMP Traffic only. ICMP is a network layer protocol that relays messages concerning network connection issues. Ping uses this protocol. Ping tests connectivity between hosts. When I filtered Wireshark to only capture ICMP packets and ping the private IP address of the Linux machine I visually saw the packets on Wireshark. 
</p>
<p>
<img src="https://i.imgur.com/iFYdOiV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
I inspected each individual packet and saw the actual data that was being sent in each ping. The picture below demonstrated that. 
</p>
<p>
<img src="https://i.imgur.com/tIG5VAl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Next I initiated a perpetual ping in the Linux machine with the command ping -t. This continually ping the machine until I decided to stop it, while the Windows machine is pinging the Linux machine we went back to the Linux machine and blocked inbound ICMP traffic on its firewall. Once I did that, I stopped receiving echo replies from the Linux machine. I went further to deny ICMP traffic by creating a new Network Security Group on the Linux machine that will be set to block ICMP. I can allow traffic by authorizing ICMP on the Linux Network Security Groups page on Azure. 
</p>
<p>
<img src="https://i.imgur.com/aTG3sxe.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/bTuyDQm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/DKZnm6u.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Next, I connected the Windows machine to the Linux machine through SSH (Secure Shell Protocol). SSH has no GUI, it just gives the user access to the machine's CLI. I set the Wireshark filter to capture SSH packets only. When I SSH into the Linux machine with the command prompt "ssh labuser@10.0.0.5" I saw that Wireshark starts to immediately capture SSH packets. For example, when I entered the command "id", I observed that it spammed traffic, transmitting source from the Windows 10 machine to the Linux machine and vice versa . To log out from SSH, I entered the exit command (or Crtl+D Key) and the connection closed.
</p>
<p>
<img src="https://i.imgur.com/cPvIvNN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Next, I used Wireshark to filter for DHCP. DHCP is the Dynamic Host Configuration Protocol. This works on ports 67/68. It's used to assign IP addresses to machines. I requested for a new IP address with the command "ipconfig /renew". Once I entered the command, Wireshark captured DHCP traffic and got my IP address reissued to me again. 
</p>
<p>
<img src="https://i.imgur.com/rJxQlMd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
I observe some DNS traffic. I set Wireshark to filter DNS traffic. I initiated DNS traffic by typing in the command "nslookup www.google.com" this command essentially asks the DNS server what is Google's IP address. And it returned some of the IP adresses google uses.
</p>
<p>
<img src="https://i.imgur.com/fCLX8ZZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Lastly, I observe Remote Desktop Protocol (RDP) traffic on the VM. When I entered tcp.port==3389, I observe that the traffic is spamming non-stop and that's because there's a live active RDP interaction from my actual host computer to the Virtual Machine.
</p>
<p>
<img src="https://i.imgur.com/acWI0Hj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

This project was interesting and fun navigating through various network protocols and using powershell commands.
