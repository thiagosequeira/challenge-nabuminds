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
| airline_sentiment_confidence | % probability of being true |
| negativereason | Negative reason of Tweet |
| negativereason_confidence | % probability of being true |
| airline | Airline name |
| airline_sentiment_gold | Null column |
| name | User @ |
| negativereason_gold | Null column |
| retweet_count | How many retweets has the Tweet |
| text | Tweet text |
| tweet_coord | Coords of Tweet |
| tweet_created | Date of Tweet |
| tweet_location | Location that user configured in its profile |
| user_timezone | Time zone user tweeted |

## Data Cleaning
I decided to implement data cleaning using Python (Pandas), because I noticed the column `tweet_location` had incorrect and unclean information.

As I mentioned in my `.ipynb` file, my first option was implementing [dataprep](https://docs.dataprep.ai/user_guide/clean/clean_country.html) library, using `clean_country()` function to clean this column. However, I faced some unexpected issues trying to download the library on my PC. That's why I decided to implement dictionaries with AI help.

I also noticed that I could get the location of the tweet by the `user_timezone` column, which contains clean data. Therefore, I sent the list of `user_timezone` and `tweet_location` to the AI, specifying that if the value belongs to any city, county, state, province, location, or time zone, it should return the country of origin. Else, it returns null.

I convert `tweet_location` to lowercase and normalize it using the `tweet_location_dic` dictionary, and I use `user_timezone_dic` for `user_timezone`. Then, it fills any null values in `user_timezone` with the normalized values from `tweet_location`.

```
df['tweet_location'] = df['tweet_location'].str.lower().map(tweet_location_dic)

df['tweet_location'] = df['user_timezone'].map(user_timezone_dic).fillna(df['tweet_location'])
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
- **Tweets by Location:** I use `# Tweets` for values and `tweet_location` for the country location. (using the normalized data from python).
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
- **Sentiments by Negative Reason:** I use `negativereason` in y and `# Tweets` in x. Also, `% Negatives` for more insights.
- **Tweets by Location:** I use `# Tweets` for values and `tweet_location` for the country location. (using the normalized data from python)
- **Tweets by Date:** I use `tweet_created` in x and `# Tweets` in y. Also, `# Negatives`, `# Neutrals `, `# Positives` in details for insights.

## What I would add if I had more time
If I had more time, I'd add the next things to the challenge:
- A/B testing.
- Connect Tableau/Power BI directly to a database.
- Automations, for example using Microsoft Data Factory/Fabric.
- Add security roles.
