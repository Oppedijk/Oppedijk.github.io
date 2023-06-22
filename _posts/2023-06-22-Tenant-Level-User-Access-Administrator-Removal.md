---
layout: post
title: "Removing tenant level user access administrator"
excerpt_separator: <!--more-->
---
An Azure AD administrator can give itself permission on all Azure Subscriptions.
This can be seen on the IAM page of a subscription, where a *User Access Administrator* with scope *"/"* is found.

A person can only clear this in the UI for himself, not for another user.
By using the Azure CLI, we can remove this permission<!--more-->

### Manual
You can remove this by logging in as the affected user, going to the Azure Portal, Azure Active Directory.

Then select the Properties screen in the left menu. https://portal.azure.com/#view/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/~/Properties

The bottom Yes/No is for Access Management for Azure Resources, this will control the user access administrator.

### Automated

Through Powershell or CLI you can also remove this as a Global Admin, with the User Access Administrator role active(root level Azure Subscription access is required):

`az role assignment delete --assignee "user@example.com" --role "User Access Administrator" --scope "/"`
Or AZ CLI:

`az role assignment delete --assignee user@example.com --role "User Access Administrator" --scope "/"`

### Background info

The User Access Administrator is a temporarily solution to gain access to the Azure Subscriptions which are tied to the same Azure Active Directory.

The Global Administrator is the only one allowed to do so, and after gaining access, new/direct permissions can be applied to the Azure Subscription, after which the User Access Administrator role needs to be disabled again. (principle of least privilege)

Access to Subscriptions tied to a different Azure AD can not be resolved this way.