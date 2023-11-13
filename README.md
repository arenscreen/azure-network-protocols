<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />





<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, ICMP, DHCP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic




<h2>Actions and Observations</h2>


 1.) We'll start by establishing a resource group to accommodate both virtual machines. Once the resource group is set up, our next step is to create the initial virtual machine. The first virtual machine in question will be a Windows 10 VM. Choose the previously created resource, assign the name VM1 to the virtual machine, and specify Windows 10 Pro, version 22H as the operating system. Regarding the machine specifications, opt for a minimum of 2 vCPUs and 16 GB of memory. Generate a username and password according to your preference, and retain the default options for inbound port rules.

<p>
<img src="https://imgur.com/WgPD275.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  

<p>
<img src="https://imgur.com/X6ZMTJG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
2.) Following this stage, proceed to click on "Next" until you reach the networking page, where a virtual network and subnet will be automatically generated for you. 
  

<p>
<img src="https://imgur.com/XzdSPoR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
  Click review and create our VM.
  
  Having successfully set up our initial VM, we'll now proceed to establish our second VM, opting for an Ubuntu Server 20.04 LTS configuration. The procedure remains identical to the creation of our first machine, with the exception that this time we'll switch from an SSH public key to a password for authentication. 
  
<p>
<img src="https://imgur.com/0KT3Fmb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
<p>
<img src="https://imgur.com/pyxsHfF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
  Click next until we get to the networking page again.
  
  The networking should automatically give us the virtual network from VM1 as well as the subnet. 
  
<p>
<img src="https://imgur.com/3fQXRcw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
 Click review and create, and it will create our second VM.
 
 2.) Having both virtual machines operational, our next step involves connecting to the Windows 10 VM through the Remote Desktop Connection app. After establishing the connection, proceed to your browser and download, then install Wireshark.
 
 "Wireshark is a free and open-source packet analyzer. It is used for network troubleshooting, analysis, software and communications protocol development, and education." 
 
 3.) Open wireshark and filter for ICMP traffic only.
 
 <p>
<img src="https://imgur.com/RrtChUe.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
 4.)To proceed, we aim to obtain the private IP address of our Ubuntu VM and subsequently initiate a ping from within our Windows 10 VM using Wireshark. To ping the private IP address of the Ubuntu machine, open either CMD or PowerShell on the Windows machine and enter the command: `ping 10.0.0.5` (or replace with the specific private IP address of your Ubuntu machine).
 
<p>
<img src="https://imgur.com/zmJzyne.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
<p>
<img src="https://imgur.com/pp4eZdK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
 In Powershell, ping www.google.com and observe the traffic in wireshark.
 
5.) Now we are going to initiate a non-stop ping from our Windows 10 VM to our Ubuntu VM.
 
6.) Access the Network Security Group for our Ubuntu machine and deactivate incoming (inbound) ICMP traffic. To disable incoming ICMP traffic, initiate the addition of a new rule, ensuring that all details are precisely replicated from the provided image. Upon completion, create the rule, and it will be automatically generated and displayed as a new rule.
 
 <p>
<img src="https://imgur.com/r3dH3Yy.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
<p>
<img src="https://imgur.com/qiSIrsX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
 With incoming ICMP traffic disabled from VM2, returning to VM1 will reveal that the ping request is now experiencing timeouts.
 
 7.)Re-enable ICMP traffic for the Network Security Group associated with your Ubuntu VM. Upon doing so, go back to the Windows 10 VM and monitor the ICMP traffic in WireShark along with the command line Ping activity, which should resume functioning. Once observed, halt the ping activity.
 
 8.) The next thing we are going to do is Observe SSH Traffic.

9.) Go back to PowerShell on VM1. Type "ssh labuser@(VM2 private ip address)", press enter. 
 
<img src="https://i.imgur.com/43EFWpq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

 10.)Enter password for VM2(YOU WILL NOT SEE PASSWORD ON THE SCREEN), press enter.

<img src="https://i.imgur.com/6Gd15x7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

11.)You should now have a connection running on Powershell between the Ubuntu VM2, and the Windows VM1. Test the connection on Powershell using a linix command. You should see an established SSH connection with VM2 running on Wireshark.

 <img src="https://i.imgur.com/w6LfwbB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 
 12.)The next thing we are going to do is Observe DHCP Traffic.
 
 13.)Let's go back to Wireshark, which is opened on VM1, and clear the search column. Type in "DHCP" in the search column. 
 
 ![Screenshot 2023-11-12 184459](https://github.com/arenscreen/azure-network-protocols/assets/150059046/edcead36-6331-44e2-b6ad-8b7c00d66113)

14.) Now, lets go back to Powershell on VM1 and type in "IPCONFIG /RENEW". 

![Screenshot 2023-11-12 184618](https://github.com/arenscreen/azure-network-protocols/assets/150059046/2877f017-011f-4d69-af3a-0ce4ea4fe9ba)


15.)Observe the traffic running on Wireshark. You should now see running DHCP connection on Wireshark.
 
 ![Screenshot 2023-11-12 184508](https://github.com/arenscreen/azure-network-protocols/assets/150059046/d0f62487-f15c-421c-ad82-015c6b9799e4)

 
 
 
 
 
 
  
  
