---
title: Using Spotify’s Spotipy for Exploratory Data Analysis.
description: 
published: true
date: 2023-04-21T10:49:32.818Z
tags: spotify
editor: markdown
dateCreated: 2023-04-21T10:49:28.812Z
---

# Using Spotify’s Spotipy for Exploratory Data Analysis.

> An article describing how to use Spotify’s Web API, Spotipy for data analysis.

## INTRODUCTION.
About a month ago, after completing a [project](https://github.com/Neto-A/Zentel-Analysis) on Zentel Network Service Center’s tickets, I stumbled upon an eye-catching [Medium](https://medium.com/dev-genius/spotify-data-analysis-with-python-a727542beaa7) article. This article discussed using Spotify’s lightweight API, Spotipy for data wrangling and exploratory data analysis.

<img src="https://miro.medium.com/max/1352/1*ffOOut5qhD_SGGPARB3XqA.webp">

Being a huge music fan, I was immediately intrigued by this API and decided to create my own project. I opted to work with a playlist, Top Tracks of 2022- Nigeria, curated by Spotify. It is a collection of the 50 songs that Nigerian consumers listened to the most, as the name implies.

I will be guiding you on the process and flow taken to complete this project, as follows:

- Data Wrangling: Gathering, Assessing and Cleaning
- Data Visualization and Inferences

Before we begin, come ku lo sa and listen to the playlist here. And here we go!

## DATA WRANGLING.
In this project, I performed data wrangling using Python. In my previous [article](https://medium.com/@neto_a/zentel-network-service-center-an-analysis-of-ticket-data-8802c955aeb2), I described data wrangling as all the steps taken in gathering and transforming raw data to prepare it for analysis. The first step in every wrangling process is to actually gather the data.

To use Spotipy for gathering data, I needed to obtain client credentials: Client ID and Client Secret (both of which are kept hidden). To get these, follow these steps:

- Sign up for or sign into (as your case may be) the Spotify for Developers [portal](https://developer.spotify.com/). You can sign in normally if you already have a Spotify account.
- Click on Dashboard.
- Create an app for your project and write a brief description.
- You will then be given your credentials.

After getting them, I installed and imported all necessary libraries, the most important being Spotipy itself and SpotifyClientCredentials from spotify.oauth2. Using the required keys, I got authorized using the [code](https://blog.devgenius.io/spotify-data-analysis-with-python-a727542beaa7) in the block below.

It is important to note that while trying to scrape the data, I kept running into connection time out issues. So I included a solution obtained from [here](https://stackoverflow.com/questions/57994570/readtimeout-httpsconnectionpoolhost-port-443-read-timed-out-read-timeo) to rectify that.

Finally, I used two libraries that were new to me, Image from PIL and rembg to remove the background of one of the pictures I used in my PowerBI visualization.

```python
#importing libraries
import spotipy 
from spotipy.oauth2 import SpotifyClientCredentials

#use client keys here
client_id = 'HIDDEN'
client_secret = 'HIDDEN'

client_credentials_manager = SpotifyClientCredentials(client_id= client_id, client_secret=client_secret)
sp = spotipy.Spotify(client_credentials_manager=client_credentials_manager)

import pandas as pd

import time

import  json

#connection timeout correction
import urllib3, socket
from urllib3.connection import HTTPConnection
    
HTTPConnection.default_socket_options = (HTTPConnection.default_socket_options + [
    (socket.SOL_SOCKET, socket.SO_SNDBUF, 1000000), #1MB in byte
            (socket.SOL_SOCKET, socket.SO_RCVBUF, 1000000)
        ])

#image background
from PIL import Image
import rembg
```

Next, scrape the API to get the data. I created two functions: getTrackID to get the unique track IDs for each track in the playlist and getTrackInfo to get basic information about the tracks using their IDs. The information I needed were name, album, artist, release date, duration (which I converted from milliseconds to minutes) and track popularity.

```python
# function to scrape track ids
def getTrackID(playlist_id):
    track_ids=[]
    play_list = sp.playlist(playlist_id)
    for item in play_list['tracks']['items']:
        track=item['track']
        track_ids.append(track['id'])
    return track_ids

track_ids = getTrackID('37i9dQZF1DX1W96TRrqJX4')

# function to scrape track information
def getTrackInfo(track_id):
    meta = sp.track(track_id)
    track_details = {'name' : meta['name'], 'album':meta['album']['name'], 'artist':meta['album']['artists'][0]['name'],
                'release_date':meta['album']['release_date'], 'duration':round((meta['duration_ms']*0.001)/60.0, 2),
                    'popularity':meta['popularity']}
    return track_details
```

Afterwards, I appended this data into a dictionary, track_list, and read it into a dataframe, tracks_df.

```python
#create a data dictionary 
track_list =[]
for i in range(len(track_ids)):
    time.sleep(.5)
    track = getTrackInfo(track_ids[i])
    track_list.append(track)

#create dataframe
tracks_df = pd.DataFrame(track_list, columns = ['name', 'album', 'artist', 'release_date', 'duration', 'popularity'])
tracks_df
```

Then, I saved it into a csv file, tracks.csv, for easy access later. The dataframe looks like this:

<img src="https://miro.medium.com/max/640/1*7lGLg4FtzrxNkbbyMjD9zg.webp">

The track details dataset
Furthermore, I needed the audio features of the tracks. These features are numerical values defined and calculated by Spotify. They include danceability, energy, key etc. They each will be explained afterwards. To get these features, I used the track IDs scraped earlier. I created another data dictionary, track_features, to collect the audio features for each track. I then read these information into a dataframe, feat_df.

```python
# to obtain track features
track_features = []
for id in track_ids:
        features = sp.audio_features(id)
        track_features.append(features)
feat_df = pd.DataFrame(columns = ['danceability', 'energy', 'key', 'loudness', 'mode', 
                                  'speechiness', 'acousticness', 'instrumentalness', 'liveness', 
                                  'valence', 'tempo', 'type', 'id', 'uri', 'track_href', 'analysis_url', 
                                  'duration_ms', 'time_signature'])
for item in track_features:
      for feat in item:
        feat_df = feat_df.append(feat, ignore_index=True)
feat_df.head()
```

Finally, I saved this information into a csv file, track_features.csv, also for easy access later. The dataframe looks like this:

<img src="https://miro.medium.com/max/1400/1*9fdHXspt04NoBbQ29UuKXA.webp">

*The track features dataset*


Now, I had to merge these dataframes together to create my actual dataset. This part was tricky because there were no common columns between them. So I created a new column in each of them, ‘one’. Using this, I was able to create the master dataset, full_tracklist, using inner merge while keeping both the right and left indexes as True.

```python
full_tracklist = pd.merge(tracks_df, feat_df, how='inner', left_index=True, right_index=True)
full_tracklist
```

This merge, however, led to two columns, one_x and one_y, from each of the dataframes getting included in the dataset. I then dropped these columns and saved the dataframe as a csv file, track_list.csv. This dataset would be used for assessing and cleaning.

<img src="https://miro.medium.com/max/1400/1*J3fcN1djC5zJae-CbzNaNg.webp">

*A section of the master dataset*

Before moving on to the second stage of wrangling, assessing, I posed these questions to be answered by my EDA:

1. What are the most popular songs overall?
1. In what month were most of these songs released?
1. How did certain audio features influence popularity?

This was important as it guided assessment of the data. It was pretty straightforward and I noted the following issues with my dataset:

- Datatype changes to be made: release_date to datetime and time_signature to string.
- Extraneous columns: type, uri, track_href, analysis_uri, duration_ms to be dropped.

Stage Three of wrangling, cleaning, was up next. As I like to do, I made a copy of the dataset. This copy was cleaned, to preserve the original dataset. All I did to clean the dataset was to tackle the issues as listed above. I did this using the Define-Code-Test system: define the problem, write the code to fix the problem, test your solution.

The cleaned dataset was saved into a csv file, track_cleaned.csv, ready for visualization.

<img src="https://miro.medium.com/max/640/1*cVFNJ788p9_s_QibLc4QjA.webp">

*The cleaned column list (above) and a section of the dataset (below)*

<img src="https://miro.medium.com/max/1400/1*RDIXLXeMoAaIRWqx8X0mBA.webp">

Wrangling is generally an iterative process, meaning that you can always go back to refine work done. Recall that I stated at the beginning of this article that I used two libraries to remove the background of a picture I wanted to use in my visualization: Image from PIL and rembg. This is a form of gathering.

I used this [code](https://dev.to/0xkoji/only-7-lines-python-code-to-remove-background-with-rembg-4g74) to carry out the task:

```python
#import image
img = Image.open('/Users/netoa/Downloads/Asake_2.webp')
img

#remove background
img_bg_removed = rembg.remove(img)
img_bg_removed

#save image
img_bg_removed.save('/Users/netoa/Downloads/asake_final.png')
```

Et voilà!

<img src="https://miro.medium.com/max/640/1*IKu1fCaXNUgSU9TfGaM7Sw.webp">

*Before*

<img src="https://miro.medium.com/max/640/1*1QJW0houmFhSfP-mrTpUmg.webp">

*After*

## DATASET DESCRIPTION.
Now that I have the cleaned dataset down, I will describe some of the 19 features for easier understanding.

1. Name: The name of the track.
1. Artist: The name of the performing artist.
1. Duration: The duration of the track in minutes, originally in milliseconds.
1. Popularity: The score assigned to each track based on its total streams, how recently and how frequently the song has been played.
1. Danceability: Danceability describes how suitable a track is for dancing based on a combination of musical elements including tempo, rhythm stability, beat strength, and overall regularity. A value of 0.0 is least danceable and 1.0 is most danceable.
1. Energy: Energy is a measure from 0.0 to 1.0 and represents a perceptual measure of intensity and activity. Typically, energetic tracks feel fast, loud, and noisy. For example, death metal has high energy, while a Bach prelude scores low on the scale. Perceptual features contributing to this attribute include dynamic range, perceived loudness, timbre, onset rate, and general entropy.
1. Speechiness: Speechiness detects the presence of spoken words in a track. The more exclusively speech-like the recording (e.g. talk show, audio book, poetry), the closer to 1.0 the attribute value.
1. Acousticness: A confidence measure from 0.0 to 1.0 of whether the track is acoustic. 1.0 represents high confidence the track is acoustic.
1. ID: The Spotify ID for the track.

> Descriptions of most of the columns were obtained from Spotipy’s [documentation](https://developer.spotify.com/documentation/web-api/reference/#/operations/get-audio-features). The others can be found in the [repository](https://github.com/Neto-A/Spotify-Top-Tracks-NG-2022) containing the project.
{.is-info}


## DATA VISUALIZATION.

<img src="https://miro.medium.com/max/1400/1*PdYHG7FKVB7lWjqsq1CXog.webp">

*The Overview page of my report.*

I visualized the data using Microsoft PowerBI. The report is divided into three parts:

- **Overview**: This part contains a general outlook of the playlist; the number of songs in the playlist, unique artistes, average length of the tracks (in minutes). It also includes the number of likes the playlist had received as at project completion, which refers to how many users added the playlist to their libraries. Finally, it contains the most popular songs by popularity scores, the artiste with the most songs in the playlist, the monthly trends in song release dates and the distribution of time signatures.
- **Audio Features**: This section purely describes how certain features influenced the popularity of the songs. It was going to be excessive to include all the features gathered. So, for the purpose of this project, I chose Duration, Acousticness, Speechiness, Energy, Danceability and Valence.
- **Inferences**: This simply contains the summary of all insights gained.

> There are information buttons beside the charts to explain them, as the figures generally have no units.
> 
{.is-info}

Here are some of the insights gained from the visualization:

- 8 of the songs that made the list were released in September 2022, the highest of all the release months.
- Songs with a speechiness of 0.15 (low speechiness) were the most preferred. This means that songs made up primarily of beats and instrumentals were greatly preferred.
- Nigerians enjoyed songs with a midrange energy (0.65), i.e., moderately fast, loud and intense.
- Songs with a mid-upper (0.6–0.75) valence were most preferred. Therefore, Nigerians enjoyed songs with some level of positivity to them.

> All other inferences are listed in this complete interactive [report](https://app.powerbi.com/view?r=eyJrIjoiMDAwMTBmNWItOWYwMy00NmE1LWFjMWYtOTdhNjY0NWM0MTUzIiwidCI6ImEzMjNmYmMzLTM3NzUtNDNhMi05MWYxLTA4YWY1ZTA1MTVhZSJ9).
{.is-info}


## FINAL WORDS.
Although this project was tricky and tasking, I thoroughly enjoyed working on it. I learned a lot, from using new libraries on Python to unlocking certain features on PowerBI. It gave me a decent insight into what exactly Nigerians enjoy and look out for in good music. I hope it helped you too, dear reader.

As always, I am open to questions and corrections on this project and collaborations on future projects.

source : https://medium.com/@neto_a/using-spotifys-spotipy-for-exploratory-data-analysis-84ecbbb24887