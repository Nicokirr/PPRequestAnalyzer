In this article : 
- [Overview](#overview)
    - [Introduction](#introduction)
    - [Understanding Requests and limits](#understanding-requests-and-limits)
- [Report Content](#report-content)
  - [Global View](#global-view)
  - [Users](#users)
    - [Overview](#overview-1)
    - [Details](#details)
    - [Licenses](#licenses)
    - [By Envs](#by-envs)
  - [Integrations](#integrations)
    - [Overview](#overview-2)
    - [Details](#details-1)
  - [Others](#others)
    - [Processes](#processes)
    - [AI Builder](#ai-builder)
    - [Power Pages](#power-pages)


#  Overview

### Introduction

All usage in Power Platform generates requests. Understanding the details of these requests can help you in different ways : 
- Stay compliant with consumption limits by highlighting who/what overconsumes, how often, and through which resources
- Improve license assignment process by identifying usages which require additional licenses
- Manage internal billing more efficiently

### Understanding Requests and limits

Consumption in the the tenant can either be directly assigned to : 
- a specific user : Each user's limit is different and based on licenses assigned to this user
- a service principal : All requests from service principals are pooled at the tenant level. The tenant limit depends on the number of licenses purchased in the tenant (Power Platform & Dynamics 365)

# Report Content

The report includes 10 pages grouped into 4 sections : 
- Global View
- Users
- Integrations
- Others

It also includes a date slicer that applies to all pages of the report and allows to zoom on a specific period when needed.

## Global View

In roadmap. This will provide a aggregated view of the tenant.

## Users

This provides detailed analysis about all active users in the tenant

### Overview

To be completed

### Details

To be completed

### Licenses

When users overconsume, a recuring question is about the cost impact of staying compliant. This page allows you to answer this question.

The first approach to this is to count how many requests are above the limits for each user, and purchase as many *"Power Platform Request"* addon SKU as needed to cover for this number. This is the number calculated in the top left (1st Analysis).

However, this is the recommended approach ase : 
- this is not the most cost efficient way
- this is a missed opportunity to give more functionalities to users for the same price. 

A better way is to identify for each user if it would make sense to assign additional  licenses, and only consider the remaining overage after these assignements. This is what the top rioght corner is about (Optimized Licenses). 

![Licenses](/Docs/Images/UG-Licenses.png)

For each individual user, it looks if it makes sense to assign more licenses, or to switch to a Process license. The rules implemented are as follows, for each individual user :  
- Look if an additional premium license make sense 
  -  If he has overage and no licenses
     -  If he uses both Power Apps and Power Automate, recommend to assign 2 premiums.
     -  If he uses only one product, recommend only 1 premium
  -  If he has overage and one premium license, recommend an additional premium license only if he uses both Power Automate and Power Apps
-  Then look if there is enough usage of Power Automate to justify one of more process licenses
-  At the end, look if there would still be overconsumption after these assignements, and calculate how many addons would be required only for this remaining overconsumption.
  
The table in the bottom part if here to help understand abode rules, and check that they apply in the right way. 

Finaly, there are some parameters to play with : 
- **Pricing parameters**: simply put your own SKU prices here to make sure the calculation are based on your current license price.
  - P.Apps Premium : your price for the SKU Power Apps Premium per User
  - Request Addon : your price for the SKU Power Platform Requests
  - Process : your price for the SKU Power Automate per Process
- **Limits extensions** : you may want to tweak the data in some cases to make more accurate assumption about required licenses:
  - Ignore top outliers : Some overconsumption detected by the report are abnormal spikes and you'll work with users to avoid them in the future. In this case, you may want to remove these pikes from the report to make more accurate estimates. This parameter allows to remove the top X consumption day(s) of each user, to focus on more recurring consumption. 
    - Default value : 0 (all days are considered). 
    - Example : A value of 2 will ignore the top 2 days of the highest consumption for each user.
  - Entitlement margin : Some users have reasonable and non recurring overconsumption that you may want to take the responsability to ignore. Using this parameter, you can artificialy increase each user's entitlement by few % 
    - Default value : 0 (Apply strictly the entitlement)
    - Example : A value of 1 will increase the entitlement of a user with a premium license from 40000 to 40400.

### By Envs

To be completed

## Integrations

### Overview

To be completed

### Details

To be completed

## Others

### Processes

To be completed

### AI Builder

To be completed

### Power Pages

To be completed
