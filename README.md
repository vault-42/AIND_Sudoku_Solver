# Artificial Intelligence Nanodegree
## Introductory Project: Diagonal Sudoku Solver

# Question 1 (Naked Twins)
Q: How do we use constraint propagation to solve the naked twins problem?  
A: A naked twin occurs when two identical digits are possibilities in two separate boxes from a unit (column, row, square, or diagonal). If this occurs, the constraint is that no other box in that unit can contain those digits as possibilities. Therefore, those digits (that satisfy the naked twins condition) are removed from all other boxes in the unit other than the boxes that satisfy the naked twins condition. This constraint, with others, can be used iteratively to reduce the possibilities. The goal is that all boxes have only one "possibility". See code comments for definitions of "box" and "unit".

# Question 2 (Diagonal Sudoku)
Q: How do we use constraint propagation to solve the diagonal sudoku problem?  
A: "Normal" sudoku requires the digits 1 thru 9 to occupy every box in each row, column, and 3x3 unit exactly once. Diagonal sudoku adds the requirement that 1 through 9 must occupy each box in the diagonal exactly once. [Note: There are two diagonals. One starts in the top left box and moves down one, right one until it reaches the bottom right-most box. The other starts in the bottom left most box moving up one right one until it reaches the top right-most box] So if initial condition for the sudoku grid has a 3 in a box no other box in each unit (row, col, diagonal, 3x3) that the 3 box belongs can have a 3 therefore it is removed from the "peer" box's possibilities. These row, column, diagonal, and other constraints can be applied iteratively to reduce the box possibilities. The goal is to apply these constraints iteratively to reduce box possibilities to the point that each box has only one "possibility" (if possible).
    

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
![Alt text](/images/SudokuSolver_PygameOutput.png?raw=true "Pygame Output")

### Summary
This program solves 9x9 grid Sudoku puzzles using constraint propogation and search techniques. As you may recall from the Sudoku craze from the '00s, the objective of Sudoku is to fill in a square 9x9 grid of boxes with digits 1 through 9 so that each unit contains all of the digits 1 through 9 without repeating. A unit in "normal" sudoku is a column, row or one of the nine 3x3 subgrids. For each box, all the other boxes that share the same row, column, or subgrid will be called a "peer". The initial puzzle is the grid only partially filled and it is up to the player to fill in the remaining boxes following the rules outlined above. The picture below shows the initial puzzle to the left and the completed puzzle to the right. Image modified from original artwork done by [1].
![Alt text](/images/Sudoku_Puzzle_Example.jpg?raw=true)

#### Constraint Propagation
In order to solve Sudoku puzzles players typically keep track of all the possible values a box could have and then work to eliminate potential boxes using strategies around the Sudoku rules. A more in depth article on Sudoku strategies can be found at [2]. This program runs a (while not stalled) loop over three possible value reduction strategies:
* 'Elimination' - For each box with a single value, remove its value from the possible values of all its peers.
'''python
def eliminate(values):
    """
    Go through all the boxes, and whenever there is a box with a value, eliminate this value from the 
    values of all its peers.
    Args:
        A sudoku in dictionary form.
    Returns:
        The resulting sudoku in dictionary form.
    """
    solved_values = [box for box in values.keys() if len(values[box]) == 1]
    for box in solved_values:
        digit = values[box]
        for peer in peers[box]:
            assign_value(values, peer, values[peer].replace(digit,''))
    return values
'''
* 'Only-choice' - If a box in a unit only allows a specific value, then that box must be assigned that value.
'''python
def only_choice(values):
    """
    Go through all the units, and whenever there is a unit with a value that only fits in one box,
    assign the value to this box.
    Args:
        A sudoku in dictionary form.
    Returns:
        The resulting sudoku in dictionary form.
    """
    for unit in unitlist:
        for digit in '123456789':
            dplaces = [box for box in unit if digit in values[box]]
            if len(dplaces) == 1:
                #values[dplaces[0]] = digit
                assign_value(values, dplaces[0], digit)
    return values
'''
* 'Naked-twins' - If two boxes in the same unit have the same two digits (ie they are naked twins) then remove their digits from the potential values of peer boxes.
'''python
def naked_twins(values):
    """Eliminate values using the naked twins strategy.
    Args:
        values(dict): a dictionary of the form {'box_name': '123456789', ...}

    Returns:
        the values dictionary with the naked twins eliminated from peers.
    """
    for unit in unitlist:
        unitvalues = [values[box] for box in unit]
        # Find all instances of naked twins
        towDuplicateDigits = [boxVal for boxVal,count in Counter(unitvalues).items() if count==2 and len(boxVal) == 2]
        # Eliminate the naked twins as possibilities for their peers
        for duplicate in towDuplicateDigits:
            rmvDigits = duplicate[0] + '|' + duplicate[1]
            for box in unit:
                if duplicate != values[box]:
                    assign_value(values,box,re.sub(rmvDigits, '', values[box]))
    return values
'''

