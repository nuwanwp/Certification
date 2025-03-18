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
- 
