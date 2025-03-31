## Deploy and Manage Azure compute resources

Think about before creating a virtual machine
- The names of your resources
- The location where the resources are stored
- The size of the virtual machine
- The maximum number of virtual machines that can be created
- The operating system that the virtual machine runs
- The configuration of the virtual machine after it starts
- The related resources that the virtual machine needs

## Name
Name cannot contain special characters \/""[]:|<>+=;,?*@&#%, whitespace, or begin with '_' or end with '.' or '-'

## Components
- VM  - It self has cost
- Vnet - Free
- IP address - Cost
- NSG - Free
- OS disk - Cost
- OS - Some operating systems has licensing cost

## Creating VM
Create resource group & VM - CLI
- az group create --name $MY_RESOURCE_GROUP_NAME --location $REGION
- az vm create \
    --resource-group $MY_RESOURCE_GROUP_NAME \
    --name $MY_VM_NAME \
    --image $MY_VM_IMAGE \
    --admin-username $MY_USERNAME \
    --assign-identity \
    --generate-ssh-keys \
    --public-ip-sku Standard

Create resource group & VM - Power Shell
- New-AzResourceGroup -Name 'myResourceGroup' -Location 'EastUS'
- New-AzVm `
    -ResourceGroupName 'myResourceGroup' `
    -Name 'myVM' `
    -Location 'East US' `
    -image Debian11 `
    -size Standard_B2s `
    -PublicIpAddressName myPubIP `
    -OpenPorts 80 `
    -GenerateSshKey `
    -SshKeyName mySSHKey

Install nginx
- Invoke-AzVMRunCommand -ResourceGroupName 'myResourceGroup' -Name 'myVM' -CommandId 'RunShellScript' -ScriptString 'sudo apt-get update && sudo apt-get install -y nginx'

Shutdown VM itself not deallocating, some cost is calculating. Whole VM deallocating when perform shutdown form Portal
No computation cost, only cost for disk and IP, Data in the temporary disk isn't persisted. E and D series only
VM cannot be moved seprate region once created since disk, public IP and NSG are regional resources

## CPU Size
- General purpose  A,B,D (A - Entry-level economical,B -  Burstable, D - Enterprise-grade applications, Relational databases, In-memory caching, Data analytics)
- Compute optimized F (Medium traffic web servers,Network appliances,Batch processes,Application servers)
- Memory optimized E (Relation databases, Medium to large caches)
- Storage optimized L (High disk throughput and IO, Big Data, SQL and NoSQL databases, Data warehousing)
- GPU accelerated NC/ND (Compute-intensive,Graphics-intensive Visualization)
- FPGA accelerated NP (machine learning)
- High performance computer (HB/ HC)( Weather modeling, Computational chemistry)

- You can't change a virtual machine's generation after you've created it( size changing the family requires diffrent underlying HW) 
- If the virtual machine is currently running, changing its size will cause it to restart. It is a distrucptive procedure.
- You can't resize a VM size that has a local temp disk (D: drive) to a VM size with no local temp disk and vice versa
- You can't resize a VM size that has a SCSI-based VM to a VM size that has a remote NVMe-enabled VM
- NVM Express (NVMe) is a communication protocol that facilitates faster and more efficient data transfer between servers and storage systems 
- Generation 2 VMs use the new UEFI-based boot architecture rather than the BIOS-based architecture but no price difference.
- Azure Compute offers virtual machine sizes that are Isolated to a specific hardware type and dedicated to a single customer. The Isolated sizes live and operate on specific hardware generation and will be deprecated when the hardware generation is retired
- Isolated virtual machine sizes are best suited for workloads that require a high degree of isolation from other customers’ workloads

## VM billing
![image](https://github.com/user-attachments/assets/1c74ca10-a3ca-4eb1-9e1a-ef12c16afd88)
- If VM shutdwon it seld it is billing
- Stop using portal is is deallocating - no bill
1. Creating
2. Starting
3. Running
4. Stopping
5. Stopped
6. Deallocating
7. Deallocated

## Azure Compute Gallery (ZRS offers)
- Helps you build structure and organization around your Azure resources, like images and applications
- revisit Instances section
- we can create image and uploaded to compute gallery can recreate on another region. but disk is region serverice disk is stay in sam region. We can move disk by takinf snaphost or Azure site recovery. Snapshot takin a read only copy from disk and create new disk in new region.
- You can also create an App registration to share images between tenants.
- RBAC sharing / RBAC + Direct shared gallry / RBAC + Community gallery
- Limitations in communiity gallery
- 1. cannot convert privilage galley as community gallery
  2. 3rd party images cannot publish as comminuty images
  3. Encrypted images are not supported
  4. Not available for gov clods
  5. Image resources need to be created in the same region as the gallery
- Azure Shared Image Gallery (now known as Azure Compute Gallery) allows you to replicate custom images across multiple Azure regions for high availability and disaster recovery
- Shared Image Gallery cannot be used with VM Scale Sets.
- you can't move the gallery image resource to a different subscription. You can replicate the image versions in the gallery to other regions or copy an image from another gallery
- VM applications - VM Applications are a resource type in Azure Compute Gallery (formerly known as Shared Image Gallery) that simplifies management, sharing, and global distribution of applications for your virtual machines
- Compute Gallery Sharing Admin role at the subscription or gallery level will be able to enable group-based sharing
  

## Disk
 - Managed disk porvides ZRS and LRS redundancy options
 - Managed disk - Managed disks are designed for 99.999% availability. Managed disks achieve this availability by providing three replicas of your data
 - OS level disk and Data disk (Seperate Az Resource)
 - Azure managed disks are block-level storage volumes that are managed by Azure 
- Disk types
- ![image](https://github.com/user-attachments/assets/5f4a8028-84f7-4a16-9aa1-604653bf4fd8)
  
- Ultra Disks also feature a flexible performance configuration model that allows you to independently configure IOPS and throughput / Ultra Disks can't be used as an OS disk. / doesnot support  Compute Gallery. Can be attached to VM without downtime, Ultra Disks don't support availability sets. /Encrypting Ultra Disks with customer-managed keys using Azure Key Vaults stored in a different Microsoft Entra ID tenant isn't currently supported. /Azure Site Recovery isn't supported for VMs with Ultra Disks.
  
- Premium SSD v2 - transaction-intensive database may need a large amount of IOPS / gaming application may need a large amount of IOPS / can't be used as an OS disk. / doesnot support  Compute Gallery / Encrypting Ultra Disks with customer-managed keys using Azure Key Vaults stored in a different Microsoft Entra ID tenant isn't currently supported. /Azure Site Recovery isn't supported for VMs with Premium SSD v2 disks / Premium SSD v2 disks can't be attached to VMs in Availability Sets
  
- Premium SSDs - suitable for mission-critical production applications/ offer disk bursting 
- Standard SSDs -  suitable for web servers, low IOPS application servers, lightly used enterprise applications, and non-production workloads / offer disk bursting
- Standard HDDs - disks for dev/test scenarios and less critical workloads
  
- Manage disk has two redundancy options (LRS and ZRS)/ ZRS for managed disks is only supported with Premium SSD and Standard SSD managed disks. Premium SSD v2 is not supported
- ZRS replicates your Azure managed disk synchronously across three Azure availability zones in the selected region
- There are several types of encryption available for your managed disks, including **Azure Disk Encryption** (ADE), **Server-Side Encryption** (SSE), and **encryption at host**.
- Upgrade a disk type directly from Standard HDD to Premium SSD via the Azure Portal or Azure CLI, without detaching the disk.
- Disk size cannot reduce

- Shared Disk
- 1. Currently, only Ultra Disks, Premium SSD v2, Premium SSD, and Standard SSDs can be used as a shared disk

- VM temporary storage goes away when restarting. Only data disk and OS disk remains.
- VMs contain a temporary disk, which isn't a managed disk. The temporary disk provides short-term storage for applications and processes. It's intended for storing only data such as page files, swap files

## VM images
- Image definition contains meta data and version info
- Two types specialized VM images and Generic Images.
- Generalized and specialized VM images contain an operating system disk and all the attached disks, if there any.
- Specialized VM images contain all computer names and admin user info.
- Generic VM images do not contain those. After it creates original VM will not be usable.
- Gen1 and Gen 2 images. Gen1 VM cannot create Gen2 images
- Once you generalize a VM in Azure, you cannot restart it because the VM is permanently marked as a generalized image.
- Managed images can be used to create multiple VMs, but they have many limitations. Managed images can only be created from a generalized source (VM or VHD). They can only be used to create VMs in the same region and they can't be shared across subscriptions and tenants.

## Best practices for achieving high availability with Azure virtual machines
- 1. Applications running on a single VM - Single VMs using only Premium SSD disks as the OS disks, and either Ultra Disks, Premium SSD v2, or Premium SSD disks as data disks have the highest uptime service level agreement (SLA), and these disk types offer the best performance. and enable ZRS disks
- 2.Applications running on multiple VMs - use scal sets with availability zones, flexible orchestration mode or by deploying VMs and disks across three availability zones. Deploy VMs and disks across multiple fault domains


## Disk Encryption (no extra cost)
- Type of encryptions
- 1. Azure Disk Storage Server-Side Encryption (support PMK and Custom amanged keys)/ OS and data disk/ no encrypt temp disk
  2. Encryption at host - Virtual Machine option that enhances Azure Disk Storage Server-Side Encryption to ensure that all temp disks and disk caches are encrypted at rest and
  3. Azure Disk Encryption - ADE encrypts the OS and data disks of Azure virtual machines (VMs) inside your VMs (bit locker)
  4. Confidential disk encryption - binds disk encryption keys to the virtual machine's TPM and makes the protected disk content accessible only to the VM. Only os disk

- Temporary disks are not managed disks and are not encrypted by SSE, unless you enable encryption at host.
- Azure Disk Storage Server-Side Encryption (PMK and Custom Managed Keys through the Key Vault) encryption-at-rest - AES-256
- Azure Disk Encryption helps protect and safeguard your data to meet your organizational security and compliance commitments. Ex:- Bit Locker
- Encryption at host is a Virtual Machine option that enhances Azure Disk Storage Server-Side Encryption to ensure that all temp disks and disk caches are encrypted at rest
- Virtual Machines aren't rebooted during automatic key rotation.
- When a key is either disabled, deleted, or expired, any VMs with either OS or data disks using that key will automatically shut down.
- Soft delete and Purge protection is required for using a Key Vault for encrypting managed disks.
  
- **Restriction on custom managed keys**
- Disks encrypted with customer-managed keys can only move to another resource group if the VM they are attached to is deallocated.  
- If this feature is enabled for a disk with incremental snapshots, it can't be disabled on that disk or its snapshots
- A disk and all of its associated incremental snapshots must have the same disk encryption set.](https://learn.microsoft.com/en-us/azure/virtual-machines/disk-encryption?view=rest-compute-2024-11-04#restrictions)
- Limitation of customer managed keys for snapshots
-	Encryption key sets must be in the same subscription as your image.
-	Encryption key sets are regional resources, so each region requires a different encryption key set.
-	You can't go back to using platform-managed keys for encrypting those images
- Use of Disk Encryption set
- Disks, snapshots, and images encrypted with customer-managed keys can't be moved between subscriptions.
- Managed disks currently or previously encrypted using Azure Disk Encryption can't be encrypted using customer-managed keys.
- Changing disk encrption - Changes to encryption settings can only be made when the disk is unattached or the managing virtual machine(s) are deallocated.
- Custom manage keys using KeyVault 
1. Key Vault Adminitrator role
2. Create Disk encryption Set (azResource)
3. De attached disk from running vm
4. Change the encryptio setting to use encryption set
   

## Snapshots
- Create snaphost (az resource) and create disk from the snapshot and attach to the VM
- Disk snapshot can be created from Disk Snaphot serive and create back disk and attached to VM
- Snapshots are billed based on the used size. Not for full disk size
- Managed disks support creating managed custom images.
- Images versus snapshots (A snapshot is a copy of a disk at a point in time. It applies only to one disk. If you have a VM that has one disk (the OS disk), you can take a snapshot or an image of it and create a VM from either the snapshot or the image)

- Managed disk size: Managed disks are billed according to their provisioned size
- Snapshots are billed based on the size used
  
## Troubleshooting
- Reset your RDP connection
- Verify Network Security Group rules
- Review VM boot diagnostics.
- Reset the NIC for the VM
- Check VM Resource Health
- Reset user credentials
- Verify routing
  
## Custom script extenstion
- Reposible for preparing initial software installation and preparing works
- 90 minutes to run
- Cloud Init for linux
- Custom extentions can be added to scale sets as well

## Boot diagnostics
- Boot diagnostics is a debugging feature for Azure virtual machines (VM) that allows diagnosis of VM boot failures

## Run command in VM
To run scripts, installings etc.

## Redeploy and ReApply
- Redeploy the VM, virtual machine will be restarted and you will lose any data on the temporary drive.
- Reaplly -  Reapplying your virtual machine's state

## Availablity options for VMs
- 1. Availability zones
  2. Virtual Machines Scale Sets
  3. Availability sets
  4. Load balancer
  5. Azure Site Recovery

## Availability Set
- limited to a single Azure region and a single datacenter within that region.
- Fault domains are used to define the group of virtual machines that share a common source and network switch. You can have up to 3 fault domains.
- Update domains are used to group virtual machines and physical hardware that can be rebooted at the same time. You can have up to 20 update domains.

- Sets place VMs in different fault domains and update domains
- Microsoft guarantees a 99.95% SLA
- Azure Availability Set cannot be moved directly to a different subscription.
- Azure Availability Set cannot be moved directly to a different region because it is a regional resource.
- Azure Availability Set cannot be removed or deleted while there are VMs assigned to it
- You cannot directly detach a Virtual Machine (VM) from an Azure Availability Set because an Availability Set is assigned when the VM is created

## Availability Zones
- Separated groups of datacenters within a region. NO cost (only the data transfer has cost or no).
- Each Availability zone is a unique physical location in an Azure region. 99.99% availability.

- VM or scale set can be deployed as **Zonal** (VM resources are deployed to a specific, self-selected availability zone to achieve more stringent latency or performance requirements.)
- or **Zone redundant** (VM resources are replicated to one or more zones within the region to improve the resiliency of the application and data in a High Availability)

- if a Virtual Machine (VM) is deployed in a ZRS (Zone-Redundant Storage) Availability Set, its disk must also be ZRS-enabled to ensure the same level of redundancy.
- Both availability sets and av zone does not replicate the app. Both giving iaas service.

## Scale sets
- Same as other 2 data/app is not replicating
- The orchestration mode is defined when you create the scale set and cannot be changed or updated later.
- No additional cost for using scale sets. You are charged based on the compute, network, and storage resources that the scale set uses.
- Scale is another azure resource and VM are attached to that.
- Manual scaling and Auto scaling with rules
- Duration, Cool down parameters.
- The VM must be in the same resource group as the scale set.

- On scale-out, autoscale runs if any rule is met.
- On scale-in, autoscale requires all rules to be met.

-Scaling flapping and how to reduce it. Use logs to identify the flapping operations
-Action that can be taken to minimize the flapping
-1.	increase threshold gaps
-2.	increase instance count
-3.	use different metric for scale in and out

-Orchestration Mode : Flexible or uniform
	-Flexible mode all vm, nic and disk can be visible and access
	-Uniform mode individual resources are not visible
 - Cannot directly change the orchestration mode of an Azure Virtual Machine Scale Set (VMSS) from Flexible to Uniform, or vice versa
   
- Flexible Mode is best when you need manual VM control, multiple VM sizes, and fault tolerance.
- Uniform Mode is best when you need Azure to manage scaling and high availability automatically.

-Availability zones cannot be changed with flexible orchestration mode
-Apply custom script extensions at scale sets.
	-Update option for updating the vms (Latest Model)

- Attaching new VM to scale set
- 1. The VM must be in the same resource group as the scale set.
  2. Regional virtual machines (no availability zones specified) can be attached to regional scale sets.
  3. Zonal virtual machines can be attached to scale sets that specify one or more zone
  4. The scale set must be in Flexible orchestration mode

- Attach an existing Virtual Machine to a Virtual Machine Scale Set
- 1. Attaching an existing VM to a scale set with a fault domain count of 1 doesn't require downtime.
  2. The scale set must use Flexible orchestration mode.
  3. The scale set must have a platformFaultDomainCount of 1.
  4. The VM can't be in a ProximityPlacementGroup.
  5. The VM can't be in an Azure Dedicated Host.
  6. The VM must have a managed disk.
  7. The VM and scale set must be in the same resource group. The VM and target scale set must both be zonal, or they must both be regional. You can't attach a zonal VM to a regional scale set.

-Limitations for attaching an existing Virtual Machine to a scale set
	-The scale set must use Flexible orchestration mode.
	-The VM and scale set must be in the same resource group.
	-The VM and target scale set must both be zonal, or they must both be regional. You can't attach a zonal VM to a regional scale set
	-The VM can't be in a ProximityPlacementGroup
	-The VM can't be in an Azure Dedicated Host
	-The VM must have a managed disk

- Standby pools for Virtual Machine Scale Sets
- Standby pools for Virtual Machine Scale Sets enables you to increase scaling performance by creating a pool of pre-provisioned virtual machines.
- Moving virtual machines between the standby pool into the scale set happens automatically whenever a scale out event is triggered
- Standby pool size	Standby pool size = maxReadyCapacity– instanceCount

-Can apply custom script both Orchestration modes to scale set (Remember latest model flag)

## Azure Site Recovery
- Site Recovery replicates workloads running on physical and virtual machines (VMs) from a primary site to a secondary location. When an outage occurs at your primary site, you fail over to secondary location
  
## Proximity Groups
- Defining the VM size in a Proximity Placement Group is necessary for proper resource allocation, performance optimization, and effective scaling
- Sometime application required VM to be closer to reduce the latency. By placing them part of proximity group they will physically located close each other.
- Proximity group assigned to data center when creating the first vm inside it and removed with the last vm.
- When moving a resource into a proximity placement group, you should stop (deallocate) the asset first since it will be redeployed potentially into a different data center.
- VM, scale set or availability set should be in same region as the proximity group.
- Proximity group cannot assign when spreading across multiple AV zones. Onlt one AV zone is supported.
-  alignment status ( aligned, unknown, not aligned)
-  intent parameter - By selecting one or more sizes that you intend to deploy to this proximity placement group, you can make virtual machine and virtual machine scale set deployments more likely to succeed.
-  recall the setps of Adding VMs in an availability set to a proximity placement group
-  recall the setps of Adding existing VM to placement group

## Azure web app service (paas)
- Fully managed service in App Service.


## Deployment slots
- Recall the usage of this

## APP service plan
- An Azure App Service plan defines a set of compute resources for a web app to run
- Auto-scaling for Azure web apps can only be configured for web apps that are part of the Standard App Service Plan or higher.
- - Azure App Service cannot be directly moved to a different region.
- Pricing tiers
- 1. Shared compute (free and shared)  / cannot scale out
  2. Dedictaed (basic, standard, premium v2,v3) share the compute resource
  3. Isolated / uns on dedicated VN and provide compute and NW isolation. provide fill scale out features
- You can move an app to another App Service plan, as long as the source plan and the target plan are in the same resource group and geographical region and of the same OS type
- you can scale out an App Service Plan while the apps are online in Azure App Service. Scaling out increases the number of instances running your app without downtime
- scale up ( add more resources) / scale out ( add more instances)
- Revisit app service plan backups
  
- **Cost of App Service plans**
- Shared tier: Each app receives a quota of CPU minutes, so each app is charged for the CPU quota.
- Dedicated compute tiers (Basic, Standard, Premium, PremiumV2, PremiumV3): The App Service plan defines the number of VM instances that the apps are scaled to, so each VM instance in the App Service plan is charged
- IsolatedV2 tier: The App Service Environment defines the number of isolated workers that run your apps, and each worker is charged.
- Moving app between app servie plans
- You can move an app to another App Service plan, as long as the source plan and the target plan are in the same **resource group** and **geographical region** and of the **same OS type**. Any VNET integration configured on the app must be disabled prior to changing App Service plans.
- You can't change an App Service plan's region.CLone should be help
- Pricing tier and redundancy options are available
- Enabled: Your App Service plan and the apps in it will be zone redundant. The minimum App Service plan instance count will be three.
- Disabled: Your App Service Plan and the apps in it will not be zone redundant. The minimum App Service plan instance count will be one.

- Basic tier does not have auto scaling, it has manual scaling from 1to 2 machines.
- Standard (max 10 machines) and premium tiers has auto scaling based on rules .
- Rule based scaling again same as VM scale set

## Azure Container Registry
Azure Container Registry is a managed registry service based on the open-source Docker Registry 2.0. Create and maintain Azure container registries to store and manage your container images and related artifacts

## Containerization
- Install docket on vm, Create an docket image from local linux + nginx server. Copy it to container and run
- Publish image
- Go to VM -> login docker with image registry credentials -> docker tag command to tag the image with docker url
- docker push command to push image to registry
- Image registry, we have the image in registry

## Container instance
- Azure Container Instances enables exposing your container groups directly to the internet with an IP address and a fully qualified domain name (FQDN). When you create a container instance, you can specify a custom DNS name label so your application is reachable
- az container create (Azure CLI command)
- for small workload

## Container Groups
- Have to deploy using yml for ARM template
- A container group is a collection of containers that get scheduled on the same host machine. The containers in a container group share a lifecycle, resources, local network, and storage volumes
- Resource allocation.
- If you don't specify a resource limit, the container instance's maximum resource usage is the same as its resource request.
- If you specify a limit for a container instance, the instance's maximum usage could be greater than the request, up to the limit you set
- 
## Container apps
- Azure Container Apps is a serverless platform that allows you to maintain less infrastructure and save costs while running containerized applications.
- Applications built on Azure Container Apps can dynamically scale based on the following characteristics:
    HTTP traffic/
    Event-driven processing/
    CPU or memory load

Also we can deploy container based app using Azure web apps

## App Service Environment
- App Service Environment (ASE) is a fully isolated and dedicated environment for running App Service apps securely at high scale
- Higher Performance & Isolation → Dedicated compute resources ensure low latency and high performance.
-  Stronger Security → Fully controlled VNet integration, private IPs, and network security controls.
-  Scalability → Can scale up to 200 instances with autoscaling.
-  Compliance Support → Meets regulatory standards for sensitive applications (Finance, Healthcare, etc.).
-  Advanced Networking → Supports ExpressRoute, VPN Gateway, and network security policies.
-  Custom Domains & SSL → Enables secure access via custom domains with TLS/SSL support.

## Note
- Resource group is a logical grouping. Since changing it doesnot change any thing.
- Resource group is logical groups doesn't cost
- To resize one of the VMs in availability set all should have to stop
-  Add-AzVhd cmdlet uploads an on-premise virtual hard disk to a managed disk or a blob storage account.
-  The Linux diagnostic extension helps a user monitor the health of a Linux VM running on Microsoft Azure
-  Azure tags to enforce a naming convention, To organize resources for chargeback (cost tracking)
-  1. Not all resource types support tags
   2. Each resource, resource group, and subscription can have a maximum of 50 tag name-value pairs
   3. The tag name has a limit of 512 characters and the tag value has a limit of 256 characters
- Azure Virtual Machine Scale Sets do not support different VM sizes and configurations within the same scale set
- ACR Webhooks allow automated deployment updates by triggering an action when a new approved image is pushed to the Azure Container Registry (ACR).
- Converting unmanaged disks to managed disks allows you to take advantage of Azure's built-in encryption capabilities and management features, which can help enhance the security of data at rest within your virtual machines.
- .NET 8 LTS is supported on both Windows and Linux platforms (ASP.NET 4.8 runtime stack is only windows)
