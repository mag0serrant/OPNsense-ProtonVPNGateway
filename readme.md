Setting up a VPN Gateway in OPNsense with ProtonVPN

Go to ProtonVPN, sign in with your credentials.
From the left navigation pane, select OpenVPN/IKEv2
 ![Alt text](Assets/Images/Picture1.png)
Under OpenVPN configuration files, select the router radio button, select the UDP radio button, then scroll down and select a mirror of your choice. Download the .ovpn file.
 
Open the .ovpn file with a text editor, and set to the side.
 
Log into OPNsense. From the left navigation pane, select System > Trust > Authorities.
 


Press the + button to add a new certificate authority. Enter in a description. Copy the certificate portion from the opened .ovpn text file and paste it into certificate data. Then press save.
 
Go to VPN > OpenVPN > Clients. 
 
Press the + button to create a new OpenVPN client. Provide a name. Set protocol to UDP 4. Set interface to WAN. Enter the server information from the .ovpn file. Check the select remote server at random box (if more than one server/port combination was entered). Check the Infinitely resolve remote server box.
 
Under User Authentication Settings, enter the username and password shown to you on ProtonVPN’s website.
 

Uncheck the automatic TLS key box, and copy and paste the TLS certificate information into the TLS Shared Key Box. Set Certificate Authority as ProtonVPN. Set Auth Digest Algorithm as SHA512.
 










Under tunnel Setting, check the Don’t Pull Routes box and Don’t add/remove routes box. Set Verbosity level to 3, and save.
 


Go To Interfaces > Assignments. There should be a new interface listed with an orange + symbol next to it. Click the +. The click on the hyperlink of that new interface and edit. Create a description. Check the block bogon networks box. Check the Dynamic gateway policy box. Click save and apply changes.
 



Go to Firewall > NAT > Outbound. Select the Hybrid outbound NAT rule generation radio button and press save. Create a new manual rule with the + button.
 
Select the ProtonVPN interface. Leave everything as default and save, then select apply changes.
 
Go to System > Gateways > Single. Edit the named (non WAN) dhcp 4 gateway with the pencil icon. For me, it was the PROTONVPN_GA69_VPNV4 seen below. (All your play buttons would likely be green at this stage)
 
Uncheck Disable Gateway Monitoring. Enter a Monitor IP (I used Cloudflare’s DNS). Set priority to a number lower than the priority number of the WAN interface (It defaults to 254).
 

If desired, go back and disable all other gateways. Hooray. Now all your traffic goes through ProtonVPN.
You can set up multiple VPN providers and/or different servers from the same provider in the same way. Then set up gateways for each of them as shown above. If you do this, you can set up Gateway groups that automatically failover.
