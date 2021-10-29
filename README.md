# Spotify Recommender Model

The goal of this project is to build a Spotify recommender engine based on my own music streaming playback history. In a previous project, I had developed an ETL job to pull my music listening history every 24 hours (https://github.com/vijay-ss/spotify-ETL). I will leverage this live data stream to build a predictive ML model and provide recommendations on songs which may interest me.

## Data Sources
As stated in the intro, I have used the Spotify API to pull my listening history. The API is very intuitive and allowed my to pull each track as well as the details on each song such as the genre and individual song features. I will also use a Kaggle data set as testing data in order to apply my model and evaluate the predictions.

- streaming_history: a local SQL table which contains each track listened to
- genres: a local SQL table which contains the genre information from each track of the streaming_history table
- song_features: a local SQL table which contains song attributed for each song in the streaming_history table
- SpotifyFeatures.csv: a dataset downloaded from Kaggle which will be used for testing the model on unseen data. (https://www.kaggle.com/zaheenhamidani/ultimate-spotify-tracks-db)

SpotifyFeatures.csv has 232,725 tracks with 26 genres, so approximately 10,000 tracks per genre. This is an important note, as I tend to listen to multiple genres.

The song_features table contains key attributes which will be used to train the ML model:

### Numerical columns:
- acousticness: The relative metric of the track being acoustic, (Ranges from 0 to 1)
- danceability: The relative measurement of the track being danceable, (Ranges from 0 to 1)
 -energy: The energy of the track, (Ranges from 0 to 1)
 -duration_ms: The length of the track in milliseconds (ms), (Integer typically ranging from 200k to 300k)
- instrumentalness: The relative ratio of the track being instrumental, (Ranges from 0 to 1)
- valence: The positiveness of the track, (Ranges from 0 to 1)
- popularity: The popularity of the song lately, default country = US, (Ranges from 0 to 100)
- tempo:The tempo of the track in Beat Per Minute (BPM), (Float typically ranging from 50 to 150)
- liveness: The relative duration of the track sounding as a live performance, (Ranges from 0 to 1)
- loudness: Relative loudness of the track in decibel (dB), (Float typically ranging from -60 to 0)
- speechiness: The relative length of the track containing any kind of human voice, (Ranges from 0 to 1)
- year: The release year of track, (Ranges from 1921 to 2020)
- id: The primary identifier for the track, generated by Spotify

### Categorical columns:
- key: The primary key of the track encoded as integers in between 0 and 11 (starting on C as 0, C# as 1 and so on…)
- artists: The list of artists credited for production of the track
- release_date: Date of release mostly in yyyy-mm-dd format, however precision of date may vary
- name: The title of the track
- mode: The binary value representing whether the track starts with a major (1) chord progression or a minor (0)
- explicit: The binary value whether the track contains explicit content or not, (0 = No explicit content, 1 = Explicit content)


## Prediction Target Variable
The target variable used to make the prediction will be whether the song is one of my 'favourite' tracks. The majority of tracks were played less than 10 times (frequency). On this basis, I can consider a playback count of >10 times to be one of my more favourite songs.

![](images/frequency.png)

## Feature Engineering

As we can see below, the ratio of favourite tracks to non-favourite tracks is heavily imbalanced, so I opted oversample (SMOTE) the minority class to provide more balance. This will prevent the ML model from predicting most songs as the majority class (i.e. non-favourite).

- Non-Favourite: 232,670 songs
- Favourite: 55 songs


## Model Building
In order to find the most accurate method of predicting future songs, I compared 3 ML models for classification:
- Logistic Regression
- Decision Tree
- Random Forest

## Results
Based on the results below, it was concluded that the Random Forest provides the most accurate predictions.

- Logistic Regression F1 Score: 80.55%
- Decision Tree F1 Score: 99.74%
- Random Forest F1 Score: 99.94%

## Conclusion

On an anecdotal note, the model seems to be relatively accurate, as the top predictions are weighted into genres which I typically listen to (Rap, Hip-Hop, Dance). However there are a few genres which seem questionable such as Children's Music, Country and Folk. Perhaps the song features from thos particular songs match those from the training data. The only way to find out is to give the tracks a listen!

Update: It turns out Linkin Park is classified as Children's Music :P

![](images/prediction_genres.png)
