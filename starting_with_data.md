Question 1: Which cities and countries have the highest level of transaction revenues on the site?

SQL Queries:
SELECT SUM(a.revenue), al.city, al.country FROM analytics a
JOIN all_sessions al USING(visitid)
WHERE a.revenue IS NOT NULL 
GROUP BY al.city, al.country
ORDER BY SUM(a.revenue) DESC

Answer: The United States, Canada, Germany Switzerland, and Israel had the highest total revenue, with New York, TorontoTel Aviv-Yafo, and Zurich having the highest total revenues in their respective countries. California and cities with large numbers of university students placed high in the revenue rankings. The U.S. cities of Palo Alto, Cali., Cambridge, Mass., and Ann Arbor, Mich., are all notable university towns, and the California cities of Meadow View, Sunnyvale, and San Bruno also have universities. While New York had the most total revenue of listed cities, it should be noted that the highest-ranked revenues were for a U.S. city or cities whose names are missing from the database, and thus getting aggregated together, as was the case for Germany.



Question 2: What is the average number of products ordered from visitors in each city and country?

SQL Queries:


Answer:



Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?

SQL Queries:

Answer:



Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?

SQL Queries:
SELECT DISTINCT ON (al.city) al.city, p.productname, SUM(p.orderedquantity) AS totalordered
FROM all_sessions al
JOIN products p on p.productname = al.v2productname
WHERE al.city IS NOT NULL AND al.city != 'not available in demo dataset'
	AND al.city != '(not set)'
GROUP BY al.city, p.productname

Answer:



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
***Results in 105 rows***

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
***"not available in demo dataset" is 7.12% of the total revenue.***

Answer:
