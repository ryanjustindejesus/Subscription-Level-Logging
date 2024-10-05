<h1>Subscription Level Logging</h1>
<b>This tutorial outlines the configuration </b>

<h2>Environments and Technologies Used</h2>

- <b>Microsoft Azure</b> 
- <b>Description</b>

<h2>Operating Systems</h2>

- <b>Windows 10</b>

<h2>Configuration Steps</h2>

``` 
SecurityEvent
| where EventID == 4625
| where TimeGenerated > ago(60m)
| summarize FailureCount = count() by SourceIP = IpAddress, EventID, Activity
| where FailureCount >= 10
```
