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

# Power BI 

## Upload the report on PowerBI
- Download the [Request Analyzer template](/Sources/RequestAnalyzer-V1.21.pbit)

- Go to [Power BI](https://app.powerbi.com), and navigate to the workspace you have chosen in the prepare steps.
- Upload the report

## Configure the Semantic model

Open the settings of the semantic model

![Semantic Settings](/Docs/Images/Install-PBI-SemanticSettings.png)

### Configure parameters

Go to the parameters.

Fill all SharePoint parameters with the same values as the ones defined in [Step 2](/Docs/Install-2_Solution.md).

![Semantic Settings](/Docs/Images/Install-PBI-SemanticParameters.png)

The last parameter is the Dataverse URL. Use the one of the CoE Starter Kit. If you don't know it, you can find it by selecting the environment in the admin portal. 

![Dataverse URL](/Docs/Images/Install-DataverseURL.png)

> DO NOT FORGET to click on APPLY, otherwise you will loose this configuration

### Configure credentials

Go to Data source credentials and define them for the 4 data sources : 

![Credentials Dataverse](/Docs/Images/Install-PBI-Credentials-DV.png)

- CommonDataService : Dataverse used by the CoE Starter Kit.
- SharePoint : the website hosting the downloaded CSV reports.
- Web : this first web source is Microsoft Official Github documentation. No authentication is required (Anonymous access)
- Web : This is again the CoE Starter Kit database. As the standard connector does not provide access to the system users, a dedicated datasource is required. 

Once all credentials are provided, there should be no remaining error or warning messages in this section.

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
You can refer to the below picture that displays default data included in the report(Yours should be different). Otherwise, refere to the [Troubleshooting](Install-4_Troubleshooting.md) page.

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

Once approved, you are done with the installation. The screen should look like this (of course with your own data).

![PBI install end](/Docs/Images/Install-PBI-End1.png)