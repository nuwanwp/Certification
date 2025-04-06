
**Monitor and back up Azure resources**

## Azure Backup
- Offload on-premises backup
- Back up Azure IaaS VMs
- Support LRS, GRS, and ZRS
- Azure Backup helps protect your critical business systems and backup data against a ransomware attack
 
 ## Monitor and back up Azure resources

- Azure Monitor Service
- Azure monitor allow to collect data from azure resource and gives metrics, activity logs and alert features (against metrics).
- Activity log on Azure monitor log all the activities (control pane actions) - all the admin level actions
- Create alert on Azure Monitor (scpe, conditions, actions (action group))
- Supress alerts (create alert processing rule)
-  You can monitor VMs across different regions, but ensure latency and data transfer costs are considered.
-  Azure Monitor collects logs and metrics from VMs using the Log Analytics agent (deprecated) or Azure Monitor Agent (recommended).
-  The collected data is sent to a Log Analytics workspace, which can be in any region.
-  You can configure alerts, dashboards, and insights for VMs across multiple regions from a single Azure Monitor instance.

## Log analytic workspace
 - data can be collected from various resource and store them in form of table
 - TO send data to log anaalytic workspace we need to setup data collection rule
 - Once setup we can see the data in Event table and use KQL to query data
 - Data collection endpoint - Create endpoint and connet with data collection rule
 - Sedning custom logs to Log Analytic WS
   1. Create new table in Log Analytic WS
   2. Create data collectnoi endpoint
   3. Create data collection rule
   4. Verify the on Log Analytic WS by selecting the created table
  - Log Analytics Contributor built-in role required here

## VM Insights
- Enable it on VM -> Insights ( it will create the data collection rule and collect data)
- Given VM performance data to review


## VM backup
 - Back of VM are stored in recovery vaults
 - Duting VM back it takes snapshot and saved in VM it self
 - Both VM and recovery service vault should be same region
1. create recovery service vault
2. VM backup -> select the vault -> select policy sub type (Enhanced / Standard)
   - Standard - onece a day backup
   - Enhanced - multiple backups per day / support trusted launch / support ultra disk vm
3. Setup backup policy
4. You have to manually trigge the backup now
- This is how to take VM snapshot within the VM
- It creates restore points as well ( There are two options (file recovery and Restore VM)
- BY using file recovery we not connecting to the VM. COnneting only to the restore point

## VM restore
- It is required to have Azure storage account in the same region as the VM and service vault
- Either create new VM / Restore VM disk to create new VM
- Storage account is required
- You can backup and restore ADE encrypted VMS within the same subscription
- ADE encrypted VMS cannot recovered at file filder level
  
## Recovery service agent
- Download recovery service agent from the Service Vault properties page
- Install them on VM
- Register the vault and configure
- Cretae backup policy and trigger the backup now

-** Restore from another VM**
- Download recovery service agent on restore VM
- Select Recever data
- Select File and Folder and recover
- New drive will be mounted with recovered file and folders
- Go backup agaents in Vault page and remove the agent for clean up

  ## File Share backups
  - Same as VM backup and register FIle Share in Receovery service vault


  ## Backup for app services
  - backup and resotres is supported in the basic, standard and permium, isolated tiers
  - backup and restore in same web app page

  ## Backup report
  - for audit backup and restores
  - Workspace doesnot need to in the same regionas the Recovery Service Vault
  - Go to service vault diagnostic setting and attach the log analytic workspace
  - Then go to backup reports ( it may take 24h)

  ## Azure Site Recovery
  - replicate the workload that run on physical or VMs
  - replacate to another region when failure in primary location
  - It replicate the vm and disks (both os and data ) to another secondary region (involve cost of maintaining secondary site)
  - This is usefull when backup restore is not giving latest data.
  - Stroage account is required here in source location
  - It create recovery service vaults in target location
  - Go to VM disaster recovery -> create new vnet in target location -> select the target reosurces -> and start the replication
  - Simulate the failover


  ## Backup vault
  - Store the backup data items
  - Create Backup Vault from portal
   - Asssign Disk backup Reader role to the backup vauult
   - Assing Disk Snacpshot Contributor role to the backup vauult
   - Create backup poicy
   - Then create backup instance and select data sources
   - When backup the storage account we should grant Storage Account backup contributor to the backup vault
   
