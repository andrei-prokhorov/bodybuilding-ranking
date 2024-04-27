# Muscle Metrics: Revolutionizing Bodybuilding Competitions with Elo Ratings
## Erdős Institute Data Science Bootcamp Spring 2024

### Team members
1. Jessica De Silva
2. Andrei Prokhorov

## Project objective
Create a metric that predicts the outcome of professional bodybuilding competitions based on competitor’s historical performance in amateur and professional competitions.

## Data collection
Using the *BeautifulSoup* Python package, we scraped the results of bodybuilding competitions starting from 2012 from the [NPC News Online website](https://npcnewsonline.com/). The code for this data pipeline can be found at [this GitHub repo](https://github.com/jessicadesilva/pipeline_project). The dataset contains 783,708 entries in total, with 57,393 entries in the IFBB organization, 616,391 entries in the NPC organization, 92,896 entries in the NPCW organization,  and 17,027 entries in the CPA organization.

<img width="666" alt="Screenshot 2024-04-26 at 2 33 37 PM" src="https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/e98702a3-e19b-458c-80a4-b24e6f14cb0e">

## Models

### Elo rating
Using the [Elopy](https://pypi.org/project/elopy/) Python package, we measured each competitor's skill using the Elo rating model and their historical competition performance. This model is designed for 1v1 match-ups, however, bodybuilding competitions are 1v1v1v...v1 so, we paired all the competitors artificially and assumed that all these pairs compete independently. The starting Elo rating for all new competitors was set to 1500 in this particular model. Since we do not have data regarding the home countries of competitors nor the locations of the competitions, we set the home court advantage (HCA) parameter to zero. The k-factor parameter influences how quickly the Elo rating changes after a new competition occurs. Varying the k parameter did not produce significant improvement in the performance of our rankings, so we used the default value of 20.

<img width="1038" alt="Screenshot 2024-04-26 at 2 56 14 PM" src="https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/accc7b0c-1443-4df0-a565-a9413255a87d">

### Adjusted Elo rating
In this adjusted Elo rating model, we modified the starting Elo rating for the competitors for whom we did not have any amateur data. The starting Elo rating for a pro was calculated by the average Elo ratings of competitors in the same division for whom we do have amateur data immediately after winning a pro card.

![Starting Elo](https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/1af4a48e-9959-49f7-a40e-837ba34c9779)

### Multi-Elo rating
The [multielo](https://github.com/djcunningham0/multielo) Python package implements a multi-player extension of the Elo rating system. This model can produce the probability of a given ranking.

<img width="1154" alt="Screenshot 2024-04-26 at 3 01 39 PM" src="https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/ddeb17c3-b7cd-4c80-84b5-e95ce42d6e12">

### Truescale rating
The TrueSkill ranking system is a skill-based ranking system for Xbox Live developed at Microsoft Research. This ranking system allows us to model 1v1v1v...v1 competitions. We implemented this ranking system using the [truescale](https://trueskill.org) Python package.

<img width="1034" alt="Screenshot 2024-04-26 at 2 59 49 PM" src="https://github.com/jessicadesilva/bodybuilding-ranking/assets/158493309/f818ecff-6373-434d-b66f-f39e68604dd4">

## Key Performance Indicators

### Kendall-tau correlation coefficient
The [Kendall-tau correlation coefficient](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.kendalltau.html#scipy.stats.kendalltau) measures the similarity between a predicted ranking and the actual ranking. It is proportional to the "difference" between correctly and incorrectly predicted rankings, where difference is measured by the number of pairwise inversions required to obtain the actual ranking from the prediction. This metric tends to zero for a random ranking, and a perfect or reversed ranking gives a value of 1 and -1, respectively.  Below is a plot comparing our models according to the 30-day rolling averages of this metric for the IFBB organization, bikini division, open class competitions. We see that all models presented here produce nearly the same results.

![image](https://github.com/jessicadesilva/bodybuilding-ranking/assets/74026509/90947980-0123-46ac-9f27-70b4ba84879d)

### Precision@k (k=5)

Precision@k metric with k=5 gives the fraction of correct predictions among the top 5 competitors in the competition. As with the Kendall Tau Correlation Coefficient, the performances of the models presented here are similar with respect to this metric.

![image](https://github.com/jessicadesilva/bodybuilding-ranking/assets/74026509/a14fe74c-e10a-4b65-bd98-5456d56cf61a)

## Observations
1. We had to adjust starting elo rating for competitors without amateur data.
2. NPC athletes compete significantly less often than professional athletes.
3. We ranked the division's competetiveness.
4. We observed that majority of top Mr Olympia winners earned their PRO card at the US competition. 

## Future work
Get additional data by scraping
1. Scorecards photos
2. Athlete's photos.



