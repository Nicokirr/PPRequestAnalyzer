In this article : 
- [Pre-requisites](#pre-requisites)
  - [Mandatory](#mandatory)
    - [Platform](#platform)
    - [Licenses](#licenses)
    - [Security and privileges](#security-and-privileges)
  - [Optional Power App](#optional-power-app)
    - [Platform](#platform-1)
    - [Licenses](#licenses-1)
    - [Security and privileges](#security-and-privileges-1)
- [Hosting Choices](#hosting-choices)
  - [Power Platform environment](#power-platform-environment)
    - [Connectors](#connectors)
    - [About installing inside the CoE Starter Kit environment](#about-installing-inside-the-coe-starter-kit-environment)
  - [Power BI Workspace](#power-bi-workspace)
- [Prepare Sharepoint Directories](#prepare-sharepoint-directories)
- [Let's Start](#lets-start)

# Pre-requisites

## Mandatory

### Platform 
- Power Platform Admin Portal (PPAC) API - **NOT SUPPORTED** - to request and download API usage raw data
- A SharePoint library to host the downloaded files
- Multiple Cloud flows to orchestrate the gathering of the data
- A PowerBI workspace to host the dataset and the report

### Licenses

- A **Power App Premium** license is required to Install and run the solution. A Power Automate Premium can also work IF you plan to NOT use the app embedded into the report.
- **Power BI** license is required to 
  - publish the report  (can also be used on PBI desktop directly without licenses, but without sharing & auto refresh)
  - Use the report from the Power BI Online service
- **SharePoint** user license to access the file library

### Security and privileges

- The connection used to call the PPAC API need to be "System admin" of all environments. The easiest way to assure this is to assign the  "Power Platform admin" role to the user. Otherwise, data will be incomplete (the download provides data only for the envs where the user has admin rights)
- The user installing PowerBI needs to have permissions to create datasets and reports in the given Workspace

## Optional Power App

The report includes an app (Areas 2,3 and 4 in bellow screen) to display more details about a specific user. Using the app is not mandatory and the report will work even without configuring the app. However, investigation to understand the context of an overconsumption will be a lot easier using this app. 

You can use it with or without the CoE Starter Kit. No CoE Starter kit means that the bottom green will be empty.

![Troubleshoot Main](/Docs/Images/Troubleshooting-Main.png)

### Platform 
A PowerApp to display individual user details and better understand his context (licenses & usage)
- External data sources 
  - For data about Users  ("Licensed User" reports)
      - Office 365 to get all licenses assigned to the user (this gives GUIDs only, the tool includes hardcoded mapping to provide meaningfull license names)
      - The COE Starter Kit database to get user's usage details (optional)
  - For data about Service Principles ("Non-Licensed User" reports)
      - Dataverse to translate GUIDs into meaningfull names 
      - Official documentation page on GitHub to provide more details about the purpose of each app registration managed by Microsoft.

### Licenses

A Power App Premium per user license is required to use the Power App embedded into the report  (optional)

### Security and privileges

 The user viewing the report needs to have read access on the following CoE Starter Kit tables : 
  - Microsoft Entra IDs
  - Makers
  - Audit Logs
  - PowerApps Apps

# Hosting Choices

## Power Platform environment

### Connectors

Make sure these connectors are authorized by the DLPs applied to the chosen environment

- HTTP with EntraID (preauthorized)
- Office 365 User
- SharePoint
- Dataverse

### About installing inside the CoE Starter Kit environment

Some items of this solution rely on Dataverse data hosted by the CoE Starter Kit. You can choose to host this solution in the same environment. This way you will have all tools for admins in the same place. However, this is not mandatory, and you can choose to deploy this solution in any environment of your choice.

>Note : the recommendation is to choose an environment with English as base language as this is the most tested configuration.

## Power BI Workspace

You need a workspace to host the dataset and the report. As for the environment, you can choose to reuse the one create for the CoE Starter kit report, or create a new one.

# Prepare Sharepoint Directories

This tool is designed to store each reports in dedicated directories. Identify a Sharepoint where you will store these reports, and create a directly for each report : 
- Licensed User
- Non Licensed User
- AIBuilder
- PerProcess (or PerFlow if you prefer the old name)
- Power Pages Anonymous
- Power Pages Authenticated

Here is an example

![Sharepoint Directories](/Docs/Images/Install-SharepointDirectories.png)

# Let's Start

You've prepared all requiered content ? So you are ready to proceed with the installation : [Installing  Power Platform Solution](Install-2_Solution.md)