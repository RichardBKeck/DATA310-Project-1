# DATA310 Project 1 

This project was compleated as a part of DATA 310 at William & Mary. 
The project consisted of three programming assignments. In addition to the README file, this repository contains a folder which has the .ipynb files for all three assignments.

### Question One
Question One asked the following:
```markdown
You're about to get on a plane to Boston. You want to know whether it is raining. You call 4 random friends of 
yours who live there and ask each one independently, if it's raining. The first two of your friends have a 1/2
chance of telling you the truth and, the other two have 1/4 chance of messing with you by lying. All 4 friends
tell you that "No" it isn't raining. What is the probability that it's raining in Boston?
```
This question gives you the following information:
- Two of your friends have a 50% chance of lying to you and two of your friends have a 25% chance of lying to you. These friends will be called "Friend A" and "Friend B" respectively.
- All friends answered "No" when asked about whether it is raining in Boston.
- For the purposes of this question, if one friend is lying then then that means that it is raining in Boston.
-  The probability of all your friends being truthful is equal to
```math
1/2*1/2*3/4*3/4 = 9/64
```
I approached this question using a Monte Carlo simulation.

```Python
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
Running this code for 1,000,000 trials resulted in all four friends telling the truth in 141,040 times. The probability of all four friends telling the truth (and therefore it not raining in Boston) is 0.14104. This value similar to the 9/64 value estimated above.
However, the question asks for the probability that it is raining, meaning that at least one friend. In the remaining 858,960 simulations, at least one friend lied, meaning that the probability of it raining in Boston is 0.85896. This value is similar to 55/64, the complement of the estimated probability.

### Question Two
Question Two asked the following:
```markdown
Mad Max wants to travel from New York to Dallas by the shortest possible route. He may travel over the routes shown 
in the table below. Unfortunately, the Wicked Witch can block one road leading out of Atlanta and one road leading 
out of Nashville. Mad Max will not know which roads have been blocked until he arrives at Atlanta or Nashville. 
Should Mad Max start toward Atlanta or Nashville?
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

I approached this question using a Monte Carlo simulation.

```Python
WW_Atlanta = ['St. Louis','New Orleans']
WW_Nashville = ['St. Louis','New Orleans']

dist = {'ny-atl': 866,
        'ny-nas':900,
        'nas-stl':309,
        'nas-new':542,
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

Running this code 200,000 showed that starting towards Nashville minimized the average distance that MadMax had to travel. The following graph visualizes this conclusion:
![image](https://user-images.githubusercontent.com/109169036/178625578-9229e48e-7e0f-4252-bdbd-a21e14ba8135.png)

