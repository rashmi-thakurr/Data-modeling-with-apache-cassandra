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

## Data Modeling
Created the following tables for the requested queries.

### songinfo_by_session_by_item
Query Requested : Give me the artist, song title and song's length in the music app history that was heard during sessionId = 338, and itemInSession = 4

Query to create table:
 CREATE TABLE IF NOT EXISTS songinfo_by_session_by_item (
            sessionId INT,
            itemInSession INT,
            artist TEXT,
            song TEXT,
            length FLOAT,
            PRIMARY KEY (sessionId, itemInSession));

Query to select data:
SELECT artist, song, length
    FROM songinfo_by_session_by_item
    WHERE
        sessionId=338 AND
        itemInSession=4;          

The combination of sessionId and itemInSession is unique, and we need results based on sessionId and itmeInSession, so these two columns could be the Composite Primary Key of the table.

### songinfo_by_user_by_session
Query Requested : Give me only the following: name of artist, song (sorted by itemInSession) and user (first and last name) for userid = 10, sessionid = 182

Query to create table:
 CREATE TABLE IF NOT EXISTS songinfo_by_user_by_session (
            userId INT,
            sessionId INT,
            itemInSession INT,
            artist TEXT,
            song TEXT,
            firstName TEXT,
            lastName TEXT,
            PRIMARY KEY ((userId, sessionId), itemInSession));
            
Query to select data:
SELECT artist, song, firstName, lastName
    FROM songinfo_by_user_by_session
    WHERE
        userId=10 AND
        sessionId=182
    ORDER BY itemInSession;
        
Since we need results based on userid and sessionid, it could make good perfomance to use userid and sessionid as partition key to make sure the sessions belonging to the same user could be distributed to same node

### userinfo_by_song
Query Requested : Give me every user name (first and last) in my music app history who listened to the song 'All Hands Against His Own'

Query to create table:
 CREATE TABLE IF NOT EXISTS userinfo_by_song (
            song TEXT,
            firstName TEXT,
            lastName TEXT,
            userId INT,
            PRIMARY KEY (song, userId));
            
Query to select data:
SELECT firstName, lastName
    FROM userinfo_by_song
    WHERE song='All Hands Against His Own';
    
Since we need the results based on song name, firstName and lastName might be not unique, we need to use song name and userid as Primary Key

## Project files
1. Project_1B_ Data_Modelling_With_Cassandra.ipynb Create a denormalized dataset, load the data into tables and run test queries
2. README.md Provide project info
