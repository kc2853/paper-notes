# Twitter mood predicts the stock market (2010, Bollen et al.)

## Background
* While Efficient Market Hypothesis (EMH) and random walk theory refer to how stock prices are largely driven by *new* information (i.e. news) which are unpredictable, EMH is losing its relevance, as multiple types of alternative data (e.g. online chat activity, Google search queries, blog sentiments, etc.) have indeed been used to make meaningful predictions.
* Indirect sources of public mood: results of soccer games, weather conditions
* Rise of Twitter

## How tweets were analyzed
* OpinionFinder (OF): binary value signifying positive vs negative mood
* Google Profile of Mood States (GPOMS): 6-dimensional value comprising Calm, Alert, Sure, Vital, Kind, Happy
* Hence a total of 7 dimensions were considered per tweet
* Feb. 28 - Dec. 19 of 2008: 9,853,498 tweets posted by 2.7 million users
* Removed: punctuations, uppercase, spam (if links detected)

## How association was measured
* Those 7-dimensional mood values to daily closing values of Dow Jones Industrial Average (DJIA)
* Two approaches (linear vs nonlinear): 1. Granger causality analysis (linear); 2. Self-organizing Fuzzy Neural Network (5-layer SOFNN)
* ![Overview](/images/twitter_overview.png)
* Granger: ![Granger](/images/twitter_L.png)
* Note that the above L_1 was the "baseline" prediction such that the null hypothesis is that L_1 ~= L_2

## Notable results
* Reject the above null hypothesis as per (for Granger): ![Table 2](/images/twitter_table2.png)
* Observe (for SOFNN): ![Table 3](/images/twitter_table3.png)
* Calm correlates most with DJIA with lag days 2-5
* OF's metric (binary positive vs negative) doesn't do much
* Interestingly, for our nonlinear association, we note that the nonlinear combination of Calm and Happy actually yielded the least MAPE (Mean Absolute Percentage Error) whereas Happy (by itself) yielded a poor association as per Granger causality analysis.
* ![FR](/images/twitter_FR.png)
* In other words (somehow): Calm (linearly associated with DJIA) + Happy (linearly not associated with DJIA) > Calm -- when nonlinearly evaluated

## Questions/Comments
* The paper excluded Presidential Election and Thanksgiving seasons for the purpose of Granger causality analysis, but maybe they shouldn't?
* It seems "sentiment analysis" here only involved frequencies of positive and negative words from a certain lexicon per tweet -- which may not be the best way to conduct sentiment analysis?
* Instead of SOFNN, what would happen with more recent deep learning techniques?
* It would be interesting to see what happens if the "baseline" case when conducting Granger causality analysis could be a bit more varied and sophisticated.
