# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
(fill in your description and goals here)
This project represents my first attempt at going out on my own with a fairly raw databased and trying to process it into some meaninngful and efficient for analyses. My goals are to clean the most egregious errors and problems, strive to document what I do to a degree where someone new to the database could understand the process, and successfully utilize SQL code, of which I knew absolutely nothing two weeks ago, to answer questions the database can answer.

## Process
### (your step 1)
### (your step 2)

## Results
(fill in what you discovered this data could tell you and how you used the data to answer those questions)
My larger discoveries were about what the data could NOT tell me because of all the missing values. Tantalizing column names suggested possible analyses that turned out to be impossible because of insufficient or completely absent data. "WHERE [column_name] IS NOT NULL" was probably my most commonly used line of code so that I could examine the data that did exist, and compare the number of rows in those results to the columns' total rows. With a dataset this large, getting a sample of sufficient size could occur even with significant missing values, and often did occur, but not always.

## Challenges 
(discuss challenges you faced in the project)
My chief issue, after getting the tables to import, was figuring out what should be deleted, could be deleted, how to determine that (if fullvisitorid is the same but productsku is not in two rows, are the two rows truly distinct?), and finally, how to accomplish the deletion. Dropping entire column was simple, but deleting rows and cell was not and is the biggest problem remaining in my current version of the database.

## Future Goals
(what would you do if you had more time?)
With more time, I would like to continue to clean the data to eliminate extreme, inaccurate values, eliminate duplicates, and otherwise streamline the database. I also would like to explore why there are so many null values and in particular why so many columns only have nulls. Is the data not being collected or perhaps not being correctly generated? The column names suggest information that could be informative, such as all_sessions.productrefundamount, which could be used to detect is certain products end in more returns than others. The column analytics.socialengagementtype only had "Not Socially Engaged" for every row. Is that useful information, or should the column be deleted? An error?
