# Red_Blue_Nim_Game

## Introduction
The Red Blue Nim Game is a strategic two-player game involving red and blue marbles. Players take turns removing marbles, with the goal of forcing their opponent into a position where they cannot make a valid move. This game can be played in two versions: standard and misère. In the standard version, the player who removes the last marble wins. In the misère version, the player who removes the last marble loses.

## Objective
The objective of this project is to implement the Red Blue Nim Game using Python, allowing one player to be a human and the other to be a computer. The computer uses the minimax algorithm with alpha-beta pruning to determine its moves, providing a challenging experience for the human player.

## Scope
This project covers:
- Implementation of the game logic.
- Human and computer player interactions.
- Minimax algorithm with alpha-beta pruning for the computer's decision-making process.
- Two game versions: standard and misère.
- Scoring system to keep track of each player's moves.

## Source Code and Its Working

### Source Code

The source code for the Red Blue Nim Game is implemented in Python and is structured into a single class, `RedBlueNimGame`. This class contains all the necessary methods to initialize the game, manage the game state, handle player moves, and implement the computer's decision-making using the minimax algorithm with alpha-beta pruning. Below is a detailed explanation of the source code.

### Detailed Explanation of the Source Code

```python
class RedBlueNimGame:
    def __init__(self, num_red, num_blue, version, first_player, depth):
        self.num_red = num_red  # Number of red marbles
        self.num_blue = num_blue  # Number of blue marbles
        self.version = version  # Game version: 'standard' or 'misère'
        self.current_player = first_player  # Current player: 'human' or 'computer'
        self.depth = depth  # Depth for minimax algorithm
        self.human_score = 0  # Score for human player
        self.computer_score = 0  # Score for computer player
```
- This initializes the game with the given parameters: the number of red and blue marbles, the version of the game, the player who will start first, and the depth of the minimax algorithm.

```python
    def display_state(self):
        print(f"Red marbles: {self.num_red}, Blue marbles: {self.num_blue}")
        print(f"Current player: {self.current_player}")
```
- This method displays the current state of the game, including the number of red and blue marbles remaining and the current player's turn.

```python
    def is_game_over(self):
        return self.num_red == 0 and self.num_blue == 0
```
- This method checks if the game is over by verifying if both the red and blue marbles are zero.

```python
    def calculate_score(self):
        if self.version == 'standard':
            return 1 if self.is_game_over() else -1
        else:  # misère
            return -1 if self.is_game_over() else 1
```
- This method calculates the score based on the game version. In the standard version, the player who takes the last marble wins. In the misère version, the player who takes the last marble loses.

```python
    def make_move(self, red_taken, blue_taken):
        self.num_red -= red_taken
        self.num_blue -= blue_taken
```
- This method updates the state of the game by subtracting the number of red and blue marbles taken by the player.

```python
    def human_move(self):
        self.display_state()
        red_taken = int(input("Number of red marbles to take: "))
        blue_taken = int(input("Number of blue marbles to take: "))
        self.make_move(red_taken, blue_taken)
```
- This method handles the human player's move by prompting the user to enter the number of red and blue marbles they want to take. It then updates the game state accordingly.

```python
    def computer_move(self):
        self.display_state()
        best_score = -float('inf')
        best_move = None

        for red in range(self.num_red + 1):
            for blue in range(self.num_blue + 1):
                if red == 0 and blue == 0:
                    continue
                self.make_move(red, blue)
                score = self.minimax(self.depth, False, -float('inf'), float('inf'))
                self.make_move(-red, -blue)
                if score > best_score:
                    best_score = score
                    best_move = (red, blue)

        red_taken, blue_taken = best_move
        print(f"Computer takes {red_taken} red marbles and {blue_taken} blue marbles.")
        self.make_move(red_taken, blue_taken)
```
- This method handles the computer's move using the minimax algorithm with alpha-beta pruning to determine the best move. It evaluates all possible moves and chooses the one with the highest score.

