# azure-network-protocols

<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines (VMs)</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups.
<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (VMs/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create Resources in Azure (Resource Group, Windows and Linux VMs)
- Observe Internet Control Message Protocol (ICMP) Traffic
- Observe Secure Shell (SSH) Traffic
- Observe Dynamic Host Congifuration Protocol (DHCP) Traffic
- Observe Domain Name System (DNS) Traffic
- Observe Remote Desktop Protocol (RDP) Traffic

<h2>Actions and Observations</h2>

<h3 align="center">
  Setting up your virtual environment
</h3>
<p>
 <h4> Log into your Azure account and create a Resource Group: </h4>
  Once you are logged into your Azure account, click on either the "Resource Groups" icon or type "resource groups" in the search bar
  <img src="https://i.imgur.com/slum6C6.png" height="75%" width="100%" alt="Resource Group"/>
<br/> 
  Click on either "create" buttons on the Resource Groups page
  <img src="https://i.imgur.com/XJFYC5u.png" height="75%" width="100%" alt="Create resource group"/>
  <h4>Create a Windows 10 VM.</h4>
  From the Azure homepage, click on either the "Virtual machines" icon or type "virtual machines" in the search bar. Once on the "Virtual Machines" page, click on the "create" button and choose "Azure virtual machine". While creating the VM, select the previously created Resource Group and allow it to create a new Virtual Network (Vnet) and Subnet.
  <img src="https://i.imgur.com/3XhSTuZ.png" height="75%" width="100%" alt="VM"/>
  On the next page, fill out the fields. From the drop-down choices under the "Images" field, choose "Windows 10 Pro".
  <img src="https://i.imgur.com/oq2Vtgu.png" height="75%" width="100%" alt="Windows VM"/>
</p>
<p>
<h4>Create a Linux (Ubuntu) VM</h4>
  Follow the same steps for creating a Windows 10 VM, except when choosing from the drop-down chocices under the "Images" field. This time, choose "Ubuntu Server". Additionally, selct "Password" from "Authentication type". While creating the VM, select the previously created Resource Group and Vnet
  <img src="https://i.imgur.com/Rx81z45.png" height="75%" width="100%" alt="Ubuntu VM"/>
  <img src="https://i.imgur.com/bM8yeWe.png" height="75%" width="100%" alt="Ubuntu VM"/>
</p>
<p>
  <h4>Observe Your Vnet within Netwrok Watcher</h4>
  <img src="https://i.imgur.com/h5KdApc.png" height="75%" width="100%" alt="Network Watcher"/>
</p>
<br />
<p>
  <h3 align="center">Observing ICMP Traffic</h3>
  <h4>Remote into your Windows 10 VM</h4>
  <p>
    Use Remote Desktop to connect to your Windows 10 VM by copying VM1's "Public IP address" and inputting that information in Remote Desktop (**Note** If you're using a Mac, you'll have to download Microsoft Remote Desktop app)
    <img src="https://i.imgur.com/XFuOfMj.png" height="75%" width="100%" alt="Remote Desktop"/>
  </p>
  <p>
    <h4>Install Wireshark within your Windows 10 VM</h4>
    <img src="https://i.imgur.com/GoeqrmS.png" height="75%" width="100%" alt="Install Wireshark"/>
  </p>
  <p>
    <h4>Open Wireshark and filter for ICMP traffic only</h4>
    Hover your cursor in the pictured field below and type "ICMP"
    <img src="https://i.imgur.com/AT3OSXo.png" height="75%" width="100%" alt="ICMP Traffic"/>
  </p>
  <p>
    <h4>Create ping requests between VMs</h4>
    Retrieve the private IP address of the Ubuntu VM and attempt to ping it from within the Windows 10 VM via Windows Powershell. Observe ping requests and replies within Wireshark:
    <img src="https://i.imgur.com/bheYasG.png" height="75%" width="100%" alt="Ubuntu VM IP Address"/>
    <img src="https://i.imgur.com/DGbE0FX.png" height="75%" width="100%" alt="Ping Requests between VMs"/>
  </p>
  <p>
    <h4>Ping a public website</h4>
    From The Windows 10 VM, open command line or PowerShell and attempt to ping a public website (such as www.google.com) and observe the traffic in WireShark
    <!<img src="???" height="75%" width="100%" alt="ICMP traffic - public address"/>
  </p>
  <p>
    <h4>Initiating, disabling, and re-enabling a perpetual ping between VMs using NSG</h4>
    Initiate a perpetual/non-stop ping from Windows 10 VM to Ubuntu VM
    <!<img src="???" height="75%" width="100%" alt="ICMP traffic - perpetual ping"/>
    <br/>
    Open the NSG in the Ubuntu VM and disable incoming (inbound) ICMP traffic
    <!<img src="???" height="75%" width="100%" alt="NSG - inbound denial"/>
    <br/>
    Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity
    <!<img src="???" height="75%" width="100%" alt="ICMP traffic - inbound denial"/>
    <br/>
    Re-enable ICMP traffic for the NSG in the Ubuntu VM
    <!<img src="???" height="75%" width="100%" alt="NSG - inbound allow"/>
    <br/>
    Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity (should start working again)
    <!<img src="???" height="75%" width="100%" alt="ICMP traffic - re-enabled"/>
    <br/>
    Stop the ping activity
    <!<img src="???" height="75%" width="100%" alt="ICMP traffic - stop"/>
  </p>  
</p>
<br/>
<p>
  <h3 align="center">Observing SSH Traffic</h3>
  <h4>Filter for SSH traffic only in Wireshark</h4>
  <!<img src="???" height="75%" width="100%" alt="Filter for SSH"/>
  <h4>"SSH into" the Ubuntu VM (via its private address)</h4>
  <!<img src="???" height="75%" width="100%" alt="???"/>
  Type commands (username, pwd, etc) into the linux SSH connection and observe SSH traffic spam in WireShark
  <!<img src="???" height="75%" width="100%" alt="SSH Traffic"/>
  <br/>
  Exit the SSH connection by typing ‘exit’ and pressing [Enter]
  <!<img src="???" height="75%" width="100%" alt="SSH traffic - exit"/>
</p>
<br/>
<p>
  <h3 align="center">Observing DHCP Traffic</h3>
  Back in Wireshark, filter for DHCP traffic only
  <!<img src="???" height="75%" width="100%" alt="Filter for DHCP"/>
  <br/>
  From your Windows 10 VM, attempt to issue your VM a new IP address from the command line (ipconfig /renew)(***Note***Observe the DHCP traffic appearing in WireShark)
  <!<img src="???" height="75%" width="100%" alt="DHCP Traffic - new"/>
</p>
<br/>
<p>
  <h3 align="center">Observing DNS Traffic</h3>
  Back in Wireshark, filter for DNS traffic only
  <!<img src="???" height="75%" width="100%" alt="Filter for DNS"/>
  <br/>
  From your Windows 10 VM within a command line, use nslookup to see what google.com and disney.com’s IP addresses are (***Note***Observe the DNS traffic being shown in WireShark)
  <!<img src="???" height="75%" width="100%" alt="DNS Traffic"/>
</p>
<br/>
<p>
  <h3 align="center">Observing RDP Traffic</h3>
  Back in Wireshark, filter for RDP traffic only (tcp.port == 3389)
  <!<img src="???" height="75%" width="100%" alt="Filter for RDP"/>
  <br/>
  Oserve the immediate non-stop spam of traffic? Why is it non-stop spamming vs only showing traffic when a command is inputted?
  <br/>
  Answer: Because the RDP (protocol) is constantly showing you a live stream from one computer to another, therefor traffic is always being transmitted
  <!<img src="???" height="75%" width="100%" alt="Filter for RDP"/>  
</p>
<br/>
<p>
  Thank you for checking out my turotial! I hope you learned a little bit about NSGs and network traffic protocols between virtual machines. Have a wonderful day. 
</p>



