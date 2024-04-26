# Muscle Metrics: Revolutionizing Bodybuilding Competitions with Elo Ratings
## Erdős Institute Data Science Bootcamp Spring 2024

### Team members
1. Jessica de Silva
2. Andrei Prokhorov

## Project description
Create a metric that predicts the outcome of professional bodybuilding competitions based on competitor’s historical performance in amateur and professional competitions.

## Data
Using *BeautifulSoup*, we scraped the results of bodybuilding competitions starting from 2012 from the website https://npcnewsonline.com/. We had 783,708 entries in total, with 57,393 entries in the IFBB organization, 616, 391 entries in the NPC organization, 92, 896 entries in the NPCW organization,  17, 027 entries in the CPA organization.
<img width="666" alt="Screenshot 2024-04-26 at 2 33 37 PM" src="https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/e98702a3-e19b-458c-80a4-b24e6f14cb0e">

## Models
### Elo
We used the [Elopy](https://pypi.org/project/elopy/) package. It is designed for 1v1 competitions so, we paired all the competitors artificially and assumed that all these pairs compete independently. It is important to put hca(home court advantage) parameter to zero, since we didn't have the location data for competitors and we need the probabilities of win of two competitors to sum up to 1. We used the default valuee of parameter k=20. It seemed like changing it didn't produce significant improvement to the performance. 
<img width="1038" alt="Screenshot 2024-04-26 at 2 56 14 PM" src="https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/accc7b0c-1443-4df0-a565-a9413255a87d">
### Adjusted Elo
We modified the starting Elo rating for the competitors without amateur data. We put different number for different divisions based on the Elo ratings of the athletes when their earned their Pro card.
![Starting Elo](https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/1af4a48e-9959-49f7-a40e-837ba34c9779)
### Truescale
We used [truescale](https://trueskill.org) package. This ranking system is designed for the multiplayer games and is used by Xbox Live. It produced similar performance to the Elo rating.
<img width="1034" alt="Screenshot 2024-04-26 at 2 59 49 PM" src="https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/f818ecff-6373-434d-b66f-f39e68604dd4">

### Multielo
We used [multielo](https://github.com/djcunningham0/multielo) package. This is generalization of Elo ranking to multiplayer games. It can produce the probability of given ranking.
<img width="1154" alt="Screenshot 2024-04-26 at 3 01 39 PM" src="https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/ddeb17c3-b7cd-4c80-84b5-e95ce42d6e12">
## KPIs
### Kendall-tau correlation coefficient
[Kendall-tau correlation coefficient](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.kendalltau.html#scipy.stats.kendalltau) is the metric for the prediction of raning of competitors. It is proportional to the difference between corretly guessed placings and incorrectly guessed placings. It tends to zero for the random guessing.  Below are the plots of 30 day rolling averages of this metric for IFBB organization, bikini division, open class. Orange graphs correspond to the random choice of the ranking.

![truescale_kt](https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/f3504ae0-f4be-47e5-a549-d3474a4e0a2c)
![adjusted_elo_kt](https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/c783a0a8-3867-406d-b4f8-314c9274f2eb)
![multielo_kt](https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/17d30738-aa78-4b67-aefd-a6c28419b747)
![elo_kt](https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/de5946c5-025f-4c75-a981-3b8ea247b8fa)

### Precision@k

Precision@k metric gives the fraction of correct predictions among top 5 competitors in the competition. Below are the plots of 30 day rolling averages of this metric for IFBB organization, bikini division, open class. Orange graphs correspond to the random choice of the ranking.

![Elo](https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/18994d41-d42b-4d9a-8a4a-0be676c16d14)
![Adjusted elo](https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/e3e883c7-e415-4ff8-90f1-7604898db088)
![multielo](https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/e02ef0f8-c142-4463-a062-bee4f21334b9)
![truescale](https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/9dab4501-fd83-449e-a500-345748fa268b)

### NDCG score
![multielo_ndcg](https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/2e1ccce9-aced-406d-aaae-a2395710d122)
![truescale_ndcg](https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/f938dc2d-cf66-409b-a8ba-f09a0e78240a)
![elo_ndcg](https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/cdd706d6-30f9-47c5-b4f1-fdf04aa7e04a)
![adjusted elo ndcg](https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/526d7f5d-d875-4e19-822b-a13f3bd95f25)


### Multielo probability
![multielo_prob](https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/eb00e20f-5e63-41c0-b45f-993c7de7f798)


## Observations
1. We had to adjust starting elo rating for competitors without amateur data.
2. NPC athletes compete significantly less often than professional athletes.
3. We ranked the division's competetiveness.
4. We observed that majority of top Mr Olympia winners earned their PRO card at the US competition. 

## Future work
Get additional data by scraping
1. Scorecards photos
2. Athlete's photos.



