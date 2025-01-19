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

### How to detect this in the log files

There are 2 locations where this can be detected, on the Entra ID side, where in the Audit logs a record is made with of the Service `Azure RBAC (Elevated Access)` with the Category `AzureRBACRoleManagementElevateAccess`. The Activity Type is `User has elevated their access to User Access Administrator for their Azure Resources`

The other location is on the Azure Side, look in the audit logs, but select `Directory Logs` (next to edit columns) instead of the default selected `Activity Logs`. An entry from the `Microsoft.Authorization` resource provider is there, with an operation name of `Assigns the caller to User Access Administrator role`

(For both scenario's you need read permissions)

### A better solution

The better solution is to use Privileged Identity Management(PIM), assign a group permission as User Access Administrator on a management group, and let people (maybe after an approval) gain membership to this group for a limited time.

PIM will prevent permanent access to this role.

### Background info

The User Access Administrator is a temporarily solution to gain access to the Azure Subscriptions which are tied to the same Azure Active Directory.

The Global Administrator is the only one allowed to do so, and after gaining access, new/direct permissions can be applied to the Azure Subscription, after which the User Access Administrator role needs to be disabled again. (principle of least privilege)

Access to Subscriptions tied to a different Azure AD can not be resolved this way.