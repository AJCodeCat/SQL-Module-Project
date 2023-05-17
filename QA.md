What are your risk areas? Identify and describe them.

-Duplications
  Rows within a table, i.e. duplicate rows with matching all_sessions.fullvisitorid
  Columns in different tables without benefit, i.e. the entire "sales-by-sky" table

-Null values at the row and column level

-Missing values causing large losses in analytic capabilities

-Inaccurate/impossible values, i.e. -89 units sold

-Standardization of column names across tables, i.e. "sku" vs. "productsku"


QA Process:
Describe your QA process and include the SQL queries used to execute it.
1) Explore the data to learn: 
  -what types of information are available, 
  -see what values are in categorical and nominal columns, 
  -the minimum and maximum values
  -which columns have significant numbers of null or missing rows.
  
**Syntax**

SELECT [column_name]
    FROM [table_name];

  --Examine row values and number of rows returned by query
  
  SELECT [column_name]
    FROM [table_name]
    WHERE [column_name] IS NOT NULL;
    
    --How many fewer rows returned?

2) Analyze the data to find outliers, spurious values, and where analyses might be limited by missing values.

**Syntax**

SELECT MIN([column_name]) FROM [table_name];

SELECT MAX([column_name]) FROM [table_name];


3) Make decisions about any deletions or changes to be made to rows, columns, or tables, and enact them.

**Syntax for Dropping Columns**
ALTER TABLE [table_name]
DROP COLUMN [column_name]


**Syntax for sizing down impossibly, national-GDP-size large monetary values in entire columns**

UPDATE analytics

SET unit_price = unit_price/1000000

WHERE unit_price > 0;


**Syntax for standardizing ambiguous column names**

ALTER TABLE sales_report

RENAME COLUMN name to productname;
