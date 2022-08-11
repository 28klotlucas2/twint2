# TWINT2 - Maintained Twitter Intelligence Tool
![2](https://i.imgur.com/iaH3s7z.png)
![3](https://i.imgur.com/hVeCrqL.png)

[![PyPI](https://img.shields.io/pypi/v/TWINT2.svg)](https://pypi.org/project/TWINT2/) [![Build Status](https://travis-ci.org/TWINT2project/TWINT2.svg?branch=master)](https://travis-ci.org/TWINT2project/TWINT2) [![Python 3.6|3.7|3.8](https://img.shields.io/badge/Python-3.6%2F3.7%2F3.8-blue.svg)](https://www.python.org/download/releases/3.0/) [![GitHub license](https://img.shields.io/github/license/haccer/tweep.svg)](https://github.com/haccer/tweep/blob/master/LICENSE) [![Downloads](https://pepy.tech/badge/TWINT2)](https://pepy.tech/project/TWINT2) [![Downloads](https://pepy.tech/badge/TWINT2/week)](https://pepy.tech/project/TWINT2/week) [![Patreon](https://img.shields.io/endpoint.svg?url=https:%2F%2Fshieldsio-patreon.herokuapp.com%2FTWINT2project)](https://www.patreon.com/TWINT2project) ![](https://img.shields.io/twitter/follow/noneprivacy.svg?label=Follow&style=social) 

>No authentication. No API. No limits.

TWINT2 is an advanced Twitter scraping tool written in Python that allows for scraping Tweets from Twitter profiles **without** using Twitter's API.

TWINT2 utilizes Twitter's search operators to let you scrape Tweets from specific users, scrape Tweets relating to certain topics, hashtags & trends, or sort out *sensitive* information from Tweets like e-mail and phone numbers. I find this very useful, and you can get really creative with it too.

TWINT2 also makes special queries to Twitter allowing you to also scrape a Twitter user's followers, Tweets a user has liked, and who they follow **without** any authentication, API, Selenium, or browser emulation.

## tl;dr Benefits
Some of the benefits of using TWINT2 vs Twitter API:
- Can fetch almost __all__ Tweets (Twitter API limits to last 3200 Tweets only);
- Fast initial setup;
- Can be used anonymously and without Twitter sign up;
- **No rate limitations**.

## Limits imposed by Twitter
Twitter limits scrolls while browsing the user timeline. This means that with `.Profile` or with `.Favorites` you will be able to get ~3200 tweets.

## Requirements
- Python 3.6;
- aiohttp;
- aiodns;
- beautifulsoup4;
- cchardet;
- dataclasses
- elasticsearch;
- pysocks;
- pandas (>=0.23.0);
- aiohttp_socks;
- schedule;
- geopy;
- fake-useragent;
- py-googletransx.

## Installing

**Git:**
```bash
git clone --depth=1 https://github.com/TWINT2project/TWINT2.git
cd TWINT2
pip3 install . -r requirements.txt
```

**Pip:**
```bash
pip3 install TWINT2
```

or

```bash
pip3 install --user --upgrade git+https://github.com/TWINT2project/TWINT2.git@origin/master#egg=TWINT2
```

**Pipenv**:
```bash
pipenv install git+https://github.com/TWINT2project/TWINT2.git#egg=TWINT2
```

### March 2, 2021 Update

**Added**: Dockerfile

Noticed a lot of people are having issues installing (including me). Please use the Dockerfile temporarily while I look into them. 

## CLI Basic Examples and Combos
A few simple examples to help you understand the basics:

- `TWINT2 -u username` - Scrape all the Tweets of a *user* (doesn't include **retweets** but includes **replies**).
- `TWINT2 -u username -s pineapple` - Scrape all Tweets from the *user*'s timeline containing _pineapple_.
- `TWINT2 -s pineapple` - Collect every Tweet containing *pineapple* from everyone's Tweets.
- `TWINT2 -u username --year 2014` - Collect Tweets that were tweeted **before** 2014.
- `TWINT2 -u username --since "2015-12-20 20:30:15"` - Collect Tweets that were tweeted since 2015-12-20 20:30:15.
- `TWINT2 -u username --since 2015-12-20` - Collect Tweets that were tweeted since 2015-12-20 00:00:00.
- `TWINT2 -u username -o file.txt` - Scrape Tweets and save to file.txt.
- `TWINT2 -u username -o file.csv --csv` - Scrape Tweets and save as a csv file.
- `TWINT2 -u username --email --phone` - Show Tweets that might have phone numbers or email addresses.
- `TWINT2 -s "Donald Trump" --verified` - Display Tweets by verified users that Tweeted about Donald Trump.
- `TWINT2 -g="48.880048,2.385939,1km" -o file.csv --csv` - Scrape Tweets from a radius of 1km around a place in Paris and export them to a csv file.
- `TWINT2 -u username -es localhost:9200` - Output Tweets to Elasticsearch
- `TWINT2 -u username -o file.json --json` - Scrape Tweets and save as a json file.
- `TWINT2 -u username --database tweets.db` - Save Tweets to a SQLite database.
- `TWINT2 -u username --followers` - Scrape a Twitter user's followers.
- `TWINT2 -u username --following` - Scrape who a Twitter user follows.
- `TWINT2 -u username --favorites` - Collect all the Tweets a user has favorited (gathers ~3200 tweet).
- `TWINT2 -u username --following --user-full` - Collect full user information a person follows
- `TWINT2 -u username --timeline` - Use an effective method to gather Tweets from a user's profile (Gathers ~3200 Tweets, including **retweets** & **replies**).
- `TWINT2 -u username --retweets` - Use a quick method to gather the last 900 Tweets (that includes retweets) from a user's profile.
- `TWINT2 -u username --resume resume_file.txt` - Resume a search starting from the last saved scroll-id.

More detail about the commands and options are located in the [wiki](https://github.com/TWINT2project/TWINT2/wiki/Commands)

## Module Example

TWINT2 can now be used as a module and supports custom formatting. **More details are located in the [wiki](https://github.com/TWINT2project/TWINT2/wiki/Module)**

```python
import TWINT2

# Configure
c = TWINT2.Config()
c.Username = "realDonaldTrump"
c.Search = "great"

# Run
TWINT2.run.Search(c)
```
> Output

`955511208597184512 2018-01-22 18:43:19 GMT <now> pineapples are the best fruit`

```python
import TWINT2

c = TWINT2.Config()

c.Username = "noneprivacy"
c.Custom["tweet"] = ["id"]
c.Custom["user"] = ["bio"]
c.Limit = 10
c.Store_csv = True
c.Output = "none"

TWINT2.run.Search(c)
```

## Storing Options
- Write to file;
- CSV;
- JSON;
- SQLite;
- Elasticsearch.

## Elasticsearch Setup

Details on setting up Elasticsearch with TWINT2 is located in the [wiki](https://github.com/TWINT2project/TWINT2/wiki/Elasticsearch).

## Graph Visualization
![graph](https://i.imgur.com/EEJqB8n.png)

[Graph](https://github.com/TWINT2project/TWINT2/wiki/Graph) details are also located in the [wiki](https://github.com/TWINT2project/TWINT2/wiki/Graph).

We are developing a TWINT2 Desktop App.

![4](https://i.imgur.com/DzcfIgL.png)

## FAQ
> I tried scraping tweets from a user, I know that they exist but I'm not getting them

Twitter can shadow-ban accounts, which means that their tweets will not be available via search. To solve this, pass `--profile-full` if you are using TWINT2 via CLI or, if are using TWINT2 as module, add `config.Profile_full = True`. Please note that this process will be quite slow.
## More Examples

#### Followers/Following

> To get only follower usernames/following usernames

`TWINT2 -u username --followers`

`TWINT2 -u username --following`

> To get user info of followers/following users

`TWINT2 -u username --followers --user-full`

`TWINT2 -u username --following --user-full`

#### userlist

> To get only user info of user

`TWINT2 -u username --user-full`

> To get user info of users from a userlist

`TWINT2 --userlist inputlist --user-full`


#### tweet translation (experimental)

> To get 100 english tweets and translate them to italian

`TWINT2 -u noneprivacy --csv --output none.csv --lang en --translate --translate-dest it --limit 100`

or

```python
import TWINT2

c = TWINT2.Config()
c.Username = "noneprivacy"
c.Limit = 100
c.Store_csv = True
c.Output = "none.csv"
c.Lang = "en"
c.Translate = True
c.TranslateDest = "it"
TWINT2.run.Search(c)
```

Notes:
- [Google translate has some quotas](https://cloud.google.com/translate/quotas)

## Featured Blog Posts:
- [How to use TWINT2 as an OSINT tool](https://pielco11.ovh/posts/TWINT2-osint/)
- [Basic tutorial made by Null Byte](https://null-byte.wonderhowto.com/how-to/mine-twitter-for-targeted-information-with-TWINT2-0193853/)
- [Analyzing Tweets with NLP in minutes with Spark, Optimus and TWINT2](https://towardsdatascience.com/analyzing-tweets-with-nlp-in-minutes-with-spark-optimus-and-TWINT2-a0c96084995f)
- [Loading tweets into Kafka and Neo4j](https://markhneedham.com/blog/2019/05/29/loading-tweets-TWINT2-kafka-neo4j/)

## Contact

If you have any question, want to join in discussions, or need extra help, you are welcome to join our TWINT2 focused channel at [OSINT team](https://osint.team)
