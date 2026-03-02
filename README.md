
<p align="center">
<p align="center"> <img src="https://imgur.com/N4r9nQ7.png" alt="osTicket logo"/> </p>
</p>

<h1>Setting up Virtual IP/Portforwarding on Fortigate</h1>
In this repository you will see my steps on setting a Virtual IP as well as the policy that will go behind this. 
Virtual IP's are very crucial for networks with public facing resources. We will set this up so that I can RDP onto a
computer on my home network.

<h2>Environments and Technologies Used</h2>
 
- Fortigate GUI
- Remote Desktop
- Two Windows Computers

<h2>Operating Systems Used </h2>

- Windows 11 Pro/b> (21H2)
- Fortigate OS

<h2>Prerequisites</h2>

- Already have Fortigate setup
- Networking Fundamentals
- Allow RDP on the target computer ( Will have to be windows business, pro, or enterprise)

<h2>Creating the Virtual IP</h2>

<p>
<img src="https://imgur.com/qS7Bh9e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
First lets get logged into the Fortigate Web GUI</p>
<br />

<p>
<img src="https://imgur.com/bnVTXF5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we are going to navigate to our Virtual IPss.
This will be under
  
  Policy and Objects > Virtual IPs

Click New
</p>
<br />

<p>
<img src="https://imgur.com/vHaufGo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
There are a couple things we are going to need for this and I'll explain how we get them. We are first going to need your public IP.
If you don't know your public IP you can visit this link. Its called [IP chicken](https://www.ipchicken.com/)
Next we are going to need the private IP of the computer that will be remoted onto. This can be obtained by running <code>ipconfig</code>
in command prompt.
Put your public IP in the External IP section and your private IP in the IPv4 address section.
Toggle port forwarding at the bottom of the screen. For our purposes, protocol is TCP and Port Mapping Type is one to one.
External service port can reallistcally be anything. Just remember that using common ports can be a security risk. I chose 3890 for the demonstration.
The Map to IPv4 HAS to be 3389 since we are using RDP. If you don't know, RDP uses port 3389. This section has to be the port for the service
you are trying to use. 
Now lets click save
</p>
<br />
<h2> Creating a firewall policy</h2>
<p>
<img src="https://imgur.com/lLxYENr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We will now need to make a firewall policy to allow this traffic to come through. Lets get started by navigating to Policy and Objects > Firewall Policy
Click new.
</p>
<br />

<p>
<img src="https://imgur.com/ryguiUc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
For the specific firewall policy we are making, the incoming interface is going to be your WAN port. My active WAN port is WAN2.
The outgoing interface will be internal. If you were going to use this in practice, it would be better to only allow from certain sources, but 
this is just for testing.
Now lets set the destination as our Virtual IP. Lets also go ahead and put the RDP service in there since that is what we are going to be working with.
Schedule should be set to always, unless you only plan on allowing the RDP during certain hours.
Make sure to turn off NAT so that there isn't any issues. We don't need NAT since the virtual IP is already doing DNAT.
Click save.
</p>

<br />
<h2> Testing it out</h2>
<p>
<img src="https://imgur.com/BxkUSDu.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
The first test we are running is in powershell. On a computer that is off network, we are going to run this command in powershell.
  <code>Test-NetConnection <YOUR PUBLIC IP> -port 3390</code><br />
This should come back successful.
</p>
<br />
<p>
<img src="https://imgur.com/hUBrqEJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Second test would be to just try RDPing onto the computer.
Open RDP and input the socket for your computer name. This is going to consist
of your public IP and the external port we set (e.g 56.432.23.1:3390).
Click connect. You are going to need the credentials to an account that has access to the computer.
Even just getting prompted with credentials proves this is successful.
<br />
</p>

<p>Now you know how to set up a virtual IP, and a firewall policy to allow traffic to it. Be safe, and have fun.</p>
