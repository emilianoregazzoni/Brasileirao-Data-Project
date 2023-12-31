#  Brazilian Football Confederation :factory: :memo: :soccer: 
## __Brasileirao (CBF) Data Project__  

Brasileirao is the most important competition in South America.
In 2021 the competition was chosen by the IFFHS as the strongest national league in South America as well as the strongest in the world. Surpassing Premier League.

Scenario: I am passionate about soccer. Since I was a child I love this sport and one of my dreams, maybe one day, work for a soccer team, 
in a data position area.
I want to work with Brasileirao information, and try to discover things about Brazilian soccer. Search on the data, clean the data, create some visualizations, identify some patterns and train a model with the data.

:scroll: The stages I am going to follow are:  
Prepare - Process - Analyze & Share - Train a model


## Prepare

I have searched on Internet about Brasileirao. I found in Kaggle a nice dataset, but not fully completed. Thanks a lot to Jose Michelin for this 16 years of information about Brasileirao. 

Here the original dataset: https://www.kaggle.com/code/josevitormichelin/brasileirao-2003-2019

I completed the dataset with information about 2020, 2021 and 2022 taken from Wikipedia.

My dataset combination here: https://www.kaggle.com/datasets/regazze/brasileirao-2003-2022

Also I created a new column called "Percentage win" that means the amount of points each team reached every tournament. Since Brasileirao 2003 edition, not every edition had  the same amount of matches.
After this step, I have all information needed for start with the process.

## Process
First I need to import the necesesary libraries to work. I will use the most commons for this research.

```
import pandas as pd 
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns 
```

After that I will take the file with information of Brasileirao that I mentioned before.

```
file = ('brasileirao3.xlsx')
dataset = pd.read_excel(file)
dataset.head()
```

