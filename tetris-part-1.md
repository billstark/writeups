# Build an Artificial Intelligence agent to play Tetris

### To begin with
To be honest this project is from a school project _(CS3243, Intro to Artificial Intelligence)_. However, the topic itself is quite interesting. All of us have been playing Tetris before but we even have never thought about how to play it in order to score higher. Based on the Tetris rules, you can have high scores with two main factors

  - Of course, __number of lines cleared__.
  - If you could __eliminates multiple lines__ at the same time, the score will be high.

And there are a lot of tricks as well. For instance, you could "insert" a Tetris piece into a crack. To simplify the problem (basically, __how to create a good Tetris agent?__), we are going to have two assumptions:

  - Do no try to clear multiple lines at a time, just clear as many lines in a game as possible.
  - We do not allow inserting Tetris pieces into cracks.

### Let's get started!
Okay so basically we are going to create an AI that could play the game. But how? Let us think about how we are dealing with this game as a human first :). When you see the game board and a new Tetris piece, what would you do? It could be natural to try every possible __orientations__ and __positions__ of the piece and see how will the game go right? So that that is the point! Since we could get the information of the current game board and the next Tetris piece, we are able to "foresee" what will be the game like after placing the piece at certain place. Then to make sensible move, we just "somehow" rank those different possible new states and see which will lead to a longer game.

### How to evaluate a state?
Fine then, we are getting to the core issue of this problem. How would you evaluate what is a good state? Or, what should a good state look like? A state that will allow you to play further? __What arrangements do you hate most when you are playing the game?__ Yes! Our team has found some of them:

  - The number of "holes". A hole is defined as an empty block that has a filled block on top.
  - The pits. A pit is defined as an arrangement that a column has few filled blocks while its neighbours have many filled blocks. Here we evaluate this factor just by the total differences between two adjacent columns.

And... __What do you love to see in the game?__

  - A piece is landed to fill in the low locations in the game board.
  - Many lines are cleared.
  - The arrangement is flat.
  - There are not many filled blocs in the game!

### Then comes the Math
Let us define those evaluation variables. Let them be `num_of_holes`, `sum_of_difference`, `landing_height`, `lines_cleared`, `height_range` and `total_height`. Then we have:

___Score = w<sub>1</sub> holes + w<sub>2</sub> difference + w<sub>3</sub> landing + w<sub>4</sub> cleared + w<sub>5</sub> range + w<sub>6</sub> total height___

Where w<sub>i</sub> are just some constants that represents the weight of each factor. If you are a bit confused, you can think it this way: for instance, if w<sub>1</sub> is negative, then the more number of holes a state has, the smaller the score will be. Another example would be w<sub>4</sub> is larger than w<sub>5</sub>, which means that even when `lines_cleared` and `height_range` has the same value (under same measurement), `lines_cleared` will contribute more the the score than `height_range`, which means that `lines_cleared` is more likely to affect the final score.

### So how to find the weights?
Yes, this is the most difficult part of designing the agent. The general idea is that, we can initialise all the weights randomly at the beginning and then improve the weights along the way (while using these weights to play the Tetris games). Seems straightforward? It is, but how can you find the correct weights efficiently? Weights are Doubles and can be any value, how can the AI know the "direction" of updating weights after it finished a game? Here we have found two possible algorithms:

  - [__Genetic Algorithm__](https://en.wikipedia.org/wiki/Genetic_algorithm)
  - [__Particles Swarm Optimisation__](https://en.wikipedia.org/wiki/Particle_swarm_optimization)

And maybe we could discuss about it the next time.
