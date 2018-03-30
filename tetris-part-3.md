# Particle Swarm Optimisation Implementation

### A quick recap on what we have done
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
updateParticles():

  for p in particles:

    # Note here, positions, velocities are stored as array
    # every position and velocity need to be updated separately
    for index, v in velocity:
      cognitive <- random1 * c1 * selfBestPosition[index] - currentPosition[index]
      social <- random2 * c2 * globalBestPosition[index] - currentPosition[index]
      v <- inertia * v + cognitive + social
    end

    for index, w in position:
      w <- w + position[index]
    end
  end
end
```

Yes then we have finished! Quite simple right? However, this implementation does not behave very well in the exact training. In my next chapter I will be talking about why and how to improve it!
