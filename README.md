# Data-modeling-with-apache-cassandra

## Overview
A startup called sparkify wants to analyze the data they have been collecting on songs and user activity on their music streaming app. The analysis team is particularly interested in understanding what songs users are listening to. Currently there is no easy way to query the data to generate the results, since the data reside in a directory of csv files on user activity on the app. 

## Objective
- Creating an apache cassandra database which can create queries on song play data to answer the required questions.
- Creating a database for the analysis.
- Modeling data by creating tables in Apache Cassandra to run queries.
- ETL pipeline that transfers data from a set of CSV files within a directory to create a streamline CSV file model.
- Insert data into apache cassandra tables.

## Datasets
- One dataset : event_data
- Directpry of csv files partitioned by date
- Example : event_data/2018-11-08-events.csv

  Below is an example of what an original single event data file, 2018-11-08-events.csv, looks like.
  
    {
      "artist": "Slipknot", 
      "auth": "Logged In", 
      "firstName": "Aiden", 
      "gender": "M", 
      "itemInSession": 0, 
      "lastName": "Ramirez", 
      "length": 192.57424, 
      "lvevel": "paid", 
      "location": "New York-Newark-Jersey City,NY-NJ-PA"
      "method": "PUT"    
      "page": "NextSong"
      "regisgration":1.54028E+12
      "sessionId":19
      "song":"Opinum Of The People"
      "status":200
      "ts":1.5416E+12
      "userId":20    
  }
