In this article : 
- [Troubleshooting](#troubleshooting)
  - [Understand Issue's scope](#understand-issues-scope)
  - [1-Main Power BI](#1-main-power-bi)
    - [No Data displayed](#no-data-displayed)
    - [Data looks incomplete](#data-looks-incomplete)
  - [2-User's picture](#2-users-picture)
  - [3-User's details and usage](#3-users-details-and-usage)
    - [User details](#user-details)
    - [Maker activity](#maker-activity)
    - [User activity](#user-activity)
  - [4-User's licenses](#4-users-licenses)
- [Solving Most common issues](#solving-most-common-issues)
  - [Misconfiguration of Connections](#misconfiguration-of-connections)

# Troubleshooting

## Understand Issue's scope

The first thing to do is to understand where the data is coming from to create the report : 

![Troubleshoot Main](/Docs/Images/Troubleshooting-Main.png)

1. Requestiong CSV files stored in **Sharepoint**. These files are downloaded by Power Automate **Cloud flows**.
2. Power App displaying user's picture from O365 connector
3. Power App displaying user's details from Dataverse tables  **Microsoft Entra IDs**.
4. Power App displaying user's licenses from a cloud flow calling the Graph API

It's also important to identify what are the technical components involved to make this work.

![Troubleshoot Architecture](/Docs/Images/Troubleshooting-MainArchi.png)

Other pages of the report are similar to the **zone 1**.

## 1-Main Power BI

### No Data displayed
It means the report cannot find the right csv files. The easiest way to investigate this is to start from data source and check every step. Start from the SharePoint directories. 
- There are **no CSV files** : it means the flows are not running properly. Look at the executions of the cloud flow called "Data Load - Child - Create and download a single report". You should see 6 runs of this flow for each day of data collection. 
   - No runs: 
      - Make sure the 2 "Data Load" flows are **ON**
      - Check the "Data Load - Main" flow runs and solve any errors. (he is the one orchestrating the calls to the child). 
   - Runs with errors : 
     - Look at the details. The most common issue here is that there mismatch between the URL configured (probably graph API), and the URL called (licensing API).
    ![Troubleshoot Connection](/Docs/Images/Troubleshooting-FlowConnection.png) To solve this, go to [Misconfiguration of Connections](#misconfiguration-of-connections)
- There are **several CSV files** : It's likely that the PowerBI is not looking into the right directory. Check the configuration of the PowerBI parameters and make sure the URLs and directories are defined properly.

### Data looks incomplete

This is likely to be a privilege issue. The reports downloaded from the admin portal contains data only for the environments where the account used to connect has the privilege "System Administrator". Check : 
- the account used to connecto to the licensing API inside the cloud flow "Data Load - Child - Create and download a single report. 
- Privilèges assigned to this account (only **Power Platform admin** will access all tenant data)

## 2-User's picture

Having no picture here is usually because the user did not define any in his O365 profile.

## 3-User's details and usage

Issues on this part usually come from lack of privileges, or lack of data in the CoE. Privileges required here should be assigned to the user viewing the report. This also allows for some segregation if you need to avoid personal data being seen by some users reading the report : just make sure they have no read privileges on the corresponding dataverse tables.

### User details

This comes from the table *Microsoft Entra IDs*. This is a virtual table pointing directly to Entra ID data. 
If you don't see data here, check your user's privileges on this table.

### Maker activity

This comes from the CoE Starter Kit table *Maker*. This is a CoE Starter Kit table.
If you don't see data here : 
- check the data exists in the CoE Starter Kit environment
- check your user's privileges on this table

### User activity

This comes from the CoE Starter Kit tables *PowerApp Apps* and *Audit Logs*.
If you don't see data here : 
- check the data exists in the CoE Starter Kit environment
- check your user's privileges on this table

## 4-User's licenses

This data comes from the graph API through Power Automate Cloud flow. The most common issue here is seeing licenses named "Unreferenced Licenses". This is because the API does not provide names but GUIDs, and sometimes these GUIDs cannot be mapped to the official list of licenses. The only way of solving this is to update the mapping embedded into the app. Please open a github ticket if you want this to be investigated.

# Solving Most common issues

## Misconfiguration of Connections

1. From Power Apps portal, open the **default** solution.
2. Look for Connection references
3. Find the right connection (the ones used in this tool all start with **RA-**)
4. Edit the reference and point it to the right connection (order stays the same, so keep in mind which one you assigned to one connection reference

![Reconfigure Connection Reference](/Docs/Images/Troubleshooting-ReconfigureConnections.png)
