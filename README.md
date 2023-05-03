Download Link: https://assignmentchef.com/product/solved-cs161-homework-3-sokoban-game
<br>
In this assignment, you will write components of a program to play a variant of a game called <strong><em>Sokoban</em></strong>. This assignment will give you some experience in defining a search space for a practical problem and coming up with a good heuristic to make your program more efficient.

Honor code

Remember that you <strong>cannot</strong> use any outside reference for this or any assignment. However, you are allowed (and encouraged) to experiment with actual Sokoban games (which can be found online). Obtaining test problems and playing Sokoban from the Internet are acceptable. It is <strong>not acceptable </strong>to copy a solution for any function you have to write (even for a heuristic). In general, any idea that is not originally yours must be attributed to the appropriate sources.

If you have any questions, please contact the TA.

Background

The original <em>Sokoban</em> is a well-known puzzle video game that was first introduced in Japan in 1980. The word <em>Sokoban </em>means “warehouse keeper” in Japanese which refers to the main character of the game. In this game, the player controls the warehouse keeper (sokoban) who is placed in a closed warehouse with a number of boxes that need to be put in some predefined goal locations. The keeper can walk around the warehouse and push boxes around in order to get them into goal positions. For the purpose of this project, the goal is to let the keeper push all boxes to a goal position and return to any of the remaining goal position in the fewest number of moves. The initial states of two instances of this game are shown in Figure 1 below.







<strong>Figure 1: </strong>A Sokoban game. Boxes are presented by gems and goals are represented by circles.

<h1>How to play</h1>

The whole game (the warehouse) is on a grid. In each step, the keeper can move in any of the four basic directions (up, down, left, right). The keeper cannot walk into a wall or into a box. However, the keeper can push a box if the square next to the box (in the pushing direction) is either a goal or an empty square. Only one box can  be pushed in any given step. In the case of multiple goals, there is no specific goal that a box has to be in. Boxes can be placed in goals in any order. A box can also be pushed out of a goal if needed (to make way for other moves). The game ends as soon as every movable entity, including every box and the keeper, is in a goal position (even if there are more goals than the number of movable entities). In general, the minimum number of moves is not strictly required by the actual Sokoban game, because even finding a solution to the problem is already hard for a human player. Nevertheless, it is an objective of this assignment.

<h1>Sokoban as a search problem</h1>

In this assignment, we will turn this game into a search problem. As you have seen from class, a number of components are required for defining a search problem. In this case, they are

<ul>

 <li>A state representation</li>

 <li>A successor function that indicates which states can be reached from a given state in one step 3) A goal test</li>

</ul>

4) A cost function

Once all these components are defined (in practice as functions), the problem can be solved by a generic search engine that implements any search algorithm (e.g. DFS, BFS, A*, etc). Of course, the efficiency of the program and the quality of the solution depends on the type of search algorithm used.

Because of the time constraint, in this assignment, we will provide you with a search space formulation. You will then write some LISP functions to implement the above components based on the given formulation. Also, we will assume a uniform cost function. Every move in this game will have cost 1.

<h1>Game state representation</h1>

Each state of the game will be represented by a list of lists. The content of each square in the game will be represented by an integer. Each row of the game can then be represented by a list of integers that represent the content of each square in the row (left to right). Finally, a state is simply a list of rows (top to bottom). For simplicity, in this assignment we will assume that every row contains the same number of columns. We will assume that each state contains <strong>exactly </strong>one keeper, at least one box, and that the number of goals is greater than or equal to the number of boxes plus one to accommodate all the boxes and the keeper in the state.

Let us now introduce the representation of square contents. Each square in Sokoban can only contain so many things. A square may contain one of the following:

<ul>

 <li>Nothing (empty floor)</li>

 <li>A wall</li>

 <li>A box</li>

 <li>The keeper</li>

 <li>A goal</li>

 <li>A box on top of a goal</li>

 <li>The keeper on top of a goal.</li>

</ul>

Table 1 shows a mapping between square contents and integers that we will use to represent them. An ASCII representation of each type of content is also shown in this table. This will be discussed later.

<strong>Table 1: </strong>Mapping of square contents.

<table width="280">

 <tbody>

  <tr>

   <td width="103"><strong>Content </strong></td>

   <td width="62"><strong>Integer </strong></td>

   <td width="114"><strong>ASCII </strong></td>

  </tr>

  <tr>

   <td width="103">Blank</td>

   <td width="62">0</td>

   <td width="114">‘ ‘ (white space)</td>

  </tr>

  <tr>

   <td width="103">Wall</td>

   <td width="62">1</td>

   <td width="114">‘#’</td>

  </tr>

  <tr>

   <td width="103">Box</td>

   <td width="62">2</td>

   <td width="114">‘$’</td>

  </tr>

  <tr>

   <td width="103">Keeper</td>

   <td width="62">3</td>

   <td width="114">‘@’</td>

  </tr>

  <tr>

   <td width="103">Goal</td>

   <td width="62">4</td>

   <td width="114">‘.’</td>

  </tr>

  <tr>

   <td width="103">Box + goal</td>

   <td width="62">5</td>

   <td width="114">‘*’</td>

  </tr>

  <tr>

   <td width="103">Keeper + goal</td>

   <td width="62">6</td>

   <td width="114">‘+’</td>

  </tr>

 </tbody>

