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

<img src="https://imgur.com/a/sAMFmjj" alt="wireshark2" border="0">

Filter what packets are being shown by filtering by internet control message protocol (ICMP). To do that go to the search bar at the top of Wireshark and type in ICMP and hit enter. It should show no packets of data. As you may know, ICMP is the network protocol that ping uses to test connections between computers.

Now to test the connectivity we will ping VM-2 from VM-1. To do that we need to know either the private or public IP address. Since, the 2 VMs are on the same network we can use the private IP. Go into Azure and note down what the private IP is for VM-2. It's found in the network settings in Azure for VM-2. Just below, where it has the public IP.

In my case the private IP for VM-2 is:

10.0.0.5

Here's a screenshot showing where the private IP is located in Azure:<br>

<img src="https://i.ibb.co/6NDsLdg/private-ip-vm-2.jpg">

Next on VM-1 open up powershell or the command line. If you've never done it before go to the start menu and type in powershell, and click on it when it comes up.

We will ping VM-2 to see that it has connectivity. To that type in exactly:<br><br>
ping 10.0.0.5<br><br>

And hit enter. For the IP put in your private IP address for VM-2 if it's different. Below, is a screenshot that shows how it will look:<br>
<img src="https://i.ibb.co/Fm5bKGF/ping-vm-2.jpg" alt="ping-vm-2" border="0">

This shows some detailed information. It shows the source IP address, and destination IP address. Ping automatically executes 4 requests. You will see that all 4 went through.

An interesting exercise to do is to ping well known websites on the internet. Such as, Google.com. To do that type in: <br>
<br>
ping www.google.com -4
<br>
The -4 forces it to use ICMP traffic.
You can see the request go from VM-1 to one of Google's servers and reply back.

Next, we will do some tinkering with the firewall. To do that set up an endless ping from VM-1 to VM-2.
In powershell or the command line type in: <br><br>
ping 10.0.0.5 -t

<br>
The -t causes it to ping forever until you stop it or the connection is disconnected.

Next we will traffic filter to VM-2 to stop ICMP traffic. This can be done a few ways. But, in this case we will do it using the network security group in Azure. To do that go to Network Security Groups for the Azure homescreen. Then click on the network security group for VM-2. Click on inbound rules on the left hand side, and give the following settings (screenshot below), leaving everything else as default: <br><br>

<img src="https://i.ibb.co/7bhtJQg/icmp-filter.jpg" alt="icmp-filter" border="0">

Next go back to VM-1 and see what Wireshark and Powershell or the command line are showing. It should say timed out because ICMP traffic is being blocked. Here's a screenshot:

<img src="https://imgur.com/a/ntMaxaK" border="0">
<img src="https://imgur.com/a/ntMaxaK" alt="icmpfilter2" border="0">
<h3>3. Observe SSH traffic</h3>
<h3>4. Observe DHCP traffic</h3>
<h3>5. Observe DNS traffic</h3>
<h3>6. Observe RDP traffic</h3>
