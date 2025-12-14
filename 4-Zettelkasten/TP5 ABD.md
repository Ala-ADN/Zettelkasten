
## Add a new filegroup called SECONDARY to the database

```sql
ALTER DATABASE [AdventureWorks2022DW]
ADD FILEGROUP SECONDARY;
```

## Move dbo.DimAccount to SECONDARY filegroup

```sql
CREATE CLUSTERED INDEX IX_DimAccount_AccountKey
ON dbo.DimAccount(AccountKey)
WITH (DROP_EXISTING = ON)
ON SECONDARY;
```
## Move dbo.DimAccount back to PRIMARY filegroup

```sql
CREATE CLUSTERED INDEX IX_DimAccount_AccountKey
ON dbo.DimAccount(AccountKey)
WITH (DROP_EXISTING = ON)
ON [PRIMARY];
```

## Partition dbo.DimDate by FiscalYear

```sql
CREATE PARTITION FUNCTION PF_FiscalYear (INT)
AS RANGE RIGHT
FOR VALUES (2010);
```
```sql
CREATE PARTITION SCHEME PS_FiscalYear
AS PARTITION PF_FiscalYear
TO ([PRIMARY], SECONDARY);
```
```sql
CREATE NONCLUSTERED INDEX IX_DimDate_DateKey_FiscalYear
ON dbo.DimDate(DateKey, FiscalYear)
ON PS_FiscalYear(FiscalYear);
```