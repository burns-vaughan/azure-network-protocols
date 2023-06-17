<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
<p align="center">[in progress]</p>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

Overall, we will be creating 2 VM's and using Wireshark to filter and observe traffic between them using the command line.

Step 1: Create 2 virtual machines in Microsoft Azure<br>
Step 2: Observe ICMP traffic<br>
Step 3: Observe SSH traffic<br>
Step 4: Observe DHCP traffic<br>
Step 5: Observe DNS traffic<br>
Step 6: Observe RDP traffic<br>

<h3>1. Create 2 virtual machines in Microsoft Azure</h3>
The first step is to create 2 virtual machines in Azure. If you've never created a virtual machine in Azure before refer to this project I did that shows detailed step by step instructions how to do it.<br> Otherwise, refer to the steps below.

One VM should be running on Windows 10. The other should running on Linux Ubuntu. Ensure they are using the same virtual network. A virtual network will be created by default when you create your first VM. <br>

So, with the second VM ensure you select the virtual network that has been created. It should select it by default.<br>

It's best to wait for the first VM to be fully created before creating the second VM. That way you can be sure the virtual network has been created.

<h3>2. Observe ICMP traffic</h3>
Now, you should have 2 VM's. I named mine VM-1, and VM-2-Linux. Below, is a screenshot:<br>

<img src="https://i.ibb.co/87zLxQY/2-vms.jpg" alt="2-vms" border="0">

Next, log into the virtual machine running Windows and download WireShark

As a reminder you use Windows Remote Desktop, put the public IP for the VM, and then input your username and password.

Here's a <a href="https://www.wireshark.org/download.html">link</a> to the official download page for Wireshark or just go a Google search. The main thing is ensure you install on the Windows virtual machine not your personal computer.

Below is a screenshot showing the installation of WireShark <br>
<img src="https://i.ibb.co/VVnRMwt/install-wireshark.jpg" alt="install-wireshark" border="0">

After it installs open Wireshark, and click the shark fin icon on the top left to start capturing packets. It will show the traffic on the network. Below, is a screenshot of how it should look:<br>

<img src="https://i.ibb.co/0t91vYB/wireshark2.jpg" alt="wireshark2" border="0">

Filter what packets are being shown by filtering by internet control message protocol (ICMP). To do that go to the search bar at the top of Wireshark and type in ICMP and hit enter. It should show no packets of data. As you may know, ICMP is the network protocol that ping uses to test connections between computers.



<h3>3. Observe SSH traffic</h3>
<h3>4. Observe DHCP traffic</h3>
<h3>5. Observe DNS traffic</h3>
<h3>6. Observe RDP traffic</h3>
