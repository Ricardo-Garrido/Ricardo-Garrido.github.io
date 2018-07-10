Optimize Indexes

```sql
EXECUTE dbo.IndexOptimize @Databases = 'USER_DATABASES',
@FragmentationLow = NULL,
@FragmentationMedium = 'INDEX_REORGANIZE,INDEX_REBUILD_ONLINE,INDEX_REBUILD_OFFLINE',
@FragmentationHigh = 'INDEX_REBUILD_ONLINE,INDEX_REBUILD_OFFLINE',
@FragmentationLevel1 = 5,
@FragmentationLevel2 = 30,
@SortInTempdb = 'Y',
@MaxDOP = 0
```

Check if archive is on legal hold

```sql
USE EVVSACAVS_1
SELECT Distinct 
    count(idtransaction) AS "Number of Items"
    ,VSAV.ArchivePointId
    ,VSAV.RetentionCategoryIdentity
    ,CASE WHEN VH.SavesetIdentity is null THEN 'Not on Legal Hold' ELSE 'On hold' END AS OnHold_DA

FROM view_saveset_archive_vault VSAV
    LEFT OUTER JOIN view_Holds VH on VH.SavesetIdentity = VSAV.SavesetIdentity
WHERE VSAV.ArchivePointID = '<ARCHIVEID>'
GROUP BY VSAV.ArchivePointId, VSAV.RetentionCategoryIdentity, VH.SavesetIdentity
```

Get number of items in an archive

```sql
USE EVVSACAVS_1
SELECT count(*) 
FROM view_Saveset_Archive_Vault
WHERE ArchivePointId = '<ARCHIVEID>'
```

Get archives marked for deletion

```sql
USE EnterpriseVaultDirectory
SELECT ArchiveName, VaultEntryID, ArchiveStatus
FROM ArchiveView
WHERE ArchiveStatus = 4
```

Get index location

```sql
USE EnterpriseVaultDirectory
SELECT CE.ComputerName, CE.ComputerNameAlternate, IRP.IndexRootPathEntryID, IRP.IndexRootPath, 
ISE.ServiceEntryID, ISE.Description
FROM ComputerEntry CE, IndexingServiceEntry ISE, IndexRootPathEntry IRP
WHERE CE.ComputerEntryID = ISE.ComputerEntryID AND ISE.ServiceEntryID = IRP.IndexServiceEntryID
ORDER BY CE.ComputerName
```

