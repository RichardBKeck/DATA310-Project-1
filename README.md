# DATA310 Project 1 

This project was compleated as a part of DATA 310 at William & Mary. 
The project consisted of three programming assignments. In addition to the README file, this repository contains a folder which has the .ipynb files for all three questions.

### Question One
Question One asked the following:
```markdown
You're about to get on a plane to Boston. You want to know whether it is raining. You call 4 
random friends of yours who live there and ask each one independently, if it's raining. The 
first two of your friends have a 1/2 chance of telling you the truth and, the other two have 
1/4 chance of messing with you by lying. All 4 friends tell you that "No" it isn't raining. 
What is the probability that it's raining in Boston?
```
This question gives you the following information:
- Two of your friends have a 50% chance of lying to you and two of your friends have a 25% chance of lying to you. These friends will be called "Friend A" and "Friend B" respectively.
- All friends answered "No" when asked about whether it is raining in Boston.
- For the purposes of this question, if one friend is lying then then that means that it is raining in Boston.
-  The probability of all your friends being truthful is equal to:
```math
1/2*1/2*3/4*3/4 = 9/64
```
As such, the probability that at least one friend is lying to you is:
```math 
55/64
```

I approached this question using a Monte Carlo simulation.

```Python
import numpy as np

friend_a = ['truth','lie']
friend_b = ['truth','truth','truth','lie']
def Boston_rain (trials):
  all_four_truth = 0 
  for i in range(trials):
    responses = [np.random.choice(friend_a,replace=True),
                 np.random.choice(friend_a,replace=True),
                 np.random.choice(friend_b,replace=True),
                 np.random.choice(friend_b,replace=True)]
    lies = (sum(1 for i in responses if i =='lie'))
    if lies == 0:
      all_four_truth += 1
  prob_truth = all_four_truth/trials
  prob_lie = (1-prob_truth)
  
  #Outputs
  print('Number of Trials: '+str(trials))
  print ('All four of your friends told the truth',str(all_four_truth),'times')
  print('Therefore, the probability that your friends all told the truth is', str(prob_truth))
  print('Since all your friends answered "No", and probability that at least one lied is',str(prob_lie), 
  "the chance that it's raining in Boston is",str(prob_lie))
```
Running this code for 100,000 trials resulted in all four friends telling the truth in 13,902 times. The probability of all four friends telling the truth (and therefore it not raining in Boston) is 0.13902. This value similar to the 9/64 value estimated above.
However, the question asks for the probability that it is raining, meaning that at least one friend. In the remaining 86,098 simulations, at least one friend lied, meaning that the probability of it raining in Boston is 0.86098. This value is similar to 55/64, the complement of the estimated probability.

### Question Two
Question Two asked the following:

```markdown
Mad Max wants to travel from New York to Dallas by the shortest possible route. He may travel over 
the routes shown in the table below. Unfortunately, the Wicked Witch can block one road leading 
out of Atlanta and one road leading out of Nashville. Mad Max will not know which roads have been 
blocked until he arrives at Atlanta or Nashville. Should Mad Max start toward Atlanta or Nashville?
```
|       Route             | Length of Route (Miles) |
| ------------------------| ----------------------- |
| New York - Atlanta      |          866            |
| New York - Nashville    |          900            |
| Nashville - St. Louis   |          309            |
| Nashville - New Orleans |          532            |
| Atlanta - St. Louis     |          555            |
| Atlanta - New Orleans   |          470            |
| St. Louis - Dallas      |          662            |
| New Orleans - Dallas    |          505            |

This diagram visualizes the possible routes between New York and Dallas

