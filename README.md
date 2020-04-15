# Project 1: Data modeling with Postgres

## Introduction

This is part of the Udacity Data Engineering nanodegree for data engineering. In this project I created postgres database `sparkifydb` for a music app, *Sparkify*. I was provided with folders containing JSON files that hold song data (artist, song, year, etc) and log data (user, starttime, song played). The goal of this exercise is to apply the ETL process and create a database with start schema that can handle queries for song play analysis. I used a combination of SQL and Python to extract the data from the JSON files, transform the if needed, create a postgresql database with the relevant tables, and write some queries in a jupyter notebook to do some basic analysis of the data.

## Data

### Song Dataset

The first dataset is a subset of real data from the [Million Song Dataset](https://labrosa.ee.columbia.edu/millionsong/). Each JSON file contains data about one song, the JSON files are odered in folders acording to the first three letters of the track ID. i.e. data/song_data/A/B/C/TRABCEI128F424C983.json. The content of each file looks like this:

<pre>
{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}
</pre>

### Log Dataset

This data set also is a collection JSON files. These represent log data (https://github.com/Interana/eventsim). Each file contains several logs and the files are ordered in a directory divided by day. The file path looks like this:

<pre>
data/log_data/2018/11/2018-11-12-events.json
</pre>



![log-data](./img/log-data.png)

If you would like to look at the json data within log_data files, you will need to create a Pandas df to read the data. Remember to first import _json_ and _pandas_ libraries.

```python
df = pd.read_json(filepath, lines=True)
```

> Note: Make sure to set `lines= True` so that each line of the JSON file is read as a new row.

For e.g., `df = pd.read_json('data/log_data/2018/11/2018-11-01-events.json', lines=True)` would read the data file _2018-11-01-events.json_

Here is a helpful [video](https://www.youtube.com/watch?v=hO2CayzZBoA) from **Udacity's Data Wrangling course** talking about the JSON files format.

<a id="schema"></a>

## Schema for Song Play Analysis

Using the song and log datasets, we'll create a star schema optimized for queries
on song play analysis. This includes the following tables.

<a id="fact"></a>

## Schema

Starschema with songplay data as fact table and 4 dimension tables: 
- d_artist: dimension table that hold descriptors for the artist
- d_time: dimension table that hold descriptors for time dimension
- d_song: dimension table that hold descriptors for the song
- d_user: dimension table that hold descriptors for the user

![](erd_db_sparkify.png?raw=true)

## ETL

- **etl.py** gets data from `data/song_data` and transform the data and writes it to **d_songs** and **d_artists** tables.
- **etl.py** gets data from `data/log_data` and transform the data and writes it to **d_time and **d_time** tables.
- Next a **SELECT** query was created that retrieves data song and artist data from the **d_song and d_artist table**. This data is writen to the **f_songplays** table

## Files

<pre>
.
├── create_tables.py------# Drops and creates tables. Running this file resets
|                           the tables before each time we run ETL scripts.
├── etl.ipynb-------------# Reads and processes a single file from song_data and
|                           log_data and loads the data into our tables.
├── etl.py----------------# Reads and processes files from song_data and
|                           log_data and loads them into our tables. We'll fill
|                           this based on ETL notebook.
├── sql_queries.py--------# Contains all our SQL queries, and is imported into
|                           the last three files above.
└── test.ipynb------------# Displays the first few rows of each table to let us
                            check our database.
</pre>


