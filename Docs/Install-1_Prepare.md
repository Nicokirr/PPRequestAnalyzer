In this article : 
- [Pre-requisites](#pre-requisites)
  - [Platform](#platform)
  - [Licenses](#licenses)
  - [Security and privileges](#security-and-privileges)
- [Hosting Choices](#hosting-choices)
  - [Power Platform environment](#power-platform-environment)
    - [Connectors](#connectors)
    - [About installing inside the CoE Starter Kit environment](#about-installing-inside-the-coe-starter-kit-environment)
  - [Power BI Workspace](#power-bi-workspace)
- [Let's Start](#lets-start)

# Pre-requisites

## Platform

This tool uses : 
- Power Platform Admin Portal (PPAC) API - **NOT SUPPORTED** - to request and download API usage raw data
- A SharePoint library to host the downloaded files
- Multiple Cloud flows to orchestrate the gathering of the data
- A PowerBI workspace to host the dataset and the report
- A PowerApp to display individual user details and better understand his context (licenses & usage)
- External data sources 
  - For data about Users
      - Office 365 to get all licenses assigned to the user (this gives GUIDs only, the tool includes hardcoded mapping to provide meaningfull license names)
      - The COE Starter Kit database to get user's usage details (optional)
  - For data about App registrations
      - Dataverse to translate app registrations GUIDs into meaningfull names 
      - Official documentation page on GitHub to provide more details about the purpose of each app registration managed by Microsoft.

## Licenses

- A Power App Premium per user license is required to :
  - Install and run the solution
  - Use the Power App embedded into the report
- Power BI license is required to 
  - publish the report  (can also be used on PBI desktop directly without licenses, but without sharing & auto refresh)
  - Use the report from the Power BI Online service
- SharePoint license to access the file library

## Security and privileges

- The connection used to call the PPAC API need to be "System admin" of all environments. The easiest way to assure this is to assign the  "Power Platform admin" role to the user. Otherwise, data will be incomplete (the download provides data only for the envs where the user has admin rights)
- The user viewing the report needs to have read access on the following CoE Starter Kit tables : 
  - Microsoft Entra IDs
  - Makers
  - Audit Logs
  - PowerApps Apps
- The user installing PowerBI needs to have permissions to create datasets and reports in the given Workspace

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

# Let's Start

You've prepared all requiered content ? So you are ready to proceed with the installation : [Installing  Power Platform Solution](Install-2_Solution.md)