![image](https://user-images.githubusercontent.com/109169036/178830749-c2168d31-f56f-47b7-85eb-b5a02b9a017f.png)

I approached this question using a Monte Carlo simulation where the Wicked Witch selects one road to block out of Atlanta and one road to block out of Nashville. These random selection inform two conditional statements which select the unblocked road and add the distance of that route to a counter outside of the loop. After the loop runs for all the range of the input value, the average distance for Mad Max is computed based on the initial choice. Using that average, the initial decision with the lowest average distance is identified as the best option.

```Python
import numpy as np
import matplotlib.pyplot as plt

WW_Atlanta = ['St. Louis','New Orleans']
WW_Nashville = ['St. Louis','New Orleans']

dist = {'ny-atl': 866,
        'ny-nas':900,
        'nas-stl':309,
        'nas-new':532,
        'atl-stl':555,
        'atl-new':470,
        'stl-dal':662,
        'new-dal':505}
def MadMax(trials):
  Atlanta_Dist = 0
  Nashville_Dist = 0
  atl_graph = []
  nas_graph = []
  for i in range(trials):
    Atlanta_Choice = np.random.choice(WW_Atlanta,replace=True)
    if Atlanta_Choice == 'St. Louis':
      travel = (dist['ny-atl']+dist['atl-new']+dist['new-dal'])
      Atlanta_Dist += travel
    else:
      travel2 = (dist['ny-atl']+dist['atl-stl']+dist['stl-dal'])
      Atlanta_Dist += travel2
    Nashville_Choice = np.random.choice(WW_Nashville,replace=True)
    if Nashville_Choice == 'St. Louis':
      travel = (dist['ny-nas']+dist['nas-new']+dist['new-dal'])
      Nashville_Dist += travel
    else:
      travel = (dist['ny-nas']+dist['nas-stl']+dist['stl-dal'])
      Nashville_Dist += travel
    

    atl_graph.append(Atlanta_Dist/(i+1))
    nas_graph.append(Nashville_Dist/(i+1))
  plt.plot(atl_graph, label='Atlanta')
  plt.plot(nas_graph, label='Nashville')
  plt.tick_params(axis='x', colors='navy')
  plt.tick_params(axis='y', colors='navy')
  plt.xlabel('Repetitions of Experiment',fontsize=14,color='green')
  plt.ylabel('Experimental Probability',fontsize=14,color='green')
  plt.legend()
  plt.show()

  avg_dist_atl = Atlanta_Dist/trials
  avg_dist_nas = Nashville_Dist/trials

  #Outputs
  print('Number of Trials: '+str(trials))
  print('Average Distance When Starting towards Atlanta: '+ str(avg_dist_atl)+' miles')
  print ('Average Distance When Starting towards Nashville: '+ str(avg_dist_nas)+' miles')

  if avg_dist_atl < avg_dist_nas:
    print('Mad Max should start towards Atlanta')
  elif avg_dist_atl > avg_dist_nas:
    print ('Mad Max should start towards Nashville')
  else:
    print ('A tie exists')
```

Running this code 1,000 showed that starting towards Nashville minimized the average distance that Mad Max had to travel. The following graph visualizes this conclusion:
![image](https://user-images.githubusercontent.com/109169036/178846992-1e5d3232-2cf4-4d43-907b-b653ab2527c8.png)

### Question Three
Question Three asked the following:
```markdown
Simulate a population of 20000 individuals from a beta distribution that has the parametrization
a=1.6 and b=2.1. Select400 simple random samples of size 32 from this population and show that 
the sample means are normally distributed by using histograms, distributional plots, 
Quantile-Quantile plots and normality tests
```
The first step with is question is to generate the Beta Distribution. Then, using that distribution, the mean of 400 random samples of 32 will be taken. from there, the mean will be used to generate a histogram, a distributional plot, and Quantile-Quantile plot, and ran through the Kolomogorov-Smirnov Test and the Anderson-Darling Test.
The following code accomplishes all of those steps.

```Python
# Inputs
import numpy as np
from scipy import stats
from scipy.stats import beta
from scipy.stats import norm
import statsmodels.api as sm
import seaborn as sns
sns.set(color_codes=True)
CDF = norm.cdf
import warnings
warnings.simplefilter(action='ignore')

# Functions
def zscore(x):
  return(x-np.mean(x))/np.std(x)


# Generate the Beta Distribution
beta_data = beta.rvs(a=1.6,b=2.1,size=20000)

#Generate the random samples and the means
trials=400
values = []

for i in range(trials):
  test = np.random.choice(beta_data,size=32,replace= False ,p=None)
  values.append(np.mean(test))

#Generate the Histogram
print('Histogram')
import seaborn as sns
import matplotlib.pyplot as plt

ax1 = sns.histplot(values,
                  kde=False,
                  color='lightpink')
ax1.set(xlabel='Normal', ylabel='Frequency')
plt.show()

#Generate the Distributional Plot
print('Distributional Plot')
ax2 = sns.distplot(values,
                  kde=False,
                  color='deepskyblue',
                  hist_kws={"color":'lightpink'},
                  fit=stats.norm,
                  fit_kws={"color":'deepskyblue'})
ax2.set(xlabel='Normal', ylabel='Frequency')
plt.show()

#Generate the Quantile-Quantile plots
print('Quantile-Quantile Plots')

sm.qqplot((values-np.mean(values))/np.std(values), loc = 0, scale = 1, line='s',alpha=0.5)
plt.xlim([-3,3])
plt.ylim([-3,3])
plt.axes().set_aspect('equal')
plt.grid(b=True,which='major', color ='grey', linestyle='-', alpha=0.5)
plt.grid(b=True,which='minor', color ='grey', linestyle='--', alpha=0.15)
plt.minorticks_on()
plt.show()

#Conduct the Normality Tests
#Kolmogorov-Smirnov Test
KMT = stats.kstest(zscore(values),'norm')
#Anderson-Darling Test
ADT = stats.anderson(zscore(values),'norm')

#Outputs
print('Kolomogorov-Smirnov Test:',KMT)
print('Anderson-Darling Test:',ADT)
```
Running this code resulted in the following graphs:

***Histogram***
![image](https://user-images.githubusercontent.com/109169036/178628198-b203b7ed-6a95-415e-bd5c-ca474b847275.png)

***Distributional Plot***
![image](https://user-images.githubusercontent.com/109169036/178628266-4484a72d-72ab-45e3-8d4f-446aaea59f9c.png)

***Quantile-Quantile Plot***
![image](https://user-images.githubusercontent.com/109169036/178630579-bcdbfce8-fa6a-4151-abc1-001536914240.png)

These plots, combined with the results from the Kolomogorov-Smirnov Test and the Anderson-Darling Test strongly suggests that the means of random samples taken from a beta distribution are normally distributed.

The following block of code was used for all questions that involved a graph:

```Python
# this is for displaying plots with high resolution
#@title
%matplotlib inline
%config InlineBackend.figure_format = 'retina'
import matplotlib.pyplot as plt
import matplotlib as mpl
mpl.rcParams['figure.dpi'] = 120
```
