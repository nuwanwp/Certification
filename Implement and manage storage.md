## **Implement and manage storage**

## Indexes
- 1. Azure storage accounts have two types
  2. 4 services
  3. Storage account types with usages
  4. Redundency
  5. DNS mapping
  6. Moving storage accounts
  7. Billing factors
  8. Blob storage
  9. Container Access levels
  10. Access Keys
  11. Access Policy
      - 1. Stored access policy
        2. Immutable blob storage
  12. Access Policy
  13. Life cycle management
  14. Object replication

## Properties
- Pass offering
- Blob storage - unstructured data
- File Share -  managed service
- Queue - queue service for apps
- Table - no relational unstrctured data
- Blob Storage offers three types of resources:
- 1. The storage account 
  2. A container in the storage account
  3. A blob in a container
 
- Blob Storage uses three resources to store and manage your data:
- 1 .An Azure storage account
- 2. Containers in an Azure storage account
- 3. Blobs in a container
     
![image](https://github.com/user-attachments/assets/b5390c06-d298-4242-bb79-7e77ae82bbaa)

![image](https://github.com/user-attachments/assets/b8a60b66-bcc1-42b4-b154-c41efb5dd7b6)

## Storage account types
- **Standard** storage accounts are backed by magnetic hard disk drives (HDD). A standard storage account provides the lowest cost per GB. You can use Standard storage for applications that require bulk storage or where data is infrequently accessed.

 - **Premium** storage accounts are backed by solid-state drives (SSD) and offer consistent low-latency performance. You can use Premium storage for Azure virtual machine disks with I/O-intensive -applications like databases.

## Type of storage accounts
- 1. General-purpose v2 - standard tier ( support LRS< GRS,RA_GRS, ZRS, RA_ZRS)
  2. Block blob - premium tier ( high transactions, smaller object , low latency) (LRS, ZRS, RA-GZRS)
  3. Page blob - premium tier (only for page blobs) page blobs are ideal for storing index-based and sparse data structures like OS and data disks for Virtual Machines and Databases (LRS, ZRS, RA-GZRS)
 
## Azure Blob storage support 3 types of blobs
 - Block blobs
 - Append blobs
 - Page blobs

## Cost cutting
- reduce no of rehydrations from archive tier to higher tiers
- disabled SFTP  endpoint
- Disable unwanted encryption scopes
- Store large fil ein cooloer tiers

  ![image](https://github.com/user-attachments/assets/794aa0f9-e764-422d-bb8f-660a94bfdc4e)

## Anonymouse access
- This can be controller from Storage account and individual container level (private/ blob/ blob and container)

## Access Keys
- Giving complete access to all storage types (blob, files, queus and tables). Storage account level

- ![image](https://github.com/user-attachments/assets/78bc835f-b585-453d-840a-ef6370fbe398)


## Shared access signatures
- SAS can be created on blob level and container levels
- all permissions are embeded to the url

## Stored Access Policy
- in Container level 
- It will be eaiser to change the permissions since no need to recreate the token url

## Immutable Storage
- Container level
- Policy which defined to restrict the modification.
- 1. Legal hold - we have remove it once completed
  2. Time based retention policy - period

 ## Redendency
 - LRS - 3 copies in single data center
 - ZRS - data synchonously copy across three AV zones in primary region
 - GRS - three copies of data avaible in primary region, three copies of data avaible in secondary region. (paired region)
 - GZRS - data synchonously copy across three AV zones in primary region and 3 copies in secorary region as LRS
   
## Access Tiers
- Configured for the storage account
```
{
 "rules": [
   {
     "enabled": true,
     "name": "hot_to_cool",
    "type": "Lifecycle",
     "definition": {
       "actions": {
         "version": {
            "delete": {
              "daysAfterCreationGreaterThan": 90
            }
          },
         "baseBlob": {
           "delete": {
            "daysAfterModificationGreaterThan | daysAfterCreationGreaterThan | daysAfterLastAccessTimeGreaterThan | daysAfterLastTierChangeGreaterThan": 2
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
- When having overlapping rule. least cost action will be taken
-  For example, action delete is cheaper than action tierToArchive. Action tierToArchive is cheaper than action tierToCool.
-  Premium tier have life management policy but cannot move to tiers

## Encryption
There is no additional cost for Azure Storage encryption.

  ## Object replication
  - asyn operation |  rule based
  - source and dest should be Gen Purpose V2 or Premium
  - Both ends versioning should be enabled
  - Source account "change feed" should be enabled
  - allowed diffrent regions / subscriptions
  - Object replication supports block blobs only; append blobs and page blobs aren't supported.
  - Object replication isn't supported for blobs in the source account that are encrypted with a customer-provided key
  - If your storage account has object replication policies in effect, you cannot disable blob versioning for that account. You must delete any object replication policies on the account before disabling blob versioning.
  - Object replication doesn't support blob snapshots.
  - If a storage account does not currently participate in any cross-tenant object replication policies, then setting the AllowCrossTenantReplication property to false prevents future configuration of cross-tenant object replication policies with this storage account as the source or destination.

  ## Blob snapshot
   -  blob level feature to create snaphosts and promote
 
  ## Blob versioning
  - Enable versioning in Data protection and re call how version created and Make current version
 
  ## Azure FIle share types
  - 1. Transaction optimized
    2. Hot
    3. Cool

 ## Data encription
  - default encrypted MGK
  - when suing customer manage keys from Key Vault we need to have "Key Vault Crypto Service Encryption ) user.

## AZ Copy utility
- Familier with the few command

- ## Networking Service
- Has a firewall
- Service endpoints (enable connection between VM and storage account)
- Default setting is 'Enabled for all networks'
- Go to VM -> Service endpoints -> Select storage and create service endpoint
- It enable secure communication in between VM and storage account. GO to networking page of storage account and add network as enabled
- Private Endpoints :- brinng all the way in Vnet. private communication is possible withn Vnet
- It is seperate deployment of private endpoint resource

## Network routing
- MS glabal net working - maximum nw performance / higher cost

## Custom Domain
- two ways to configure a custom domain
- 1. Direct mapping - lets you enable a custom domain for a subdomain to an Azure storage account. For this approach, you create a CNAME record that points from the subdomain to the Azure storage account
  - Subdomain: blobs.contoso.com
  - Azure storage account: \<storage account>\.blob.core.windows.net
  - Direct CNAME record: contosoblobs.blob.core.windows.net
 2. Intermediary domain mapping - is applied to a domain that's already in use within Azure. This approach might result in minor downtime while the domain is being mapped. To avoid downtime, you can use the asverify intermediary domain to validate the domain.
- CNAME record: asverify.blobs.contoso.com
- Intermediate CNAME record: asverify.contosoblobs.blob.core.windows.net

## Premium Storage service
- optimised for low latency and hi speed
- only LRS and ZRS
- Note Account Kind
- No tier change option
  

  NOTE
  - We can update Encryption type once account is created.
  - Azure blob container can only be backed up in Azure Backup vault as it supports Azure blob storage service. Azure Recovery service vault on the other hand does not support Azure Blob storage.
  - Azure file share can only be backed up in Azure RS vault as it supports Azure file share service. Azure Backup vault on the other hand does not support Azure File Share.
  - Simply having the role of Global Admin does not allow the user access to file share data
  - read access to the content of blob storage. Hence User1 cannot read File1 content inside container.
  -  Storage Account Contributor role has all the permissions to manage a storage account, but not access to it
  -  Azure file shares can only be mounted to container instances that are in the same region as the storage account.
  -  ZRS ensures data residency by keeping the data within the same region, which complies with data sovereignty requirements.
  -  ExpressRoute private peering allows on-premises resources to access Azure resources directly, bypassing firewall rules that apply to public endpoints.
  -  Low latency, High transaction throughput, Global read access - Block Blob Storage account with RA-GRS
  -  Private Endpoints ensure secure access to the Azure File Share without exposing it over the internet.
  -  Using VPN (or ExpressRoute private peering) allows on-premises servers to securely access the Azure File Share without requiring internet-based SMB access.
  -  You can't convert a Standard storage account to a Premium storage account or vice versa.
  -  Microsoft recommends using GZRS for applications that require consistency, durability, high availability, excellent performance, and resilience for disaster recovery. Enable RA-GZRS for read access to a secondary region when there's a regional disaster.
  -  Move a storage account to a different subscription  - can
  -  Move a storage account to a different resource group - can
  -  Move a storage account to a different region - cannot
  -  Upgrade to a general-purpose v2 storage account - cannot
  -  Billing factors (region, account types, acccess tier, capacity, redundency, transactions, data egress( data tranfer out of region)
  -  Container name :- only contain lowercase letters, numbers, and hyphens, and must begin with a letter or a number. Each hyphen must be preceded and followed by a non-hyphen character
  -  Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.
  -  Access tiers are only supported in General Purpose V2 storage accounts.
  -  AzCopy does not have a command to rename files.
![image](https://github.com/user-attachments/assets/802152b5-065e-4154-8838-4687b0f046d8)
- You can persist data for Azure Container Instances with the use of Azure Files.
- Live migration does not work for Blob Premium storage accounts.
-  General Purpose V2 storage accounts that have the current replication type as Locally-redundant storage can convert the replication to Zone-redundant storage by requesting Azure support for a live migrationmigration
-  Azure DNS zone endpoints are supported for accounts created with the Azure Resource Manager deployment model only
-  To upgrade a general-purpose v1 or Blob storage account to a general-purpose v2 account, use Azure portal, PowerShell, or Azure CLI.
-  Deleted storage accounts can be recoverd.( but there is criteria)
-  ReadOnly lock is in effect :- List Keys operation is POST request and all post oepration are blocked. Cannot use access keys. only RBAC
-  if you just want to download files, then verify that the Storage Blob Data Reader role (Azure Blob Storage) or the Storage File Data Privileged Reader role (Azure Files) has been assigned to your user identity, managed identity, or service principal.
-  Least privilage to read file from FIleShare :- Storage File Data Reader
-  When having delete resource lock either container it self of containers or file shares cannot be deleted.
-  Secure transfer required property must be disabled in order for NFS Azure file shares. But recommended for blobs
-  Storage firewall rules apply to the public endpoint of a storage account. You don't need any firewall access rules to allow traffic for private endpoints of a storage account
-  A SAS secured with Microsoft Entra credentials is called a user delegation SAS, because the OAuth 2.0 token used to sign the SAS is requested on behalf of the user
- You can create a snapshot of a blob in the hot or cool tier. Snapshots on blobs in the archive tier aren't supported.
-  Point-in-time restore provides protection against accidental deletion or corruption by enabling you to restore block blob data to an earlier state. REquires soft delete, change feed, blob versioning)
-  blobs encrypted with custom keys cannot be moved to archive tier
-  In an account that has soft delete enabled, a blob is considered deleted after it is deleted and retention period expires. Until that period expires, the blob is only soft-deleted and is not subject to the early deletion penalty.
-  If you toggle the default access tier setting to a cooler tier in a general-purpose v2 account, then you're charged for write operations
-  You're charged for both read operations (per 10,000) and data retrieval (per GB) if you toggle to a warmer tier
-  Soft delete can be enabled on either new or existing file shares and blobs
-  Create storage account
-  1. Powershell      
      - Create storage account
      - ```
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
        
        ```
      
   -  2. Azure Cli
        - ```
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
          
        - ```
