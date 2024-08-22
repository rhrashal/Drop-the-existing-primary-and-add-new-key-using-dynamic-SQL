# Drop the existing primary key using dynamic SQL and Add the new primary key using dynamic SQL

```
DECLARE @PK_PRODUCT_STOCK NVARCHAR(500);
-- Step 1: Get the name of the existing primary key
SELECT @PK_PRODUCT_STOCK = name 
FROM sys.key_constraints 
WHERE type = 'PK' AND OBJECT_NAME(parent_object_id) = 'PRODUCT_STOCK';

-- Step 2: Drop the existing primary key using dynamic SQL
IF @PK_PRODUCT_STOCK IS NOT NULL
BEGIN
    EXEC('ALTER TABLE [dbo].[PRODUCT_STOCK] DROP CONSTRAINT ' + @PK_PRODUCT_STOCK);
END

-- Step 3: Add the new primary key using dynamic SQL
DECLARE @NewPK NVARCHAR(500) = 'PK_PRODUCT_STOCK';
EXEC('ALTER TABLE [dbo].[PRODUCT_STOCK] ADD CONSTRAINT ' + @NewPK + ' PRIMARY KEY ([STORE_CODE] ASC, [SAL_BARCODE] ASC, [SAL_PRICE] ASC)');
```
