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
    <img src="https://i.imgur.com/o4kM7O2.png" height="75%" width="100%" alt="ICMP traffic - public address"/>
  </p>
  <p>
    <h4>Initiating, disabling, and re-enabling a perpetual ping between VMs using NSG</h4>
    Initiate a perpetual/non-stop ping from Windows 10 VM to Ubuntu VM by typing "ping [Ubuntu VM private IP address] -t" in Windows Powershell inside of the Windows 10 VM 
    <img src="https://i.imgur.com/aoMQCbx.png" height="75%" width="100%" alt="ICMP traffic - perpetual ping"/>
    <br/>
    Disable inbound ICMP ping requests to Ubuntu VM. Navigate to Ubuntu VM's NSG. Click "Inbound security rules", then click "Add", then select "ICMPv4", then select "Deny", then change priority by inputting a number in correspondence to its priority, and then click "Add".
    <img src="https://i.imgur.com/OGp5taJ.png" height="75%" width="100%" alt="NSG - inbound denial"/>
    <br/>
    Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity
    <img src="https://i.imgur.com/EMvLlCh.png" height="75%" width="100%" alt="ICMP traffic - inbound denial"/>
    <br/>
    Re-enable ICMP traffic to Ubuntu VM by either deleting previously created rule in NSG or changing the rule to "Allow" instead of "Deny"
    <img src="https://i.imgur.com/DhjZYdT.png" height="75%" width="100%" alt="NSG - inbound allow"/>
    <br/>
    Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity (should start working again)
    <img src="https://i.imgur.com/eliCNQv.png" height="75%" width="100%" alt="ICMP traffic - re-enabled"/>
    <br/>
    Stop the ping activity by pressing CTRL + C
    <img src="https://i.imgur.com/HsOGobc.png" height="75%" width="100%" alt="ICMP traffic - stop"/>
  </p>  
</p>
<br/>
<p>
  <h3 align="center">Observing SSH Traffic</h3>
  <h4>Filter for SSH traffic only in Wireshark</h4>
  <img src="https://i.imgur.com/a5cjdB5.png" height="75%" width="100%" alt="Filter for SSH"/>
  <h4>"SSH into" the Ubuntu VM (via its private address)</h4>
  Open Powershell in Windows 10 VM and type "ssh [Ubuntu VM's username]@[Ubuntu VM's private IP address]" and then input password
  <img src="https://i.imgur.com/G5Cxbo2.png" height="75%" width="100%" alt="SSH into Ubuntu VM"/>
  Type commands (username, pwd, etc) into the linux SSH connection and observe SSH traffic spam in WireShark
  <img src="https://i.imgur.com/hPGiOwm.png" height="75%" width="100%" alt="SSH Traffic"/>
  <br/>
  Exit the SSH connection by typing ‘exit’ and pressing [Enter]
  <img src="https://i.imgur.com/M17FgPN.png" height="75%" width="100%" alt="SSH traffic - exit"/>
</p>
<br/>
<p>
  <h3 align="center">Observing DHCP Traffic</h3>
  Back in Wireshark, filter for DHCP traffic only
  <img src="https://i.imgur.com/vsiseJt.png" height="75%" width="100%" alt="Filter for DHCP"/>
  <br/>
  From your Windows 10 VM, attempt to issue your VM a new IP address from the command line (type "ipconfig /renew" in Powershell)(***Note***Observe the DHCP traffic appearing in WireShark)
  <img src="https://i.imgur.com/FrVsuFx.png" height="75%" width="100%" alt="DHCP Traffic - new"/>
</p>
<br/>
<p>
  <h3 align="center">Observing DNS Traffic</h3>
  Back in Wireshark, filter for DNS traffic only by typing "DNS" or "udp.port==53"
  <img src="https://i.imgur.com/ePQS90f.png" height="75%" width="100%" alt="Filter for DNS"/>
  <br/>
  From your Windows 10 VM within a command line, use nslookup to see what google.com and disney.com’s IP addresses are (***Note***Observe the DNS traffic being shown in WireShark)
  <img src="https://i.imgur.com/lXtN6JB.png" height="75%" width="100%" alt="DNS Traffic"/>
</p>
<br/>
<p>
  <h3 align="center">Observing RDP Traffic</h3>
  Back in Wireshark, filter for RDP traffic only by typing "RDP" or "tcp.port == 3389"
  <img src="https://i.imgur.com/Ufb2tWH.png" height="75%" width="100%" alt="Filter for RDP"/>
  <br/>
  Oserve the immediate non-stop spam of traffic? Why is it non-stop spamming vs only showing traffic when a command is inputted?
  <br/>
  Answer: Because the RDP (protocol) is constantly showing you a live stream from one computer to another, therefor traffic is always being transmitted
  <img src="https://i.imgur.com/8G6TZ9x.png" height="75%" width="100%" alt="Filter for RDP"/>  
</p>
<br/>
<p>
  <h3 align="center">Thank you for checking out my tutorial! I hope you learned a little bit about network traffic protocols between virtual machines and creating VMs & NSGs in Azure. Thank you once again for stopping by and have a wonderful day.</h3> 
</p>



