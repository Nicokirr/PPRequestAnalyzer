In this article : 
- [Deploy the PP solution](#deploy-the-pp-solution)
  - [Import Solution](#import-solution)
- [Configure Environment Variables](#configure-environment-variables)
  - [SharePoint directories](#sharepoint-directories)
  - [Dataverse connection](#dataverse-connection)
  - [Data collection](#data-collection)
- [Checking import results](#checking-import-results)
  - [Status \& connections](#status--connections)
  - [Runs \& downloads](#runs--downloads)
- [Solution Import Completed](#solution-import-completed)

# Deploy the PP solution

## Import Solution

[Download the latest version](/Packages/RequestAnalyzer_2_0_0_2_managed.zip) of the solution, and import it in the chosen host environment.

![Install-ImportSolution](/Docs/Images/Install-ImportSolution.png)

You will need to define the connections for each data source. This should be easy for Office 365 Users & Sharepoint. (you may need to refresh after creating a connection to actually see it)

![Install-Connections1](/Docs/Images/Install-Connections-1.png)

"HTTP with Entra ID" connnections require to configure URLs. 
Create the first connection for the licensing API using this URL "https://licensing.powerplatform.microsoft.com" in both fields

> It is recommended to use an account that has Power Platform Admin rights to call the licensing API. This API provides data only for environments where the account used is sytem admin. With this privilege, you are sure to get all tenant data.

![Install-Connections2](/Docs/Images/Install-Connections-2.png)

![Install-Connections3](/Docs/Images/warning.png)

Be carefull , this step is where most frequent configuration error happen.

You **MUST NOT REUSE** the http licensing connection for the *Graph API* graph API connection. 

Instead, create a new connection with this URL: "https://graph.microsoft.com" in both fields.

![Install-Connections3](/Docs/Images/Install-Connections-3.png)

# Configure Environment Variables

After definining the connections, you need to configure the environment variables. There is a definition and an example provided for each variable in the import screen. If you need more details, refer to the paragraphs below.

![Install-EnvironmentVariables](/Docs/Images/Install-EnvironmentVariables.png)

## SharePoint directories

There are multiple reports to download. This tool is designed to have one folder for each report type. 
These directories where created in the prepare phase. If you missed it, go back to [Prepare Sharepoint Directories](#prepare-sharepoint-directories)

There is one variable to define the SharePoint root URL, and then relative URLs for each folder. 
![Install-Connections3](/Docs/Images/warning.png)

This step is the 2nd  most frequent place for configuration error. 
Be **BE CAREFULL** with the "/" that should obe included at the begining of **ALL** relative URLs. Look at examples to see what is expected : 
- *RA-SP_site URL* : root URL of the SharePoint Site  (ex : https://xxxxxx.sharepoint.com/sites/RequestAnalyzer)
- *RA-SP_Folder-LicensedUser* : relative URL of the directory to host the reports for "licensed users"
- *RA-SP_Folder-NonLicensedUser* : relative URL of the directory to host the reports for "non-licensed users"
- *RA-SP_Folder-PerFlow* : relative URL of the directory to host the reports for "Per Process" flows 
- *RA-SP_Folder-AIBuilder* : relative URL of the directory to host the reports for AI Builder
- *RA-SP_Folder-PowerPagesAnonymous* : relative URL of the directory to host the reports for PowerPages anonymous usage
- *RA-SP_Folder-PowerPagesAuthenticated* : relative URL of the directory to host the reports for PowerPages authenticated usage

## Dataverse connection

This tool uses CoE Starter Kit data to provide details about a specific user. As it can be installed in any environment, you need to select the environment hosting the CoE Starter Kit in the variable *RA-DataverseWithCoE*.

> **IF** you made the choice to not use the CoE Starter Kit database, you can point to any other Dataverse where users have read access to the table *Microsoft Entra IDs*. This will allow to display user names even if the usage data will be missing

## Data collection
All these parameters have default values that should fit most tenants. 
You may want to see how to adjust them if :
- you have a very large tenant (>100k active users) as the size brings challenges to collect efficiently the data.
- you want to optimize data collection for your tenant size. (can reduce run time and get latest data earlier)
- you are curious and want to understand how this works

In this case, go to [Configure Data Collection Parameters](Install-DataLoadConf.md)

# Checking import results

Once the import is complete, it is strongly advised to cperform few checks to make sure the data collection is well configured. This automation is based on 2 cloud flows : 
- Data Load - Main
- Data Load - Child - Create and download a single report

## Status & connections

Check that :
- Each cloud flow is *ON*

  ![Check flow ON](/Docs/Images/Install-Check-ON.png)

- The URL defined in the connection of the cloud flow *Data Load - Child - Create and download a single report* is the licensing one. To do this, open the cloud flow page and simply click on *See all* in the connection References tile. 
  
    >Tip : If the link is not visible, try to switch to Power Automate.

    ![Check flow configuration](/Docs/Images/Install-Check.png)

## Runs & downloads

If the configuration is correct, then the automation should be in progress right now. Check that : 
- the main flow is running  (if not, run it manually now)

    ![Check Main Run](/Docs/Images/Install-Check-MainRun.png)

- several instances of the child flow are running too. 
    
    ![Check Child Run](/Docs/Images/Install-Check-ChildRun.png)

- no previous execution failed for the 2 flows. (If some failed, refere to the [Troubleshooting](Install-4_Troubleshooting.md) page).

# Solution Import Completed

Congratulation, you have installed and configured the engine to automate data collection. The full data load is now in progress. This can take few hours to complete based your tenant size and on the configuration selected. 

While automation does the work, you can move to the next step : [Installing  PowerBI](Install-3_PBI.md)