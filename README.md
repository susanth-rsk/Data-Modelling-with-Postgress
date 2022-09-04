#### Introduction and Motivation
A startup called Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. The analytics team is particularly interested in understanding what songs users are listening to. The created architechture should serve the purpose of serving queries raised by Sparkify's analytics team.

#### Tasks at hand
To create a Postgres database with tables designed to optimize queries on song play analysis. This is achieved by creating a database schema and ETL pipeline. The fact and dimension tables for the star schema for a particular analytic focus must be created along with an ETL pipeline that transfers data from files in two local directories into these tables in Postgres using Python and SQL.

#### Available data
Two datasets are available - the song dataset and the log dataset. Both datasets contain multiple files are in JSON file format. The song dataset is a subset of real data from the Million Song Dataset and each file in the dataset contains metadata about a song and the artist of that song. The files are partitioned by the first three letters of each song's track ID. The log dataset consists of log files a event simulator based on the songs in the song dataset. 

#### Files in the repository

- data folder : This folder contains the data for building the tables and the database. Inside it contains the two datasets: the song dataset and the log dataset in two folders

- create_tables.py : This python script contains relevant functions to create, drop, connect to databases and tables. This file to reset the tables before running the ELT scirpts each time.

- etl.ipynb : Reads and processes a SINGLE file from song dataset and log dataset and loads into the tables in Jupyter notebook. This notebook contains detailed instructions on the ETL process for each of the tables.

- etl.py : Reads and processes ALL the files from song dataset and log dataset and loads them into your tables. 

- sql_queries.py : This contains all the sql queries, and is imported into the last three files above.

- test.ipynb : This is used for testing purposes. It displays the first few rows of each table and checks whether databases are created properly. It consists of tests confirm the creation of tables with the correct columns, column data types, primary key constraints and not-null constraints as well look for on-conflict clauses wherever required.

- README.md : Description of the project and the repository

#### How to run the scripts

1. First execute the command for creating tables and connecting to the database `python3 create_tables.py`.
2. Then execute this command to start the ETL process to load the data into the database `python3 etl.py`.

#### Database Schema design

We have utilized a star schema design that is ideal for analytical query handling but also for data integrity. With star schema, the queries from analytical team can be handled with both high efficiency and performance. The fact table and the dimentsion tables are listed below with their corresponding columns. The first column in the list is the PRIMARY KEY for the corresponding table.

###### Fact Table

- songplays - records in log data associated with song plays i.e. records with page NextSong
    - songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent

##### Dimension Tables

- users : users in the app
    - user_id, first_name, last_name, gender, level

- songs : songs in music database
    - song_id, title, artist_id, year, duration

- artists : artists in music database
    - artist_id, name, location, latitude, longitude

- time - timestamps of records in songplays broken down into specific units
    - start_time, hour, day, week, month, year, weekday

#### ETL Pipeline
The ETL pipeline actively segregates the read, processing and write process for each table to ensure the right information enteres in the databases. Once the tables are created and 'sparkifydb' database is intialized, the ETL process in the file etl.py is carried out in the following steps:

1. Connection to the intialized database is established.
2. List of all files in the song dataset is obtained and each JSON file is read and processed one-by-one.
    1. The information pertaining to song metadata is entered into the **songs** table
    2. The information pertaining to artists for each song is entered into the **artists** table
3. List of all log files in the log dataset is obtained and each JSON file is read and processed one-by-one.
    1. The timestamp is converted into the unix format and broken down to insert into the **time** table
    2. The information pertaining to the users from the log file is entered into the **users** table
    3. The log information about all songs accessed by users is inserted into **songplays** table
4. Disconnect from database.



