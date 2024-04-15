---
layout: post
title: MDC Enable Defender for Containers through Azure CLI
excerpt_separator: <!--more-->
---
Enabling the Defender for Containers on Microsoft Defender for Cloud through the Azure CLI

<!--more-->
### Enable the base plan (through the pricing setting)
    az login
    az provider register --namespace Microsoft.Security
    
    az security pricing create -n Containers --tier standard --extensions name=ContainerRegistriesVulnerabilityAssessments isEnabled=True --extensions name=AgentlessDiscoveryForKubernetes isEnabled=True

This will enable the base plan, together with the 2 extensions. However this will leave the UI still in a "Partial" state. We need to deploy some policies to enable the 2 remaining components.

### Policy deployment
Use the following AZ CLI commands to enable the Defender for Sensors and the Azure Policy for Kubernetes components.

    az policy assignment create --name 'config-arc-extension' --display-name 'config arc extension' --scope subscriptions/{subscription_guid} --policy 708b60a6-d253-4fe0-9114-4be4c00f012c --description '[Preview]: Configure Azure Arc enabled Kubernetes clusters to install Microsoft Defender for Cloud extension' --location eastus --mi-system-assigned

    az policy assignment create --name 'config-arc-extension2' --display-name 'config arc extension2' --scope subscriptions/{subscription_guid} --policy 64def556-fbad-4622-930e-72d1d5589bf5 --description 'Configure Azure Kubernetes Service clusters to enable Defender profile' --location eastus --mi-system-assigned


    az policy assignment create --name 'azure-pol-add-on-k8' --display-name 'azure pol add-on k8' --scope subscriptions/{subscription_guid} --policy 0a15ec92-a229-4763-bb14-0ea34a568f8d --description 'Azure Policy Add-on for Kubernetes service (AKS) should be installed and enabled on your clusters' --location eastus --mi-system-assigned

    az policy assignment create --name 'azure-pol-add-on-k8-2' --display-name 'azure pol add-on k8 - 2' --scope subscriptions/{subscription_guid} --policy a8eff44f-8c92-45c3-a3fb-9880802d67a7  --description 'Deploy Azure Policy Add-on to Azure Kubernetes Service clusters' --location eastus --mi-system-assigned

    az policy assignment create --name 'azure-pol-add-on-k8-3' --display-name 'azure pol add-on k8 - 3' --scope subscriptions/{subscription_guid} --policy 0adc5395-9169-4b9b-8687-af838d69410a  --description 'Configure Azure Arc enabled Kubernetes clusters to install the Azure Policy extension'  --location eastus --mi-system-assigned


