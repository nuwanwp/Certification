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

## VM backup
 - REcevery service vault
 - Both VM and service vault should be same region
   
