# Data Cleaning in MySQL

## Project Overview
This project involves cleaning a dataset related to employee layoffs using MySQL. The aim is to ensure data quality and consistency which enables more accurate and insightful analysis. This documentation outlines the steps taken, the challenges encountered, and the solutions implemented during the data cleaning process.

## Objectives
1) remove duplicates
2) Standardize the data
3) Remove or populate NULL values/ BLANK values
4) Remove columns which are not required.

## Dataset Description
The dataset "employee layoffs" have the following fields:
- company
- location
- industry
- total_laid_off
- percentage_laid_off
- date
- stage
- country
- funds_raised_millions

## Process
### Importing Data
- Imported the dataset [layoffs](https://github.com/alnmrts02/Data-Cleaning-Project-MySQL-/blob/main/layoffs.csv) into MySQL database.
- Using the Table-Data import wizard, the data was loaded into the database.

### Data Exploration
- Conducted an initial exploration of the dataset using `SELECT` queries.
- Identified key data issues such as missing values, duplicates, and inconsistent formats.

### Creating a Staging Database
- Created a duplicate of the raw dataset to work on the data and to make the required adjustments.
- Creating a staging database ensures that, in the event of faults or mistakes, the original raw data remains safeguarded.
```sql
create table layoffs_staging   
like layoffs;

INSERT layoffs_staging
select *
From layoffs;
```
### Standardizing data
- To remove unwanted spaces from the `company` column.
```sql
SELECT company, (TRIM(company))
FROM layoffs_staging2;

UPDATE layoffs_staging2
SET company = TRIM(company);
```
- To identify and standardize similar names within `industry` column.
```sql
SELECT *
FROM layoffs_staging2
WHERE industry LIKE 'Crypto%';

UPDATE layoffs_staging2
SET industry = 'Crypto'
WHERE industry LIKE 'Crypto%';
```  

- Trimming unwanted spaces from `country` column
```sql
SELECT country, (TRIM(TRAILING '.' FROM country))
FROM layoffs_staging2;

UPDATE layoffs_staging2
SET country = (TRIM(TRAILING '.' FROM country))
WHERE country LIKE 'United States%';
```

- Changing the `date` format from text to date
```sql
SELECT `date`,
str_to_date(`date`, '%m/%d/%Y')
FROM layoffs_staging2;

UPDATE layoffs_staging2
SET `date` = str_to_date(`date`, '%m/%d/%Y');

ALTER TABLE layoffs_staging2
MODIFY COLUMN `date` DATE;
```

### Since some cells are empty it is important to change it to NULL value to reduce complexity
```sql
UPDATE layoffs_staging2
SET industry = NULL 
WHERE industry = '';
```









