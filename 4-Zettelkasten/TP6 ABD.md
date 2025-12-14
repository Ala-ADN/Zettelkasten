### Open SQL Server Profiler and use the template TSQL_Duration to monitor when T-SQL statements have been completed.
![[Pasted image 20251201135316.png]]
![[Pasted image 20251201135332.png]]
###  Run the following query in SSMS
```sql
USE AdventureWorksDW
go
select count(*) from [dbo].[DimDate]
```
### Go back to SQL Server Profiler and see what the results are
![[Pasted image 20251201135350.png]]
### Do the same thing in Extended Events. Make sure you select/check the following checkboxes: “Start the event session immediately after session creation”, and “Watch live data on screen as it is captured"
![[Pasted image 20251201135823.png]]
### Open Performance Monitor and monitor the “% Processor Time” (the default). Add to this, in the Processor category, “% Privileged Time”
![[Pasted image 20251201140009.png]]
