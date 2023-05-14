Question 1: Which cities and countries have the highest level of transaction revenues on the site?

SQL Queries:
SELECT SUM(a.revenue), al.city, al.country FROM analytics a
JOIN all_sessions al USING(visitid)
WHERE a.revenue IS NOT NULL 
GROUP BY al.city, al.country
ORDER BY SUM(a.revenue) DESC

Answer: The United States, Switzerland, and Israel had the highest total revenue, with New York, Tel Aviv-Yafo, and Zurich having the highest total revenues in their respective countries. The U.S. cities of Sunnyvale, Mountain View, Seattle, San Francisco, Chicago, San Jose, and Palo Alto all ranked higher than Tel Aviv-Yafo or Zurich. While New York had the most total revenue of listed cities, it should be noted that the highest-ranked revenues were for a U.S. city whose name is missing from the database.



Question 2: What is the average number of products ordered from visitors in each city and country?

SQL Queries:

Answer:



Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?

SQL Queries:

Answer:



Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?

SQL Queries:

Answer:



Question 5: Can we summarize the impact of revenue generated from each city/country?

SQL Queries:

Answer:
