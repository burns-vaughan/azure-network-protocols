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
The first step is to create 2 virtual machines in Azure. If you've never created a virtual machine in Azure before refer to <a href="https://github.com/burns-vaughan/virtual-machine-azure">this project</a> I did that shows detailed step by step instructions how to do it.<br> Otherwise, refer to the steps below.

One VM should be running on Windows 10. The other should running on Linux Ubuntu. Ensure they are using the same virtual network. A virtual network will be created by default when you create your first VM either the Windows or Ubuntu virtual machine. By default it will create a new virtual network. 

The second VM ensure you select the virtual network that has been created. It should select it by default.<br>

It's best to wait for the first VM to be fully created before creating the second VM. That way you can be sure the virtual network has been created.

<h3>2. Observe ICMP traffic</h3>
Now, you should have 2 VM's. I named mine VM-1, and VM-2-Linux. Below, is a screenshot:<br>

<img src="https://i.ibb.co/87zLxQY/2-vms.jpg" alt="2-vms" style="border: 3px solid red" />

Next, log into the virtual machine running Windows and download WireShark

As a reminder you use Windows Remote Desktop, put the public IP for the VM, and then input your username and password.

Here's a <a href="https://www.wireshark.org/download.html">link</a> to the official download page for Wireshark or just do a Google search. The main thing is ensure you install it on the Windows virtual machine not your personal computer.

Below is a screenshot showing the installation of WireShark <br>
<img src="https://i.ibb.co/FDH9w5v/install-wireshark.jpg" alt="install-wireshark" border="1">

After it installs open Wireshark, and click the shark fin icon on the top left to start capturing packets. It will show the traffic on the network. Below, is a screenshot of how it should look:<br>

<img src="https://i.ibb.co/ySqPPbX/11.jpg" alt="11" border="0">

Next, we will add a filter in Wireshark to observe connectivity. We will add an ICMP filter. As you may know ICMP is the network protocol used for the ping command. To do that go to the search bar at the top of Wireshark and type in ICMP and hit enter. It should show no packets of data. Here's a screenshot of how it will look:
<img src="https://i.ibb.co/94WCpb3/icmp-filter.jpg" alt="icmp-filter" border="0">

Now to test the connectivity we will ping VM-2 from VM-1. To do that we need to know either the private or public IP address. Since, the 2 VMs are on the same network we can use the private IP. Go into Azure and note down what the private IP is for VM-2. It's found in the network settings in Azure for VM-2. Just below, where it has the public IP.

In my case the private IP for VM-2 is:

10.0.0.5

Here's a screenshot showing where the private IP is located in Azure:<br>

<img src="https://i.ibb.co/6NDsLdg/private-ip-vm-2.jpg">

Next on VM-1 open up powershell or the command line. If you've never done it before go to the start menu and type in powershell, and click on it when it comes up.

We will ping VM-2 to see that it has connectivity. To that type in exactly:<br><br>
ping 10.0.0.5<br><br>

And hit enter. For the IP put in your private IP address for VM-2 if it's different. Below, is a screenshot that shows how it will look:<br>
<img src="https://i.ibb.co/860w2zR/ping-vm2.jpg" alt="ping-vm2" border="0">

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

<img src="https://i.ibb.co/Csgmpp0/icmp-filter2.jpg" alt="icmp-filter2" border="0">

Sometimes, you need to cancel the infinite ping by pressing Ctrl + c on Powershell and start it again. Now, disable the inbound traffic rule in Azure, and go back to VM-1, with the ping running, or restart the ping if you stopped it, and then see that ICMP traffic is now coming through on Wireshark and Powershell. 

<h3>3. Observe SSH traffic</h3>
Next we will Secure Socket Shell (SSH) into VM-2 from VM-1. To that we execute the following command into Powershell:

ssh username@private ip address.
The command depends on:
The username for VM-2 and the private IP address for VM-2.

In my case my username is the same as I created for VM-1 which is adminuser.
In the network settings for VM-2 it says the private IP address is 10.0.0.5

So the command I put:
ssh adminuser@10.0.05

And hit enter.

It will show the following in Powershell or Command Prompt:
<img src="https://i.ibb.co/xs9RsDg/ssh-into-vm2.jpg" alt="ssh-into-vm2" border="0">

You then type yes and hit enter.

It will then ask you for the password. Put that in and hit enter. If you run into any issues connecting via SSH to your Linux VM, then you should reset the password for your VM in Azure. As well as, ensure you are running PowerShell or the Command Prompt as an administrator. Here's a screenshot of how it will look after you log in:

<img src="https://i.ibb.co/xs9RsDg/ssh-into-vm2.jpg" alt="ssh-into-vm2" border="0">
You may have noticed that in the example I used PowerShell and Command Prompt. Both work the same and you can use either or.

Now, when you type anything into the command line, you can see that it's sending data from VM-1 to VM-2. This is because what we have done is access the command line for VM-2. Therefore, whenever you type something it sends the instructions from VM-1 to VM-2. Regardless, of whether you press enter on the command line or not.

Try this out using a few commands and observe what you see in Wireshark. Some example commands you can try are:
id
uname -a 
pwd

As you may know, it's possible create, move, and edit files in the command line. If you're familiar with Linux commands feel free to tinker around a bit.

After that exit the SSH connection by typing in exit and hitting enter.

You will now be on the command line for VM-1.

<h3>4. Observe DHCP traffic</h3>
Now, we will observe DHCP traffic. As you may know, DHCP traffic is used to assign machines an IP address. First, go into Wireshark and change the ICMP filter to dhcp by typing in dhcp at the top of Wireshark. Here's a screenshot showing how it will look. At this stage no DHCP traffic should be coming through.

<img src="https://i.ibb.co/c8SJYTR/dhcp3.jpg" alt="dhcp3" border="0">

Go into PowerShell or the Command Prompt and type in:

ipconfig /renew

Doing so will issue a new IP address to VM-1. Wireshark will show the DHCP traffic. Below, is a screenshot showing mine:

<img src="https://i.ibb.co/b7NvYMs/dhcp1.jpg" alt="dhcp1" border="0">

When you do this it will commonly disconnect VM-1 from the network and then reconnect again. This will typically take 30 seconds, and there will be a notification on the screen to tell you it's doing so.

<h3>5. Observe DNS traffic</h3>
Now, we will observe some DNS traffic. Go into Wireshark and change the filter for DHCP to DNS by typing in DNS in the box near the top of the screen. As we have done for all of the previous filters in Wireshark.

Then execute the following command in the Command Prompt or Powershell:
nslookup www.google.com

This will ask Google what the ip address info is for google.com

Take a look in Wireshark and see the new DNS traffic being sent over the network.

<img src="https://i.ibb.co/q9Dp3BV/dns-traffic.jpg" alt="dns-traffic" border="0">

<h3>6. Observe RDP traffic</h3>

Lastly, let's take a look at Remote Desktop Protocol (RDP) traffic. As you may be aware, this is traffic between a local computer and a remote computer.

To do that, change the DNS filter to:
tcp.port == 3389

By typing in the box at the top of Wireshark.
It can take 30 seconds or so to load the data.

After doing that observe what you see in Wireshark. Here's a screenshot of mine:

<img src="https://i.ibb.co/jVgLpqj/rdp-traffic.jpg" alt="rdp-traffic" border="0">

There is consistent traffic being sent from the local computer to the virtual computer. Showing the constant stream of data between your local computer and the remote computer.
