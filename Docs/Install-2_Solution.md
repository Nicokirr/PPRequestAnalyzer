In this article : 
- [Deploy the PP solution](#deploy-the-pp-solution)
  - [Import Solution](#import-solution)
- [Configure Environment Variables](#configure-environment-variables)
  - [SharePoint directories](#sharepoint-directories)
  - [Dataverse connection](#dataverse-connection)
  - [Configuration of Daily data collection](#configuration-of-daily-data-collection)
  - [Configuration of Full data collection](#configuration-of-full-data-collection)
- [Checking import results](#checking-import-results)
  - [Status \& connections](#status--connections)
  - [Runs \& downloads](#runs--downloads)
- [Solution Import Completed](#solution-import-completed)

# Deploy the PP solution

## Import Solution

Download the latest version of the solution, and import it in the chosen host environment.

![Install-ImportSolution](/Docs/Images/Install-ImportSolution.png)

You will need to define the connections for each data source. This should be easy for Office 365 Users & Sharepoint. (you may need to refresh after creating a connection to actually see it)

![Install-Connections1](/Docs/Images/Install-Connections-1.png)

"HTTP with Entra ID" connnections require to configure URLs. 
Create the first connection for the licensing API using this URL "https://licensing.powerplatform.microsoft.com" in both fields

> It is recommended to use an account that has Power Platform Admin rights to call the licensing API. This API provides data only for environments where the account used is sytem admin. With this privilege, you are sure to get all tenant data.

![Install-Connections2](/Docs/Images/Install-Connections-2.png)

**DO NOT REUSE** this connection for the *Graph API* 

![Install-Connections3](/Docs/Images/Install-Connections-3.png)

Instead, create a new connection with this URL: "https://graph.microsoft.com" in both fields.

# Configure Environment Variables

After definining the connections, you need to configure the environment variables. There is a definition and an example provided for each variable in the import screen. If you need more details, refer to the paragraphs below.

![Install-EnvironmentVariables](/Docs/Images/Install-EnvironmentVariables.png)

## SharePoint directories

There are multiple reports to download. This tool is designed to have one folder for each report type. There is one variable to define the SharePoint root URL, and then relative URLs for each folder : 
- *RA-SP_site URL* : root URL of the SharePoint Site  (ex : https://xxxxxx.sharepoint.com/sites/RequestAnalyzer)
- *RA-SP_Folder-LicensedUser* : relative URL of the directory to host the reports for "licensed users"
- *RA-SP_Folder-NonLicensedUser* : relative URL of the directory to host the reports for "non-licensed users"
- *RA-SP_Folder-PerFlow* : relative URL of the directory to host the reports for "Per Process" flows 
- *RA-SP_Folder-AIBuilder* : relative URL of the directory to host the reports for AI Builder
- *RA-SP_Folder-PowerPagesAnonymous* : relative URL of the directory to host the reports for PowerPages anonymous usage
- *RA-SP_Folder-PowerPagesAuthenticated* : relative URL of the directory to host the reports for PowerPages authenticated usage

## Dataverse connection

This tool uses CoE Starter Kit data to provide details about a specific user. As it can be installed in any environment, you need to select the environment hosting the CoE Starter Kit in the variable *RA-DataverseWithCoE*.

## Configuration of Daily data collection

Collecting telemetry takes time. You can only request for usage data of a period in the past (no live collection). And the more active users on the tenant, the more you need to wait before requesting the report. This tool is designed to request, every morning, usage data for a single day in the past. 

| Variable Name | Unit | Default Value | Description |
| --- | --- | --- | ------------ |
| RA-DelayForTelemetryHarvesting | Days | 4 | This is the delay in days between the runtime moment and the data to download |

Here is how it behaves with default value, assuming we are the 6th of December.

![RA-DelayForTelemetryHarvesting-equals-4](/Docs/Images/DelayForTelemetryHarvesting-4.png)

If you have a small tenant, you can reduce this value up to 0 to have more recent data.

![RA-DelayForTelemetryHarvesting-equals-0](/Docs/Images/DelayForTelemetryHarvesting-0.png)

## Configuration of Full data collection

Nobody likes to wait. So the tool is designed to request as much data as possible at the time of the installation (only once). This is quite intensive in terms of request, and again the time needed by PPAC to respond depends on the activity on the tenant. So there are few parameters to adjust these waiting times

| Variable Name | Unit | Default Value | Description |
| --- | --- | --- | ------------ |
| RA-CollectAllDaysAtNextRun | Boolean | True | If true, forces a full data load, on all possible days. This full load will then set this parameter to false to ensure it happens only once. | 
| RA-NumberOfDaysToCollect | Days | 40 | This is the number of days to collect when running a full data load. |
RA-DelayBetweenRequests | Seconds | 240 | Delay to wait after sending all report requests for a specific day, before sending requests for the next day. Typically less than a minute for small tenants, but can require several minutes if not more for large tenants. |

![Full data collection sequence](/Docs/Images/UnderstandingFullLoadSequence.png)

After defining the environment variable values, you can import the solution (~ 2 minutes to complete).

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