![1](https://user-images.githubusercontent.com/20979227/226175783-1bd1bfbf-afe5-4f56-8d14-8488956abf46.JPG)

Something I figured out is RedBull Bragantino previously called "Bragantino" , then "RB Bragantino" and now "Red Bull Bragantino", so if let this situation going on like this, Python will consider three diferents clubs. I fixed the name and there is only "Red Bull Bragantino".

I Check the columns if every is ok.
```
dataset.info()
```
![2](https://user-images.githubusercontent.com/20979227/226175796-1e147f95-2531-4a2f-a2de-db885630271e.JPG)

Now I will check if there is any missing values, just to be sure before start the analyses.
```
dataset.isnull().sum()
```
![image](https://user-images.githubusercontent.com/20979227/226175889-9d12c733-7fa4-4c59-900b-b7c3719cc992.png)

There isn't any missing values at all.
Info function says the dataset has 410 inputs of data and it has 12 columns, not much information but I hope it is enough.

## Analyze and Share

Now I want to create a simple bar char to explore points obtained

```
totalPoints = dataset.groupby("team")["points"].sum().reset_index().sort_values(by="points",ascending=False)
plt.figure(figsize=(20,8))
plt.bar(totalPoints.team[:10], totalPoints.points[:10])
plt.ylim(1000,1400)
plt.ylabel("Number of points")
plt.xlabel("Teams")
plt.title("Most points reached by club")
plt.show()
```
![4](https://user-images.githubusercontent.com/20979227/226175952-a2f8fefa-ee3b-4094-a880-9641f09451f6.JPG)

Sao Paulo is the team wich most points have made, if it is considered all the points in the Brasielirao era. The graphic is very consistent because Sao Paulo, Flamengo and Santos (the first three) are the only brazilian teams that have never been relegated to second division.
Lets see the rank of goals scored by team. Create a dataframe with the sum of goals grouped by team.

```
totalGoals = dataset.groupby("team")["goals_scored"].sum().reset_index().sort_values(by="goals_scored",ascending=False)

df = pd.DataFrame({ "Team": totalGoals.team,
                    "Goals": totalGoals.goals_scored})
df
```
![5](https://user-images.githubusercontent.com/20979227/226176097-5acf208a-563e-4d75-aa06-55a56a6c1551.JPG)

Lets try seaborn library to create some visualizations.

```
sns.barplot(x=df.Goals[:10] , y=df.Team[:10] , data=df).set(title='Goals scored by team')
```

![6](https://user-images.githubusercontent.com/20979227/226176151-e3505e94-0ad5-4947-9990-9737d1975275.JPG)

The graphic seems to be consistent, because the same 3 three teams that have never been relegated lead this barplot too, but in a different order.

Now I want to see how many points got every champion each year since the start of Brasileirao in 2003.  What team was the best champion?

```
#Got just the champions, take number 1 each year
dataChampions = dataset.loc[dataset['position'] <2]
#print(dataChampions.to_string())

dataChampions2 = pd.DataFrame({
    'Year': dataChampions.year,
    'Name': dataChampions.team,
    'Points': dataChampions.points
})

dataChampions2

plt.figure(figsize=(11,6))
sns.pointplot(x=dataChampions2.Year, y=dataChampions2.Points)

plt.title('Points per year')
plt.show()
```

![7](https://user-images.githubusercontent.com/20979227/226176213-32f63fbb-239d-4ce4-a7de-79316acac9d5.JPG)

I see the champion with most points gained in Brasileirao era was Cruzeiro in 2003, in the first edition the team has reached the amazing amount of 103 points. Also there was more games than other recent editions. So it's impossible to confirm if it was the best champion ever.

```
bestPercentage = dataset.groupby(['team','year'])["percentage_win"].sum().reset_index().sort_values(by="percentage_win",ascending=False)

df = pd.DataFrame(bestPercentage)
df = df[:3]

df['TeamYear'] = df['team']  + ' '+ df['year'].astype(str)
plt.title('Top 3 best champions')

sns.barplot(x='TeamYear',y=df.percentage_win,data=df)

plt.ylim(50,100)
plt.show()
```

![8](https://user-images.githubusercontent.com/20979227/226176278-61bbf6b8-a4d1-43f1-98c1-3f877cc9e459.JPG)

Here I can see the best champions in Brasileirao era definitely was Flamengo squad of 2019. An insane squad leaded by Gabigol, De Arrascaeta, Vitinho, Everton Ribeiro. One of the best squads Brasileirao has ever had.

Now I want to see the goal difference from Flamengo through the Brasileirao era.

```
df2 = pd.DataFrame(dataset)
flamengoDataset = df2.loc[df2['team'] == 'Flamengo']
flamengoDataset['year'] = flamengoDataset['year'].astype('category')
sns.lineplot(data=flamengoDataset, x='year', y='goals_difference')
plt.xticks([2005, 2010, 2015, 2019 ,2022])
```

![9](https://user-images.githubusercontent.com/20979227/226176412-924657fa-5d75-477a-9593-31568d448da6.JPG)

At this point I can see that the year Flamengo became the best champion ever in Brasileirao, it was also the year the team has reached the + highest goal difference ever in his entire history.

Finally I want to see how many championships the teams have reached along the years.

```
df = pd.DataFrame(dataset)
dfChampions = df.loc[df['position'] == 1]

dfChampions
```
![10](https://user-images.githubusercontent.com/20979227/226176472-5757653a-aa33-4b57-8a35-538c1abed8ef.JPG)

Now it's time to count it.

```
# try to count the championships of the teams in a new column by team
dfChampions2=dfChampions
qty = dfChampions['team'].value_counts()

# add the column
dfChampions['qty'] = dfChampions['team'].apply(lambda x: qty[x])

# keep the 2 columns
dfChamp = dfChampions2.loc[:, ['team', 'qty']]

# group by team and quantity and order
dfChamp = dfChamp.groupby('team')['qty'].count().reset_index().sort_values(by='qty',ascending=False)
#dfChamp

plt.title('Titles by team')
sns.barplot(x=dfChamp.team,y=dfChamp.qty,data=dfChamp)
plt.xticks(fontsize=6)

plt.ylabel("Quantity of titles")
plt.xlabel("Teams")
plt.ylim(0,5)
plt.show()
```
![11](https://user-images.githubusercontent.com/20979227/226176522-e2af8130-f56e-4aff-a2fb-07ea0e77c1f8.JPG)

The results are consistent for what Brasileirao is. The most competitive soccer league in the world has his championships mostly equally divided. There is not a dominant team, but Corinthians is the team with most championships. Not too far are teams like Cruzeiro, Flamengo, Palmeiras and Sao Paulo with 3 titles.

## Train a model

Time to start with some machine learning techniques.

```
import datetime
from statsmodels.stats.weightstats import DescrStatsW 
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import r2_score
from sklearn.metrics import mean_absolute_error
```

I will create a new dataframe with the dataset information to work better with the data.
```
dataset.dtypes
dfDataset = pd.DataFrame(dataset)
dfDataset
```

Now I will cut the columns year and games, and use get_dummies function to replace team name for a number that will represent each team.
In a regression model you need all the variables in a numeric format.
I have tried to implement get_dummies() function to create a new column for each team, and put a 1 in each cell that represent the team row. But working on this way, dataframe change a lot and obviously I got a ridiculous amount of columns. Also R2 change got worse, so I decided to implement a dictionary. Maybe something I should have done before, but I don't faced a situation like this one, in which one I am forced to get a dictionary somehow.
I need a dictionary that link team with an identifier number. 

```
# copy the original dataset, and create a new one to work in the model
dfDatasetModel = dfDataset

# create new dictionary
team_ids = {}

# iterate through the rows of the dataFrame
for index, row in dfDatasetModel.iterrows():
    # check if the team already has an ID
    if row['team'] not in team_ids:
        # if not, I assign it one
        team_ids[row['team']] = len(team_ids) + 1
    # keep the ID in a new column of the dataframe
    dfDatasetModel.at[index, 'team_id'] = team_ids[row['team']]

# cast to int type, I don't want an id column in a float type
dfDatasetModel['team_id'] = dfDatasetModel['team_id'].astype(int)

pd.set_option('display.max_rows', 7)
(dfDatasetModel)
```

![12](https://user-images.githubusercontent.com/20979227/226176696-e3efa193-b5f9-48d7-9141-6acb07d1c12d.JPG)

Now I will check the ids briefly
```
# Try to see each team with its indicator
dfTeams = pd.DataFrame.from_dict(team_ids, orient='index', columns=['team_id'])
pd.set_option('display.max_rows', 7)
dfTeams
```

![13](https://user-images.githubusercontent.com/20979227/226176748-302e08ca-3571-42c0-8948-974b3b313aca.JPG)

Now I know what ID Python assigned to each team through the all competitions.
It's known what teams will participate in the next 2023 Brasileirao, so I think the best way to map it, it's create a new xlsx file with the information.

I need give to the model what team will be participating in the next edition of the tournament, so this file will be very useful.
```
dfTeams2023 = pd.read_excel('teams2023.xlsx')
dfDataset = df[df['team'].isin(dfTeams2023['team'])]
dfDataset
```
The dataset reduce considerably because I won't have the information about teams that don't participate in edition of 2023.
![14](https://user-images.githubusercontent.com/20979227/226176802-96560b5f-5956-4952-89e5-846d1e8a0623.JPG)

Now it's time to keep just the columns that will be neccesary for the predictions. I already know the information of the columns:    "Year", "team", "games". 
It will be: 
 
Year: 2023

team: not neccesary, I already have the "team_id"

games: 38

```
# just keep the columns needed
dfDatasetPredictor = dfDataset[['position','points', 'victories', 'draws', 'losses', 'goals_scored', 'goals_against', 'goals_difference', 'percentage_win', 'team_id']]
dfDatasetPredictor
```

![15](https://user-images.githubusercontent.com/20979227/226176921-9c2836aa-0fa5-4337-8837-5853c0c9cad7.JPG)

```
# Split dataset into training and test sets
X_train, X_test, y_train, y_test = train_test_split(dfDatasetPredictor[['points', 'victories', 'draws', 'losses', 'goals_scored', 'goals_against', 'goals_difference', 'percentage_win','team_id']], dfDataset['position'], test_size=0.2, random_state=42)

# create regressor object
reg = LinearRegression()

# train the model
reg.fit(X_train, y_train)

# evaluate the model
score = reg.score(X_test, y_test)

# Final R2
print("R^2 score:", score)
```
R^2 score: 0.9021042875040574

0.9021042875040574 is an excellent coefficient. It means the model has a good performance and the data fits well. :blush:

90.21% of the variability of the results can be explained by the variables included in the model.

The model is so accurate with the information provided.

