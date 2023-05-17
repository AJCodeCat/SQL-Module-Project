Question 1: What products got the most pageviews?

SQL Queries:

SELECT SUM(al.pageviews), sr.productname
FROM all_sessions al
JOIN sales_report sr USING(productsku)
WHERE al.pageviews IS NOT NULL
GROUP BY sr.productname
ORDER BY SUM(al.pageviews) DESC

Answer: 

**Top 5 Products by Pageviews**
1268	" Men's 100% Cotton Short Sleeve Hero Tee White"
1038	" Twill Cap"
747	"22 oz  Bottle Infuser"
734	" Men's Vintage Tank"
733	" Men's 100% Cotton Short Sleeve Hero Tee Navy"


Question 2: What dates had the most site visits?

SQL Queries:

SELECT al.date, COUNT(a.visitid), sr.productname
FROM all_sessions al
JOIN sales_report sr USING(productsku)
JOIN analytics a USING(fullvisitorid)
WHERE a.visitid IS NOT NULL
GROUP BY al.date, sr.productname
ORDER BY COUNT(a.visitid) DESC


Answer:

**Top 10 Dates by Site Visits**
"2017-06-27"	2790	" Power Bank"
"2017-03-13"	1582	"Android Twill Cap"
"2016-12-02"	1582	" Lunch Bag"
"2017-05-24"	1558	"Clip-on Compact Charger"
"2017-06-05"	1109	"Android Wool Heather Cap Heather/Black"
"2017-07-08"	969	" Device Stand"
"2017-05-01"	852	"7 Dog Frisbee"
"2017-05-01"	852	" Collapsible Pet Bowl"
"2017-05-09"	840	" Custom Decals"
"2017-01-11"	839	" Custom Decals"

You get the exact same results if you replace "a.visitid" with "al.fullvisitorid".

Question 3: What products had the highest sentiment scores?

What products had the highest sentiment magnitude?

SQL Queries:

--Top Avg Sentiment Scores--
SELECT productname, AVG(sentimentscore)
FROM products p
GROUP BY productname
ORDER BY AVG(sentimentscore) DESC

--Top Avg Sentiment Magnitude--
SELECT productname, AVG(sentimentmagnitude)
FROM products p
GROUP BY productname
ORDER BY AVG(sentimentmagnitude) DESC

Answer:

**Top 4 Products by Sentiment Score**
"USB wired soundbar - in store only"	1
" Men's Vintage Tee"	0.9
" Pocket Bluetooth Speaker"	0.9
" G Noise-reducing Bluetooth Headphones"	0.8500000000000001
[12 products tied with scores of 0.8.]

**Top 5 Products by Sentiment Magnitude**
" Women's Colorblock Tee White"	2
"Rocket Flashlight"	1.4
" Men's Vintage Tee"	1.4
" Executive Umbrella"	1.35
" Men's Convertible Vest-Jacket Pewter"	1.35

**There was one product in both Top sentiment lists: Men's Vintage Tee.**




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