#### Search
If the constraint propagation strategies can no longer make any reductions and the puzzle is still not solved, the program will search for a solution.  The program implements a depth-first search by starting with one of the unfilled squares with the fewest possibilities and trying to solve the puzzle with the remaining box possibilities. This is essentially a trial and error approach to solving the puzzle. Peter Norvig's Solving Every Sudoku Puzzle [3] contains some performance analysis showing that depth-first search with constraint propagation strategies can solve "hard" Sudoku puzzles in seconds.
'''python
def search(values):
    """
    Using depth-first search and propagation, try all possible values.
    Args:
        A sudoku in dictionary form.
    Returns:
        The resulting sudoku in dictionary form.
    """
    # First, reduce the puzzle using the constraint propagation strategies
    values = reduce_puzzle(values)
    if values is False:
        return False # Failed earlier
    if all(len(values[s]) == 1 for s in boxes):
        return values # Solved
    #Choose one of the unfilled squares with the fewest possibilities
    n,s = min((len(values[s]), s) for s in boxes if len(values[s]) > 1)
    # Now using recurrence to solve each one of the resulting sudokus,
    #and if one returns a value (not False), return the answer.
    for value in values[s]:
        new_sudoku = values.copy()
        new_sudoku[s] = value
        attempt = search(new_sudoku)
        if attempt:
            return attempt
'''

#### Diagonal Sudoku Support
This program also supports diagonal Sudoku puzzles. The only difference between diagonal Sudoku rules and the rules discussed above is an additional rule that the digits 1 through 9 must be used only once for the grids diagonals (top-left corner to bottom-right corner and top-right corner to bottom-left corner). The only change necessary to support this puzzle type is to add diagonal peers to the boxes in the grid's diagonals. The code for this is in the program's constructor below:
'''python
boxes = cross(rows, cols)
row_units = [cross(r, cols) for r in rows]
column_units = [cross(rows, c) for c in cols]
square_units = [cross(rs, cs) for rs in ('ABC','DEF','GHI') for cs in ('123','456','789')]
diagonal_units = []
diagonal_units = [s+t for (s,t) in zip(rows,cols)]
diagonal_units = [diagonal_units] + [[s+t for (s,t) in zip(reversed(rows),cols)]]
if diagonal_sudoku:
    unitlist = row_units + column_units + square_units + diagonal_units
else: #"Normal" Sudoku
    unitlist = row_units + column_units + square_units
units = dict((s, [u for u in unitlist if s in u]) for s in boxes)
peers = dict((s, set(sum(units[s],[]))-set([s])) for s in boxes)
'''

### References
[1] Tim Stellmach, CC0, https://commons.wikimedia.org/w/index.php?curid=57831926
[2] Sudoku Dragon, http://www.sudokudragon.com/sudokustrategy.htm
[3] Peter Norvig, http://norvig.com/sudoku.html
