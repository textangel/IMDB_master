Required Python Version: 2.7

The files in this project perform multiple levels of analysis on the IMDB/TMDB datasets. 
Source:
IMDB Dataset: https://data.world/popculture/imdb-5000-movie-dataset
TMDB Dataset: https://www.kaggle.com/tmdb/tmdb-movie-metadata

Each dataset contains 5000 movies, which when joined retain about ~4500 movies. We consider only movies that have buget of over USD1 million for the purpose of this analysis, retaining ~2500 movies.

The dataset is analyzed for trends and genres. Drivers and correlations of movie success are explored. Lingusistic features are gathered from the keywords provided in the data. Both language models trained on these keywords and publicly available language models are used to analyze the impact of keywords on movie success. Social-Network Features are generated from the cooperation network of actors defined by Actors who starred in the same movie. Other features are generated based on data description. We use many generated features (some latent, some numeric, some discrete categorical) to train several predictive models for log movie return rate and for IMDB score.

Feature selection is preformed using LASSO on L1 loss, resulting in around half of the features selected for each model. Generated Categorical variables seem less likely to be selected as compared to numeric features; Some features selected are surprising.

We try RandomForest and SVM Regression on each prediction problem, optimizing hyperparameters with GridSearch. RandomForest seems to perform better than SVM Regression in all cases.

We choose log return rate as our target variable as it does not seem trivially correlated with budget, profit, and so on in a simple fashion, and thus is a difficult and interesting feture to predict for. We were able to describe well over half the variance of this feature (R2 .602) in a RandomForest model taking into account features such as Genres, Keywords, Actor PageRank, but no numerical financial or popularity information about budget, profit, facebook likes and so forth. Adding in the financial information we achieved R2 of 0.63.

We also ran the model to predict for IMDB score. Currently, without using any other popularity or finacial based features (only using features for PageRank, keywords, genre and so on) we achiece R2 of 0.374, and work is underway to improving the model.

This project provides many interesting insights into the interconnections between movies, and is the foundational baseline for understanding what drives movie success.

# Files:
 - '''Data Modeling and Analysis.ipynb''': The main file in this project. In Main, we produce the main data cleaning, exploration, and analysis. All other files depend on the data prepared by this file. We present many functions to preform data analysis of the IMDB data at many levels.
 
  - '''Model Training.ipynb''': The main file in this project. In Main, we produce the main data cleaning, exploration, and analysis. We also unify features generated in the other files to train a regression model

 - '''Factors in Film Success.ipynb''': This file breaks down correlates to various factors behind film success as measued in revenue, return rate, imdb score, tmdb popularity, or number of facebook likes. It considers the median effect of factors such as genre, keywords, production company, and movie rating, providing a very detailed description of which values for factors are most correlated with different aspects of movie success.

- '''Actors PageRank.ipynb''': Implements the Google PageRank algorithm, generating a PageRank value for every actor weighted by a particular column in the actors database. PageRank values are used to compare actors afainst each other and are also used in our final model for film success.

- '''Keyword Processing.ipynb''': Implements two independent functions. First, implements Word2Vec on provided movie keywords as well as keywords generated by from standardized and tokenized movie overviews provided by IMDB, creating several IMDB-specific keyword embedding models that capture correlation between keywords. Secondly, provides the functinoality to use these embeddings, as well as a public pre-trained GLoVE word embedding to classify movies according to keywords. The actual usage of these embeddings are in '''Main.py''', where we use them to generate document embeddings for each movie using the weighted tf-idf word average approach. The document embeddings are used to classify movies and used in our final predictive model.

- '''Interactive Console.ipynb''': Implements a console that queries the user for a movie genre and a target variable with a percentile, and optionally a budget range. Outputs a sample movie for the user closest to the percentile for the target variable within the budget range. We may extend this using keyword features to include a 'movie most like' feature.

- '''Data''': Folder for the storage and backup of data tables, language models, and other kinds of data.


v 0.0.1
