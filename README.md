<p align="center">
<p align="center"> <img src="https://imgur.com/N4r9nQ7.png" alt="osTicket logo"/> </p>
</p>

<h1>Setting up Virtual IP/Port Forwarding on FortiGate</h1>
In this repository, you will see the steps for setting up a Virtual IP as well as the firewall policy required to support it. Virtual IPs are crucial for networks with public-facing resources. In this demonstration, we will configure a VIP to allow RDP access to a computer on my home network.

<h2>Environments and Technologies Used</h2>

- FortiGate GUI
- Remote Desktop
- Two Windows Computers

<h2>Operating Systems Used</h2>

- Windows 11 Pro (21H2)
- FortiGate OS

<h2>Prerequisites</h2>

- FortiGate already set up
- Basic networking fundamentals
- RDP enabled on the target computer (Windows Pro, Business, or Enterprise)

<h2>Creating the Virtual IP</h2>

<p>
<img src="https://imgur.com/qS7Bh9e.png" height="80%" width="80%" alt="FortiGate Login"/>
</p>
<p>
First, log in to the FortiGate Web GUI.
</p>
<br />

<p>
<img src="https://imgur.com/bnVTXF5.png" height="80%" width="80%" alt="Virtual IP Navigation"/>
</p>
<p>
Now navigate to your Virtual IPs. This is located under:

Policy and Objects > Virtual IPs

Click "New".
</p>
<br />

<p>
<img src="https://imgur.com/vHaufGo.png" height="80%" width="80%" alt="VIP Configuration"/>
</p>
<p>
There are a few things we need for this configuration. First, we need your public IP address. If you don’t know it, you can visit <a href="https://www.ipchicken.com/">IP Chicken</a>.

Next, we need the private IP of the computer you want to RDP into. You can find this by running <code>ipconfig</code> in Command Prompt.

Enter your public IP in the External IP section and the private IP in the IPv4 Address section.

Toggle Port Forwarding at the bottom. For this demonstration:
- Protocol: TCP
- Port Mapping Type: One-to-One
- External Service Port: Any unused port (avoid common ports for security). I used 3890.
- Map to IPv4 Port: 3389 (RDP’s default port)

Click Save.
</p>
<br />

<h2>Creating a Firewall Policy</h2>

<p>
<img src="https://imgur.com/lLxYENr.png" height="80%" width="80%" alt="Firewall Policy Navigation"/>
</p>
<p>
Next, we need to create a firewall policy to allow this traffic. Navigate to:

Policy and Objects > Firewall Policy

Click "New".
</p>
<br />

<p>
<img src="https://imgur.com/ryguiUc.png" height="80%" width="80%" alt="Firewall Policy Configuration"/>
</p>
<p>
For this policy:

- Incoming Interface: Your WAN port (mine is WAN2)
- Outgoing Interface: Internal
- Source: All (for testing; in production, restrict this)
- Destination: The Virtual IP you created
- Service: RDP
- Schedule: Always (or restrict as needed)
- NAT: OFF — the VIP already performs DNAT, so NAT is not needed here

Click Save.
</p>

<br />

<h2>Testing It Out</h2>

<p>
<img src="https://imgur.com/BxkUSDu.png" height="80%" width="80%" alt="PowerShell Test"/>
</p>
<p>
The first test is from an off-network computer using PowerShell:

<code>Test-NetConnection &lt;YOUR PUBLIC IP&gt; -Port 3890</code>

This should return a successful result.
</p>
<br />

<p>
<img src="https://imgur.com/hUBrqEJ.png" height="80%" width="80%" alt="RDP Test"/>
</p>
<p>
Next, try connecting via RDP. Open Remote Desktop and enter the socket for your computer:

<YOURPUBLICIP>:3390
Click Connect. You will need valid credentials for the target machine. Even receiving a credential prompt confirms the setup is working.
</p>

<p>
Now you know how to set up a Virtual IP and the firewall policy required to allow traffic through a FortiGate firewall. Be safe, and have fun.
</p>
