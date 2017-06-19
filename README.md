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

We recommend students install [Anaconda](https://www.continuum.io/downloads), a pre-packaged Python distribution that contains all of the necessary libraries and software for this project. 
Please try using the environment we provided in the Anaconda lesson of the Nanodegree.

##### Optional: Pygame

Optionally, you can also install pygame if you want to see your visualization. If you've followed our instructions for setting up our conda environment, you should be all set.

If not, please see how to download pygame [here](http://www.pygame.org/download.shtml).

### Code

* `solution.py` - You'll fill this in as part of your solution.
* `solution_test.py` - Do not modify this. You can test your solution by running `python solution_test.py`.
* `PySudoku.py` - Do not modify this. This is code for visualizing your solution.
* `visualize.py` - Do not modify this. This is code for visualizing your solution.

### Visualizing

To visualize your solution, please only assign values to the values_dict using the ```assign_values``` function provided in solution.py

### Submission
Before submitting your solution to a reviewer, you are required to submit your project to Udacity's Project Assistant, which will provide some initial feedback.  

The setup is simple.  If you have not installed the client tool already, then you may do so with the command `pip install udacity-pa`.  

To submit your code to the project assistant, run `udacity submit` from within the top-level directory of this project.  You will be prompted for a username and password.  If you login using google or facebook, visit [this link](https://project-assistant.udacity.com/auth_tokens/jwt_login for alternate login instructions.

This process will create a zipfile in your top-level directory named sudoku-<id>.zip.  This is the file that you should submit to the Udacity reviews system.

