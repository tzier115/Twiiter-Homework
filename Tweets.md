

```python
import tweepy
import json
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd

#Vader
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
analyzer = SentimentIntensityAnalyzer()

#Twitter API Keys
consumer_key = "gGJeIAmtDHEhn6J0ceJ0n9FSd"
consumer_secret = "GyCJnSp7Lxd2fTg9APGl5frkj1QkzQBwOtim0WNiC0btumfKRJ"
access_token = "922954914761977856-dxkGnqYCUlabtzj98ibbdAKz9EBSU8X"
access_token_secret = "kF9pZBKi42wN188kmZwYZYCoZxCK7Yep958XaBAoQN5e1"

#Tweepy Authenitcation
auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth, parser=tweepy.parsers.JSONParser())
```


```python
# Target Account
target_terms = ("@BBC", "@CBS", "@CNN",
                "@FoxNews", "@nytimes")

counter = 1

# Variables for holding sentiments
sentiments = []

for target in target_terms:

    # Loop through 5 pages of tweets (total 100 tweets)
    for x in range(5):

        # Get all tweets from home feed
        public_tweets = api.user_timeline(target)

        # Loop through all tweets 
        for tweet in public_tweets:
        
            # Run Vader Analysis on each tweet
                compound = analyzer.polarity_scores(tweet["text"])["compound"]
                pos = analyzer.polarity_scores(tweet["text"])["pos"]
                neu = analyzer.polarity_scores(tweet["text"])["neu"]
                neg = analyzer.polarity_scores(tweet["text"])["neg"]
                tweets_ago = counter
                
        # Add sentiments for each tweet into an array
                sentiments.append({"Date": tweet["created_at"], 
                            "Text" : tweet["text"],
                           "Compound": compound,
                           "Positive": pos,
                           "Negative": neu,
                           "Neutral": neg,
                            "Account": target,
                           "Tweets Ago": counter})
                
                counter = counter + 1

```


```python
# Convert sentiments to DataFrame
sentiments_pd = pd.DataFrame.from_dict(sentiments)
sentiments_pd.head(10)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Account</th>
      <th>Compound</th>
      <th>Date</th>
      <th>Negative</th>
      <th>Neutral</th>
      <th>Positive</th>
      <th>Text</th>
      <th>Tweets Ago</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>@BBC</td>
      <td>0.4877</td>
      <td>Sat Nov 04 15:32:06 +0000 2017</td>
      <td>0.843</td>
      <td>0.000</td>
      <td>0.157</td>
      <td>Amy Winehouse, @Adele, @LoyleCarner &amp;amp; @Ell...</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>@BBC</td>
      <td>0.0000</td>
      <td>Sat Nov 04 15:00:12 +0000 2017</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>Trucker culture is dying with automation – and...</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>@BBC</td>
      <td>0.5994</td>
      <td>Sat Nov 04 14:53:43 +0000 2017</td>
      <td>0.837</td>
      <td>0.000</td>
      <td>0.163</td>
      <td>RT @BBCMOTD: Peter Crouch = super sub.\n\nHis ...</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>@BBC</td>
      <td>0.0000</td>
      <td>Sat Nov 04 14:53:11 +0000 2017</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>RT @BBCSport: FT England 29-10 Lebanon\n\nGet ...</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>@BBC</td>
      <td>0.0000</td>
      <td>Sat Nov 04 14:52:17 +0000 2017</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>RT @BBCSport: Felipe Massa will retire from #F...</td>
      <td>5</td>
    </tr>
    <tr>
      <th>5</th>
      <td>@BBC</td>
      <td>0.4588</td>
      <td>Sat Nov 04 14:44:58 +0000 2017</td>
      <td>0.857</td>
      <td>0.000</td>
      <td>0.143</td>
      <td>RT @BBCOne: Welcome to the family business. \n...</td>
      <td>6</td>
    </tr>
    <tr>
      <th>6</th>
      <td>@BBC</td>
      <td>0.0000</td>
      <td>Sat Nov 04 14:26:38 +0000 2017</td>
      <td>1.000</td>
      <td>0.000</td>
      <td>0.000</td>
      <td>RT @bbcthree: This barber reminds us how small...</td>
      <td>7</td>
    </tr>
    <tr>
      <th>7</th>
      <td>@BBC</td>
      <td>0.6369</td>
      <td>Sat Nov 04 14:26:06 +0000 2017</td>
      <td>0.846</td>
      <td>0.000</td>
      <td>0.154</td>
      <td>Did you know there’s a best time to eat, think...</td>
      <td>8</td>
    </tr>
    <tr>
      <th>8</th>
      <td>@BBC</td>
      <td>-0.6249</td>
      <td>Sat Nov 04 14:03:04 +0000 2017</td>
      <td>0.638</td>
      <td>0.362</td>
      <td>0.000</td>
      <td>Police discover a suspected 'WW2 bomb' is actu...</td>
      <td>9</td>
    </tr>
    <tr>
      <th>9</th>
      <td>@BBC</td>
      <td>-0.3612</td>
      <td>Sat Nov 04 13:30:07 +0000 2017</td>
      <td>0.878</td>
      <td>0.122</td>
      <td>0.000</td>
      <td>'We have our stories, struggles &amp;amp; power. W...</td>
      <td>10</td>
    </tr>
  </tbody>
</table>
</div>




```python
sentiments_pd.to_csv("Tweets.csv", index=False)
```


```python
plt.plot(np.arange(len(sentiments_pd["Compound"])),
        sentiments_pd["Compound"], marker="o", linewidth=0.5,
        alpha=0.8)

plt.title("Sentiment Analysis 11/4/17")
plt.grid(True)
plt.ylabel("Tweets Polarity")
plt.xlabel("Tweets Ago")
plt.savefig("Sentiments.png")
plt.show()
```


![png](output_4_0.png)



```python
accounts = sentiments_pd.groupby(["Account"])
mean = accounts['Compound'].mean()
mean.head()
```




    Account
    @BBC        0.112850
    @CBS        0.332260
    @CNN       -0.058275
    @FoxNews   -0.136865
    @nytimes   -0.018400
    Name: Compound, dtype: float64




```python
mean = pd.DataFrame(mean)
mean.reset_index(inplace=True)
mean
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Account</th>
      <th>Compound</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>@BBC</td>
      <td>0.112850</td>
    </tr>
    <tr>
      <th>1</th>
      <td>@CBS</td>
      <td>0.332260</td>
    </tr>
    <tr>
      <th>2</th>
      <td>@CNN</td>
      <td>-0.058275</td>
    </tr>
    <tr>
      <th>3</th>
      <td>@FoxNews</td>
      <td>-0.136865</td>
    </tr>
    <tr>
      <th>4</th>
      <td>@nytimes</td>
      <td>-0.018400</td>
    </tr>
  </tbody>
</table>
</div>




```python
my_colors = 'rgbkymc'
mean.plot.bar(x="Account", y="Compound", color= my_colors)
plt.title("Overall Media Sentiments 11/4/17")
plt.xlabel("Account")
plt.ylabel("Tweet Polarity")
plt.grid(True)
plt.savefig("BarSentiments.png")
plt.show()
```


![png](output_7_0.png)



```python
#Trends
#1. Most tweets are relatively neutral
#2. The overall senitments of all tweets is neutral
#3. CBS has the most positive tweets while Fox News has the most negative tweets
```
