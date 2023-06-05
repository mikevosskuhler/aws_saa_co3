available ranges:
- 10.0.0.0./8
- 172.16.0.0/16
- 192.168.0.0/16
The primary VPC CIDR block cannot be changed after VPC creation
- secondary CIDR blocks --> can be specified after VPC creation, but cannot overlap with primary CIDR block
- max /16 AND min /28
# Default VPC
- only 1 per region
- some services might behave oddly when the default VPC is missing
- can be recreated --> using the console or CLI or API
# IPv6
- AWS provides a /56 range
- can bring your own in the (/48-\*) range
# subnets
- once an instance has been created, it cannot be moved out of the subnet
- the first four and last IP address are reserved by AWS
- ip range cannot be changed after creation
- subnet may cover the whole VPC CIDR range, but then you can only have 1 subnet
# Availability zones
every subnet can exist in only one AZ
# IPv6 subnets
the IPv6 subnet size will always be /64
- these subnets still need an IPv4 CIDR block even if you only intend to use IPv6
# Elastic network interfaces
ENI is a virtual network interface attached to an instance
- first ENI attach to an instance is the primary network interface, this is connected to only 1 subnet
- the primary ENI cannot be removed to changed to a different subnet
- the primary ENI always has a primary (private) IP. A secondary can be created (but only in the same subnet)
- additional ENI may be attached, but they can only be added to subnets in the same AZ
- ENI are a separate resources that can exist without being attached to an instance
# enhanced networking
There are faster alternatives to ENI's that directly utilize the server hardware
- Elastic Network Adapter (ENA) --> up to 100 Gbps, supported by most instance types
- Intel 82599 Virtual Functional Interface --> up to 10 Gbps, only available for limited number of instances that do not support ENA
# gateways
these resources are refered to by its resource ID (not its IP address), this is different from conventional networking. Each VPC can have only gateway
# route tables
VPC use implied/implicit routers, which you dont manage. You only get to specify the route tables
- every VPC has a main route table --> associated with each subnet by default
- you can make your own custom route table and associate it with subnet(s)
- Each subnet need to have a route table, if not custom the main route table will be used
- every route table should have a local route --> traffic within the VPC between subnets
- the default route (0.0.0.0/0) usually goes to an internet gateway (specified by its resource ID)
- each subnet can only have one route table associated
# security groups
firewall arround a ENI
- every ENI should have at least one security group (multiple are allowed)
- one SG can be attached to multiple ENIs
- by default the SG denies traffic, unless a rule permits it
- statefull firewall
- source or destination can be another RG allowing for easy coms between instances
- each VPC contains a default SG --> cannot be deleted, but rules may be modified
## inbound rules
the default rules will deny everything
inbound rules contain these fields:
- Source (CIDR)
- Protocol --> layer 4
- Destination Port range 
## outbound rules
the default rules create for a SG will allow all (0.0.0.0/0 on all ports and all protocols)
outbound rules contain these fields;
- Destination (CIDR)
- Protocol --> layer 4
- Port range
# Network access control lists
stateless firewall attached to subnet
- each VPC has a default NACL
- each subnet can have only one NACL
- changes to both NACL and SGs are applied nearly instantaniously (within seconds)
## inbound rules
contains the followign fields:
- Rule number
- Protocol (layer 4)
- Port range
- Source CIDR
- Action --> allow / deny
## outbound rules
contains the following fields:
- Rule number
- Protocol (layer 4)
- Port range
- Destination CIDR
- Action --> allow / deny
# AWS network firewall
protects multiple VPCs (accross accounts) including:
- web filtering
- ids/ips
- statefull / stateless filtering
- centralized visibility of traffic
pay per GB or pay per hour
# elastic IPs
- can only be attached to one ENI at once
- if assinged to ENI with public IP, the public IP will be replaced
- regional, cannot be moved to different region
## Bring your own IP
at most 5 address blocks per region
# global accelerator
a service utilizing anycast IP addresses that can route to one of 30 points of presence
# NAT
three types in AWS
## internet gateway
The internet gateway translates the private IP to the instances assigned public IP. This is called on-to-one-NAT
## NAT gateway
- Managed by AWS
- still sends traffic through the IG
- must be assigned an EIP
- resides in a subnet that can have a NACL to restrict traffic
- No ENI so no SG
## NAT instance
preconfigured EC2 instance that acts as a NAT gateway
# AWS private link
bypassing the internet by utilizing private carier line
# VPC peering
point to point private link connection between two VPC
# AWS site to site VPN
connection between on prem and VPC that utilizes AES 256 or 128 bit encryption
- allows for global accelerator integration --> allows for multiple on prem regions
# AWS transit gateway
service to connect multiple VPCs / on prem regions in a managed and highly available way. 
- supports VPN and direct connect
different opperating modes
## centralized router
all traffic is routed through the central transit gateway. utilzed BGP to discover the available routes and centralize in the transit gateway
## Isolated VPCs
multiple VPCs isolated from each other, but on prem network has access to resources in all of them
## isolated VPC with shared services
only share access to a select number of services, while maintaining network isolation. Services may include AD or LLDP
## transit gateway peering
peer transit gateways together, even across regions
## Multicast
multicast support between VPC
## Blackhole routes
drop any traffic to specific routes
# direct connect
utilizes private link to create connection to AWS resources
## dedicated
terminates at an AWS direct connect location, you will have place your hardware at the location. Speeds available: 1 Gbps / 10 / 100
## Hosted
extends to your datacenter from the direct connect location. 50 Mbps - 10 Gbps
## direct connect gateways
allows access to multiple VPCs using BGP and transit gateway
## virtual interfaces
to utilize direct connect you use Virtual Interfaces (VIFs). three types:
- PRivate --> access to private IP addresses in single VPC
- Public --> utilize the public endpoint of AWS resources
- Transit --> connectivity to transit gateway --> access to multiple VPCs --> available with connection speeds of 1 Gbps and up
## direct connect site link
connect two on prem locations using direct connect
