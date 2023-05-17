Question 1: Which cities and countries have the highest level of transaction revenues on the site?

SQL Queries:
SELECT SUM(a.revenue), al.city, al.country 

FROM analytics a

JOIN all_sessions al USING(visitid)

WHERE a.revenue IS NOT NULL 

GROUP BY al.city, al.country

ORDER BY SUM(a.revenue) DESC

Answer: The United States, Canada, Germany Switzerland, and Israel had the highest total revenue, with New York, TorontoTel Aviv-Yafo, and Zurich having the highest total revenues in their respective countries. California and cities with large numbers of university students placed high in the revenue rankings. The U.S. cities of Palo Alto, Cali., Cambridge, Mass., and Ann Arbor, Mich., are all notable university towns, and the California cities of Meadow View, Sunnyvale, and San Bruno also have universities. While New York had the most total revenue of listed cities, it should be noted that the highest-ranked revenues were for a U.S. city or cities whose names are missing from the database, and thus getting aggregated together, as was the case for Germany.



Question 2: What is the average number of products ordered from visitors in each city and country?

SQL Queries:
--AVG NUMBER OF PRODUCTS BY COUNTRY--
SELECT al.country, AVG(p.orderedquantity) AS avgquantity
FROM all_sessions al
JOIN products p on p.productname = al.v2productname
WHERE al.country IS NOT NULL 
GROUP BY al.country
ORDER BY avgquantity DESC

--AVG NUMBER OF PRODUCTS BY CITY--
SELECT al.city, AVG(p.orderedquantity) AS avgquantity
FROM all_sessions al
JOIN products p on p.productname = al.v2productname
WHERE al.city IS NOT NULL AND al.city != 'not available in demo dataset'
	AND al.city != '(not set)'
GROUP BY al.city
ORDER BY avgquantity DESC

Answer:
Top 5 Countries by Avg Quantity Ordered
"Bulgaria"	1241.20
"Kazakhstan"	1184.00
"Saudi Arabia"	1047.33
"Serbia"	984.50
"Costa Rica"	879.00

Top 5 Cities by Avg Quantity Ordered
"Santa Fe"	3682
"Zhongli District"	3582
"Riyadh"	3071
"Brno"	3071
"Saint Petersburg"	3071
The closeness of the values for the top cities and in particular the three matching values is suspicious. More detailed examination is necessary to determine if the results are accurate.


Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?

SQL Queries:
--TOP PRODUCT CATEGORIES BY COUNTRY--
SELECT DISTINCT ON(al.v2productcategory) al.v2productcategory, al.country, 
	SUM(p.orderedquantity)           
FROM all_sessions al
JOIN analytics a USING(fullvisitorid)
JOIN products p on p.productname = al.v2productname
WHERE v2productcategory LIKE '%/' AND p.orderedquantity > 0
GROUP BY v2productcategory, country

--TOP PRODUCT CATEGORIES BY CITY--
SELECT DISTINCT ON(al.v2productcategory) al.v2productcategory, al.city, 
	SUM(p.orderedquantity)           
FROM all_sessions al
JOIN analytics a USING(fullvisitorid)
JOIN products p on p.productname = al.v2productname
WHERE v2productcategory LIKE '%/' AND p.orderedquantity > 0
GROUP BY v2productcategory, city

Answer:
In countries' product categories, Australians wanted men's clothing, with men's outerwear (384) and t-shirts (5406) being ordered as well as other apparel (8064) but nothing topped stickers (10,164). Canadians bought both notebooks (73,704) and writing instruments (60,476), a possibly complementary choice. The British went for items included in the Spring Sale (23,400).
Looking at the cities, Hyderabad, India, loves stickers (12,000), and Ann Arborites acquired many items from the Clearance Sale (15,990).


Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?

SQL Queries:
SELECT DISTINCT ON (al.city) al.city, p.productname, SUM(p.orderedquantity) AS totalordered
FROM all_sessions al
JOIN products p on p.productname = al.v2productname
WHERE al.city IS NOT NULL 
	AND al.city != 'not available in demo dataset'
	AND al.city != '(not set)'
GROUP BY al.city, p.productname

Answer: Some of the same items, like the "SPF-15 Slim & Slender Lip Balm" were popular in more than one location. The "26 oz Double Wall Insulated Bottle" sold an equal 845 units in both Barcelona and Bangkok, for example.



Question 5: Can we summarize the impact of revenue generated from each city/country?

SQL Queries:
--COUNTRY PERCENTAGE OF TOTAL REVENUE--
SELECT al.country, SUM(a.revenue) as totalcountryrevenue, 
	(SUM(a.revenue)/1167078437957 * 100) AS PercentTotalRevenue
FROM all_sessions al
JOIN analytics a USING(fullvisitorid)
WHERE a.revenue IS NOT NULL
GROUP BY al.country
ORDER BY totalcountryrevenue DESC
***Results in 5 rows***

SELECT al.country, SUM(a.revenue) as totalcountryrevenue, 
	(SUM(a.revenue)/1167078437957 * 100) AS PercentTotalRevenue
FROM all_sessions al
JOIN analytics a USING(fullvisitorid)
GROUP BY al.country
ORDER BY totalcountryrevenue DESC
***Results in 105 rows, with 100 countries lacking data for revenue.***

--CITY PERCENTAGE OF TOTAL REVENUE--
SELECT al.country, al.city, SUM(a.revenue) as totalcityrevenue, 
	(SUM(a.revenue)/1167078437957 * 100) AS PercentTotalRevenue
FROM all_sessions al
JOIN analytics a USING(fullvisitorid)
WHERE a.revenue IS NOT NULL
	AND al.city IS NOT NULL 
	AND al.city != 'not available in demo dataset'
	AND al.city != '(not set)'
GROUP BY al.country, al.city
ORDER BY totalcityrevenue DESC

SELECT al.city, SUM(a.revenue) as totalcityrevenue, 
	(SUM(a.revenue)/1167078437957 * 100) AS PercentTotalRevenue
FROM all_sessions al
JOIN analytics a USING(fullvisitorid)
WHERE a.revenue IS NOT NULL
GROUP BY al.city
ORDER BY totalcityrevenue DESC
***The "missing city" code "not available in demo dataset" had revenues accounting for 7.12% of the total***

Answer:
These queries showed what percentage of the total revenue each city or each country contributed to the whole, with all results being under 10 percent and almost all being below one percent. The United States made up the biggest single piece of the revenue pie with 9.8%. However, the vast amounts of missing city and country information constrains our understanding about from where orders are coming. For example, in the analysis of revenue generated in different cities, the missing-city designation "not available in demo dataset" collectively accounted for 7.12% of the total revenue. Properly apportioned, some cities might move up in the ordered ranks, or reveal themselves as noticeable revenue sources.

