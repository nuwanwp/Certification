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

## Size
- General purpose  A,B,D (A - Entry-level economical,B -  Burstable, D - Enterprise-grade applications, Relational databases, In-memory caching, Data analytics)
- Compute optimized F (Medium traffic web servers,Network appliances,Batch processes,Application servers)
- Memory optimized E (Relation databases, Medium to large caches)
- Storage optimized L (High disk throughput and IO, Big Data, SQL and NoSQL databases, Data warehousing)
- GPU accelerated NC/ND (Compute-intensive,Graphics-intensive Visualization)
- FPGA accelerated NP (machine learning)
- High performance computer (HB/ HC)( Weather modeling, Computational chemistry)

- You can't change a virtual machine's generation after you've created it
- If the virtual machine is currently running, changing its size will cause it to restart.
- You can't resize a VM size that has a local temp disk to a VM size with no local temp disk and vice versa
- NVM Express (NVMe) is a communication protocol that facilitates faster and more efficient data transfer between servers and storage systems 
- Generation 2 VMs use the new UEFI-based boot architecture rather than the BIOS-based architecture but no price difference.
- Azure Compute offers virtual machine sizes that are Isolated to a specific hardware type and dedicated to a single customer. The Isolated sizes live and operate on specific hardware generation and will be deprecated when the hardware generation is retired

## Troubleshooting
- Reset your RDP connection
- Verify Network Security Group rules
- Review VM boot diagnostics.
- Reset the NIC for the VM
- Check VM Resource Health
- Reset user credentials
- Verify routing

## Disk
 - OS level disk and Data disk (Seperate Az Resource)
 - Azure managed disks are block-level storage volumes that are managed by Azure 
- Disk types
- ![image](https://github.com/user-attachments/assets/5f4a8028-84f7-4a16-9aa1-604653bf4fd8)
- Manage disk has two redundancy options (LRD and ZRS)
- ZRS replicates your Azure managed disk synchronously across three Azure availability zones in the selected region
- There are several types of encryption available for your managed disks, including **Azure Disk Encryption** (ADE), **Server-Side Encryption** (SSE), and **encryption at host**.
- VM temporary storage goes away when restarting. Only data disk and OS disk remains.
- Disk snapshot can be created from Disk Snaphot serive and create back disk and attached to VM

## Disk Encryption (no extra cost)
- **Azure Disk Storage Server-Side Encryption** (PMK and Custom Managed Keys through the Key Vault) encryption-at-rest - AES-256
- **Azure Disk Encryption** helps protect and safeguard your data to meet your organizational security and compliance commitments. Ex:- Bit Locker
- Encryption at host is a Virtual Machine option that enhances Azure Disk Storage Server-Side Encryption to ensure that all temp disks and disk caches are encrypted at rest
- Virtual Machines aren't rebooted during automatic key rotation.
- **Restriction on custom managed keys**
[- If this feature is enabled for a disk with incremental snapshots, it can't be disabled on that disk or its snapshots
- A disk and all of its associated incremental snapshots must have the same disk encryption set.](https://learn.microsoft.com/en-us/azure/virtual-machines/disk-encryption?view=rest-compute-2024-11-04#restrictions)
- Limitation of customer managed keys for snapshots
-	Encryption key sets must be in the same subscription as your image.
-	Encryption key sets are regional resources, so each region requires a different encryption key set.
-	You can't go back to using platform-managed keys for encrypting those images
- Use of Disk Encryption set

## Azure compute galley
- gallery, you can share your resources to everyone. disk snapshots
  
## Custom script extenstion
- Reposible for preparing initial software installation and preparing works
- 90 minutes to run
- Cloud Init

## Boot diagnostics
- Boot diagnostics is a debugging feature for Azure virtual machines (VM) that allows diagnosis of VM boot failures

## Run command in VM
To run scripts, installings etc.

## Redeploy and ReApply
- Redeploy the VM, virtual machine will be restarted and you will lose any data on the temporary drive.
- Reaplly -  Reapplying your virtual machine's state

## Availability Set
- Sets place VMs in different fault domains and update domains
- Microsoft guarantees a 99.95% SLA
- Azure Availability Set cannot be moved directly to a different subscription.
- Azure Availability Set cannot be moved directly to a different region because it is a regional resource.
- Azure Availability Set cannot be removed or deleted while there are VMs assigned to it
- You cannot directly detach a Virtual Machine (VM) from an Azure Availability Set because an Availability Set is assigned when the VM is created

## Availability Zones
- Separated groups of datacenters within a region. NO cost (only the data transfer has cost or no).
- Each Availability zone is a unique physical location in an Azure region. 99.99% availability.
- 
- VM or scale set can be deployed as **Zonal** (VM resources are deployed to a specific, self-selected availability zone to achieve more stringent latency or performance requirements.)
- or **Zone redundant** (VM resources are replicated to one or more zones within the region to improve the resiliency of the application and data in a High Availability)

- if a Virtual Machine (VM) is deployed in a ZRS (Zone-Redundant Storage) Availability Set, its disk must also be ZRS-enabled to ensure the same level of redundancy.
- Both availability sets abd av zone does not replicate the app. Both giving iaas service.

## Scale sets
- Same as other 2 data/app is not replicating
- No additional cost for using scale sets. You are charged based on the compute, network, and storage resources that the scale set uses.
- Scale is another azure resource and VM are attached to that.
- Manual scaling and Auto scaling with rules
- Duration, Cool down parameters.

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

-The VM must be in the same resource group as the scale set.

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

-Can apply custom script both Orchestration modes to scale set (Remember latest model flag)

## VM images
- Image definition contains meta data and version info
- Two types specialized VM images and Generic Images.
- Specialized VM images contain all computer names and admin user info.
- Generic VM images do not contain those. After it creates original VM will not be usable.
- Gen1 and Gen 2 images. Gen1 VM cannot create Gen2 images
- Once you generalize a VM in Azure, you cannot restart it because the VM is permanently marked as a generalized image.

## Proximity Groups
- Separate azure resource
- Sometime application required VM to be closer to reduce the latency. By placing them part of proximity group they will physically located close each other.
- Proximity group assigned to data center when creating the first vm inside it and removed with the last vm.
- When moving a resource into a proximity placement group, you should stop (deallocate) the asset first since it will be redeployed potentially into a different data center.

## Azure web app service (paas)
- Fully managed service in App Service.
- Azure App Service cannot be directly moved to a different region.

## APP service plan
- An Azure App Service plan defines a set of compute resources for a web app to run
- Shared (Free, Shared)/ Dedicated (Basic, Standard, Premium) / Isolated (IsolatedV2)
- Auto-scaling for Azure web apps can only be configured for web apps that are part of the Standard App Service Plan or higher.
  
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
  
## Note
- Resource group is a logical grouping. Since changing it doesnot change any thing.
- Resource group is locagical groups doesn't cost
- To resize one of the VMs in availability set all should have to stop
-  Add-AzVhd cmdlet uploads an on-premise virtual hard disk to a managed disk or a blob storage account.
-  The Linux diagnostic extension helps a user monitor the health of a Linux VM running on Microsoft Azure
