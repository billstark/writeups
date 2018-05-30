# Particle Swarms Algorithm for Tetris

### A quick recap on what we have done
Previously I have introduced the interesting Tetris AI problem and how we are going to model the problem. We came up with a ___Heuristic function___ that evaluates "how good" is an arrangements of blocks on the game board. Basically, we are going to have a linear combination of `num_of_holes`, `sum_of_difference`, `landing_height`, `lines_cleared`, `height_range` and `total_height`. The problem that we left previously was that how to find corresponding coefficient for each factor. There are basically two ways to do that, __Particle Swarm Optimisation__ and __Genetic Algorithm__. Today we are going to talk about PSO.

### An interesting idea
From my understanding of this algorithm, it is trying to simulate the so called "society". So how does the algorithm work? Imagine that you have a group of people who do not know each other before, everyone of them has his/her own ___ideas and principles___. Of cause, they have their own ___position___ in this society at the beginning. Then they work, communicate, each time they have some changes in their lives, they might consider

  - "I want to reach the ___best position___ in the society!"
  - "Oh, ___from my own experience___, where is my best position? When I had the best result, reach the best position, how was I dealing with my affairs?"
  - "Oh, ___from my observation of others___, how the person who has very good result deal with his/her affairs?"

Then people might tend adjust their ___ideas and principles___ according to the considerations above. In the end, all people will be heading to the best position range in the society. That is it!

### How would the idea be applied?
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
