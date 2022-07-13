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
I approached this question using a Monte Carlo Simulation written in Python

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
  print('Since all your friends answered "No", and probability that at least one lied is',str(prob_lie), "the chance that it's raining in Boston is",str(prob_lie))
```

