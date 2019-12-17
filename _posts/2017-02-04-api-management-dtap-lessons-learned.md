---
layout: post
title: API Management DTAP lessons learned
permalink: Blog/api-management-dtap-lessons-learned
---
With API management we have seperate environments for Development, Test and Production.

The best way to move an API from one environment to the other is through GIT at this moment.

[https://docs.microsoft.com/nl-nl/azure/api-management/api-management-configuration-repository-git](https://docs.microsoft.com/nl-nl/azure/api-management/api-management-configuration-repository-git)

## Visualize differences

After setting up the 3 GIT environments and cloning them locally

We use a 3 way merge tool ([http://meldmerge.org](http://meldmerge.org)) to display all 3 branches.

## Best practices

Try to keep all files the same across all environments

Use properties for all Server URL's, (you need to set this from a policy through the set-backend-service expression, because the API ServerURL cannot contain properties at this moment yet (feb 2017)

Create a property "ENVIRONMENT" and use the "choose" policy expression to select parts of the policy you want to apply. This way all settings are always in GIT available. Future enhancement would be for instance use Azure Key Vault when it is supported by APIM. We use this for different ip-address restrictions on each environment.

Keep all GUIDS the same across all environments. You can change all guids on demand, except the "Product" guid, where all subscriptions are tied to. We even updated the default "group" guids (administrator, developer, guest) to be the same across all environments.

Properties can be exported through powershell.

Don't create new items in Production manually
