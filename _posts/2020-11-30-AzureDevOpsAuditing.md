---
layout: post
title:  "AzureDevOpsAuditing with Sentinel"
excerpt_separator: <!--more-->
---

### Introduction
(this post will be updated with new tips and tricks, and when changes happen on Azure DevOps Auditing and Sentinel integration)

Azure DevOps streaming https://docs.microsoft.com/en-us/azure/devops/organizations/audit/auditing-streaming?view=azure-devops was recently introduced
This allows you to directly get the Azure Audit logs into a SIEM solution like Azure Sentinel, without writing custom Powershell/Rest code.

<!--more-->
### Setting up
The setup is easy, connect the Azure Log Analytics workspace id and key and the data is streamed to the Log Analytics environment
### Query data
All data ends up in the System table called `AzureDevOpsAuditing`

### Example reports
For example to get all Group changes:
```
AzureDevOpsAuditing 
| where Area == "Group"
| project ActorUPN, OperationName, Details, IpAddress, TimeGenerated
```

### Resources
More info on the AzureDevOpsAuditing table can be found here:
[AzureDevOpsAuditing](https://docs.microsoft.com/en-us/azure/azure-monitor/reference/tables/azuredevopsauditing)

### Conclusion
This is a nice solution to quickly get the insight into what is happening in your DevOps Environment.