</table>




With this convention, we can represent the game state in Figure 1 in LISP (and assign it to variable p1) as:

(setq  p1  ‘((0 0 1 1 1 1 0 0 0)

(1 1 1 0 0 1 1 1 1)

(1 0 0 0 0 0 2 0 1)

(1 0 1 0 4 1 2 0 1)                                (1 0 4 0 4 1 3 0 1)

(1 1 1 1 1 1 1 1 1)) .

<h1>What is provided to you</h1>

For this assignment, we have provided you with an A* search engine in the file “a-star.lsp”. You do not need to understand the code in this file. You will call the top level A* function defined in it. To call A*, after loading the file, type at the LISP prompt

(a* start-state #’goal-test #’next-states #’heuristic).

Notice that goal-test, next-states, and heuristic have #’ in front of them. In LISP, you must put #’, called “sharp-quote”, in front of functions that you pass as arguments to other functions. You will need to define these functions in your submission. ‘heuristic’ must be substituted by the name of the heuristic function you want to use.




The above call to A* will return a solution of the search problem defined by the initial state and functions in the argument. The solution is returned in the form of a list of states along the solution path. So, if the solution returned takes 10 moves, the list returned will contain 11 elements.




In addition to “a-star.lsp”, we have also provided you with some helper functions that you may need for completing the assignment. These functions are defined in “hw3.lsp”.

<strong> </strong>

<h1>What you have to do</h1>

<ul>

 <li>(10 pts) Write a function called <strong>goal-test </strong>that takes a state as the argument and returns true (t) if and only if the state is a goal state of the game. A state is a goal state if it satisfies the game terminating condition described in “How to play” section.</li>

 <li>(50 pts) Write a function called <strong>next-states</strong> that takes a state as the argument and returns the list of all states that can be reached from the given state in one move. This function may require several helper functions. It will be explained in more details in the next section. Your score for this component will depend solely on the correctness. Efficiency will not be evaluated (each function call should never take more than a second anyway. Otherwise there might be a problem).</li>

 <li>(5 pts) Write a heuristic function called<strong> h0</strong> that returns the constant 0. This is a trivial admissible heuristic.</li>

 <li>(10 pts) Write a heuristic function called <strong>h1 </strong>that returns the number of boxes which are not on goal positions in the given state. Is this heuristic admissible? (Put you answer in the header comment of the function)</li>

 <li>(20 pts) Write an admissible heuristic to make the A* search engine perform better. Name this function <strong>hUID</strong>, where UID is your student ID (for example, <strong>h123456789</strong> if 123456789 is your UID). This heuristic function takes in a state and returns an integer (&gt;= 0). The heuristic will be scored using both the number of solved instances under a time limit and the total running time for solving the problem. (minimizing number of expanded nodes is not enough).</li>

</ul>

The remaining 5 percent of the score will be based on comments. All we look for is a brief description of and the logic behind each function you write.

<h1>Generating next states</h1>

Given a state of the game, each next state can be generated by moving the keeper in one of the valid directions. According to this formulation, each state can have at most four children. The following is one possible strategy for generating next states. You <strong>do not</strong> need to follow this, as long as your next-states function works as specified.

<h2>Implementation strategy</h2>

<ul>

 <li>Write a function called <strong>get-square </strong>that takes in a State S, a row number r, and a column number c. It should return the integer content of state S at square (r,c). If the square is outside the scope of the problem, return the value of a wall.</li>

 <li>Write a function called <strong>set-square </strong>that takes in a state S, a row number r, a column number c, and a square content v (integer). This function should return a new state S’ that is obtained by setting the square (r,c) to value v. This function should not modify the input state.</li>

</ul>

(For the above two functions, how to index rows and columns is totally internal to your program. However, one reasonable scheme would be to index the top left corner of the game with (0,0) and increase the row and column indices as you move down and move to the right respectively.)

<ul>

 <li>Write a function <strong>try-move</strong> that takes in a state S and a move direction D. This function should return the state that is the result of moving the keeper in state S in direction D. NIL should be returned if the move is invalid (e.g. there is a wall in that direction). How you represent a move direction is up to you. Remember to update the content of every square to the right value. Refer to Table 1 for the values of different types of square.</li>

</ul>

You will probably need several small helper functions to make your job easier. In any case, the highlevel plan is that your <strong>next-states</strong> function can call <strong>try-move</strong> in each of four directions from the current position of the keeper (some of them will return NIL) and collect all returned values into a list. Helper functions are provided for identifying the keeper square and for removing NILs from a list.

The function <strong>try-move</strong> has to check whether the move is legal. If it is, it will need to update some squares of the current state to reflect the changes. Use <strong>set-square</strong> to help you do this. According to this strategy, <em>at most</em> three squares need to be updated for any single move (make sure you agree).

More information about each helper function can be found in the comment in “hw3.lsp”.

<h1>Visualizing solutions</h1>

To make your experience more enjoyable, we have provided in “hw3.lsp” a printing function that will print out a state using the ASCII mapping in Table 1. In particular, you might find it useful to be able to visualize the solution returned by A*. To do so, type at the LISP prompt.

(printstates (a* start-state #’goal-test #’next-states #’heuristic) 0.2).

The last argument of printstates is the delay (in seconds) between successive state display. For example, calling

(printstates (a* start-state #’goal-test #’next-states #’heuristic) 0.2) with start-state set to p1 (defined above) results in something like Figure 2 (only the solution visualization part). An individual state can also be displayed using the printstate function. This might be useful for visualizing initial states. Please note that the visualization example that we show here has a different objective from this project. In this project, each movable entity, including the keeper, has to land in a goal position, while the keeper in this visualization example does not end in a goal position.




<strong>Figure 2: </strong>The first 20 states of an optimal solution of p1. States are order top to bottom and then left to right.

<h1>Sample outputs of next-states</h1>

In this section, we provide some sample outputs of the function <strong>next-states. </strong>Using printstates or printstate to visualize these inputs/outputs may be useful.

<h2>Example 1</h2>

(setq s1 ‘((1 1 1 1 1)

(1 4 0 4 1)

(1 0 2 0 1)

(1 0 3 0 1)

(1 0 0 0 1)

(1 1 1 1 1)

))

(next-states s1) should return

(((1 1 1 1 1) (1 4 2 4 1) (1 0 3 0 1) (1 0 0 0 1) (1 0 0 0 1) (1 1 1 1 1))

((1 1 1 1 1) (1 4 0 4 1) (1 0 2 0 1) (1 0 0 3 1) (1 0 0 0 1) (1 1 1 1 1))

((1 1 1 1 1) (1 4 0 4 1) (1 0 2 0 1) (1 0 0 0 1) (1 0 3 0 1) (1 1 1 1 1))

((1 1 1 1 1) (1 4 0 4 1) (1 0 2 0 1) (1 3 0 0 1) (1 0 0 0 1) (1 1 1 1 1))).

All four moves are possible in this case. Note how the upward move results in box pushing.

<h2>Example 2</h2>

(setq s2 ‘((1 1 1 1 1)

(1 0 0 4 1)

(1 0 2 3 1)

(1 0 0 0 1)

(1 4 0 0 1)

(1 1 1 1 1)

))

(next-states s2) should return

(((1 1 1 1 1) (1 0 0 6 1) (1 0 2 0 1) (1 0 0 0 1) (1 4 0 0 1) (1 1 1 1 1))

((1 1 1 1 1) (1 0 0 4 1) (1 0 2 0 1) (1 0 0 3 1) (1 4 0 0 1) (1 1 1 1 1))

((1 1 1 1 1) (1 0 0 4 1) (1 2 3 0 1) (1 0 0 0 1) (1 4 0 0 1) (1 1 1 1 1))).

Only three moves are possible in this case: up, left, down. Note that the upward move puts the keeper on a goal square (keeper+goal is represented by the number 6, according to Table 1).

<h2>Example 3</h2>

(setq s3 ‘((1 1 1 1 1)

(1 0 0 6 1)

(1 0 2 0 1)

(1 0 0 0 1)

(1 0 0 4 1)

(1 1 1 1 1)

))

(next-states s3) should return

(((1 1 1 1 1) (1 0 0 4 1) (1 0 2 3 1) (1 0 0 0 1) (1 0 0 4 1) (1 1 1 1 1))

((1 1 1 1 1) (1 0 3 4 1) (1 0 2 0 1) (1 0 0 0 1) (1 0 0 4 1) (1 1 1 1 1))).

Note how the keeper was on a goal square in s3. In this case, there are two possible moves: left and down. Both moves take the keeper out of the goal square.

<h2>Example 4</h2>

(setq s4 ‘((1 1 1 1 1)

(1 4 2 4 1)

(1 0 0 0 1)

(1 0 0 0 1)

(1 0 5 3 1)

(1 1 1 1 1)

))

(next-states s4) should return

(((1 1 1 1 1) (1 4 2 4 1) (1 0 0 0 1) (1 0 0 3 1) (1 0 5 0 1) (1 1 1 1 1))

((1 1 1 1 1) (1 4 2 4 1) (1 0 0 0 1) (1 0 0 0 1) (1 2 6 0 1) (1 1 1 1 1))).

A box started in a goal state in s4. There are two possible moves in this case: up and left. Moving the keeper to the left pushes the box out of the goal state and lands the keeper on the goal state.