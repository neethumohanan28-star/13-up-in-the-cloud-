*Cloud provider + region*

a. Cloud Provider :- Microsoft Azure
b. Region :- Korea Central (vnet-koreacentral-1)

*VM OS + size/tier and why it is free-tier eligible*
a. OS:- Linux (Ubuntu 24.04 LTS) — Canonical server image
b. Size/Tier: Standard B2as v2 (2 vCPUs, 8 GiB RAM)
c. Why it is free-tier eligible?
      The selected virtual machine configuration for this task is Standard B2as v2 (2 vCPUs and 8 GiB RAM), which is not included in Microsoft Azure’s 12-month free-tier eligible offerings. Azure’s free tier typically allows only the B1s size (1 vCPU and 1 GiB RAM) for up to 750 hours per month for new users. But new accounts include credits that allow for higher-tier B-series (burstable) instances like the B2as v2 for testing and development within the trial period.

Auth Method: SSH Public Key (RSA). I restricted local key permissions using chmod 400 on my Kali machine to secure the connection.

*VM Creation & Configuration*

a. VM Name: MyCloudVM.
b. Public IP Address: 20.249.212.45.
c. Private IP Address: 172.17.0.4.
d. Network/Security Rules (NSG):
   i) Inbound SSH (Port 22): Allowed for remote terminal management.
  ii) Inbound HTTP (Port 80): Allowed for web traffic.
 iii) Inbound ICMP: Manually added a custom rule to allow "All ICMP" traffic             so the VM would respond to pings.

 *Ping VM from Kali*
  
 a. I pinged the public IP 20.249.212.45 from my Kali Linux terminal.
 b. Change Made: I had to modify the Network Security Group (NSG) MyCloudVM-nsg to allow inbound ICMP traffic, as Azure blocks this by default.

 *SSH into VM + ip a*

 a. Successfully logged into the VM via SSH.
 b. Command used: ip a.
--> Observation: The command confirmed the internal private IP of 172.17.0.4, matching the Azure portal's "Networking" dashboard.

*Ping your public IP from the VM*

From inside the VM, I pinged my local internet-facing IP (103.203.73.163).
But, the ping failed (100% packet loss).
--> Observation: Although the VM has outbound internet access, my local home/ISP router is configured to drop or ignore incoming ICMP requests for security, which prevented a successful reply.

*Host a webpage on port 80 and access it from your browser*

I installed and used Nginx to serve HTTP traffic on Port 80.
--> Firewall Change: Verified that the NSG rule for TCP Port 80 was active to allow external browser access.

--> Webpage Details: I created a mini-project page featuring a word input box that fetches definitions from the api.dictionaryapi.dev free API and displays them dynamically.

*Shut down + destroy the VM*

I performed a full deletion of the VM through the Azure portal. I ensured that the associated Network Interface, Public IP, and OS Disk were also selected for deletion.

--> Confirmation: The portal notification confirmed "Successfully deleted virtual machine 'MyCloudVM'," and the resource URL now returns a 404 error, ensuring no further charges are incurred.
