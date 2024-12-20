# Power Platform Request Analyzer
The purpose of this tool is to provide : 
- detailed views on the consumption of APIs request and **assess the cost impacts**  on a given Power Platform tenant. 

The package includes : 
- A PowerBI report to analyse all data
- A Power Platform solution with 4 Power Automate cloud flows to automate the collection of raw data from the admin portal

# Disclaimer 
This is **NOT** a Microsoft supported tool, but a side project created to help the community better understand API consumption in a given tenant. This tool is supported on "best effort" only, with no SLAs

# Documentation

## Install Guide

Detailed install guide is available here : 

| Id | Step | Estimated Work | Comments |
| --- | --- | --- | --- |
| 1 | [Prepare](Install-1_Prepare.md) | 30 mins | Low effort but this can take time depending on your org complexity to collect all pre-requisites (privileges, licenses)
| 2 | [Install the Power Platform Solution](Install-2_Solution.md) | 30 mins | Quite strait forward, few connections and environment variables to configure
| 3 | [Deploy & Configure PowerBI Report ](Install-3_PBI.md) | 30 mins | Upload the report, configure parameters and connections, link the report to the Power App. 

## User Guide 

Coming Soon. If this is important for you, please contact us.

# Architecture

![Request_Analyzer_Archtecture](/Docs/Images/Request-Analyzer-Architecture.png)




