# Documentation NabuMinds Challenge - Thiago Sequeira

## Topics
- Data Source & Structure
- Data Cleaning
- Data Visualization
  - Tableau
  - Power BI
- What I would add if I had more time

## Data Source & Structure
**Data source name:** Tweets.csv
| Column | Description |
| --- | --- |
| tweet_id | Unique ID of Tweet |
| airline_sentiment | Sentiment of Tweet |
| airline | Airline name |
| airline_sentiment_confidence |  |
| negativereason | Negative reason of Tweet |
| negativereason_confidence |  |
| airline_sentiment_gold | Null column |
| negativereason_gold | Null column |
| retweet_count | How many retweets has the Tweet |
| tweet_coord | Coords of Tweet |
| tweet_created | Date of Tweet |
| tweet_location | Location that user configured in its profile |
| user_timezone | Time zone user tweeted |

## Data Cleaning
I decided to implement data cleaning using Python (Pandas, Numpy), because I noticed the column `tweet_location` had incorrect and unclean information.

Has I mention in my `.ipynb` file, my first option was implementing `dataprep` library, using `clean_country()` function to clean this column. But I faced some unexpected issues trying to download the library on my pc. That's why I decided to implement dictionaries with AI help. 

I also noticed that I also can get the location of the tweet by `user_timezone` column, and it has clean data. That's why I sent the list of `user_timezone` and `tweet_location` to the AI and I specified that if it belongs to any city, county, state, province, location or time zone, I need it to return the country of origin. Else, it returns null.

I set `user_timezone` and `tweet_location` to be lowercase like the dictionaries and implement them. I create a column called `normalized_location` from `user_timezone` normalized data, then I fill the null cells by `tweet_location` that I implemented its dictionary before.

```
df['tweet_location'] = df['tweet_location'].str.lower()
df['tweet_location_normalized'] = df['tweet_location'].map(tweet_location_dic)

df['normalized_location'] = df['user_timezone'].map(user_timezone_dic)
df['normalized_location'] = df['normalized_location'].fillna(df['tweet_location_normalized'])

df['tweet_location'] = df['normalized_location']

df.drop(columns=['normalized_location', 'tweet_location_normalized'], inplace=True)
```

After the data cleaning, I write the output as "output_tweets.csv" and use it for the visualizations in Tableau and Power BI.

## Data Visualization
### Tableau
| Measure Name | Description |
| --- | --- |
| # Negatives | Count of negatives tweets |
| # Neutrals | Count of neutrals tweets |
| # Positives | Count of positives tweets |
| # Tweets | Count of total tweets |
| % of Total | Ratio of tweets to total tweets |
| KPI Flag Negative Bar | Conditional to calculate Max and Min (negatives) |
| KPI Flag Neutral Bar | Conditional to calculate Max and Min (neutrals) |
| KPI Flag Positive Bar | Conditional to calculate Max and Min (positives) |
| Min/Max | Conditional to calculate Max and Min (total) |

- **KPIs Visual:** I use `# Negatives`, `# Neutrals `, `# Positives` to show total of tweets, and % of total of each sentiment. Also, in a line chart by `tweet_created`.
- **Sentiments by Airline:** I use `airline` in x and `# Tweets` in y, by `airline_sentiment`.
- **Sentiments by Negative Reason:** I use `negativereason` in x and `# Tweets` in y. Also, `% of Total` for more insights.
- **Tweets by Location:** I use `# Tweets` for values and `tweet_location` for the country location. (using the normalized data from python)
- **Tweets by Date:** I use `tweet_created` in x and `# Tweets` in y. Also, `Min/Max` for more insights.

### Power BI
| Measure Name | Description |
| --- | --- |
| # Negatives | Count of negatives tweets |
| # Neutrals | Count of neutrals tweets |
| # Positives | Count of positives tweets |
| # Tweets | Count of total tweets |
| % Negatives | Percentage of negatives tweets |
| % Neutrals | Percentage of neutrals tweets |
| % Positives | Percentage of positives tweets |
| Update date | Date of refresh |

- **KPIs Visual:** I use `# Negatives`, `# Neutrals `, `# Positives` to show total of tweets, and % of total of each sentiment.
- **Sentiments by Airline:** I use `airline` in x and `# Tweets` in y, by `airline_sentiment`.
- **Sentiments by Negative Reason:** I use `negativereason` in x and `# Tweets` in y.
- **Tweets by Location:** I use `# Tweets` for values and `tweet_location` for the country location. (using the normalized data from python)
- **Tweets by Date:** I use `tweet_created` in x and `# Tweets` in y. Also, `Min/Max` for more insights.

## What I would add if I had more time

