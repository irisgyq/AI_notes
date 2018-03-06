# Constraint Satisfaction Problem I

## Search

Assumptions about the world:

like: a single agent, deterministic actions, fully observed state and discrete state space

- Planning: sequences of actions

	- The path to the goal is the important thing

	- Paths have various costs, depths

	- Heuristics give problem-specific guidance

- Identification: assignments to variables

	- The goal itself is important, not the path

	- All paths at the same depth (for some formulations)

	- CSPs are specialized for identification problems

## Constraint Satisfaction Problems

- A special subset of search problems

- State is defined by variables Xi with values from a domain D 

- Goal test is a set of constraints specifying allowable combinations of values for subsets of variables

E.g.

#### Map Coloring

WA is adjacent to NT & SA, NT is adjacent to WA & SA & Q, SA is adjacent to WA & NT & Q & NSW & V, Q is adjacent to NT & SA & NSW, NSW is adjacent to SA & Q & V, V is adjacent to NT & NSW, T is independent.
         
- Variables: WA, NT, Q, NSW, V, SA, T

- Domains: D = { red, green, blue }

- Constraints: adjacent regions must have different colors

	- implicit: WA != NT

	- Explicit: (WA, NT) belongs to {(red, green), (red, blue), ... }

- Solutions are assignments satisfying all constraints, e.g.: {WA=red, NT=green, Q=red, NSW=green, V=red, SA=blue, T=green}

#### N-Queens

- Formulation 1:

	- Variables: Xij

	- Domains: {0,1}

	- Constaints: for each i,j,k (Xij, Xik) belong to {(0,0), (0,1), (1,0)}, (Xij, Xkj) belong to {(0,0), (0,1), (1,0)}, (Xij, Xi+k,j+k) belong to {(0,0), (0,1), (1,0)}, (Xij, Xi+k,j-k) belong to {(0,0), (0,1), (1,0)}

- Formulation 2:

	- Variables: Qk (第k行的皇后位于哪一列)

	- Domains: {1,2,3,...N}

	- Constraints: 
	
		- implicit: for each i,j, non-threatening (Qi, Qj)

		- Explicit: (Q1, Q2) belong to {(1,3), (1,4), ...}

#### Cryptarithmetic

TWO + TWO = FOUR

- Variables: F T U W R O X1 X2 X3 (X1, X2, X3 分别是个位，十位和百位的进位)

- Domains: {0,1,2,3,4,5,6,7,8,9}

- Constraints: alldiff(F,T,U,W,R,O) O+O = R+10*X1 .....

#### Sudoku

- Variables: Each (open) square

- Domains: {1,2,...9}

- Constraints: 9-way alldiff for each column, 9-way alldiff for each row, 9-way alldiff for each region

### Constraint Graphs

Unary: involve a single variable

Binary CSP: each constraint relates (at most) two variables.

higher-order involve 3 or more variables.


### Varieties of CSPs

- Discrete Variables: 

	- Finite domains: size d means O(d^n) complete assignments
	
	- Infinite domains: Linear constraints solvable, nonlinear undecidable

- Continuous variables

	- Linear constraints solvable in polynomial time by LP methods

## Solving CSPs

### Backtracking Search

Uninformed algorithm.

Backtracking = DFS + variable-ordering + fail-on-violation

- One variable at a time

- Check constraints as you go

### Improving backtracking

#### Filtering: Forward Checking

- Filtering: Keep track of domains for unassigned variables and cross off bad options

- Forward checking: Cross off values that violate a constraint when added to the existing assignment

#### Filtering: Constraint Propagation

Constraint Propagation: reason from constraint to constraint

Forward checking propagates information from assigned to unassigned variables, but doesn't provide early detection for all failures.

#### Filtering: Consistency of a single arc

An arc X -> Y is consistent iff for every x in the tail there is some y in the head which could be assigned without violating a constraint.

Delete from the tail!!!

Forward checking only enforce consistency of arcs pointing to each new assignment.

However, in consistency of arcs, if X loses a value, neighbors of X need to be rechecked. So arc consistency detects failure earlier than forward checking.

Runtime: O(n^2*d^3), can be reduced to O(n^2*d^2). But detecting all possible future problems will be NP-hard.

Limitations after enforcing arc consistency:

- can have one solution left

- can have multiple solutions left

- can have no solutions left

**Arc consistency still runs in backtracking search**

#### Ordering: Minimum Remaining Values

- choose the variable with the fewest legal left values in its domain

fail-fast ordering.

#### Ordering: Least Constraining Value

- choose the one that rules out the fewest values in the remaining variables


