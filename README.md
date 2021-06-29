# Project 1.5-ETL 
  ![streaming-services-cable-main](https://user-images.githubusercontent.com/68654746/123760820-0fef1780-d876-11eb-8f49-6ae04e11bc22.jpg)

***Project 2 for Rutgers Data Science Bootcamp***
###### The purpose of this project is to create a well ordered, useful database that could be utilized by an organization. Databases resulting from ETL are often the starting point for an organization's data analysis.

------------
## Reources:
> *Netflix Shows* https://www.kaggle.com/shivamb/netflix-shows
>>
> *Amazon Prime TV Shows* https://www.kaggle.com/nilimajauhari/amazon-prime-tv-shows
>>
> *Hulu TV Shows* https://data.world/chasewillden/top-1-000-most-popular-hulu-shows 

## Motivation 

For this project, I was interested in comparing tv show datasets from three different streaming services: Netflix, Amazon Prime TV Shows, and Hulu. I'm interested in finding out which streaming service had the most popular tv shows. Additionally, I wondered how many tv shows were available to watch on multiple streaming services or does one streaming service offer more specials than the others. Step one, I looked through datasets on Kaggle and Data.world and narrowed down the selected streaming services and determined that my dataset would not include movies. First we had to make sure each CSV file had similar information that we could take and agree on which columns to draw from. We decided on the following: Title of Show, Genres, No. of Seasons, TV Rating, Streaming Service.

## Steps to perform ETL:

**Extracting** the data from CSV and web-scraping 

**Transforming** by cleaning or reformatting each dataframes, then joining dataframes based on common column titles 

**Loading** or storing the resulting data into a well designed database that can be used to draw data-driven trends and popularity. The final tables will be loaded using SQL 


We organized our files by importing and transforming the files into Dataframes with the mentioned columns, making sure to rename our columns so we all match for easier merging and dropping any rows with missing information. After we were all finished with our individual assignments, we merged the 3 files into one Jupyter file to consolidate our cleaned up files. A quick count of each row in the Dataframes concluded that, with the information we were able to find, Netflix was the biggest streaming service.

One of things we felt was key to our code was creating a python file named Postgres_Login for future users in case our username and/or password was different, similar to how an API key works with urls. We made sure our local database and engine variables were the same so when we merge files, it all sends to the same local database and the tables are all together. We also added a new column to the end of each of our Dataframes to keep track of overlaps.

After creating a connection to the database and sending it out, we created another table using Postgres Query to put Netflix’s tv shows against Amazon Prime’s and Hulu’s available tv shows and to see if there was any overlap. We made sure to change any NULLs in the streaming services columns to ‘Nope’ as a representation for not available on this service.
The biggest challenge we faced was with the query. Originally with the code we had, it was split into 2 sections, one for creating a table and joining, the other was to change NULL variables to “No”. It worked when running it in separate queries but when placed as one, the table gets scuffed and titles are getting duplicated. It took some digging on the internet until we finally found the solution. We tried ISNULL, DISTINCT, CROSS JOIN, COALESCE, etc. None of it worked so until we finally tried CASE WHEN which originally didn’t work until we separated “column_pull” and “text” to “column_pull” and ‘text’. By changing the way we used double quotes and single quotes, we were able to clean the table of any duplicates and were able to run the whole thing as one query instead of 2. (Ex. “CASE WHEN “Hulu” IS NULL THEN “Nope” ELSE “Hulu” END “Hulu” to “CASE WHEN “Hulu” IS NULL THEN ‘Nope’ ELSE “Hulu” END “Hulu”.
