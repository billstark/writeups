# Tetris Ai

#### A short intro
Hi there! I am really happy that you find this short book. Basically this is my own discovery journey on how to build an Artificial Intelligence to play the game of ___Tetris___. This is my first attempt to try ai-related project and I actually find it really interesting and so I want to note down this great experience. If you are also interested in my story of building an ai to play Tetris, go ahead and have fun!

# Build an Artificial Intelligence agent to play Tetris

### To begin with
---
To be honest this project is from a school project _(CS3243, Intro to Artificial Intelligence)_. However, the topic itself is quite interesting. All of us have been playing Tetris before but we even have never thought about how to play it in order to score higher. Based on the Tetris rules, you can have high scores with two main factors

  - Of course, __number of lines cleared__.
  - If you could __eliminates multiple lines__ at the same time, the score will be high.

And there are a lot of tricks as well. For instance, you could "insert" a Tetris piece into a crack. To simplify the problem (basically, __how to create a good Tetris agent?__), we are going to have two assumptions:

  - Do no try to clear multiple lines at a time, just clear as many lines in a game as possible.
  - We do not allow inserting Tetris pieces into cracks.

### Let's get started!
---
Okay so basically we are going to create an AI that could play the game. But how? Let us think about how we are dealing with this game as a human first :). When you see the game board and a new Tetris piece, what would you do? It could be natural to try every possible __orientations__ and __positions__ of the piece and see how will the game go right? So that that is the point! Since we could get the information of the current game board and the next Tetris piece, we are able to "foresee" what will be the game like after placing the piece at certain place. Then to make sensible move, we just "somehow" rank those different possible new states and see which will lead to a longer game.

### How to evaluate a state?
---
Fine then, we are getting to the core issue of this problem. How would you evaluate what is a good state? Or, what should a good state look like? A state that will allow you to play further? __What arrangements do you hate most when you are playing the game?__ Yes! Our team has found some of them:

  - The number of "holes". A hole is defined as an empty block that has a filled block on top.
  - The pits. A pit is defined as an arrangement that a column has few filled blocks while its neighbours have many filled blocks. Here we evaluate this factor just by the total differences between two adjacent columns.

And... __What do you love to see in the game?__

  - A piece is landed to fill in the low locations in the game board.
  - Many lines are cleared.
  - The arrangement is flat.
  - There are not many filled blocs in the game!

### Then comes the Math
---
Let us define those evaluation variables. Let them be `num_of_holes`, `sum_of_difference`, `landing_height`, `lines_cleared`, `height_range` and `total_height`. Then we have:

___Score = w<sub>1</sub> holes + w<sub>2</sub> difference + w<sub>3</sub> landing + w<sub>4</sub> cleared + w<sub>5</sub> range + w<sub>6</sub> total height___

Where w<sub>i</sub> are just some constants that represents the weight of each factor. If you are a bit confused, you can think it this way: for instance, if w<sub>1</sub> is negative, then the more number of holes a state has, the smaller the score will be. Another example would be w<sub>4</sub> is larger than w<sub>5</sub>, which means that even when `lines_cleared` and `height_range` has the same value (under same measurement), `lines_cleared` will contribute more the the score than `height_range`, which means that `lines_cleared` is more likely to affect the final score.

