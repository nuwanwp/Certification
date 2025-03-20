## **Implement and manage storage**
- Pass offering
- Blob storage - unstructured data
- File Share -  managed service
- Queue - queue service for apps
- Table - no relational unstrctured data
- Blob Storage offers three types of resources:
- 1. The storage account 
  2. A container in the storage account
  3. A blob in a container
     
     ![image](https://github.com/user-attachments/assets/b5390c06-d298-4242-bb79-7e77ae82bbaa)

## Type of storage accounts
- 1. General-purpose v2 - standard tier ( support LRS< GRS,RA_GRS, ZRS, RA_ZRS)
  2. Block blob - premium tier ( high transactions, smaller object , low latency) (LRS, ZRS)
  3. Page blob - premium tier (only for page blobs) page blobs are ideal for storing index-based and sparse data structures like OS and data disks for Virtual Machines and Databases (LRS, ZRS)
 
## Type of blobs
 - Block blobs
 - Append blobs
 - Page blobs

## Cost cutting
- reduce no of rehydrations from archive tier to higher tiers
- disabled SFTP  endpoint
- Disable unwanted encryption scopes

  ![image](https://github.com/user-attachments/assets/794aa0f9-e764-422d-bb8f-660a94bfdc4e)

## Anonymouse access
- THis can be controller from Storage account and individual container level (private/ blob/ blob and container)

## Access Keys
- Giving complete access to all storage types (blob, files, queus and tables). Storage account level

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
- {
-  "rules": [
-    {
-      "enabled": true,
-      "name": "hot_to_cool",
-     "type": "Lifecycle",
-      "definition": {
-        "actions": {
-          "baseBlob": {
-            "delete": {
-             "daysAfterModificationGreaterThan | daysAfterCreationGreaterThan | daysAfterLastAccessTimeGreaterThan | daysAfterLastTierChangeGreaterThan": 2
-            }
-          }
-        },
-        "filters": {
-          "blobTypes": [
-            "blockBlob"
-          ]
-        }
-      }
-    }
-  ]
- }
- When having overlapping rule. least cost action will be taken

  ## Object replication
  - asyn operation |  rule based
  - source and dest should be Gen Purpose V2 or Premium
  - Both ends versioning should be enabled
  - Source account "change feed" should be enabled
  - allowed diffrent regions / subscriptions

  ## Blob snapshot
   -  blob level feature to create snaphosts and premote
 
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

## Premium Staorage service
- optimised for low latency and hi speed
- only LRS and ZRS
- Note Account Kind
- No tier change option
  

  
