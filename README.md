# AI Tic-Tac-Toe

## About
Created an AI program that learns to play Tic Tac Toe without supervision by adjusting behavior based on wins/losses; analyzes how different learning parameters affect adaptation and performance over time.

This lab is implemented in Python in the file `Lab4.ipynb`, which should be run using Jupyter Notebook. Open the terminal, type `jupyter notebook`, and navigate to the file through the browser interface.

The notebook consists of 7 code cells, which must be executed in order:


## Cell 1 – Library Imports:
Imports all required libraries for the program.

## Cell 2 – Global Variables:
Defines key global variables that are used and sometimes updated during gameplay:
- visual_to_logical: maps visual board positions to logical indices
- state: the current game state as a 9-character string
- turn: number of total moves made
- win_patterns: regex patterns for detecting 3-in-a-row win states for 'x' and 'o'
- state_history: logs the game states and decisions made by the program

## Cell 3 – Function Definitions:
Defines all reusable functions:
- orient_board(): determines how the visual board maps to logical state by updating visual_to_logical
- display_board(): displays the game board in a readable format
- make_move(): processes a move, prompts the player or the program to play, and logs decisions
- choose_move(): selects a move based on the STM’s learned probabilities
- update_learning(): updates STM probabilities using Algedonic compensation based on game outcome
- get_branches(): returns all possible next states for a given current state

## Cell 4 – STM Initialization:
Builds the STM used for learning. STM is a list of dictionaries indexed by game turn (0, 2, 4, 6) to narrow the search space. Each state is mapped to future possible states with associated probabilities. Terminal and duplicate states are filtered out as much as possible.

## Cell 5 – Beta Values:
Defines the beta parameters for Algedonic learning:
- betaReward: used when the program wins or ties
- betaPunish: used when the program loses

## Cell 6 – Gameplay: 
Runs a single game against the program. You can play multiple rounds; the program continuously learns across games.  
- You’re prompted to choose whether to display state strings during the game.
- The program always starts as 'x'.
- You input a letter to choose your move. Invalid inputs are rejected.
- The game ends when a player wins or the board fills.
- The result is printed and the program updates its STM accordingly.

## Cell 7 – STM Inspection:
Prints the STM. You can inspect a specific state by setting showForJustOneState to True and assigning a valid value to specificState. This is helpful for analyzing how STM probabilities evolve for particular game states.


## How the Program Works

### Reducing Redundant States
To eliminate duplicate states, the program distinguishes between the visual board (fixed layout):
```
A B C  
D E F  
G H I
```

and the logical state space (a string of 9 characters indexed 0–8). Once the first non-center move is made, the board is "oriented" so that identical board positions across different rotations/reflections map to a consistent logical state. This significantly reduces redundancy, although not perfectly.

### Terminal State Detection
Before each move:
1. The program checks for win states using regex patterns in win_patterns.
2. If no win is detected and the turn counter reaches 9, it declares a tie (treated as a reward).

### STM Construction 
The STM could not be hardcoded due to the large number of states. Instead, the get_branches() function was written to generate it dynamically in cell 4.

STM structure:
- A list with 4 elements (indexed by game turn).
- Each element is a dictionary where:
  - Keys = current states
  - Values = dictionaries of next states with probabilities
  
This makes decisions in choose_move() probabilistic and dependent on prior learning.

### Learning with Algedonic Compensation
The program uses Algedonic compensation to update STM probabilities after each game. Beta values control the amount of change:
- Rewards increase the probability of chosen actions.
- Punishments decrease them.

The update_learning() function logs state transitions from the game (state_history) and adjusts STM probabilities accordingly.


### Final Steps and Testing 
After fixing some minor bugs, further testing confirmed the program’s learning process was functioning as expected. The final system allows continuous learning and adapts over time to the player’s strategies.

Performance across various beta values is analyzed in the separate file report.txt.

