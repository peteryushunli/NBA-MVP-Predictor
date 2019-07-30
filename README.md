# NBA MVP Predictor
![Overview Image](https://cdn-images-1.medium.com/max/800/1*EnaXYr1IZ8xIzjNDNNjXIQ.jpeg)

# Introduction
The Most Valuable Player (MVP) award is the highest individual accolade in National Basketball Association (NBA). It is voted on by media members based on a relatively open-ended definition of value. It's purposely subjective to incite conversations (and heated arguments) between fans and media members about what value truly means in the game of basketball.
Through the years though, the two dominating narratives have been "best player on the best team" (i.e. 2018: James Harden, 2016: Steph Curry) or having "historical numbers" (i.e. 2017: Russell Westbrook, 2014: Kevin Durant). Using these two narratives as the criteria, I set out to create a statistical model to evaluate a player's value based on their contribution to team wins and overall individual output.
My goal was to build a model that could accurately award the MVP from the past 10 seasons (my training data) in order to make a reliable prediction for 2019. The ultimate purpose isn't to create an objective truth on value, but rather look at the MVP race through a consistent statistical lens that reflects the same criteria year-over-year.
# Model Summary
Value = .5(Win Contribution) + .5(Total Stats)
As mentioned above a player's overall value is based on an even average of their win contribution and total stats.
Note: This not a statistically-tested model. 50/50 weights were a theoretical starting point. As I loaded and tested the model on more MVP races, I tweaked the weights to see how it would affect results. Ultimately, I ended with 50/50 weight in order to maximize model accuracy across the decade.
## Win Contribution Breakdown
Win Contribution = Level of Impact * Quality of Impact
Level of Impact = (Team Wins * Games Played/82 * Minutes/48 * Usage Rate/100)
Quality of Impact = .4(VORP+Win Share)+ .2(Net Rating)
### 1. Level of Impact
First, I wanted to gauge a player's level of impact (high or low) on his team's wins through games and minutes played, and usage rate. Mathematically, the most important number here is team wins. All other variables serve as rates to assess a player's impact to those wins.
Usage may seem controversial here, but I wanted to measure how much a team relies on a player in possession in comparison with his teammates. For example in 2016, the Golden State Warriors won a record of 73 regular season games. Their 3 best players were Steph Curry, Klay Thompson and Draymond Green, each of whom played approximately 80 games and averaged 34 minutes a game. Usage rate serves as the key differentiator of impact between teammates in this situation.
While the stat is generally skewed towards guards and ball-handlers - 90% of the top 20 usage rate this season were non-centers, elite big men like Joel Embiid, Anthony Davis and DeMarcus Cousins (pre-achilles injury) consistently rank in the top 10 of usage rate due to their tremendous skill. More on this issue later.
### 2. Quality of Impact
Next, I wanted to measures a player's quality of impact (good or bad) when on the floor. This was by far the most difficult aspect of building the model. Unlike baseball, there is not one comprehensive statistic that's widely accepted by the basketball community to measure a player's quality of impact. As a result, I used weighted values of multiple advanced statistics that are accessible to the public. Below is a quick summary of what they mean.

Win Share: measures a player's contribution to a team's win total based on player production (on offense and defense), adjusted for league average production, and league and team pace

VORP: measures a player's relative impact (Box Plus/Minus), adjusted for minutes played, compared to league average. (Note: the league leader in VORP has won MVP every year since 2006 except for 5 instances)

Net Rating: the point difference between how much a player produces on offense per 100 possessions (Offensive Rating) and how much player gives up on defense per 100 possessions (Defensive Rating)

Win Share and VORP were used because to distinguish between a player's contribution relative to that of his teammates (Win Share) and to the rest of the league (VORP). Net Rating was included to distinguish players' overall output when on the floor, and help adjust the equation in favor of players with lower minutes, which VORP and Win Share penalize against.
The value of the weights are somewhat arbitrary and were determined after a series of trial-and-error to maximize the model's accuracy. Although, the reason Net Rating has a lower weight because it's heavily biased towards big-men and part of its calculation (defensive rating) is also used to calculate Defensive Win Shares, which is a part of Win Share.
## Total Stats (Individual Output) Calculation
(Points * True Shooting% + 1.5(Assists) + 1.2(Rebounds) +3(Blocks)+3(Steals)-Fouls-Turnovers)/25
I wanted to keep this output as simple as possible, and borrowed the weights from fantasy basketball. Couple of tweaks I added are multiplying true shooting % (TS%) against points scored. This allowed me to exclude stats like field goal, free throws, 3-Point attempts and makes. While these stats are considered by people when evaluating MVP, I felt that (Points * TS%) was a compact way to measure scoring output against efficiency. The last unit of 25 was a scalar I used to make the final value be close to the win contribution value (also chosen in order to maximize overall model accuracy).
I want to come back to usage rate here because when evaluating total stats, it's heavily skewed towards bigs (same for net rating but that's more attributed to a center's role in anchoring a defense). In 2019, Rudy Gobert and Andre Drummond finished 5th and 6th, respectively, in individual output over players like Paul George (9th) and Damian Lillard (14th). This is another reason why I kept usage in the win contribution formula to balance out the big-men bias in total stats.


# Testing the model against MVPs of the past decade
Before predicting the 2019 MVP, I wanted to assess the model's results and inherent biases based on the past 10 seasons. It's a fun trip down memory lane and shows a general change in league-wide perception and acceptance of analytics with time.
(Note: all data is sourced from Basketball-Reference)

## 2017–2018
![2018 Model Results](https://cdn-images-1.medium.com/max/1200/1*L5aeBXZgm3p4s_sUUjBgNg.png)
Damian Lillard is the only top-5 omission (4th actual vs. 7th predicted)
The only major deviation here is Karl-Anthony Towns who received no actual MVP votes, while teammate, Jimmy Butler (10th actual vs 22nd predicted), was regarded as the primary reason for Minnesota's success despite missing 23 games

## 2016–2017
![2017 Model Results](https://cdn-images-1.medium.com/max/1200/1*XpCXGDZCI4g9lCr_HWgrNw.png)
A historically tight MVP race, but the model showed an extremely slim margin in favor for Russell Westbrook
Steph Curry is undervalued by voters (6th actual vs. 3rd predicted) likely due to the addition of Kevin Durant during 2016 off-season

## 2015–2016
![2016 Model Results](https://cdn-images-1.medium.com/max/1200/1*0ZsU39eG7w8keEA7fsPYiQ.png)
Steph Curry wins a landslide victory (1st unanimous MVP in league history) in both actual and predicted voting after breaking the regular season record of 73 wins
1st time the model predicts the correct top-5 players despite slight deviations in actual rankings
Kawhi Leonard finishes 5th predicted vs. 2nd actual despite winning Defensive Player of the Year, likely due to relatively unspectacular total stats (21 Points, 6.8 Rebounds and 2.6 Assists)

## 2014–2015
![2015 Model Results](https://cdn-images-1.medium.com/max/1200/1*O4tYOvaAWY0R8Jm9TKpSdA.png)
The tightest margin of victory for Steph Curry over James Harden in both predicted rankings and actual voting (Curry: 1198 to Harden: 936), (Harden would win an unofficial MVP award voted only by players)
The biggest omission here is LeBron James (3rd actual vs 6th predicted) due to subpar advanced metrics for his standard (6th in VORP, 10th Win Shares) from Cleveland's slow start

## 2013–2014
![2014 Model Results](https://cdn-images-1.medium.com/max/1200/1*8HHBnSkwdufFvUtCSac6lg.png)
Kevin Love, another Minnesota big man that this model overvalues (11th actual vs. 3rd predicted), posts an amazing individual statistical season (3rd in the league in VORP behind only LeBron and KD)
Joakim Noah, the season's Defensive Player of the Year, is the biggest omission (4th actual vs. 10th predicted) likely due to his lack of offensive production leading a Bulls team to 48 wins

## 2012–2013
![2013 Model Results](https://cdn-images-1.medium.com/max/1200/1*G60YPc-QXAeIHB7L9SD3Dw.png)
Carmelo Anthony is a major omission (3rd actual vs. 15th predicted) despite being the best player on a 54 win Knicks team (Level of Impact: 12.11, 5th in the league) suffered in Quality of Win due to subpar advanced metrics (33rd in VORP, 14th in Win Shares)
Overall, as time moves back, the model will undervalue players with poor advanced statistics (while actual voters might overvalue them) due to the lack of public awareness and acceptance of these metrics

## 2011–2012
![2012 Model Results](https://cdn-images-1.medium.com/max/1200/1*zD7f0azdJ68NlzUGQDwXgw.png)
In a lock-out shortened season, Blake Griffin is the biggest surprise entry. While his individual numbers (20.7 Points, 10.9 Rebounds) were solid, he likely did not receive any actual votes due to his "Lob City" teammate Chris Paul being the focal point of the team
The biggest omission is Kobe Bryant (4th Actual vs. 19th Predicted) who suffers a similar issue as Carmelo Anthony in 2013 by being a high-volume, low efficiency player (Level of Impact: 12.82, 4th in the league) (25th in the league in VORP, 35th in the league in Win Shares)

## 2010–2011
![2011 Model Results](https://cdn-images-1.medium.com/max/1200/1*Fm6KnHll5ocHv32j0ThPaQ.png)
The model's first MVP prediction error comes in 2011 after LeBron James's infamous 2010 "Decision" to join the Miami Heat
The overwhelming reason for this loss was the narrative of Derrick Rose being the humble, hometown guard of Chicago. While LeBron was regarded as the biggest sports villain in the country at the time
Rose, however, did have an advantage to LeBron in Level of Impact, which as we've seen has a strong correlation with actual voting pre-2015 (a year of significance that I will cover later)
Lastly, Pau Gasol is the surprise entry place likely taking the spot of teammate, Kobe Bryant (4th place actual vs. 7th place predicted: Level of Impact: 14.13, 3rd in the league), continuing a trend of the model overvaluing efficient big men over their wing teammates

## 2009–2010
![2010 Model Results](https://cdn-images-1.medium.com/max/1200/1*9b4FouyKuFSSJuQJF8iUuA.png)
A season with no major errors though the model continues to undervalue Kobe Bryant, but only slightly this time! (3rd actual vs. 6th predicted)

## 2008–2009
![2009 Model Results](https://cdn-images-1.medium.com/max/1200/1*H9qIbtkG1jj9u6EmryXPcA.png)
Another accurate season where the model predicts the correct top-5 players despite minor mistake in order
An absolutely historic season by LeBron James who posted an all-time highest Box Plus/Minus (13) in NBA history, 3rd highest VORP (11.6) trailing only Michael Jordan



# 2018–2019 MVP Prediction
…and finally for the prediction of this year's race (and likely what most readers were looking for)
![2019 Model Prediction](https://cdn-images-1.medium.com/max/2560/1*M7VYsK0IqgThIxxuu8vffg.png)

The model shows James Harden as the predicted winner based on higher level of impact (more games played, more minutes played, higher usage rate) and higher total stats (36 points/game, 8th highest scoring season in league history behind only Wilt Chamberlain, Elgin Baylor and MJ, 5th highest Total Stats in the decade). The results are similar to Russell Westbrook's 2017 win where he beat James Harden despite having less wins, but better total stats
However, I think Giannis is likely still the favorite to win the MVP due to 2 factors, narrative and defense. In terms of narrative, Giannis is the proverbial "new kid on the block" beloved by the media and fans (similar to Derrick Rose in 2011), while Harden, the incumbent winner, can suffer from voter fatigue and overall negative sentiment towards his playing style.
In terms of defense, there are examples of Kawhi Leonard in 2016 and Joakim Noah in 2014 (both winners of Defensive Player of the Year) being undervalued. This year, Giannis is also in contention of the award (2nd in the league in Defensive Box Plus/Minus and 3rd in Defensive Win Shares). While both of these statistics are incorporated in the model's calculation, it's not hard to imagine the public perception of Giannis's defensive impact and overall narrative pushing him over Harden.
# Conclusion
The creation and testing of this model was an interesting thought-exercise that tested both my statistical and basketball views. It was fascinating juggling between my preconceived notions of value in basketball, what the model was telling me, and retrofitting that against a decade of results. Below are a few of my closing thoughts:
As mentioned before, 2015 was a significant year in the NBA. The Golden State Warriors become the first "jump-shooting team" to win a championship, and help shift the league-wide perception of analytics. The results were reflective in the model's consistent undervaluing of players like Kobe Bryant and Carmelo Anthony; high usage, low efficiency scorers who still played heroball instead of Moreyball.
The model's overvalue of bigs like Kevin Love, Karl-Anthony Towns and Blake Griffin was interesting because these players were able to land unexpected top-5 finishes due a combination of both great stats and favorable advanced stats. While conventional wisdom would place some of that success on their more ball-dominant teammates, it made me question whether these bigs are truly underrated or a product of favorable situations and stats.
Lastly, the undervaluing of defense was a bias reflected in the model, but also throughout basketball. Some of it is due to the lack of reliable (and publicly accessible) defensive stats for individual players. While that would certainly help the model (along with a score to measure public sentiment/narrative), I don't know how much it would affect the general fan bias towards offense. Defense may win championships, but it doesn't generally make headlines or attract sponsorships. Honestly, I don't know if any amount of analytics will change that.
