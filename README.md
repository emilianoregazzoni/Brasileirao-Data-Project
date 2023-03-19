#  Brazilian Football Confederation :factory: :memo: :soccer: 
## __Brasileirao (CBF) Data Science Project__  

Brasileirao is the most important competition in South America.
In 2021 the competition was chosen by the IFFHS as the strongest national league in South America as well as the strongest in the world. Surpassing Premier League.

Scenario: I am passionate about soccer. Since I was a child I love this sport and one of my dreams, maybe one day, work for a soccer team, 
in a data scientist position.
I want to work with Brasileirao information, and try to discover things about Brazil soccer. Search on the data, clean the data, create some visualizations, identify some patterns and train a model with the data.

:scroll: The stages I am going to follow are:  
Prepare - Process - Analyze - Share - Predict


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



## Analyze


## Share


## Predict
