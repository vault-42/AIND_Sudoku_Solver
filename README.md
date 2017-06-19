# Artificial Intelligence Nanodegree
## Introductory Project: Diagonal Sudoku Solver

# Question 1 (Naked Twins)
Q: How do we use constraint propagation to solve the naked twins problem?  
A: A naked twin occurs when two identical digits are possibilities in two seperate boxes from a unit
   (column, row, square, or diagonal).
   If this occurs, the constraint is that no other box in that unit can contain those digits as possibilities.
   Therefore, those digits (that satisfy the naked twins condition) are removed from all other boxes in the unit
   other than the boxes that satisfy the naked twins condition. This constraint, with others, can be used iteratively
   to reduce the possiblities. The goal is that all boxes have only one "possiblity". See code comments for
   definitions of "box" and "unit".

# Question 2 (Diagonal Sudoku)
Q: How do we use constraint propagation to solve the diagonal sudoku problem?  
A: "Normal" sudoku requires the digits 1 thru 9 to occupy every box in each row, column, and 3x3 unit exactly
    once. Diagonal sudoku adds the requirement that 1 thru 9 must occupy each box in the diagonal exactly once.
    [Note: There are two diagonals. One starts in the top left box and moves down one, right one until it
    reaches the bottom right-most box. The other starts in the bottom left most box moving up one right one
    until it reaches the top right-most box] So if initial condition for the sudoku grid has a 3 in a box
    no other box in each unit (row, col, diagonal, 3x3) that the 3 box belongs can have a 3 therefore it is
    removed from the "peer" box's possiblities. These row, column, diagonal, and other constraints can be
    applied iteratively to reduce the box possibilities. The goal is to apply these constraints iteratively
    to reduce box possiblities to the point that each box has only one "possibility" (if possible).
    

### Install

This project requires **Python 3**.

Project used a pre-packaged Anaconda3 environment provided by the Udacity staff for the artificial intelligence nanodegree program. The pygame library was used to visualize the Sudoku updates performed by the solver. Download pygame [here](http://www.pygame.org/download.shtml).

### Code

* `solution.py` - You'll fill this in as part of your solution.
* `solution_test.py` - Do not modify this. You can test your solution by running `python solution_test.py`.
* `PySudoku.py` - Do not modify this. This is code for visualizing your solution.
* `visualize.py` - Do not modify this. This is code for visualizing your solution.

### Results

The Sudoku solver passed all unit tests and generated the following result (pygame visualization):
![Alt text](/SudokuSolver_PygameOutput.png?raw=true "Pygame Output")