### So how to find the weights?
---
Yes, this is the most difficult part of designing the agent. The general idea is that, we can initialise all the weights randomly at the beginning and then improve the weights along the way (while using these weights to play the Tetris games). Seems straightforward? It is, but how can you find the correct weights efficiently? Weights are Doubles and can be any value, how can the AI know the "direction" of updating weights after it finished a game? Here we have found two possible algorithms:

  - [__Genetic Algorithm__](https://en.wikipedia.org/wiki/Genetic_algorithm)
  - [__Particles Swarm Optimisation__](https://en.wikipedia.org/wiki/Particle_swarm_optimization)

And maybe we could discuss about it the next time.

# Particle Swarms Algorithm for Tetris

### A quick recap on what we have done
---
Previously I have introduced the interesting Tetris AI problem and how we are going to model the problem. We came up with a ___Heuristic function___ that evaluates "how good" is an arrangements of blocks on the game board. Basically, we are going to have a linear combination of `num_of_holes`, `sum_of_difference`, `landing_height`, `lines_cleared`, `height_range` and `total_height`. The problem that we left previously was that how to find corresponding coefficient for each factor. There are basically two ways to do that, __Particle Swarm Optimization__ and __Genetic Algorithm__. Today we are going to talk about PSO.

### An interesting idea
---
From my understanding of this algorithm, it is trying to simulate the so called "society". So how does the algorithm work? Imagine that you have a group of people who do not know each other before, everyone of them has his/her own ___ideas and principles___. Of cause, they have their own ___position___ in this society at the beginning. Then they work, communicate, each time they have some changes in their lives, they might consider

  - "I want to reach the ___best position___ in the society!"
  - "Oh, ___from my own experience___, where is my best position? When I had the best result, reach the best position, how was I dealing with my affairs?"
  - "Oh, ___from my observation of others___, how the person who has very good result deal with his/her affairs?"

Then people might tend adjust their ___ideas and principles___ according to the considerations above. In the end, all people will be heading to the best position range in the society. That is it!

### How would the idea be applied?
---
Right, let us regard those people as `Particles`. Each `Particle` will have its own initial `velocity`, which is just an interpretation of their initial "ideas". Then there is a vector that contains the weights (coefficient) of each attributes defined previously, let us call it `position`. Since people's idea will have influence on their positions in the society, we just say

___x<sup>i</sup><sub>k+1</sub> = x<sup>i</sup><sub>k</sub> + v<sup>i</sup><sub>k+1</sub>___

Where __x<sup>i</sup><sub>k+1</sub>__ refers to the new `position` of an individual, similar to __v<sup>i</sup><sub>k+1</sub>__. This is rather simple, but how we are going to update our `velocity`? There are three factors that take control of the updating: __previous velocity__, __cognitive factor__ and __society factor__.

 - ___cognitive term = c<sub>1</sub>r<sub>1</sub>(p<sup>i</sup><sub>k</sub> - x<sup>i</sup><sub>k</sub>)___
 - ___social term = c<sub>2</sub>r<sub>2</sub>(p<sup>g</sup><sub>k</sub> - x<sup>i</sup><sub>k</sub>)___

Where `c1` is a number that shows how important is the cognitive influence. `c2` is similar. Then `r1` and `r2` are just two random numbers. __p<sup>i</sup><sub>k</sub>__ is the current best position for individual while __p<sup>g</sup><sub>k</sub>__ is the current best position for the whole society. __x<sup>i</sup><sub>k</sub>__ is of course, current position. Then we add them up

___v<sup>i</sup><sub>k+1</sub> = v<sup>i</sup><sub>k</sub> * inertia + cognitive term + social term___

Where here inertia is just a term that defines how hard it is to change the `velocity` of a `Particle`. This is a constant.

After all, we just keep looping, let particles "do work", evaluate the result, and update velocity and position accordingly. After sufficient number of iterations, we are expected to observe optimal solution for `position`.

So that is basically what is PSO and how it can be applied to our specific problem (over all idea). In next section I will be talking about the specific implementation (using pseudo code).

# Particle Swarm Optimisation Implementation

### A quick recap on what we have done
---
Last time we have been talking about the general idea of PSO. It is like a social simulation of population behaviour. Basically, we stores the weights of attributes as the ___position___ of the particle and consistently update the ___position___ of the particles according to their observations of "best position" from ___their own experience___ and ___the society___. After certain number of round of update, the population would possibly head to the best position. Then here, we are applying this idea and implement our AI agent trainer!

### Pseudo code (Version 1)
---
##### Note: this is just for demonstrating the high level idea, please do not try to run this code ;)
Firstly, the most important part of the game, the main training logic

```python
PSOTrain():

  InitialisesParticles()

  # Repeat the update process a reasonable number of times
  repeat k times:
    letParticlesPlay()
    evaluateParticles()

    # particularly, update particle positions
    updateParticles()
  end

end
```

Then we break it down into functions. Firstly, have a look at `InitialisesParticles`

```python
InitialisesParticles():

  # At the beginning, we have no idea about the positions
  # so we just randomly assign values
  for p in particles:
    randomlyAssignPosition()
    randomlyAssignVelocity()
  end

end
```

Then is about how the particles are going to play the game. For simplicity, we just assume that we already have a well-defined Tetris player (an interface that will tell us the game state, the game board, the next piece, the legal moves and can execute a move in the game).

```python
letParticlesPlay():

  for p in particles:
    p.playGame()

    # here we just simply get the number of lines cleared
    p.recordResult()
  end
end

# for simplicity, I just define the functions here
playGame():

  # repeat until the particle lost the game
  while(not lost):

    # assume first move is the best
    best <- testMove(firstLegalMove)
    bestMove <- firstLegalMove

    # then for every other moves
    for move in legalMoves:

      # if it is better than best, just change it
      if testMove(move) > best:
        best <- testMove(move)
        bestMove <- move
      end
    end

    # take the move!
    takeMove(bestMove)
  end

end
```

You may notice that I left the `testMove` function because this function can have multiple ways of implementation. The basic idea for this function is just return the sum of `attributeWeight * attributeData`. Then we have `evaluateParticles`. For simplicity, for now we just say the more lines that you have cleared, the more you fit this game. After that we have `updateParticles`, which will be shown below:

```python

```
