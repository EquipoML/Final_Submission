# What will make a song a Spotify hottest hit?
## Finding the best model that predicts Spotify Streams

## CodeOp Final Collab project 
### - Carmen and Nissi   -

Google Slides for the project: [Presentation Slides](https://docs.google.com/presentation/d/1NhWICQGjL1d6bgLBfr4KQ7Z4aK2S-COu8NJYPMR5Ilg/edit?usp=sharing)

--

# Analysis context

## The dataset:

contains an extensive compilation of the top-rated songs from 2023, sourced from Spotify.  Comprising details like track title, artist(s), release date, inclusion in Spotify playlists and charts, streaming metrics, presence on Apple Music and Deezer, as well as rankings on Shazam charts, alongside a range of audio characteristics.

size = 24 variables and 952 records


# Analysis overview 

## EDA and data cleaning
Linear Regression Model to predict most streamed songs based on variables that showed linearity with our target (num streams in Spotify)
More complex model (XGboost) to predict our target
First: adding all variables with correlation bigger than 0.1
Second: adding NLP features from artist_names to the previous model
Main conclusions
Next steps

--

# EDA and data cleaning

## EDA and data cleaning
Identified one row that contains a mistake in our target variable (streams) and dropped it
Created a new feature “released season” from other date related variables
Dropped 2 columns with missing values because they exceeded 5% of total records
Dropped low correlated variables ("bpm", "acousticness_%", "energy_%", "valence_%", "instrumentalness_%", "liveness_%")


“Streams” is mainly high correlated with songs in Spotify playlists, in Apple playlists and in Deezer playlists.

# Linear Regression Model

Our target variable is number of streams in Spotify (“streams”). It was positively (right) skewed, and we decided to use the Interquartile Range method to handle outliers. We went from 952 rows to 878 after this process.
For this model we decided to use as features only those variables that showed a linear relationship with our target (in Spotify playlists, in Apple playlists, in Deezer playlists) +  the categorical variables mode and released season 
We also applied standardization to our data using the StandardScaler method from Sklearn library.

### Linear Regression Model
From the LR model coefficients, we can observe that songs present in Spotify playlists and in Apple playlists explain most of the target variability. Songs that appear on Deezer playlists have a negative relationship with Spotify streams.
Regarding the released season, songs released during Spring have a positive impact on our target, while Winter has a negative effect on it.

### Linear Regression Model

After trying different adjustments and optimization techniques in this model, the highest R squared we got was 0.57.
The RMSE = 0.68
The MAE = 0.48

### First XGboost Model

In this case our criteria for chosen the feature variables was those that showed a correlation with the target higher than 0.1 
We applied standardization before running the model

### First XGboost Model

After trying different adjustments and optimization techniques in this model…
The RMSE = 0.41 (better than the Linear Regression 0.68)

### XGboost Model with NLP

We wanted to find out if the artist name (with its implications) was related with a song success.
To extract the features from the artist name variable we used NLP
We added to the previous model the tokens and the length found in the artist name
We applied standardization before running the model

After trying different adjustments and optimization techniques in this model…
The RMSE = 0.61 (better than the Linear Regression 0.68, but worse than our previous XGboost model)
XGboost Model with NLP

# Main conclusions

The model that we found that could get a more accurate prediction of next hits in Spotify is our second model (XGboost without artist name features in it)
Still the RMSE is a bit high, so we could try to find another model that performs better for this case
..

# Next steps

Running a classification model case to predict if a specific song will be on the 100 top streamed songs or not
Adding new features to our models using NLP on lyrics 
NLP on Lyrics features + PCA
Unsupervised -  Clustering on some variables to find out segmentation

# Notes: Webscrapping

We used LyricsGenius to webscrap data from Genius service (social network)
Its API is imperfect and returned a lot of noise, so we used RegEx to clean up the data
Eventually as the lyrics are multilingual and require much more time to clean up, we decided to leave the sentiment analysis using lyrics for later
One solution would be to run the lyrics through translation service to get all lyrics in one language, at least.
More RegEx will be necessary to filter out the lyrics’ content
https://github.com/johnwmillr/LyricsGenius

# Notes: Extra EDA on Lyrics

Added preliminary EDA on lyrics
Cleanup up lyrics first using outliers when it comes to number of characters and number of lines
Adding identification of lyrics language for future use
One venue can be to translate all lyrics into a common language 
Possibility to apply sentiment analysis on the lyrics associated with the top 20% streamed tracks