```python
    def minimax(self, depth, is_maximizing, alpha, beta):
        if self.is_game_over() or depth == 0:
            return self.calculate_score()

        if is_maximizing:
            max_eval = -float('inf')
            for red in range(self.num_red + 1):
                for blue in range(self.num_blue + 1):
                    if red == 0 and blue == 0:
                        continue
                    self.make_move(red, blue)
                    eval = self.minimax(depth - 1, False, alpha, beta)
                    self.make_move(-red, -blue)
                    max_eval = max(max_eval, eval)
                    alpha = max(alpha, eval)
                    if beta <= alpha:
                        break
            return max_eval
        else:
            min_eval = float('inf')
            for red in range(self.num_red + 1):
                for blue in range(self.num_blue + 1):
                    if red == 0 and blue == 0:
                        continue
                    self.make_move(red, blue)
                    eval = self.minimax(depth - 1, True, alpha, beta)
                    self.make_move(-red, -blue)
                    min_eval = min(min_eval, eval)
                    beta = min(beta, eval)
                    if beta <= alpha:
                        break
            return min_eval
```
- The `minimax` method is the core of the computer's decision-making process. It recursively evaluates the game state to a specified depth to determine the best possible move. Alpha-beta pruning is used to optimize the search by eliminating branches that cannot possibly influence the final decision.

### How the Code Works

1. **Initialization**:
    - The game is initialized with a specific number of red and blue marbles, the game version (standard or misère), the first player (human or computer), and the depth for the minimax algorithm.

2. **Gameplay Loop**:
    - The game enters a loop where it alternates turns between the human and the computer until the game is over.
    - On each turn, the current player (human or computer) makes a move by choosing a number of red and blue marbles to remove.
    - After each move, the game state is updated and displayed.

3. **Human Move**:
    - When it is the human's turn, the program prompts the user to input the number of red and blue marbles to take.
    - The game state is updated based on the user's input.

4. **Computer Move**:
    - When it is the computer's turn, the computer evaluates all possible moves using the minimax algorithm with alpha-beta pruning.
    - The computer selects the move with the highest score and updates the game state accordingly.

5. **Game Over**:
    - The game loop continues until there are no more marbles left.
    - The game determines the winner based on the game version and the final state.

### Example Usage

```bash
Enter the number of red marbles: 5
Enter the number of blue marbles: 3
Enter the game version (standard/misere): standard
Who plays first (human/computer): human
```

In this example, the game is initialized with 5 red marbles, 3 blue marbles, in the standard version, with the human player taking the first move.

### Running the Code

To run the code, follow these steps:

1. **Clone the Repository**:
    ```bash
    git clone [https://github.com/yourusername/red-blue-nim-game](https://github.com/itsEkramah/DEP_Red_Blue_Nim_Game.git
    ```
2. **Navigate to the Directory**:
    ```bash
    cd DEP_Red_Blue_Nim_Game
    ```
3. **Run the Code**:
    ```bash
    python DEP_Red_Blue_Nim_Game.py
    ```
4. **Follow the Prompts**:
    - Enter the number of red and blue marbles.
    - Select the game version (standard/misere).
    - Choose the first player (human/computer).
    - Play the game by following the prompts and making moves.

By following these steps, you can play the Red Blue Nim Game and experience the strategic decision-making process implemented in Python.
## How to Use and Run This Code
### Prerequisites
- Python 3.x
- A code editor such as Visual Studio Code (VS Code) or access to Google Colab.

### Running the Code in VS Code
1. **Clone the Repository**: Clone the repository to your local machine using the following command:
    ```bash
    git clone https://github.com/itsEkramah/DEP_Red_Blue_Nim_Game.git
    ```
2. **Navigate to the Directory**: Open the cloned repository in VS Code.
3. **Run the Code**: Open the terminal in VS Code and run the following command:
    ```bash
    python DEP_Red_Blue_Nim_Game.py
    ```
4. **Follow the Prompts**: Enter the number of red and blue marbles, select the game version, and choose the first player as prompted by the program.

### Running the Code in Google Colab
1. **Upload the Code**: Upload the `red_blue_nim_game.py` file to your Google Colab environment.
2. **Run the Code**: Open a new code cell in Colab and run the following command:
    ```python
    python DEP_Red_Blue_Nim_Game.py
    ```
3. **Follow the Prompts**: Enter the number of red and blue marbles, select the game version, and choose the first player as prompted by the program.

## Example Usage
```bash
Enter the number of red marbles: 5
Enter the number of blue marbles: 3
Enter the game version (standard/misere): standard
Who plays first (human/computer): human
```

This example initializes a game with 5 red marbles, 3 blue marbles, in standard version, with the human player taking the first move.

## Conclusion
This project provides a strategic game experience using Python, demonstrating the use of game theory and artificial intelligence algorithms. Whether you are playing against the computer or another human, the Red Blue Nim Game offers a challenging and engaging way to test your strategic thinking skills.
