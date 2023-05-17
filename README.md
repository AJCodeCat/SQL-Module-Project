# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals

This project represents my first attempt at going out on my own with a fairly raw databased and trying to process it into some meaninngful and efficient for analyses. My goals are to clean the most egregious errors and problems, strive to document what I do to a degree where someone new to the database could understand the processes I undertook, and successfully utilize SQL code to answer questions the database can answer.

## Process

1) Explore the data to learn: 
  -what types of information are available, 
  -see what values are in categorical and nominal columns, 
  -the minimum and maximum values
  -which columns have significant numbers of null or missing rows.

2) Analyze the data to find outliers, spurious values, and where analyses might be limited by missing values.

3) Make decisions about any deletions or changes to be made to rows, columns, or tables, and enact them.

## Results

My larger discoveries were about what the data could NOT tell me because of all the missing values. Tantalizing column names suggested possible analyses that turned out to be impossible because of insufficient or completely absent data. "WHERE [column_name] IS NOT NULL" was probably my most commonly used line of code so that I could examine the data that did exist, and compare the number of rows in those results to the columns' total rows. With a dataset this large, getting a sample of sufficient size could occur even with significant missing values, and often did occur, but not always.

Having said that, the data, where present, revealed where many transactions occurred using the city and country information. California emerged as a top area and a pattern of unversity towns frequently represented, such as Palo Alto, Calif., home to Stanford University, and Ann Arbor, Mich., home to the the University of Michigan's flagship campus. I also found that the site was most heavily visited in the months of May, June, and July in 2016 by counting pageviews by month. Looking at the units sold by city, certain individual products, such as travel mugs and a lip balm with SPF, were popular in cities on opposite side of the planet.

## Challenges 

My chief issue, after getting the tables to import, was figuring out what should be deleted, could be deleted, how to determine that (if fullvisitorid is the same but productsku is not in two rows, are the two rows truly distinct?), and finally, how to accomplish the deletion. Dropping entire columns was simple, but deleting rows and cells was not and is the biggest problem remaining in my current version of the database.

## Future Goals

With more time, I would like to continue to clean the data to eliminate extreme, inaccurate values, eliminate duplicates, and otherwise streamline the database. I also would like to explore why there are so many null values and in particular why so many columns only have nulls. Is the data not being collected or perhaps not being correctly generated? The column names suggest information that could be informative, such as all_sessions.productrefundamount, which could be used to detect if certain products end in more returns than others. The column analytics.socialengagementtype only had "Not Socially Engaged" for every row. Is that useful information, or should the column be deleted? Do the values represent a coding error of some kind on the website?

Taking another step back, if this website were a client or employer, I would like to discuss improvements and amendments, chiefly the addition of unique identification numbers for customers and for orders/transactions.
