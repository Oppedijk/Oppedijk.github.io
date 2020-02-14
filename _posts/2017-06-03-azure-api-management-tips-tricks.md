---
layout: post
title: "Azure API management tips & tricks"
excerpt_separator: <!--more-->
---
Just some random notes on Azure API management:

Parse JWT in Azure API management policy:

@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)

<!--more-->

API management and mobile services

http://giventocode.com/azure-api-management-and-azure-mobile-services#.VmFYAHldGpo

?$filter={filter}&$inlinecount={inlinecount}&$orderby={orderby}&$select={select}&$skip={skip}&$top={top}