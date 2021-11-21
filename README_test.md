# Project 1: Data Modeling with Postgres 

## **Overview**
For this project I need <strong>build an ETL pipeline with Postgres using Python.</strong>
<p>A startup called Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. The focus of the time are to understanding what songs their users are listening during a period of time.

In this project I use <strong>two main datafiles (json files)</strong>:
<p><strong>First</strong> is the song dataset (you can see a simple example of this file below), this is a subset of real data from the Million Song Dataset. Each file contains metadata about a song and the artist of that song
Sample Record :

	```
	{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}
	```

<strong>Second</strong> is the log dataset this data was based on the songs in the dataset before and is a simulate activities logs from a music app. You can see an example below:
Sample Record :

	```
	{"artist": null, "auth": "Logged In", "firstName": "Walter", "gender": "M", "itemInSession": 0, "lastName": "Frye", "length": null, "level": "free", "location": "San Francisco-Oakland-Hayward, CA", "method": "GET","page": "Home", "registration": 1540919166796.0, "sessionId": 38, "song": null, "status": 200, "ts": 1541105830796, "userAgent": "\"Mozilla\/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/36.0.1985.143 Safari\/537.36\"", "userId": "39"}
	```


## Schema
<p>This schema is a star schema and consists of one of more fact tables referencing any number of dimensions tables

#### Fact Table
<p>This table consists of the measurements, metrics of facts of facts of a song play

 **songplays** - records in log data associated with song plays i.e. records with page `NextSong`

```
songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent
```

#### Dimension Tables
<p>This table consists of the users, songs and artists

**users**  - users in the app
```
user_id, first_name, last_name, gender, level
```
**songs**  - songs in music database
```
song_id, title, artist_id, year, duration
```
**artists**  - artists in music database
```
artist_id, name, location, latitude, longitude
```
**time**  - timestamps of records in  **songplays**  broken down into specific units
```
start_time, hour, day, week, month, year, weekday
```

## Project Files

```sql_queries.py``` -> contains sql queries for dropping, creating fact and dimension tables and insertion query template.

```create_tables.py``` -> contains code for setting up the environment, also creates the **sparkifydb** database and we will use for our schema

```etl.ipynb``` -> a jupyter notebook to analyse we will use to explore the dataset before loading. 

```etl.py``` -> read and process **song_data** and **log_data** and loads them into our tables (based on ETL notebook)

```test.ipynb``` -> a notebook to connect to postgres db and validate the data loaded.


## How to run

The ```create_tables.py``` as below:
```
python create_tables.py 
```

The ```etl.py``` as below:
```
python etl.py 
```

## Analysis your data with ```test.ipynb``` file
<p>Execute this file to see the results. In the end of the code you can find this analysis:
  
Exist just one row with the information of song id and artist id this is not good to our analytics team because we have around `6800` records in our data set and it is impossible to `understand which artists and songs the user was listening`.

However, we can provide some statistics to then:
We have `96` different users, around `80%` of the listens are free users and over than `50%` are women
About the artists, we have `69` different artists with `71` songs released between `1961` and `2008`.

