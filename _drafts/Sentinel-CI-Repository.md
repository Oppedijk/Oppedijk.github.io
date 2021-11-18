---
layout: post
title:  "Microsoft Sentinel Repositories to push CI/CD changes to  Sentinel"
excerpt_separator: <!--more-->
---
With the latest announcement (november 8th, 2021) on Repository support in Microsoft Sentinel, we now have a supported workflow to enable Continoud Deployment integration from Azure DevOps or Github:
https://techcommunity.microsoft.com/t5/microsoft-sentinel-blog/enable-continuous-deployment-natively-with-microsoft-sentinel/ba-p/2929413

Out of the box the following content types are supported
 - Analytic rules
 - Automation rules
 - Hunting queries
 - Parsers
 - Playbooks
 - Workbooks

 But with some tweaks we can also deploy Sentinel Watchlist, with some issues regarding updating/deleting content

<!--more-->

### Templates

### Watchlist
For watchlist to be deployed, we need to tweak the 2 files in the `.sentinel` folder

Add `Watchlist` to the `sentinel-deploy-{guid}` file
```
    ScriptArguments: '-ResourceGroupName ''rg_msdn_oms'' -ContentTypes ''AnalyticsRule,AutomationRule,HuntingQuery,Parser,Playbook,Workbook,Watchlist'' -WorkSpaceName ''{MySentinelWorkspaceName}'''
```

Add the following line to the ContentTypeMap in `sentinel-deploy.ps1`
```
    "Watchlist"=@("Microsoft.OperationalInsights/workspaces/providers/Watchlists");
```

And finally add a `watchlist/mywatchlist.json' with some sample content:
```
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "workspaceName": {
          "type": "string",
          "defaultValue": "mySentinelWorkspaceName",
          "metadata": {
              "description": "Workspace name for Log Analytics where Sentinel is setup"
          }
      }
  },
    "resources": [
        {
        "name": "[concat(parameters('workspaceName'), '/Microsoft.SecurityInsights/AzurePublicIPList')]",
        "type": "Microsoft.OperationalInsights/workspaces/providers/Watchlists",
        "kind": "",
        "properties": {
            "displayName": "AzurePublicIPList",
            "source": "AzurePublicIPList.csv",
            "description": "Azure Public IPs list for reducing internet facing traffic alerts from MSFT IP Addresses",
            "provider": "Microsoft",
            "isDeleted": false,
            "labels": [
            ],
            "defaultDuration": "P1000Y",
            "contentType": "Text/Csv",
            "numberOfLinesToSkip": 0,
            "itemsSearchKey": "ExceptionIPAPIPs",
            "rawContent": "ExceptionIPAPIPs,ExceptionExpirationAPIPs,ExceptionNotesAPIPs\r\n1.66.60.119/32,9993-12-31T23:59:59Z,ActionGroup\r\n157.55.80.0/21,9999-12-31T23:59:59Z,AzureCloud.southcentralus\r\n157.55.103.32/27,9999-12-31T23:59:59Z,AzureCloud.southcentralus\r\n"
        },
        "apiVersion": "2021-03-01-preview"
        }       
    ]
}
```