---
title: '"VPC (Virtual Private Cloud)"'
draft: 
tags:
  - AWS
  - VPC
  - networking
---
## **VPC (Virtual Private Cloud)**

  

It's a custom defined virtual network on aws cloud. It's a networking layer for EC2

- VPC is Logically Isolated even if It shares the IP address 
- Can span along the availability zones 
- When creating VPC we must select a CIDR range 

- IPV4 address
- Range of CIDR can’t be changed after creation 
- Range can be between /16 (65,536 available address) or /28 (16 available address)

- VPC consist of Following Component 

- Subnets 
- Route tables
- DHCP (Dynamic Host Config Protocol) option sets 
- Security Groups 
- Network Security Control List (ACL)

- Option components 

- Internet Gateways 
- Elastic IP 
- Elastic Network Interfaces 
- EndPoints 
- Peering 
- NAT instances and Gateways 
- VPG (gateway) 
- VPN
- Customer gateway 

  

## **Subnets** 

  

- Segment of VPC IP address range where we can launch aws services 
- CIDR block defines subnet (192.0.1.0/24) or (192.0.0.0/24)
- AWS reserves first 4 address and last IP for itself. If you have a /28 or 16 IP addresses, AWS will save 5 for itself and you will end up having 11 addresses 
- You can add one to more subnets in one availability zone , therefore multiple subnets in one availability zone.
- Subnets don’t span across the availability zone for connecting availability zone you have to use the route table or a NAT 
- Classifier into 3 classes 

- Public 

- This is used for routing internet to the VPC’s Internet Gateway
- Associated with route table 

- Private 

- This is used internally in VPC, does not direct traffic to IGW of VPC 
- Associated with route table

- VPN-only 

- Aws VPC contains one public subnet in every AZ within the region with net-mask of /20. 

  

## **Route Tables**

  

- Route tables creates a logical connection(construct) in VPC. Tell’s how traffic will direct in VPC 
- VPC comes with a Default route table called local route which enables communication in VPC and this can’t be modified or deleted. 
- We can specify which subnets will become public and private by directing traffic through the IGW or not respectively 
- Using route tables EC2 in different subnets can communicate to each other. 
- We can add custom routes. 
- Points to remember 

- VPC has implicit router 
- VPC comes with main route table 
- We can create custom route tables 
- Each subnet must be connected with a route table other wise it will use the main route table 
- You can replace default with custom route table so that all custom route table automatically associates with it 
- Each route in table specifies a destination CIDR and a target

  

## **Internet Gateways** 

  

- It’s a VPC component with horizontally scaling, redundant, high availability. 
- Allows communication between instances and the internet using public IP addresses 
- How to create a public IP addresses 

- Attach IGW to VPC 
- Create sub route table rule to send all non-local traffic  IGW 
- Configure ACL to allow traffic to flow to and from the instance 
- Assign EIP or public IP to the Instance 

  

## **DHCP** 

  

Will do this later

  

  

## **Elastic IP** 

  

- Aws has a pool of public IP addresses in each region 
- EIP is a static IP address
- We can take if from the pool and release it when the work is done 
- EIP remains fixed and allows you to change the underlying infrastructure 
- Points to remember 

- First allocate the EIP for VPC then attach to the Instance 
- EIP is specific to a region
- One-to-one relationship between network Interfaces and EIP
- Can move from one instance to another to same or in another VPC  but in same region 
- EIP is associated with your account until you release it [important]
- EIP is chargeable either you use it or not 

  

## **Elastic Network Interface (still confused )**

  

- Helps you to create a management network, use network and appliance in your VPC 
- Create a dual-homed instance with workload/roles on distinct subnets 
- Create a low-budget , high availability solution 

  

## **Endpoints** 

  

- VPC endpoints enables you to create a private connection layer between AWS VPC and other services without using internet or Nat, direct connect. 
- We can create multiple endpoints for subnets for accessing same service
- Point to remember for VPC endpoints 

- Specify amazon VPC 
- Specify the service 
- Specify the policy 
- Specify the route tables 
- If the service is in other region then it will use the IGW not the specified endpoint because it only works for same region. 

  

## **Peering** 

  

- Networking Connection between two or more VPCs and act like they are in within same network 
- It can only work if both VPC are in same region 
- You can also build a networking connection to other account’s VPC 
- Its a one-to-one relationship 
- It is created through a request/acept protocol, 
- Do not support transitive routing 
- Points to remember 

- Can’t create peering if VPC is in different region 
- Can’t have more than one peering connection between the same two Amazon VPC at the same time. 
- Can’l create peering if two VPC have overlapping or matching CIDR blocks 

  

## **Security Groups** 

  

- It’s a virtual stateful firewall for AWS controls inbound and outbound, if response is allowed inbound rule to will also allow outbound rule regardless of outbound rule  
- Important rules to remember 

- By default no inbound traffic is allowed 
- You can specify allow rules not deny rules (This is the difference between ACL and security group) 
- By default all outbound rules are allowed 
- Instance of same security group can’t talk until you add rules 
- Changes in Security groups are reflected immediately 

  

## **Network Allocated Control List** 

  

- Its also a firewall but works on subnet level and stateless
- By default all inbound and outbound rules are deny 
- VPC are created with modifiable ACLS 
- Every subnet must be associated with ACL 

  

  

## **Difference between ACL and Security groups** 

  

|   |   |
|---|---|
|**Security Group**|**ACL**|
|Instance level|Subnet level|
|Allows rule only|Allow and deny rules|
|Statefull : Returned traffic is allowed|Stateless: üst be explicitly allowed y rules|
|AWS evaluates all rules before deciding whether to allow traffic|AWS processes rules in number whether to allow traffic|
|On individual instances|Automatically apply to all instance associated with ACL|

  

## **NAT and NAT Gateways** 

  

In AWS VPC by default  instance in private IP  can’t connect with the internet through IGW. For this AWS has a NATInstance, AWS recommend to use NAT Gateway rather than NAT Instance because of high availability and higher bandwidth. 

  

## **Nat Instances (Network address Translator)**

  

- Points otIts an linux AMI designed to accept traffic from private IP and translate to public IP for internet access 
- For Nat do the following 

- Create security group   

  

## **VPG, VPNs, CGWs**

  

- You can attach your data centre to a AWS VPN.
- AWS VPC offers two ways to connect to a corporate network to a VPC: VPG and CWG 
- VPG (Virtaul provate gateways)

- is a VPNs connector between two networks on AWS side

- CGW

- Is a customer side VPN connector 

- Then there is a VPN tunnel where the data will flow between AWS and customer datacenter 
- VPN 

- Is generated after traffic flows from customer side 

- you must specify the type of connection you’ll have in tunnel. 

- If customer supports border gateway then configure routing for dynamic routing 
- If Static routing then you must enter the for your network that should be comminucated of VPG. 

- VPC supports multiple CWG (Many to one)

- Each CWG have his own tunnel 

- Points to remember 

- VPG  is a endpoint of the tunnel of aws 
- You must create connection from CWG to VPG 
- VPN consist of two tunnels or higher availability