<h1>Subscription Level Logging</h1>
<b>This tutorial outlines the configuration of additional resource groups and generating logs using Log Analytics Workspace</b>

<h2>Environments and Technologies Used</h2>

- <b>Microsoft Azure</b> 
- <b>Log Analytics Workspace</b>
- <b>Resource Groups</b>

<h2>Operating Systems</h2>

- <b>Windows 10</b>

<h2>Configuration Steps</h2>

![image](https://github.com/user-attachments/assets/2a125e2f-4cc8-41bd-a6f5-493c487a407c)
- <b>Navigate to Monitor and select activity log</b>
- <b>Click Export Activity Logs</b>

![image](https://github.com/user-attachments/assets/612c552e-c8ae-402d-b5d1-013e87a67ff3)
- <b>Add diagnostic setting</b>
- <b>Diagnostic setting name: ds-azure-activity</b>
- <b>Select all logs categories and Send to Log Analytics Workspace</b>
- <b>Subscription: Azure Subscription 1</b>
- <b>Log Analytics Workspace: LAW-Cyber-Lab-01 (eastus2)</b>

![image](https://github.com/user-attachments/assets/a0e3f95b-c353-4e95-bffe-26335e5dec42)
- <b>Create a new Resource Group named: Scratch-Resource-Group</b>
- <b>Region: East US 2</b>

![image](https://github.com/user-attachments/assets/7674c352-9fd2-4b15-963e-4cdee1386b2f)
- <b>Create a new Resource Group named: Critical-Infrastructure-Wastewater/b>
- <b>Region: East US 2</b>

![image](https://github.com/user-attachments/assets/e5509a7d-7fbb-49ae-a8e9-d5028a59fcb3)
- <b>Navigate to Log Analytics Workspace and query AzureActivity</b>

![image](https://github.com/user-attachments/assets/b29dfdbc-38f4-44c4-980d-00853c84478f)
- <b>Delete Scratch Resource Group</b>

![image](https://github.com/user-attachments/assets/7d7cce32-29af-43cb-acf1-7c4756bbd528)
- <b>Delete Critical Infrastructure Wastewater Resource Group</b>

![image](https://github.com/user-attachments/assets/22f167e4-4f16-4ca8-b960-bc45c1fc995c)
- <b>Navigate to Network Security Groups</b>
- <b>Select attack-vm</b>
- <b>Select inbound security rules and click add</b>
- <b>Destination port ranges: *</b>
- <b>Priority: 100</b>
- <b>Name:DANGER_AllowAnyCustomAnyInbound</b>

![image](https://github.com/user-attachments/assets/170f12fe-3de2-41db-85d0-ab632b15d696)
- <b>Navigate to Log Analytics Workspace and use this query to observe the logs for the Scratch and Critical Infrastructure Wastewater Resource Groups:</b>
``` 
// Querying for the deletion of critical Resource Groups
AzureActivity
| where ResourceGroup startswith "Critical-Infrastructure-" or ResourceGroup startswith "Scratch-Resouce-"
| order by TimeGenerated desc
```

![image](https://github.com/user-attachments/assets/e1848ec1-f2f3-4ec7-93ef-e52e82401e8d)
- <b>Use this query to observe the deletion of both the Scratch and Critical Infrastructure Wastewater Resource Groups:</b>
``` 
// Deletion activities within a certain timespan
AzureActivity
| where OperationNameValue endswith "DELETE"
| where ActivityStatusValue == "Success"
| where TimeGenerated > ago(30m)
| order by TimeGenerated
```

![image](https://github.com/user-attachments/assets/46b6d957-d0a4-457e-8674-6819b857f177)
- <b> Use this query to observe the changes to the Network Security Groups:</b>
``` 
// Querying for changes to network security groups
AzureActivity
| where OperationNameValue == "MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/SECURITYRULES/WRITE"
// Optionally, specific Resource Groups:
// | where ResourceGroup in ("resource-group-1", "resource-group-2") 
| order by TimeGenerated
```
