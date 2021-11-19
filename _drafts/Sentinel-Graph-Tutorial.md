---
layout: post
title: Creating Graphs Tutorial for Microsoft Sentinel
excerpt_separator: <!--more-->
---
In this Tutorial we will have a look at creating a simple Graph and visualize it in Azure Sentinel
https://techcommunity.microsoft.com/t5/microsoft-sentinel-blog/graph-visualization-of-external-teams-collaborations-in-azure/ba-p/1356847

<!--more-->
### example data
```
let Nodes = datatable(Id: long, NodeName: string) [
    1, 'A',
    2, 'B1',
    3, 'B2',
    4, 'C1',
    5, 'C2',
    6, 'C3',
];
let Links = datatable(Source: long, Destination: long) [
    1, 2,
    1, 3,
    3, 4,
    3, 5,
    3, 6,
];
//Nodes
Links
```