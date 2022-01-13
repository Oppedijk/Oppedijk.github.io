---
layout: post
title: 'Microsoft Defender For Cloud' Recommendations Explained
excerpt_separator: <!--more-->
---
Reading and interpreting the 'Microsoft Defender for Cloud' Recommendations in the https://portal.azure.com can be a bit of an art.

<!--more-->
### Microsoft Defender For Cloud(Formerly known as Azure Security Center)
This is the service focussing on securing your Development/Subscriptions.
Please note that not all Microsoft Services are counted and reported in here, like Office Security, Endpoints Security, AAD Security etc

### Focus on the important stuff
The first steps during a security investigation is filtering on several fields to reduce the amount of data.
Go to the "Recommendation" blade in the Microsoft Defender for Cloud service.
This will show some aggregated scores, and below is a list with the recommendations.

Filter the list with recommendations on:
- Recommendation Status: "Active" (remove the checkmark with "Completed" items)
- Severity: "High" (remove the "Medium" and "Low" for now)

Once you've worked your way through all "High" items, proceed with clearing up all "Medium" issues.

### Recommendations

