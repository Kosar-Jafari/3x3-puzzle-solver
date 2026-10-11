3x3 puzzle solver:
3×3 Puzzle Solver This is a 3×3 puzzle solver that I made using Python. The user can enter a puzzle, and the program checks the input and whether the puzzle can be solved before starting the solving process. I also made a graphical interface using CustomTkinter, so the user can enter the puzzle and see the solving process.

How it works:
First, the program checks the numbers entered by the user. The numbers must be from 0 to 8, and no number can be repeated. The program also counts the inversions to check if the puzzle can be solved. If the number of inversions is odd, the puzzle cannot be solved, so the program shows an error message.
After the input is checked, the program starts looking for a solution. For each puzzle state, it calculates the Manhattan Distance of the tiles and uses it to calculate f with f = g + h. Here, g is the number of moves made so far, and h is the Manhattan Distance.
The open list is first sorted by f. If two states have the same f, the program also compares their h values. The state with the lower h is placed first. I added this second comparison so that the program can choose between states with the same f instead of treating them in the same way.
The program also checks for duplicate puzzle states so that the same state is not processed again. When the goal state is found, the program keeps the moves that were made to reach it and uses them to show the solution.

Interface:
The program has a graphical interface where the user can enter the puzzle and start the solving process.
After finding the solution, the program shows the moves and also displays the puzzle step by step. The number 0 represents the empty space, so it moves according to the solution and lets the user see how the puzzle changes from the starting state to the goal state.

Performance:
I also compared my algorithm with BFS using the same puzzle. BFS explored 89,597 states and took about 49 minutes to solve it, while my algorithm solved the same puzzle in about 1 second and explored only 77 states.

Installation and Usage:
First, install on your terminal:
1.pip install pillow
2.pip install customtkinter

Then run the program:
python main.py
