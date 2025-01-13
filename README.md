# Data Cleaning SQL Project - Layoffs Dataset

# Project Overview

This project involves data cleaning on the 2022 layoffs dataset obtained from Kaggle. The goal is to clean and prepare the data for analysis by following best practices, ensuring the dataset is accurate, standardized, and ready for exploratory data analysis (EDA).

Dataset Information

The dataset, world_layoffs.layoffs, contains information on layoffs across different companies in 2022, including the company name, industry, location, total laid off, percentage laid off, date, and more.

Steps in Data Cleaning

The cleaning process follows four key steps:

1. Remove Duplicates

A staging table layoffs_staging was created to ensure the raw data remains intact.

Duplicate rows were identified using ROW_NUMBER() and removed from the staging table.

To maintain data integrity, rows identified as true duplicates were carefully reviewed and deleted.

2. Standardize Data

Null and Blank Values:

Blank values in the industry column were updated to NULL for consistency.

Missing industry values were filled using non-null values from rows with matching company names.

Industry Names:

Variations in industry names (e.g., "Crypto Currency" and "CryptoCurrency") were standardized to "Crypto."

Country Names:

Standardized country names by removing trailing periods (e.g., "United States.").

Dates:

Converted the date column from text format to a DATE data type using STR_TO_DATE() and updated the column definition.

3. Handle Null Values

Columns with null values, such as total_laid_off, percentage_laid_off, and funds_raised_millions, were retained as nulls to facilitate accurate calculations during EDA.

Rows with both total_laid_off and percentage_laid_off null were deleted as they lacked meaningful data.

4. Remove Unnecessary Data

Irrelevant rows and columns were removed:

The row_num column, used during the duplicate removal process, was dropped.

Key SQL Operations

Below are some of the key SQL operations performed during the project:

Removing Duplicates

DELETE FROM world_layoffs.layoffs_staging2
WHERE row_num >= 2;

Updating Null Industries

UPDATE layoffs_staging2 t1
JOIN layoffs_staging2 t2
ON t1.company = t2.company
SET t1.industry = t2.industry
WHERE t1.industry IS NULL
AND t2.industry IS NOT NULL;

Standardizing Industry Names

UPDATE layoffs_staging2
SET industry = 'Crypto'
WHERE industry IN ('Crypto Currency', 'CryptoCurrency');

Cleaning Country Names

UPDATE layoffs_staging2
SET country = TRIM(TRAILING '.' FROM country);

Converting Date Format

UPDATE layoffs_staging2
SET `date` = STR_TO_DATE(`date`, '%m/%d/%Y');

ALTER TABLE layoffs_staging2
MODIFY COLUMN `date` DATE;

Final Dataset

The cleaned dataset (layoffs_staging2) is now:

Free of duplicates and unnecessary rows.

Standardized for key columns such as industry, country, and date.

Contains only meaningful data ready for analysis.

How to Use the Project

Clone this repository.

Import the raw dataset into your database.

Run the SQL scripts sequentially to clean the dataset.

Use the cleaned dataset for further analysis and visualization.

Future Work

Perform exploratory data analysis to identify trends in layoffs.

Develop interactive dashboards using tools like Power BI or Tableau.

License

This project is provided for educational purposes and follows the Kaggle dataset license.

