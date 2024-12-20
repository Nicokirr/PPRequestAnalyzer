In this article : 
- [Configuration of Daily data collection](#configuration-of-daily-data-collection)
- [Configuration of Full data collection](#configuration-of-full-data-collection)

## Configuration of Daily data collection

Collecting telemetry takes time. You can only request for usage data of a period in the past (no live collection). And the more active users on the tenant, the more you need to wait before requesting the report. This tool is designed to request, every morning, usage data for a single day in the past. 

| Variable Name | Unit | Default Value | Recommended Range | Description |
| --- | --- | --- | --- | ------------ |
| RA-DelayForTelemetryHarvesting | Days | 4 | 0 to 10 | This is the delay in days between the runtime moment and the data to download |

Here is how it behaves with default value, assuming we are the 6th of December.

![RA-DelayForTelemetryHarvesting-equals-4](/Docs/Images/DelayForTelemetryHarvesting-4.png)

If you have a small tenant, you can reduce this value up to 0 to have more recent data.

![RA-DelayForTelemetryHarvesting-equals-0](/Docs/Images/DelayForTelemetryHarvesting-0.png)

## Configuration of Full data collection

Nobody likes to wait. So the tool is designed to request as much data as possible at the time of the installation (only once). This is quite intensive in terms of request, and again the time needed by PPAC to respond depends on the activity on the tenant. So there are few parameters to adjust these waiting times

| Variable Name | Unit | Default Value | Recommended Range | Description |
| --- | --- | --- | --- | ------------ |
| RA-CollectAllDaysAtNextRun | Boolean | False | n/a | If true, forces a full data load, on all possible days. This full load will then set this parameter to false to ensure it happens only once. | 
| RA-NumberOfDaysToCollect | Days | 40 | 1 to 44 | This is the number of days to collect when running a full data load. |
RA-DelayBetweenRequests | Seconds | 240 | 30 to 1000+ |  Delay to wait after sending all report requests for a specific day, before sending requests for the next day. Typically less than a minute for small tenants, but can require several minutes if not more for large tenants. |

![Full data collection sequence](/Docs/Images/UnderstandingFullLoadSequence.png)

After defining the environment variable values, you can import the solution (~ 2 minutes to complete).

[Back to Solution install](/Docs/Install-2_Solution.md#checking-import-results)
