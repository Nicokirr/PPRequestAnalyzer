In this article : 
- [Power BI](#power-bi)
  - [Upload the report on PowerBI](#upload-the-report-on-powerbi)
  - [Configure the Semantic model](#configure-the-semantic-model)
    - [Configure parameters](#configure-parameters)
    - [Configure credentials](#configure-credentials)
    - [Activate Auto Refresh](#activate-auto-refresh)
  - [Check report content](#check-report-content)
- [Power App](#power-app)
  - [Link the report to the App](#link-the-report-to-the-app)
  - [Approve App connections](#approve-app-connections)
- [Check data refresh](#check-data-refresh)

# Power BI 

## Upload the report on PowerBI
- Download the [Request Analyzer report](/Packages/RequestAnalyzer-V2.1.1.pbix)

- Go to [Power BI](https://app.powerbi.com), and navigate to the workspace you have chosen in the prepare steps.
- Upload the report

## Configure the Semantic model

Open the settings of the semantic model

![Semantic Settings](/Docs/Images/Install-PBI-SemanticSettings.png)

### Configure parameters

Go to the parameter.

> **WARNING** : Don't define credentials yet, parameters need to be properly set before.

Fill all SharePoint parameters with the same values as the ones defined in [Solution Install : Configure ](/Docs/Install-2_Solution.md#sharepoint-directories).

>**WARNING** : Some values are already defined, but you need to **replace** them with your values

![Semantic Settings](/Docs/Images/Install-PBI-SemanticParameters.png)

The last parameter is the Dataverse URL. Use the one of the CoE Starter Kit. If you don't know it, you can find it by selecting the environment in the admin portal. 

![Dataverse URL](/Docs/Images/Install-DataverseURL.png)

![Install-Connections3](/Docs/Images/warning.png)

> **WARNING** :  DO NOT FORGET to click on APPLY, otherwise you will loose this configuration and the next step to setup credentials will fail.

### Configure credentials

Go to Data source credentials and define them for the 4 data sources : 

![Credentials Dataverse](/Docs/Images/Install-PBI-Credentials-DV.png)

| Data Source | Description | Authentication method | Privacy level (recommendation) |
| --- | --- | --- | --- |
| CommonDataService |  Dataverse used by the CoE Starter Kit | OAuth2 | Private |
| SharePoint | The website hosting the downloaded CSV reports | OAuth2 | Private |
| Web | This first web source is Microsoft Official Github documentation. | Anonymous | Public | 
| Web | This is again the CoE Starter Kit database. As the standard connector does not provide access to the system users, a dedicated datasource is required. | OAuth2 | Private |

Once all credentials are provided, there should be no remaining error nor warning messages in this section.

![Credentials Dataverse](/Docs/Images/Install-PBI-Credentials-Valid.png)

### Activate Auto Refresh

Configure daily auto-refresh to make sure the data stays up to date. Choose a time few hours after the end of the data collection process (ex : 08:00 AM)

![Refresh Data](/Docs/Images/Install-PBI-Refresh.png)

> DO NOT FORGET to click on APPLY, otherwise you will loose this configuration

## Check report content

Navigate back to the workspace (1) and check that the data refresh is completed (2)

![Refresh Data](/Docs/Images/Install-PBI-RefreshOK.png)

> It is likely that the full data load is not completed yet, so you will have partial data only, but this does not prevent from finalizing the configuration.

Now open the report and check that data displayed matches your tenant (Check that dates and quantities make sense in your context) 
You can refer to the below picture that displays default data included in the report(Yours should be different). Otherwise, refere to the [Troubleshooting](/Docs/Install-4_Troubleshouting.md) page.

# Power App

## Link the report to the App

Switch to *edit* mode and navigate to the User details page.

![Refresh Data](/Docs/Images/Install-PBI-defaultData.png)

Then apply the following steps to link the report to Power App on your tenant: 
1. Click on the grey banner (this will select the Power App visual)
2. Select the *Format Visual* tab
3. Reset the visual to default

![Edit App](/Docs/Images/Install-PBI-EditApp.png)

4. Close the banner
   
![Close Banner](/Docs/Images/Install-PBI-CloseBanner.png)

5. Select in the dropdown list the environemnt where you deployed the solution in the previous step [Install the Power Platform Solution](Install-2_Solution.md)
6. Click on *Choose app*
   
![Select Environment](/Docs/Images/Install-PBI-SelectEnv.png)

7. Select the *Request Analyzer App*
8. Click on *Add*

![Select App](/Docs/Images/Install-PBI-SelectApp.png)

9. Skip the proposal to open the Power App studio
10. **SAVE** the changes on the report

![Save](/Docs/Images/Install-PBI-Save.png)

## Approve App connections

The last step is to approve connections used by the app : 
- The first one is direct to O365 to display the user's picture
- The second one is through PowerAutomate, this provides the list of licenses assigned to the user.

![App Connections](/Docs/Images/Install-PBI-AppConnections.png)

> If the *Allow* button is not available, it's probably because the credentials needs to be provided for one or both connections. Scroll to the right of the App permission screen 

![Define Connections](/Docs/Images/Install-PBI-DefineConnections.png)

Once approved, you are done with the installation. The detail tab should look like this.

![PBI install end](/Docs/Images/Install-PBI-End1.png)

# Check data refresh

The report is loaded with Sample data, you may need to manually refresh it to see yours. 
Identifying sample data is easy : 
- there is a single licensed user
- his GUID starts with "aaaaaaa"

Here is the screen with Sample Data

![PBI Sample Data](/Docs/Images/Install-PBI-Sample.png)




