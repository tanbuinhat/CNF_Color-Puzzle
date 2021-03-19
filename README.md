# Resolution_CNF


Given a matrix of size m x n, where each cell will be a non-negative integer or have no value (empty cell). Each cell will have 8 adjacent cells, and here the cell itself is counted as adjacent to itself. The player will have to paint red and blue in all the cells on the matrix, so that the number of "adjacent" blue squares to a correct square is equal to the number inside that cell. 

<img width="396" alt="Screen Shot 2021-03-19 at 22 47 18" src="https://user-images.githubusercontent.com/60350737/111807874-00bb0e00-8906-11eb-9c8c-a8e8fbb0f9f5.png">

Place a logical variable in each cell on the matrix (if the value is True, the cell is colored blue and False is red), the constraints are written for each cell with a number to obtain a set of clauses CNF type binding. Then, using the pysat library to solve, we find the True / False value for each variable and deduce the result. 


Suggestions
* traverses each cell containing a number to write constraint clauses of the CNF form
* synthesize clauses and filter out duplication

How to write a constraint for each cell containing a number, for example case number 2
* Put 9 logical variables (a, b, c,…, i) corresponding to 8 surrounding cells and cell at number 2
  * if a variable is True its cell is green
  * If a variable is False, its cell is red
  
  
  <img width="241" alt="Screen Shot 2021-03-19 at 22 53 37" src="https://user-images.githubusercontent.com/60350737/111807951-17616500-8906-11eb-9ade-050097f03be7.png">

  
* Suppose we select the cell corresponding to the variable a, b is blue, then equivalent to this, the remaining 7 cells must be red. 
a ^ b <-----> -c ^ -d ^ -e ^ -f ^ -g ^ -h ^ -i
* Convert the above two-dimensional drag clause into CNF we get
(-a V -b) V (-c ^ -d ^ -e ^ -f ^ -g ^ -h ^ -i)
and
(c V d V e V f V g V h V i) V (a ^ b)
* Distribute to us

-a V -b V -c
-a V -b V -d
-a V -b V -e
-a V -b V -f
-a V -b V -g
-a V -b V -h
-a V -b V -i 

and 

c V d V e V f V g V h V i V a
c V d V e V f V g V h V i V b
* Same for the remaining sets of 2 variables, where C2n (n is the number of adjacent cells) in such a case. 
* We draw a rule here: around box 2,
  * we choose 3 arbitrary variables x, y, z to write into form clauses
(-x V -y V -z) [CASE 1]
  * and select 8 arbitrary variables q, w, e, r, t, y, u, i to write form clauses
(q V w V e V r V t V y V u V i) [CASE 2]

In general, for each cell with K, we choose K 1 variable to write the form propositions [CASE 1] and choose the number-K 1 to write the form statements [CASE 2]. This is the rule used for programming. 
