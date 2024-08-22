# Drop the existing primary key using dynamic SQL and Add the new primary key using dynamic SQL

```
DECLARE @TABLE_NAME NVARCHAR(500)='PRODUCT_STOCK'
DECLARE @PK_NAME NVARCHAR(500);
-- Step 1: Get the name of the existing primary key
SELECT @PK_NAME = name FROM sys.key_constraints WHERE type = 'PK' AND OBJECT_NAME(parent_object_id) = @TABLE_NAME;
-- Step 2: Drop the existing primary key using dynamic SQL
IF @PK_NAME IS NOT NULL
BEGIN
    EXEC('ALTER TABLE '+@TABLE_NAME+' DROP CONSTRAINT ' + @PK_NAME);
END

-- Step 3: Add the new primary key using dynamic SQL
EXEC('ALTER TABLE '+@TABLE_NAME+' ADD CONSTRAINT ' + @PK_NAME + ' PRIMARY KEY ([STORE_CODE] ASC, [SAL_BARCODE] ASC, [SAL_PRICE] ASC)');
```
