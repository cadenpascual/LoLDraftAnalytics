# Predicting the outcome of a League of Legends match with the Draft
by Caden Pascual (capascual@ucsd.edu)

---

## Introduction

### What is League of Legends?
League of Legends is a Video Game where players play in a team of 5 players, versus another team of 5 players, with the objective being to kill the opponents base. Currently, there are 169+ "champions" in the game, with each of them possessing a unique set of skills and abilities they can use in battle. At the start of every match, both teams have the option to ban 5 characters from being played that match, with two teams, leaving up to 10 champions from being banned. Then both teams draft or pick champions to use during the game from the pool of champions that aren't banned or haven't already been picked. There are more details as to what happens during the game, but this analysis aims to predict the outcomes before the game even happens, so we don't need to understand those details.

### Why this dataset?
The League of Legends dataset was the most intriguing to me, since it heavily relates to the field that I want to pursue, sports analytics. Similar to some types of sports, a League of Legends match is a competition between two opposing teams, with the goal of destroying the other teams base. 

I think that with my analysis, using data like "damage per minute" or "total kills" may not be as interesting, as you can only fully get these statistics at the end of the match.

In League of Legends, there is a draft phase at the start of the game, where teams ban and pick different characters to use in the game. My goal is to be able to accurately predict the outcome of the game by just using this data. 

If we take a look at different matches and the ban + pick% of the characters used (higher meaning they're more popular or meta), can we conclude that certain characters can expect higher win percentages, or that a matches outcome isn't that affected by the "meta", and that it really depends on the players skill/teamwork instead.

### Dataset overview

We're going to be using the dataset taken from the website [Oracle’s Elixir](https://oracleselixir.com/tools/downloads) using match data from 2022. The original dataframe has 150,180 rows and 161 columns, with each column being a single players data in a single match. The match is a 5 versus 5, so for every match there are 10 designated rows, for some matches, there are also 2 extra rows with the sum of all stats of a team.  

There are lots of statistical columns ranging from damage dealt, first kill, and the % of damage you have on your team. However, these are stats that are calculated after the game starts. We're trying to predict the outcome of the game before it starts, and after the draft, so these columns are irrelevant to our calculations. If we want to, we can predict these columns using the draft data, but I want to specifically focus on the outcome of the match, not the total "dps" of a team or who will take the first tower.

The main columns that we're going to be working with are the 'ban' and 'pick' columns. In each game of League of Legends, there is a draft before the game starts, where both teams can ban 5 different champions from being played that match. Then each team starts "drafting", or picking the champions they want to use. A champion can't be picked if it has already been pick or it has been banned, but the same character can be banned by both teams. If each team has 5 unique brawlers they drafted + 5-10 unique brawlers banned. In total for every match there are 15-20 different champions that are picked/banned.

Other important columns include
- 'gameid', which categorizes every game with a unique ID
- 'result', 1 if your team wins, 0 if your team loses
- 'date' and 'patch', which gives a specific time and patch (update) that the game is played
- 'champion', the specific champion the player is playing, empty if the row is the 'team data'.
- 'league', the competitive league the match is played in.


---

## Cleaning and EDA

### Data Cleaning

### Univariate Analysis

#### Playoff vs Non Playoff Games

The above plot is the distribution of non-playoff vs playoff games. There are much more "non-playoff" games since the playoffs are a specific subset of games, only for the best teams, to play against each other for the final championship. We can group by "playoff" and "non-playoff" games in our analysis to see if it may be easier or harder to guess the outcome of a game based off if it is a playoff game or not.

#### Games per Patch

Above is the distribution of games played for every patch (game update). There are less games played in certain patches since the time that certain patches were up may have been less than others. We can verify this by plotting the amount of games played per month. There are 9 games that aren't assigned a patch. We will figure out what patch they are in and the reason later.

#### Games per Month

The distribution of games played per month has a very similar shape to the patch notes graph. We can assume many things, maybe there are big tournaments around Feb-March and June-August.


### Bivariate Analysis

#### Times picked first vs Win Percentage (top 10 champions)
There is a slightly positive correlation between a "popular" first pick and winning, but even the most popular first pick, Jinx, only has a win pctg of 51%, which isn't that high above average. Let's take a look at the chart with all possible first picks (min 25 times picked, since the data for such champions is always skewed).

#### Times picked first vs Win Percentage (all champions)
There isn't a strong visible correlation between win % and the how popular the first pick is, we have to look futher into this, maybe some characters are stronger during some patches, and after they get nerfed (the game devs lower their stats or make their abilities worse to make them fairer), their win percentages drop. 


### Interesting Aggregates
With the previous Bivariate examples in mind, let's see what an aggregation of ALL champions, regardless of when they're picked, and their win rates.

Most champions win rates hover around the 50% line. This is what we should be expecting, since we haven't looked at any of the banned statistics, we can expect that all the strongest champions are being banned (meaning they can't be picked in matches).


---

## Assessment of Missingness

### NMAR Analysis

I don't believe there is a column in the dataset I'm using that is NMAR. There is a column in my dataset that is named "datacompleteness", so there should be some column in the  dataset that is missing based off this column. That wouldn't be NMAR data however, since the missingness would depend on the "datacompleteness" column. In a LoL game, there isn't any way for a player to "hide" a specific stat. Sure, there are competitive matches that may not be in this dataset. Not all games are public, but for these games we wouldn't be left with only some of the stats, we wouldn't see the row altogether. 

### Missingness Dependency

"Each 'gameid' corresponds to up to 12 rows – one for each of the 5 players on both teams and 2 containing summary data for the two teams." The summary data for both teams usually contains the champions picked for the whole team, although some of the picks are missing in these rows. Some competitive 'leagues' may not be as mainstream as others, let's see if this data is missing dependent on the league.

As we can see, 6/50 of the leagues have all their draft data missing. Let's see if this is statistically possible, or if the draft data missing is actually dependent on the league with permutations. 

Null Hypothesis: The missingness of pick_missing is independent of the column 'league'. <br>
Alternative Hypothesis: The missingness of pick_missing depends on the column 'league'.


Our p-value is 0.0, which is under the threshold 0.05, so we reject the Null Hypothesis, meaning that pick_missing depends on the column "league". Let's check another column that pick_missing ISN'T dependent on, such as 'side' (the current game in the series).


## Hypothesis Testing

Null Hypothesis: High-ban-rate champions do not have a statistically significantly higher mean win rate than the average champion. <br>
Alternative Hypothesis: High-ban-rate champions have a statistically significantly higher mean win rate than the average champion. <br>
Test Statistic: Difference in Means<br>
P-Value: 0.0439<br>



---

## Framing a Prediction Problem



---

## Baseline Model



---

## Final Model


___

## Fairness Analysis
