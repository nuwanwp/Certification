## Implement and manage storage
- Components of blob storage account
- ![image](https://github.com/user-attachments/assets/b5390c06-d298-4242-bb79-7e77ae82bbaa)
- UNC path :- http://mystorageaccount.blob.core.windows.net/container/file
- Pass offering

- Type of storage accounts
- **Standard** storage accounts are backed by magnetic hard disk drives (HDD). A standard storage account provides the lowest cost per GB. You can use Standard storage for applications that require bulk storage or where data is infrequently accessed.

- **Premium** storage accounts are backed by solid-state drives (SSD) and offer consistent low-latency performance. You can use Premium storage for Azure virtual machine disks with I/O-intensive -applications like databases.

- 1. General-purpose v2 Standard
  2. Block blob Premium / 190.7TB
  3. Append blobs Premium / for vm loggings
  4. Page blobs Premium / 8tb / vhd files, disks

  - Naming of stroage account and containers
  - account name, blob names are case sentive

## Architecture
 1. Reliability
  - ZRS and GZRS give max availability and durability as redundency options
 2. Security
 3. Cost Optimization
 4. Azure policies
 5. Azure Advisor (Reliability,Security,Cost Optimization,Performance,Operational Excellence). Cannot create alerts from advisor
 
## Storage explorer
- Connect to storage account
- Can create containers and blobs
- Can create SAS on account level, container level and blob level
- Create and manage snapshots
- Storage Explorer does not currently support creating a user delegation SAS

## Quickstart
- 1. Storage explorer (create container/ upload-download blobs/ generate SAS / User delegated SAS not supported
  2. Powershell

  ```
       Connect-AzAccount

       $ResourceGroup = 'MyResourceGroup'
       New-AzResourceGroup -Name $ResourceGroup -Location $Location

       $StorageHT = @{
         ResourceGroupName = 'app-rg'
         Name              = 'mystorageaccount'
         SkuName           = 'Standard_LRS'
         Location          =  'southindia'
       }
       $StorageAccount = New-AzStorageAccount @StorageHT
       $Context = $StorageAccount.Context

       New-AzStorageContainer -Name $ContainerName -Context $Context

       $Blob1HT = @{
         File             = 'D:\Images\Image001.jpg'
         Container        = $ContainerName
         Blob             = "Image001.jpg"
         Context          = $Context
         StandardBlobTier = 'Hot'
       }
       Set-AzStorageBlobContent @Blob1HT

       Get-AzStorageBlob -Container $ContainerName -Context $Context |
       Select-Object -Property Name

       $DLBlob1HT = @{
         Blob        = 'Image001.jpg'
         Container   = $ContainerName
         Destination = 'D:\Images\Downloads\'
         Context     = $Context
       }
       Get-AzStorageBlobContent @DLBlob1HT

       Remove-AzStorageAccount -Name <storage-account> -ResourceGroupName <resource-group>
  ```

- 3. Azure CLI

  ```
     az login

     az group create --name <resource-group> --location <location>

     az storage account create --name <storage-account> --resource-group <resource-group> --location <location> --sku Standard_ZRS --encryption-services blob

     az storage container create \
     --account-name <storage-account> \
     --name <container> \
     --auth-mode login

     az storage blob upload --account-name <storage-account> --container-name <container> --name myFile.txt --file myFile.txt --auth-mode login

     az storage blob list --account-name <storage-account> --container-name <container> --output table --auth-mode login

     az storage account delete --name <storage-account> --resource-group <resource-group>

     az storage blob download --account-name <storage-account> --container-name <container> --name myFile.txt --file <~/destination/path/for/file> --auth-mode login

  ```

## Storage account overview
  1. Standard general-purpose v2 - LRS, GRS, ZRS, GZRS, RA-GRS, RA-GZRS
  2. Premium block blobs - LRS/ZRS high transaction rates or that use smaller objects or require consistently low storage latency
  3. Premium fileshares - enterprise or high-performance scale applications
  4. premium page blobs - video editing applications, Azure Disks

- NOTE :- You can't change a storage account to a different type after it's created

## Storage account workloads

![image](https://github.com/user-attachments/assets/ccf2ee47-374c-474d-97d6-ec3cb71e8a88)
- ZRS, RA-GRS	is enough for most cases.

## Storage account Standard endpoints
- Recall each endpoint UNC path for each type

## Storage account Migrate a storage account
- 1. Move a storage account to a different subscription - ok
  2. Move a storage account to a different resource group - ok
  3. Move a storage account to a different region - recreate -azcopy to migrate data
  4. Upgrade to a general-purpose v2 storage account - ok

## Storage account billing
- 1. Region
  2. Account ytpe
  3. Acces Tier
  4. Capacity
  5. Redundancy
  6. Transactions
  7. Data egress (data transferred out of an Azure region)

## Storage account Different SKUs

- Standard_LRS / Standard_GRS / Standard_RAGRS/ Standard_ZRS / Standard_GZRS / Standard_RAGZRS / Premium_LRS / Premium_ZRS
- Upgrading a general-purpose v2 is permanent and cannot be undone.
- You can't convert an existing standard general-purpose v2 storage account to a premium block blob storage account. migrate data is the option

## Storage account Restore
- The storage account was deleted within the past 14 days.
- Should be created using ARM deployment model
- A new storage account with the same name has not been created
- When a storage account is deleted, any linked private endpoints are also removed. These private endpoints are not automatically recreated when the storage account is recovered.

## Storage account Locks

- CannotDelete
- ReadOnly - list key is post operation and all post methods are blocked. lock does not invalidate previously issued access keys. for new keys has come through the Entra ID
- When having delete lock it is not possible to delete the storage account as well.
- If you apply a lock to a storage account (e.g., preventing deletion or modifications), it does not immediately revoke access keys that were already issued.
- If a client (user, application, or service) already has the storage account access keys before the lock is applied, they can continue to use those keys to access the storage account's data (Blobs, Queues, Tables, or Files).
- This means previously authorized connections using access keys will remain valid even after the lock is placed.
- If a client does not already have the storage account keys, they cannot obtain them after the lock is applied. Instead, they must use Microsoft Entra ID (formerly Azure AD) authentication to access Blob or Queue data.

NOTE :- Data in Azure Files or the Table service may become unaccessible to clients who have previously been accessing it with the account keys. As a best practice, if you must apply a ReadOnly lock to a storage account, then move your Azure Files and Table service workloads to a storage account that is not locked with a ReadOnly lock.

- Locking a storage account does not protect containers or blobs within that account from being deleted or overwritten

## Storage data movement
- azcopy copy 'C:\myDirectory' 'https://mystorageaccount.dfs.core.windows.net/mycontainer' --include-path 'photos;documents\myFile.txt' --recursive'

##  block blobs
- Block blobs are optimized for uploading large amounts of data efficiently
## page bloobs
- Page blobs are a collection of 512-byte pages optimized for random read and write operations.
## apend bloobs
- An append blob is composed of blocks and is optimized for append operations. When you modify an append blob, blocks are added to the end of the blob 

## Container

- RBAC can be assigned to storage account level and container level
- SAS also same as container level and blob level


## Anonymouse access
- This can be controller from Storage account and individual container level (private/ blob/ blob and container)

## Cost cutting
- reduce no of rehydrations from archive tier to higher tiers
- disabled SFTP endpoint
- Disable unwanted encryption scopes
- Pack small file and store them on cooler tiers

## Access Keys
- Giving complete access to all storage types (blob, files, queus and tables). Storage account level
 ![image](https://github.com/user-attachments/assets/78bc835f-b585-453d-840a-ef6370fbe398)

## Shared access signatures
- SAS can be created on blob level and container levels
- all permissions are embeded to the url
- 3 types of SAS
- 1.  service SAS provides access to a resource in just one of the storage services: the Blob, Queue, Table, or File service. Created when creating SAS for container level
  2.  account SAS is similar to a service SAS, but can permit access to resources in more than one storage service. Created when creating SAS for account level
  3.  user delegation SAS is a SAS secured with Microsoft Entra credentials and can only be used with Blob Storage service.

## Stored Access Policy
- in Container level
- It will be eaiser to change the permissions since no need to recreate the token url

## Lifecycle management policies
- Lifecycle management policies are supported for block blobs and append blobs in general-purpose v2, premium block blob, and Blob Storage accounts. Lifecycle management doesn't affect system containers such as the $logs or $web containers.
- No tier is premium only delete action is available
- Account level action
- take 24h to take effect
- Rule
  ```
  {
  "rules": [
    {
      "enabled": true,
      "name": "sample-rule",
      "type": "Lifecycle",
      "definition": {
        "actions": {
          "version": {
            "delete": {
              "daysAfterModificationGreaterThan | daysAfterCreationGreaterThan | daysAfterLastAccessTimeGreaterThan | daysAfterLastTierChangeGreaterThan": 2
            }
          },
          "baseBlob": {
            "tierToCool": {
              "daysAfterModificationGreaterThan": 30
            },
            "tierToArchive": {
              "daysAfterModificationGreaterThan": 90,
              "daysAfterLastTierChangeGreaterThan": 7
            },
            "delete": {
              "daysAfterModificationGreaterThan": 2555
            }
          }
        },
        "filters": {
          "blobTypes": [
            "blockBlob"
          ],
          "prefixMatch": [
            "sample-container/blob1"
          ]
        }
      }
    }
  ]
  }
  ```
- Note :- If you define more than one action on the same blob, lifecycle management applies the least expensive action to the blob. For example, action delete is cheaper than action tierToArchive. Action tierToArchive is cheaper than action tierToCool.
- Enable access tracking to get last access time
- Lifecycle management policies are free of charge
- A lifecycle management policy can't be used to change the tier of a blob that uses an encryption scope to the archive tier.
- Premium tier have life management policy but cannot move to tiers
- Only storage accounts that are configured for LRS, GRS, or RA-GRS support moving blobs to the archive tier. The archive tier isn't supported for ZRS, GZRS, or RA-GZRS accounts. This action gets listed based on the redundancy configured for the account.
-  Delete operations are free.
-  Support 100 rules and 10  prefixes
-  A lifecycle management policy will not delete the current version of a blob until any previous versions or snapshots associated with that blob have been deleted
-  Only storage accounts that are configured for LRS, GRS, or RA-GRS support moving blobs to the archive tier. The archive tier isn't supported for ZRS, GZRS, or RA-GZRS accounts.

## Versioning
- Soft delete is feature available in Account level and it effect to both blob level and container level
- Blob versioning is available for general-purpose v2, block blob, and Blob storage accounts
- Versioning is not supported for accounts that have a hierarchical namespace.
- When versioning is enabled, selecting the Undelete button on a deleted blob restores any soft-deleted versions or snapshots, but does not restore the base blob. To restore the base blob, you must promote a previous version.
- Microsoft recommends maintaining fewer than 1000 versions per blob
- Blob versioning cannot help you to recover from the accidental deletion of a storage account or container. To prevent accidental deletion of the storage account, configure a lock on the storage account resource
- Disabling blob versioning doesn't delete existing blobs, versions, or snapshots. When you turn off blob versioning, any existing versions remain accessible in your storage account.
- Microsoft recommends that after you enable blob versioning, you also update your application to stop taking snapshots of block blobs. No point of having snapshot it will increase the cost

## Point-in-time restore
- Only block blobs in a standard general-purpose v2 storage account can be restored as part of a point-in-time restore operation. Containers doesn'y support
- Soft delete,Change feed,Blob versioning shoudld be enabled.
- The retention period for point-in-time restore must be at least one day less than the retention period specified for soft delete.
- Point-in-time restores support only block blobs, not page or append blobs. While the restore is happening, containers and blobs cannot be read or written to.
- container cannot be restored with a point-in-time restore operation
- requires Soft delete,Change feed, Blob versioning to enabled
  
## Blob snapshots
- A snapshot is a read-only version of a blob that's taken at a point in time.
- A snapshot of a blob is identical to its base blob, except that the blob URI has a DateTime value appended to the blob URI to indicate the time at which the snapshot was taken
 http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z
- You can create a snapshot of a blob in the hot or cool tier. Snapshots on blobs in the archive tier aren't supported.
- By default, when you delete a base blob in Azure Blob Storage, all of its snapshots are deleted as well.

## Immutable Storage
- Container level policy and version level policy
- Policy which defined to restrict the modification.
- 1. Legal hold - we have remove it once completed
  2. Time based retention policy - period
- If you enable blob soft delete and then configure an immutability policy, any blobs that have already been soft deleted are permanently deleted once the soft delete retention policy is expired. Soft-deleted blobs can be restored during the soft delete retention period. A blob or version that hasn't yet been soft deleted is protected by the immutability policy and can't be soft deleted until after the time-based retention policy is expired or the legal hold is removed.
- Legal hold and time base retention cannt be enable when having point-in time restore is enabled
- There are two immutability policies
1. Version-level immutability cannot be disabled after it is enabled on the storage account, although locked policies can be deleted.
2. Container level immutability policy

## Redendency
- LRS - 3 copies in single data center / easily reconstructed if data loss occurs / region due to data governance requirements
- ZRS - replicates the data within your storage accounts to three or more Azure availability zones located in the primary region / excellent performance, low latency, and resiliency for your data
- GRS - copies your data synchronously three times to one or more availability zones in the primary region , three copies of data avaible in secondary region. (paired region)
- GZRS - copied across three Azure availability zones in the primary region and 3 copies in secorary region as LRS
- Both RA-GRS and RA-GZRS provides read access to secondary region when failover
- Premium account supports on LRS and ZRS only
- Page blobs LRS olny

## Object replication
- asyn operation | rule based
- source and dest should be Gen Purpose V2 or Premium (replication from General-Purpose v2 (GPv2) to Premium storage is not supported)
- Both ends versioning should be enabled
- Source account "change feed" should be enabled
- allowed diffrent regions / subscriptions
- Object replication supports block blobs only; append blobs and page blobs aren't supported.
- Object replication isn't supported for blobs in the source account that are encrypted with a customer-provided key
- If your storage account has object replication policies in effect, you cannot disable blob versioning for that account. You must delete any object replication policies on the account before disabling blob versioning.
- Object replication doesn't support blob snapshots.
- If a storage account does not currently participate in any cross-tenant object replication policies, then setting the AllowCrossTenantReplication property to false prevents future configuration of cross-tenant object replication policies with this storage account as the source or destination.
- When having Immutable applies in dest account,operation on the source container may succeed, but replication of that operation to the destination container will fail
- Changing the access tier of a blob in the source account won't change the access tier of that blob in the destination account.

## Azure FIle share types
- 1. Transaction optimized
-  2. Hot
-  3. Cool
-   4. Premium

## Data encription
- There is no additional cost for Azure Storage encryption.
- scoped to container level and blob level
- default encrypted MGK
- storage account is encrypted with a key that is scoped to the entire storage account. Encryption scopes enable you to manage encryption with a key that is scoped to a container or an individual blob
- when using customer manage keys from Key Vault we need to have "Key Vault Crypto Service Encryption user role.
- Server side and Client side (client side is encrypt data before upload)
- You can use encryption scopes to create secure boundaries between data that resides in the same storage account but belongs to different customers.
- When you upload a new blob with an encryption scope, you cannot change the default access tier for that blob also cannot set to archive tier
- Once an encryption scope is created, it is tied to data encryption, and deletion could compromise data security and cannot delete.
- When you upload a new blob with an encryption scope, you cannot change the default access tier for that blob. You also cannot set the access tier to the archive tier for an existing blob that uses an encryption scope
- When you disable an encryption scope, any subsequent read or write operations made with the encryption scope will fail with HTTP error code 403 (Forbidden)
- Keep in mind that customer-managed keys are protected by soft delete and purge protection in the key vault

## Networking
- Has a firewall
- Service endpoints (enable connection between VM and storage account)
- Private Endpoints :- brinng all the way in Vnet. private communication is possible withn Vnet
- Secure transfer required property (e=enbaled by default) for the storage account. When you require secure transfer, any requests originating from an insecure connection are rejected
- Azure Storage firewall rules only apply to data plane operations. Control plane operations are not subject to the restrictions specified in firewall rules.
- Private endpoint for your storage account, it provides secure connectivity between clients on your VNet and your storage. The private endpoint is assigned an IP address from the IP address range of your VNet. The connection between the private endpoint and the storage service uses a secure private link.
- public endpoint restriction support Vnet and IPs
- It is seperate deployment of private endpoint resource
- Network routing preference. MS backbone is the most secured and give higher performance


## Custom Domain
- two ways to configure a custom domain
- 1. Direct mapping - lets you enable a custom domain for a subdomain to an Azure storage account. For this approach, you create a CNAME record that points from the subdomain to the Azure storage account
  - Subdomain: blobs.contoso.com
  - Azure storage account: \<storage account>\.blob.core.windows.net
  - Direct CNAME record: contosoblobs.blob.core.windows.net

2.  Intermediary domain mapping - is applied to a domain that's already in use within Azure. This approach might result in minor downtime while the domain is being mapped. To avoid downtime, you can use the asverify intermediary domain to validate the domain.

- CNAME record: asverify.blobs.contoso.com
- Intermediate CNAME record: asverify.contosoblobs.blob.core.windows.net

## AZ Copy utility
- Familier with the few command

  
## Premium Storage service
- optimised for low latency and hi speed
- only LRS and ZRS
- Note Account Kind
- No tier change option

## NOTE
  - We can update Encryption type once account is created.
  - Azure blob container can only be backed up in Azure Backup vault as it supports Azure blob storage service. Azure Recovery service vault on the other hand does not support Azure Blob storage.
  - Azure file share can only be backed up in Azure RS vault as it supports Azure file share service. Azure Backup vault on the other hand does not support Azure File Share.
  - Simply having the role of Global Admin does not allow the user access to file share data
  - read access to the content of blob storage. Hence User1 cannot read File1 content inside container.
  - Storage Account Contributor role has all the permissions to manage a storage account, but not access to it
  - Azure file shares can only be mounted to container instances that are in the same region as the storage account.
  - ZRS ensures data residency by keeping the data within the same region, which complies with data sovereignty requirements.
  - ExpressRoute private peering allows on-premises resources to access Azure resources directly, bypassing firewall rules that apply to public endpoints.
  - Low latency, High transaction throughput, Global read access - Block Blob Storage account with RA-GRS
  - Private Endpoints ensure secure access to the Azure File Share without exposing it over the internet.
  - Lifecycle management policies are supported for block blobs and append blobs in general-purpose v2, premium block blob, and Blob Storage accounts.
  - Using VPN (or ExpressRoute private peering) allows on-premises servers to securely access the Azure File Share without requiring internet-based SMB access.
  - You can't convert a Standard storage account to a Premium storage account or vice versa.
  - Microsoft recommends using GZRS for applications that require consistency, durability, high availability, excellent performance, and resilience for disaster recovery. Enable RA-GZRS for read access to a secondary region when there's a regional disaster.
  - Move a storage account to a different subscription - can
  - Move a storage account to a different resource group - can
  - Move a storage account to a different region - cannot
  - Upgrade to a general-purpose v2 storage account - cannot
  - Billing factors (region, account types, acccess tier, capacity, redundency, transactions, data egress( data tranfer out of region)
  - Container name :- only contain lowercase letters, numbers, and hyphens, and must begin with a letter or a number. Each hyphen must be preceded and followed by a non-hyphen character
  - Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.
  - Access tiers are only supported in General Purpose V2 storage accounts.
  - AzCopy does not have a command to rename files.
    ![image](https://github.com/user-attachments/assets/802152b5-065e-4154-8838-4687b0f046d8)

- You can persist data for Azure Container Instances with the use of Azure Files.
- Live migration does not work for Blob Premium storage accounts.
- General Purpose V2 storage accounts that have the current replication type as Locally-redundant storage can convert the replication to Zone-redundant storage by requesting Azure support for a live migrationmigration
- Live migration is supported only for storage accounts that use LRS or GRS replication. If your account uses RA-GRS, then you need to first change your account's replication type to either LRS or GRS before proceeding. This intermediary step removes the secondary read-only endpoint provided by RA-GRS before migration." "ZRS supports general-purpose v2 accounts only
- Azure DNS zone endpoints are supported for accounts created with the Azure Resource Manager deployment model only
- To upgrade a general-purpose v1 or Blob storage account to a general-purpose v2 account, use Azure portal, PowerShell, or Azure CLI.
- Deleted storage accounts can be recoverd.( but there is criteria)
- ReadOnly lock is in effect :- List Keys operation is POST request and all post oepration are blocked. Cannot use access keys. only RBAC
- if you just want to download files, then verify that the Storage Blob Data Reader role (Azure Blob Storage) or the Storage File Data Privileged Reader role (Azure Files) has been assigned to your user identity, managed identity, or service principal.
- Least privilage to read file from FIleShare :- Storage File Data Reader
- When having delete resource lock either container it self of containers or file shares cannot be deleted.
- Secure transfer required property must be disabled in order for NFS Azure file shares. But recommended for blobs
- Storage firewall rules apply to the public endpoint of a storage account. You don't need any firewall access rules to allow traffic for private endpoints of a storage account
- A SAS secured with Microsoft Entra credentials is called a user delegation SAS, because the OAuth 2.0 token used to sign the SAS is requested on behalf of the user
- You can create a snapshot of a blob in the hot or cool tier. Snapshots on blobs in the archive tier aren't supported.
- Point-in-time restore provides protection against accidental deletion or corruption by enabling you to restore block blob data to an earlier state. REquires soft delete, change feed, blob versioning)
- blobs encrypted with custom keys cannot be moved to archive tier
- In an account that has soft delete enabled, a blob is considered deleted after it is deleted and retention period expires. Until that period expires, the blob is only soft-deleted and is not subject to the early deletion penalty.
- If you toggle the default access tier setting to a cooler tier in a general-purpose v2 account, then you're charged for write operations
- You're charged for both read operations (per 10,000) and data retrieval (per GB) if you toggle to a warmer tier
- Soft delete can be enabled on either new or existing file shares and blobs.
- The move will fail because CMK-encrypted blobs cannot be moved to Archive. Explanation: CMK-encrypted blobs cannot be moved to the Archive tier because the Archive tier does not support external dependencies like Azure Key Vault

## **Azure File Share**
- Provides SMB and NFS file shares. NFS for linux
- this is usefull as file servers, lift and shift apps, diangnotic shares(logs and anylytics), devtest, containerization
- Mounting methods
 1. Direct mount of an Azure file share using SMB or NFS
 2. Cache Azure file share on-premises with Azure File Sync
    
- SMB - Premium, transaction optimized, hot, and cool tiers
- NFS - premium only

- SMB :- windows and linux / Premium, transaction optimized, hot, and cool / LRS, ZRS, GRS, GZRS / Identity-based authentication (Kerberos), shared key authentication / not case sensitive
- NFS :- only linux / Premium only / LRS,ZRS / Host-based authentication / Case sensitive

- There are two main types of storage accounts
  1. General purpose version 2 (GPv2)
  2. FileStorage storage accounts -  Only FileStorage accounts can deploy both SMB and NFS file shares, as NFS is only supported in Premium file shares
       
- Methods of authentication
  1. On-premises Active Directory Domain Services (AD DS, or on-premises AD DS)
  2. Microsoft Entra Domain Services 
  3. Microsoft Entra Kerberos for hybrid identities - Microsoft Entra Kerberos allows you to use Microsoft Entra ID to authenticate hybrid user identities, which are on-premises AD identities that are synced to the cloud
  4. Active Directory authentication over SMB for Linux clients
  5. Azure storage account key
     
- In addition to directly connecting to the file share using the public endpoint or using a VPN/ExpressRoute connection with a private endpoint, SMB provides an additional client access
- Azure Files currently supports LRS, ZRS, GRS, and GZRS

- SMB File shares
- SMB Multichannel enables an SMB 3.x client to establish multiple network connections to an SMB file share
- NTLM v2 (storage keys) and keyberos authentication are allowed

- NFS File Share
- Windows not supported
- NFS Azure file shares are only offered on SSD file shares
- port 2049 to let clients communicate with the NFS Azure file share
- Azure Files doesn't support encryption in transit with the NFS protocol. You need to disable the Require secure transfer setting on the storage account to use NFS Azure file shares
- The NFS protocol can only be used from a machine inside of a virtual network
- NFS file shares are mounted

  
- Encryption
  1. Encryption in transit, which relates to the encryption used when mounting/accessing the Azure file share (default method) (NFS not suppoting this)
  2. Encryption at rest, which relates to how the data is encrypted when it's stored on disk (applies to both the SMB and NFS protocols)
- Data protection
  ![image](https://github.com/user-attachments/assets/252b6efa-b71c-45cb-957d-60d3c6a0f91d)

- Soft delete - Soft delete is enabled by default for new storage accounts. 1 and 365 days
- Backup - Azure Backup for Azure file shares handles the scheduling and retention of snapshots
- Redundancy - LRS,ZRS, GRS,GZRS
- SMB Multichannel - allows creating multiple channel enables for SMB 3.x clients to establith multiple nw connections
- Only standard SMB accounts support GRS and GZRS. Premium SMB shares and NFS shares don't support GRS and GZRS.
- Standard SMB Azure file shares offer three access tiers: transaction optimized, hot, and cool. All three tiers are stored on the same standard storage hardware
- Azure File Sync enables you to cache several Azure Files shares on an on-premises Windows Server or cloud virtual machine.
- Cloud tiering - is an optional feature of Azure File Sync. Frequently accessed files are cached locally on the server while all other files are tiered to Azure Files based on policy settings.
- UNC path format \\<storageAccountName>.file.core.windows.net\<fileShareName>
- File share names can contain only lowercase letters, numbers, and hyphens, and must begin and end with a letter or a number. The name cannot contain two consecutive hyphens.
- supports Snaphots in file share (help against application error and data corruption)

- Azure Files supports the following mechanisms to tunnel traffic between your on-premises workstations and servers and Azure SMB/NFS file shares
- virtual network gateway is seperate resource it provide VPN or Express route
- Azure VPN Gateway
- 1. Point-to-Site (P2S) VPN gateway connections, which are VPN connections between Azure and an individual client.without opening up port 445
- 2. Site-to-Site (S2S) VPN, connection to mount your Azure file shares from your on-premises network, without sending data over the open internet.
- 3. ExpressRoute, which enables you to create a defined route between Azure and your on-premises network that doesn't traverse the internet. Because ExpressRoute provides a dedicated path between your on-premises datacenter and Azure, ExpressRoute can be useful when network performance is a consideration

- RBAC roles
1. Storage File Data SMB Share Reader - Allows for read access to files and directories in Azure file shares
2. Storage File Data SMB Share Contributor - Allows for read, write, and delete access on files and directories in Azure file shares.
3. Storage File Data SMB Share Elevated Contributor - Allows for read, write, delete, and modify ACLs on files and directories in Azure file shares
4. Storage File Data Privileged Contributor - Allows read, write, delete, and modify ACLs in Azure file shares by overriding existing ACLs.
5. Storage File Data Privileged Reader - Allows read access in Azure file shares by overriding existing ACLs.

- Data Transfer
- Azure File Sync ( allows cloud tiering. Use Azure File Sync if you need a hybrid cloud file solution with bi-directional sync between on-prem file servers and Azure Files./ No direct support for region wise copy. Files must be manually copied to another storage account in the new region)
- Azure Storage Mover (Use Azure Storage Mover for large-scale migrations of structured data between storage types (e.g., from SMB to Azure Files or from NFS to Azure Blob) / designed for migrating storage across Azure regions.)
- AzCopy tool (Use AzCopy for fast, scriptable, one-time data transfers between local machines and Azure Storage./  can copy between storage accounts in different regions.)

- Azure Files offers two options for how your data is replicated in the primary region : LRS, GRS
- Redundancy in a secondary region :- .(GRS or GZRS) for standard SMB file shares. Premium file shares and NFS file shares must use LRS or ZRS.
- Individual files that are deleted will still be permanently erased. If you want to be able to restore individual files that have been deleted, you can use share snapshots or Azure file share backup.
- Azure Files supports SMB file shares with snapshots, allowing you to restore or access previous versions of files. You can mount a snapshot on Windows or linux
- Vaulted backup currently supports only full share recovery to an alternate location. The target File Share selected for restore needs to be empty.
- The vault tier provides longer retention than the snapshot tier
