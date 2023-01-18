<p align="center">
<img src="https://i.imgur.com/KJgvM7a.png" height="60%" width="60%" alt="Lab Overview"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
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

<h2>Actions and Observations</h2>
<img src="https://i.imgur.com/gWyavif.png" height="70%" width="70%" alt="Virtual Machine Configuration"/>
<p>
First, you will need to create a Resource Group in Azure and create 2 Virtual Machines in the same Virtual Network and Subnet. 1 Virtual Machine will run Windows 10 and the other will run Ubuntu. The image above is an example of what your Virtual Machine configuration might look like.
</p>

<img src="https://i.imgur.com/GrdI1QW.png" height="70%" width="70%" alt="Virtual Machine Configuration"/>
<p>
Ensure that both virtual machines are using the same Virtual Network and Subnet, otherwise you will be unable to observe the upcoming traffic!
</p>

<img src="https://i.imgur.com/nBs6eCo.png" height="70%" width="70%" alt="Downloading Wireshark"/>
<p>
After using RDP to connect into VM1, we will now download Wireshark. https://www.wireshark.org/download.html
</p>

<p>
<img src="https://i.imgur.com/QS0ujcz.png" height="70%" width="70%" alt="Wireshark Ping"/>
</p>
<p>
Next, set the Wireshark filter to ICMP so we can only capture ICMP packets and ping VM2's private IP address. Notice how Wireshark has captured the ICMP packets.
</p>

<p>
<img src="https://i.imgur.com/QEKbESo.png" height="70%" width="70%" alt="Continuous Ping"/>
</p>
<p>
Now, we will send a continuous ping on VM1 to VM2 so that we can see how a Network Security Group can be used to block ICMP packets.
</p>

<p>
<img src="https://i.imgur.com/jcHE3Oi.png" height="70%" width="70%" alt="Deny ICMP Packets Network Security Group"/>
</p>
<p>
Open up VM2's Network Security Group in Azure so that we can create an Inbound security rule denying ICMP packets from anywhere.
</p>

<p>
<img src="https://i.imgur.com/yxdfjsP.png" height="70%" width="70%" alt="Denied ICMP Packets Result"/>
</p>
<p>
Open up VM1 and notice how the packets are now being denied!
</p>

<p>
<img src="https://i.imgur.com/2FMDf8N.png" height="70%" width="70%" alt="SSH Wireshark"/>
</p>
<p>
Now instead of pinging VM2, we will SSH into VM 2. Update Wireshark's filter to filter SSH packets. Next, SSH into VM2 using the "ssh <username you set up>@<VM2 Private IP>" command. If you receive a message about an ECDSA key, then type YES and when prompted, enter the password you set up. Notice how the CMD has changed and Wireshark has captured SSH packets. To close our SSH session, type exit and hit ENTER
</p>

<p>
<img src="https://i.imgur.com/40NPfSI.png" height="70%" width="70%" alt="DHCP Wireshark"/>
</p>
<p>
Next, type dhcp into the Wireshark filter and enter "ipconfig /renew" into the CMD. Notice that Wireshark has captured the DHCP packets used to request a new IP address.
</p>

<p>
<img src="https://i.imgur.com/liJCe0X.png" height="70%" width="70%" alt="DNS Wireshark"/>
</p>
<p>
After that, type dns into the Wireshark filter and (if there exists) clear the existing captures. Type "nslookup www.google.com" and observe the DNS requests used to find Google's ip address.
</p>
