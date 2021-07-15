---
layout: post
title:  "Data Cleaning in T-SQL"
date:   2021-07-10 09:29:20 +0700
tags: [SQL, SQL Server]
---

# Data Cleaning in SQL Server Management Studio (T-SQL)

In this project I'll clean real-world property data using SQL. I'll start by importing the data from csv files to database and then query tables.

```sql
USE [cleanEx];
GO

----------------------------------------------------------------------------------------------------------------------
/* Change [SaleDate] Column format to date format in new field. */

ALTER TABLE [NashvilleHousing]
Add [SaleDateNew] Date;

Update [NashvilleHousing]
SET [SaleDateNew] = CONVERT(Date, SaleDate);
GO


----------------------------------------------------------------------------------------------------------------------
/* Populate Property Address data. */


Update A
SET A.PropertyAddress = ISNULL(A.PropertyAddress, B.PropertyAddress)
FROM [NashvilleHousing] as A
JOIN [NashvilleHousing] as B
	on A.ParcelID = B.ParcelID
	AND A.UniqueID <> B.UniqueID
WHERE A.PropertyAddress is null;
GO


----------------------------------------------------------------------------------------------------------------------
/* Breaking out Address into Individual columns (Address, City, State) */


Alter TABLE [dbo].[NashvilleHousing] Add PropertySplitAddress Nvarchar(255);
Alter TABLE [dbo].[NashvilleHousing] Add PropertySplitCity Nvarchar(255);
GO

UPDATE [NashvilleHousing] SET PropertySplitAddress = SUBSTRING(PropertyAddress, 1,CHARINDEX(',',PropertyAddress)-1);
UPDATE [NashvilleHousing] SET PropertySplitCity = SUBSTRING(PropertyAddress, CHARINDEX(',',PropertyAddress)+1 , LEN(PropertyAddress));
GO


Alter TABLE [dbo].[NashvilleHousing] Add OwnerSplitAddress Nvarchar(255);
Alter TABLE [dbo].[NashvilleHousing] Add OwnerSplitCity Nvarchar(255);
Alter TABLE [dbo].[NashvilleHousing] Add OwnerSplitState Nvarchar(255);
GO

UPDATE [NashvilleHousing] SET OwnerSplitAddress = PARSENAME(REPLACE(OwnerAddress, ',','.'),3);
UPDATE [NashvilleHousing] SET OwnerSplitCity = PARSENAME(REPLACE(OwnerAddress, ',','.'),2);
UPDATE [NashvilleHousing] SET OwnerSplitState = PARSENAME(REPLACE(OwnerAddress, ',','.'),1);
GO


----------------------------------------------------------------------------------------------------------------------
/* Change Y and N to Yes and No in "SoldAsVacant" field */


SELECT DISTINCT(SoldAsVacant), count(SoldAsVacant)
FROM NashvilleHousing
GROUP BY SoldAsVacant

UPDATE NashvilleHousing SET SoldAsVacant = (CASE WHEN SoldAsVacant = 'Y' then 'Yes' WHEN  SoldAsVacant = 'N' then 'No' ELSE SoldAsVacant END);
GO


----------------------------------------------------------------------------------------------------------------------
/* Remove Duplicates */

WITH RowNumCTE AS (
SELECT *, 
ROW_NUMBER() OVER (
	Partition by ParcelID, PropertyAddress, SalePrice, SaleDate, LegalReference
	ORDER BY ParcelID) row_num
FROM NashvilleHousing
)
DELETE
FROM RowNumCTE
WHERE row_num>1
GO

----------------------------------------------------------------------------------------------------------------------
/* Remove Unused Columns */

ALTER TABLE NashvilleHousing
DROP COLUMN OwnerAddress, PropertyAddress, TaxDistrict, SaleDate

Select * from NashvilleHousing

```

Finally, we have a dataset which has meaningful data and can be pulled in BI tool to further perform analysis.