What issues will you address by cleaning the data?
I looked for duplicates and data that does not appear to be accurate, in the sense of being out of range (too big or small) for what its column represents. I also made SKU column names the same across tables for ease in "JOIN" needs. the biggest hurdle to creating a dataset for meaningful analysis is the large number of null values in many columns, some having only null values in all rows.





Queries:
Below, provide the SQL queries you used to clean your data.

--"Name" is an ambiguous column title. Product name? Visitor name? I changed it to “productname” for clarity here and in the Products table. 

–The same ambiguity issue with “SKU” in one table, which doesn’t match the other tables with “productSKU.” Now all SKU-related titles match.
ALTER TABLE sales_report
RENAME COLUMN name to productname;

–Check minimum and maximum values on numeric tables to ensure that no entries are outside the realm of reasonable possibility, such as negative numbers. Units_sold had a value of -89 in one row, which is not possible. Syntax examples:
SELECT MIN(units_sold) FROM analytics;
SELECT MAX(units_sold) FROM analytics;

–Unit_price had a maximum of 995,000,000, which seems unlikely, along with other similarly large figures in this column. The figures were updated, as were columns in the all_sessions table with the same issue.
UPDATE analytics
SET unit_price = unit_price/1000000
WHERE unit_price > 0;

UPDATE all_sessions
SET productprice = productprice/1000000;

UPDATE all_sessions
SET transactionrevenue = transactionrevenue/1000000;

UPDATE all_sessions
SET totaltransactionrevenue = totaltransactionrevenue/1000000;

–All_sessions has two columns, totaltransactionrevenue and productquantity is mistyped as “character varying” rather than a numerical types. I changed them.
DROP TABLE all_sessions;
CREATE TABLE all_sessions 
(fullVisitorId varchar,
channelGrouping varchar,
time varchar,
country varchar,
city varchar,
totalTransactionRevenue numeric,
transactions int,
timeOnSite int,
pageviews int,
sessionQualityDim int,
date date,
visitId int,
type varchar,
productRefundAmount varchar,
productQuantity int,
productPrice int,
productRevenue varchar,
productSKU varchar,
v2ProductName varchar,
v2ProductCategory varchar,
productVariant varchar,
currencyCode varchar,
itemQuantity int,
itemRevenue int,
transactionRevenue int,
transactionId varchar,
pageTitle varchar,
searchKeyword varchar,
pagePathLevel1 varchar,
eCommerceAction_type int,
eCommerceAction_step int,
eCommerceAction_option varchar
);

---TABLE and COLUMN NOTES---

--Syntax used for each column in every table:
SELECT [column_name]
FROM [table_name]
WHERE [column_name] IS NOT NULL


--ALL_SESSIONS TABLE--

--"searchkeyword" is 100% null. It adds no data, no information, and could be deleted from this database.

--"pagetitle" has two visitorID from Taiwan and one from Japan that include non-alphanumeric characters along with English words.

--"ecommerceaction_option" only has 31 rows that are not null, which may not be sufficient for any meaningful analysis. Consider for deletion.

--"itemquantity" is 100% null. It adds no data, no information, and could be deleted from this database.

--"itemrevenue" is 100% null. It adds no data, no information, and could be deleted from this database.

--"transactions" only has 81 non-null rows.

--"transactionID" only has 9 non-null rows.

--"totaltransactionrevenue" only has 81 non-null rows that perfectly coincide with the 81 non-null rows of the "transactions" column.

--"transactionrevenue" only has 4 non-null rows.

--"channelgrouping" has 15,134 non-null rows.

--"city" has 15,134 non-null rows, but 8,302 are "not available in demo dataset" and 354 are "(not set)", so there are only 6,478 meaningful rows.
Syntax for finding these figures:
SELECT city
FROM all_sessions
WHERE city = '(not set)'

SELECT city
FROM all_sessions
WHERE city = 'not available in demo dataset'

----"country" has 15,134 non-null rows, but 24 are "(not set)", so there are only 15,120 meaningful rows.
Syntax for finding these figures:
SELECT country
FROM all_sessions
WHERE country = '(not set)'

SELECT country
FROM all_sessions
WHERE country = 'not available in demo dataset'

--"currencycode" has 14,862 non-null rows.

--"date" has 15,134 non-null rows, ranging from 2016-08-01 to 2017-08-01

--"fullvisitorID" has 15,134 non-null rows.

--"pagepathlevel1" has 15,134 non-null rows.

-"pagetitle" has 15,133 non-null rows.

--"pageview" has 15,134 non-null rows.

--"sessionqualitydim" has 1,228 non-null rows.

--"time" has 15,134 non-null rows, but is mistyped as character varying and has differing numbers of digits in the field.

--"timeonsite" has 11,834 non-null rows

--"type" has 15,134 non-null rows

--"v2productcategory" has 15,134 non-null rows

--"v2productname" has 15,134 non-null rows


--PRODUCTS TABLE--

--All columns except the two "sentiment--" ones have all fields in 1092 rows, and those two each only have one null row. Beautiful.


--SALES_BY_SKU TABLE--

--Both columns have a full, 462 rows of non-null values. But they overlap with the same columns in 


--SALES_REPORT TABLE--

--Only null values are in "ratio" column when its two components, totalordered and stocklevel, are both zero. For all other columns, all 454 rows are non-null.

--ANALYTICS TABLE--

--Has 4,301,122 rows

--"bounces" has 474,839 non-null rows.

--"channelgrouping" has all non-null rows.

--"date" full 4,301,122 rows

--"fullvisitorid" full 4,301,122 rows

--"pageviews" has 4,301,050 non-null rows

--"revenue" has 15,355 non-null rows

--"socialengagementtype" full 4,301,122 rows

--"timeonsite" has 3,823,657 non-null rows

--"unit_price" full 4,301,122 rows

--"units_sold" has 95,147 non-null rows

--"userid" is 100% null values. Consider for deletion.


