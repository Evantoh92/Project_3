# Reddit Posts Classification


## Problem Statement
There are so many games out there, each one claiming to allow you to experience countless abilities and promising hours and hours of gameplay. You feel left out when your friends are talking about the hottest game. You have no idea what they are talking about. Maybe its the game that was released just last week or maybe its the game that was released yesterday. Just like them you are a geek except the academic kind. Get referred to the right reddit page based on search terms that you secretly eavesdrop from the pro-wannabes discussing their strategies. Watch countless of gameplay videos uploaded by fellow gamers, discover the latest glitches, learn the hottest meta and join in their quest - to dominate.


## Objectives
The goal of this project is to utilize Reddit posts to identify two different games that are popular for a user in a scenario described in the problem statement. The end point is to correctly classify the right game based on search terms and direct the user to the right Reddit page. Two highly popular games, War Frame and Apex Legends are chosen because both games have an active community on the Reddit page to aid in classification training.

## Context
WarFrame is a highly popular massively multiplayer online role-playing game (MMORPG) and third person shooter game that was first launched 2013 (open-beta). It embodies the perfect example of a successful free to play model that is sustainable in keeping the game balanced for players who pay. To date, there are constant updates, new expansions and a wide variety of activities for players to dwell in the seemingly infinite world. 
Reddit link here: <https://www.reddit.com/r/Warframe/>
More information about the game: <https://en.m.wikipedia.org/wiki/Warframe>

On the other hand, Apex Legends is a first person shooter game that was first launched in February 2019. It is a highly anticipated title that contains elements of survival from Player Unknown Battlegrounds(Pubg) and Fortnite, as well as elements of character-based abilities from Overwatch. Despite initial criticisms of an inability to screen hackers, poor 'Season-pass' model that gave paying players boring cosmetic upgrades and lack of fresh updates, it continues to attract many players.
Reddit link here: <https://www.reddit.com/r/apexlegends/>
More information about the game: <https://en.wikipedia.org/wiki/Apex_Legends>


## Methodology
### Data Extraction
Reddit was focused on because of the well-documented API. There is no need to use Scrapy as looping a response get based on the 'next-page' link can be done, followed by extraction of relevant data. For our classification purpose, the title and post content were used for the training. 

### Natural Language Processing (NLP) Techniques

Several models such as Logistic Regression, KNearestNeighbor, all Naive Bayes' models (Bernoulli, Gaussian and Multinomial) and both CountVectorizer and Tfidf-Vectorizer Natural Language Processing (NLP) techniques were used and evaluated upon using confusion matrix.

### Classification Modelling Comparison
The ideal model to be used in this case is *Multinomial* Naive Bayes' model as the columns of X are integer counts. Hence this model is likely to give the highest score (0.93). A fairly close result of false negatives and positives is yielded.

*Bernoulli* functions best when the columns of X are dummy variables/one-hot encoded. It is not very applicable here as we can see from the lower model score.

*Gaussian* functions best when the columns of X are Normally distributed. In this case, it is not very applicable as well. One note is it also gives a fairly close result of false negatives and positives unlike the Bernoulli function where false positives >> false negatives.

*Logistic Regression*
Grid search was not very useful and returned a best penalty of 1.0 (also the default). However, the model score is high. One advantage this model has over Multinomial is that the false negative is noticeably lower than the false positive. Despite having just a slightly lower model score on the test data, this will be the preferred model in cases where false negatives carry significant meaning.

*KNearestNeighbor (KNN)* on its own, the model scored very poorly on the train data and even worse on the test data. There is no change when using grid search to improve the parameters of KNN when using CountVectorizer. However, grid search vastly improved the score of KNN model when using Tfidf-Vectorizer (0.6 to 0.87)

Modelling wise, the train score tended to be slightly lower for TfidfVectorizer than CountVectorizer. But the test score is improved for TfidfVectorizer as compared to the CountVectorizer. This could be due to Tfidf takeing into account the most common words across the posts and giving a low weightage to common words like `just`, `game`, `like` in the classification model. The token pattern is adjusted to include only words with at least 2 characters. All numbers, punctations and symbols are excluded.

Able to use an average of 6800-7000 features for classification.


## Constraints
Max posts count imposed by Reddit per user is 1000 posts per subReddit.

Most posts contain an attention grabbing title like "check out how i got 30 kills!" followed by a short 'highlights' clip without any text. While the lack of text can seem like a disadvantage, having such titles are actually very beneficial for the model. It is short, impactful and likely to be very game specific like character names, items, weapons, abilities and less generic like 'how to be rank 1'. Nevertheless there are sufficient text features used for training.


## Data Dictionary

|Feature|Type|Description|
|---|---|---|
|**title**|*object*|The title of each Reddit Post|
|**text**|*object*|The text associated with each Reddit Post|
|**apexlegends**|*object*|apexlegends is the positive outcome in this scenario|


## Conclusion
There is no difference between a false positive and false negative. A false positive is a post that is predicted to be on Apex Legends and is in fact a WarFrame post. In scenarios where positive outcomes are more serious like detecting cancer, pregnancy or fraud, a false positive is preferred over a false negative. One way to think of this is a false negative is a positive occurence that managed to 'slip' under the radar. Whereas a false positive is negative scenario(usually good) detected as a positive outcome (usually bad and the ones we want to detect). This is a relatively straightforward binary classification case.

## Future Developments
- Deployment, improve web UI for the final end user.
- Include wider range of games.
- Daily updates of posts.
- Increase post count for analysis.
- Stemming, lemmetization.
- Use of ROC AUC.
- Think of more practical use cases.
