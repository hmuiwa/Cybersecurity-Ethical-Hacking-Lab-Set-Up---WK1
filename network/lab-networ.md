Network Architecture
The lab uses a custom VirtualBox NAT Network with the subnet 10.0.0.0/24.

Component	Configuration
Network	Custom NAT Network
IPv4 subnet	10.0.0.0/24
Kali Linux IP	10.0.0.2/24
Example gateway	10.0.0.1
Example DHCP range	10.0.0.2–10.0.0.99
Network mask	255.255.255.0
Note: The gateway, DHCP range, and static IP must be configured consistently. If Kali uses 10.0.0.2, exclude that address from the DHCP pool to prevent IP conflicts. Confirm the actual gateway in VirtualBox's NAT Network settings.
