## **Networking**
- IP addreses and CIDR block

## VNet
- Isolated Network in cloud
- VNet alway contains 1 or more subnets. default subnet
- Address space. <network id> . <host ids>
- subnet should be sub set of Vnet address space
- if devices are attached subnet you cannet delete the subnet
- if subnets are exists we cannot delete Vnet
- A VM must be in the same region as its VNet because a VNet is a regional resource.
-  You cannot assign a VM's network interface to a VNet that exists in another region.
  
## IP address
- Public and private
- Public SKUs
- 1. standard - support for security, av zones
-  2. basic will expire
- Dynamic IP :- assigned when the public IP address is assigned to a resource. released when stop or delete the resource
- When creating VM it is creating standard SKU, static ip and attached to the VM

## NIC
- NIC cannot be attached to running VM
- Adding multiple NIC for VM
- same VNet, same subnet and VM should be in same region
- Primary NIC cannot be detached
- Secondary NICs can be detached, but the VM must be stopped first.

## NSG
- Filter traffic between AZ resource
- Rule ( name, priority, source or dest , protocol, port range, action allow or deny)
- Default inbound rule is there to allow all traffic wiht in Vnet
- Default outbound rule is there to allow internet
- First matching rule taken into the consideration when evaluating the rules
- If Inabound traffic is allowed out bound should be automatically allowed. Out bound requests initiated from the host are blocked using outbound rule
- NSG can be attached to NIC or subnets. (Go NSG -> Network Inteface and disassociate it / Go to sub nets and associate it)
- NSG and Subnet must be in the same region.
- NSG and Subnet can be in different resource groups.
- NSG and Subnet must be in the same subscription.

## Application Security Group (Iaas)
- Inside VM -> Networking - asscoate the VM to ASG

## Bastion Service
- enables secure connection to VM without need of public IP
- public ip only required for the basition host not vms
- SKU (developer, basic, Standard and Premium) (developer SKU doesnot support peered VM connections)
- Standard SKU support auto scaling
- Bastion service will deploy new subnet AzureBastionSubnet
- Bastion service and communicate within VM without needing any custom rules
- Native client support ( connection from local pc) is support from Standard sku onwards

## Vnet peering
- communication in between Vnet is not possible if we don't have peering service
- cannot have overlapping CIDR blocks
- Same Azure AD Tenant (Directory).
- Different Subscriptions Allowed (within the same tenant).
- Different Regions Allowed (for Global VNet Peering).
- When creating peering it will create two connections one per each direction
- Azure does NOT support native transitive peering.
- Use VPN Gateway, Azure Firewall, or Virtual WAN for transitive routing.
- Hub-Spoke topology is the best practice for managing multiple VNets efficiently. (a -> b/ b -> c and a -> c

## User defined routes
- Create route table
- Add route table entry  ( set the destination ip range (webvm) / select the nest hop as virtual applience / next hope address should be private ip of central vm)
- GO to subnet in RouteTable resource and associate to the subnet
- GO central vm NIC and enable IP forwarding / enable os level ip forwarding

## Network watcher service
- VM firewall rule issues, NSG rules that blocking the traffic, High VM cpu/memory utilizations
- Connection troubleshoot allows to identify TCP/ICMP connection from VM, app gateway, bastion host to VM, FQDN, ip address connectivity issues
- Connection Monitor (select source / test configuration / destination). Can setup alters and it log the details log analytic workspace
- IP Flow verify - verify NSG rules. 
- Next hop - to verify data packets routed throgh the defined path - gives tthe nest hop as a result
- NSG diagnostic tool - show the each allowed and deny rules in NSG which block the traffic
- NSG flow logs - is logging feature and it allows to log traffic flowing through the NSG

## Azure Load Balancer Service ( IaaS )
- SKU (standard (SLA 99.99%) and basic -exprire)
- 1. Frontend IP configuration - public ip setup of the LB
  2. Backend pool
  3. Health prob
  4. LB rules
- Add LB resource and configure Frontend IP configuration by adding public ip
- Select vNet and select VMs (vm are not visible if vm attached with public IP - disassociate first) / standard SKU IP addreses not not compatible with basic SKU lbs
- setup health probe
- setup a rule to direct them to backend pool

## NAT rules
- how to login to backend pool machines. since we donot have public ips
- go to LB add inbound NAT rule / then take the public ip (which lb assign to it ) and connet to VM

## Azure Application Gateway
- Kind of azure load balancer
- Routing decisions are based on http request. layer 7
- Health probs are not mandatory here
- Requires subnet to deploy Application Gateway

## DNS service
- Azure DNS zone. Map the Azure DNS zone name server in dns provider and return back the azure dns. A record added to the Azure DNS zone record set.
- Azure private DNS zone is for managing internal DNS resolution.
- 



## Note
- Route table cannot delete with associated subnet and routes
- There are two way to map
- 1. Specify DNS name in public ap DNS attribute and azure will use theier own dns
  2. Map with public DNS provider with A record


