# Data-Mining-Assignment-Fall-2016
Assignment of Data Mining Fall 2016 CS235

Assignment Problem is described in **_assignment-description.pdf_**

Full report in **_Data\_Mining\_Assignment\_Report.pdf_**

All the plots are in **Plots** folder.

##Data Crawling

Crawler is written in Python. In the crawler for each section i.e Data Mining, Databases,
Machine Learning & Artificial Intelligence have called a general function `data_from_url(target_url)`
passing appropriate URL for each section.

In this function, page number are added incrementally and using request API the web-page is
requested. Then using beautiful soup URL is extracted for each conference and then requested
again the web-page for that Conference from there extracted Conference Name and Conference
Location. Appropriate time limiter was added of 10 seconds between each page requests.

Finally after getting all the required data i.e Conference_Acronym,Conference_Name and
Conference_Location for all 20 pages and for all 4 categories, added those values to the file in the tab
separated manner in appending manner using CSV Writer.

Please check **crawler.py** for the crawler code and **data_mining_original.tsv** for the tab separate values
crawled using the crawler.

##Data Cleaning

Used OpenRefine for the data cleaning task as recommended by the Instructor.

Following steps were taken in data cleaning:

1. Importing Data

2. Removing whitespaces and conference_location as N/A

3. Data Clustering using the clustering feature of OpenRefine

4. Anomalous City and Conferences fixed manually

5. Deletion of Repeated Values

6. Splitting city from location and Year from acronym

7. Removing Random Unwanted Words from Conference Name

After cleaning as much as possible in my opinion this was the my final data set of 1162 rows.

Cleaned up data in a comma separate value is in the **data_mining_refined.csv file**.

##Hadoop Implementation

###Part A: 

To calculate the number of conferences per city I first split the row using “,” as splitting point
then from separate values I used conference_city and count 1 as the value.

Reducer added up all the values for a particular city resulting in total number of conference per city.

Result generated is in the **conf_per_city.txt** file and the code is in **conf_per_city.java**.

###Part B:

To calculate the list of conferences per city I did the same thing as before by spliting the row
using “,” as splitting point then from separate values I used conference_city as the key and
conference_acronym as the value.

In Reducer I concatenated all the value I get for a particular key which resulted in list of conference per
city. But it can happen that if a particular conference happened in a particular city in different year it
will result into repeated values so I also added the check in the Reducer that if a particular conference
is already added then I wont add it again which will result only in unique conference occurred in the
city. Although there were not many cases there this were frequent only for those cities where
conferences occurs frequently.

Conference per city with repeated value is present in **list_conf_per_city.txt** and without repetition one
is present in **list_conf_per_city_without_repetition.txt** and the code is in **list_conf_per_city.java**.

###Part C:

To calculate the list of cities per conference I did the same thing as above only change was I
used conference_acronym as the key and conference_city as the value.

In Reducer I concatenated all the value I get for a particular key which will result in list of cities per
conference. Same approach was taken to reduce the redundancy of cities per conference.

Cities per conference with repeated value is present in **list_city_per_conf.txt** and without repetition
one is present in **list_city_per_conf_without_repetition.txt** and the code is in **list_city_per_conf.java**.

###Part D:

To calculate the number of year wise conference for a particular I used conference_acronym +
“:” + conference_year as the key and count ‘1’ as the value.

Reason for using such key is it wont mix all the count for a particular city and wont mix count for a
particular year. Thats why it will give me count of conference occurred in a particular city for a
particular year.

In Reducer I just summed up all the value I get for a the key which will result in time series of
conference per city.

Cities per conference with repeated value is present in **year_wise_conf.txt** and the code is in
**year_wise_conf.java**