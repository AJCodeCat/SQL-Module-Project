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

SELECT al.date, COUNT(a.visitid)
FROM all_sessions al
JOIN analytics a USING(fullvisitorid)
WHERE a.visitid IS NOT NULL
GROUP BY al.date
ORDER BY COUNT(a.visitid) DESC


Answer:

**Top 10 Dates by Site Visits**

"2017-05-24"	4435

"2017-06-27"	4411

"2017-05-22"	3574

"2017-06-05"	3402

"2017-05-01"	3401

"2017-05-18"	3204

"2017-07-25"	2815

"2017-07-08"	2745

"2017-06-21"	2733

"2017-05-09"	2641

You get the exact same results if you replace "a.visitid" with "al.fullvisitorid". From the productcategory-by-country analysis, I discovered that the United Kingdom's most-popular purchases were from a category termed "Spring Sale." Seeing how the most visits clustered in the spring, with half of the top 10 dates occurring in May, it's possible that the sale spurred more visits generally.

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

[12 products tied for fifth place with scores of 0.8.]

**Top 5 Products by Sentiment Magnitude**
" Women's Colorblock Tee White"	2

"Rocket Flashlight"	1.4

" Men's Vintage Tee"	1.4

" Executive Umbrella"	1.35

" Men's Convertible Vest-Jacket Pewter"	1.35

**There was one product in both Top sentiment lists: Men's Vintage Tee. Perhaps such high satisfaction should lead to greater promotion of the product to site visitors looking at men's apparel.**




Question 4: What is the top month for sales of the popular SPF-15 Slim & Slender Lip Balm?

SQL Queries:

SELECT DATE_TRUNC('MONTH', al.date) AS Month, SUM(a.units_sold)

FROM all_sessions al

JOIN analytics a USING(fullvisitorid)

WHERE al.v2productname = 'SPF-15 Slim & Slender Lip Balm' 

GROUP BY 1

Answer: Some of the same items, like the "SPF-15 Slim & Slender Lip Balm" were popular in more than one location. Looking at units sold, there is possibly some missing values for May and June, which appeared in the query results, but with null values for units sold.

"2017-05-01 00:00:00-04"	

"2017-06-01 00:00:00-04"	

"2017-07-01 00:00:00-04"	203


Question 5: 


Answer:


