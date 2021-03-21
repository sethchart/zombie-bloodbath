# Sorting

In this post you will learn a little bit about sorting lists of integers using python.
If you would prefer to read this post as a Jupyter notebook, you can grab it from github [here](https://github.com/sethchart/zombie-bloodbath/blob/main/Zombie_Bloodbath.ipynb).

## Why am I writing this?

Yesterday I had my first live code test as part of a technical interview for a Data Science role. 
One of the questions was:
> Write a function in python that sorts a list of integers.

People have been telling me to study data structures and algorithms since I finished my Data Science bootcamp.
I know that I should be working [LeetCode](https://leetcode.com/) problems at least a few times per week and reading up on DS&A, but I have been more focused on other things. My bad.

Anyway, I think I muddled my way through to something that was close to right, but I know that sorting a list of integers is probably the single most common DS&A question in the tech industry. 
I was a little embarrassed that I didn't have a slick answer ready to go.
That embarrassment did not do me any favors when I tried to solve the problem.

It is too late for me to be prepared, but I can learn from my mistake.
Keep reading if you would like to learn from my mistake as well.

## What you will learn.

In this post:
* I will walk through the reasoning required to get a basic sorting function written. I am going to try to make this part memorable so that I can remember it next time someone throws down this particular gauntlet.
* I will talk a little bit about the _time complexity_ of my basic sorting algorithm. I will keep this light because there are plenty of places to learn about time complexity.

# The Basic Algorithm
I am going to do my best to write this part without looking anything up, then I am going to put a link to whichever algorithm seems closest to what I come up with below so that you can go look at other examples of how to implement that sorting algorithm.

## Walk before you run

So I know that I want to be able to take a list on integers like `[12, 3, -7, 5, 2, 3]` and sort it into either `[-7, 2, 3, 3, 5, 12]` or `[12, 5, 3, 3, 2, -7]`. 
I would be happy with either one.

Before I tackle the problem of sorting the list in increasing or decreasing order, I am going to try to solve a simpler problem. 
> Find the biggest number in the list.

I want to try to make this easy to remember, so I am going make my approach a little dramatic.
I am going to make these numbers fight... to the death!
These are not fair fights.
The bigger number always wins.
If equal numbers fight, the one on the left wins.
Let's write a function to represent a fight between two numbers.


```python
def fight(a: int, b: int) -> int:
    """This function implements a fight to the death between two integers a 
    and b and returns the winner.
    """
    if a >= b:
        winner = a
    else:
        winner = b
    return winner

assert fight(-10, 5) == 5, "Negative ten is unexpectedly scrappy!"
assert fight(7, 5) == 7, "Poor show seven, better luck next time..."
assert fight(7, 7) == 7, "How did you manage to get that wrong?"
```

Cool, that seems to have worked.
Maybe I needed to stand up before I could walk?

### Finding the biggest number

Alright, I can make any two numbers fight to the death. 
If I want to find the biggest number, I can just run a bunch of fights.
A tournament of numbers!
I will pick two contenders, make them fight, then pick a new contender to fight the winner of the last fight.
I will keep doing this until only one number is left alive. 
That number had to be bigger than (or equal to) all of the dead numbers, because it fought them and won.
Therefore, the last number standing is the largest number from my list.
Let's write a function that represents this tournament of numbers.


```python
from typing import List

def tournament(contestants: List[int]) -> int:
    """This function implements a tournament. Only one will survive! Returns 
    the winner of the tournament."""
    winner = contestants[0]
    for contestant in contestants[1:]:
        winner = fight(winner, contestant)
    return winner

assert tournament([12, 3, -7, 5, 2, 3]) == 12, 'That was not all roses.'
assert tournament([6, 3, 3, 13, -9]) == 13, 'Your baker is cheap.'
assert tournament([-7, 2, 3, 3, 5, 12]) == 12, 'Why would order matter?'
```

Neat, that worked too.

## Sorting with a Zombie Bloodbath

Alright, I can find the largest number in a list by using a tournament.
So how do I sort a list of numbers?
Let's take our input list and move numbers into an output list.
We will start by moving the largest number to the output list, then iteratively append the largest of the remaining unplaced numbers until we run out of numbers.

Boring!
We will run a tournament, the winner goes to the hall of champions.
Then we resurrect all of the losers and have another tournament, the winner goes to the hall of champions and stands to the right of the first winner.
Keep resurrecting the remaining losers, running a tournament, and sending the winner to the hall of champions.
When there is only one loser left, resurrect them and send them to the hall of champions in shame.
The hall of champions is our sorted list.
Since the largest of all of the remaining numbers is always added to the right end of the hall of champions, we will end up with a list that is sorted from left to right, largest to smallest.
The best part...
This can only possibly be described as the Zombie Bloodbath sorting algorithm.

Let's write a function that represents this Zombie Bloodbath.


```python
def zombie_blood_bath(contestents: List[int]) -> List[int]:
    """This function implements a Zombie Bloodbath. Sucesive tournaments send
    thier winners to the hall of champions (sorted list)."""
    losers = contestents
    hall_of_champions = []
    while len(losers) > 1:
        winner = tournament(losers)
        losers.remove(winner)
        hall_of_champions.append(winner)
    hall_of_champions.append(losers[0])
    return hall_of_champions

assert zombie_blood_bath([12, 3, -7, 5, 2, 3]) == [12, 5, 3, 3, 2, -7], "Nope"
assert zombie_blood_bath([3, 6, 2, -34, 15]) == [15, 6, 3, 2, -34], "Also no."
```

Well that seems to have worked.
If only I would have gone into a protracted and mumbling monologue about a Zombie Bloodbath during my technical interview...


```python
zombie_blood_bath([12, 3, -7, 5, 2, 3])
```




    [12, 5, 3, 3, 2, -7]



## What did I actually come up with?

I think that the Zombie Bloodbath sort algorithm is essentially Selection sort. Check out the links below for more information.
* [Wikipedia](https://en.wikipedia.org/wiki/Selection_sort)
* [Google search](http://www.google.com/search?q=implement+selection+sort+algorithm+python)

By subjecting you to this tortured metaphor, I have diverged from Selection sort in the following ways:
* My list is sorted in descending order. 
* I have introduced some extra variables, which means I am using unnecessary memory.
* I have written three functions instead of one, which arguably makes my implementation more complicated. On the other hand, I might be able to defend my approach by appealing to the [single-responsibility principle](https://en.wikipedia.org/wiki/Single-responsibility_principle).

# How many fights in a Zombie Bloodbath?

So I have come up with an algorithm. 
I have some questions:
 1. Is it good?
 2. What do I mean by good?
 3. What are words?
 4. If I see the color red and you see the color red, do we have the same experience of the color red?
 5. Why?
 6. Why?

```python
---------------------------------------------------------------------------
QuestionError                                 Traceback (most recent call last)
<ipython-input-4.5-ca42ed42e993> in <module>
----> I have some questions:

QuestionError: Too many follow-up questions. Terminated recursive why loop.
```

Let me answer the first two questions.
 1. My algorithm is not good.
 2. By good, I mean that an algorithm has optimal worst-case [time complexity](https://en.wikipedia.org/wiki/Time_complexity). Roughly, a sorting algorithm is good if it does not require an unnecessarily large number of steps to sort the list.
 
Let's analyze the Zombie Bloodbath algorithm. 
To keep things simple, let's just count the number of fights in a Zombie Bloodbath.
For us fights will stand in for steps.

If I start with the list `[12, 3, -7, 5, 2, 3]`, then to obtain the sorted list `[12, 5, 3, 3, 2, -7]` my Zombie Bloodbath would consist of the following tournaments:
1. A tournament with all six contestants consisting of five fights.
2. A tournament with five remaining contestants consisting of four fights.
3. A tournament with four remaining contestants consisting of three fights.
4. A tournament with three remaining contestants consisting of two fights.
5. A tournament with two remaining contestants consisting of one fight.

Adding up all of the fights we get a total of fifteen.
$$ 5 + 4 + 3 + 2 + 1 = 15 $$

Time complexity is all about how the number of steps grows as the size of the input grows.
In our case, we want to know how many fights happen during a Zombie Bloodbath that starts with a list of $k$ numbers.
By the same reasoning as above, we would start with a tournament with $k-1$ fights and work our way down to the last tournament with just one fight.
If you are familiar with [Triangular Numbers](https://en.wikipedia.org/wiki/Triangular_number), then you know that when we sum up all of the fights in our Zombie Bloodbath, we will get the following.
$$ (k-1) + (k-2) + ... + 2 + 1 =  \tfrac{1}{2}k^2 - \tfrac{1}{2}k = O\left(k^2\right)$$

In short, Zombie Bloodbath is a quadratic algorithm.

## Merge sort
It turns out that we can sort our list in fewer fights. 
One algorithm that achieves the optimal worst-case time complexity is [Merge sort](https://en.wikipedia.org/wiki/Merge_sort). Merge sort is a log-linear algorithm, or in big-O notation $O\left(k\log k\right)$.

# Conclusion

In this post, have tried to give you a memorable mental model that will allow you to write a basic sort algorithm from scratch next time someone asks you. Below you can see what I might write next time. 


```python
def fight(a, b):
    if a <= b:
        winner = b
    else:
        winner = a
    return winner

def tournament(lst):
    winner = lst[0]
    for contestant in lst[1:]:
        winner = fight(winner, contestant)
    return winner

def my_sort(lst):
    sorted_lst = []
    losers = lst
    while len(losers)>1:
        winner = tournament(losers)
        sorted_lst.append(winner)
        losers.remove(winner)
    sorted_lst.append(losers[0])
    return sorted_lst
    
assert my_sort([12, 3, -7, 5, 2, 3]) == [12, 5, 3, 3, 2, -7], "Nope"
assert my_sort([3, 6, 2, -34, 15]) == [15, 6, 3, 2, -34], "Also no."
```


```python
my_sort([12, 3, -7, 5, 2, 3])
```




    [12, 5, 3, 3, 2, -7]


