What issues will you address by cleaning the data?
I looked for duplicates and data that does not appear to be accurate, in the sense of being out of range (too big or small) for what its column represents. I also made SKU column names the same across tables for ease in "JOIN" needs.





Queries:
Below, provide the SQL queries you used to clean your data.

--"Name" is an ambiguous column title. Product name? Visitor name? I changed it to “productname” for clarity here and in the Products table. 
–The same ambiguity issue with “SKU” in one table, which doesn’t match the other tables with “productSKU.” Now all SKU-related titles match.
ALTER TABLE sales_report
RENAME COLUMN name to productname;

–Check minimum and maximum values on numeric tables to ensure that no entries are outside the realm of reasonable possibility, such as negative numbers. Units_sold had a value of -89 in one row, which is not possible. 
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


