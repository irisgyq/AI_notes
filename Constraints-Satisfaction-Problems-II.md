# Constraints Satisfaction Problems II

## Solving CSPs

### Improving Backtracking

#### K-Consistency

- 1-consistency (Node Consistency): Each single node's domain has a value which meets that node's unary constraints.

- 2-consistency (Arc Consistency): For each pair of nodes, any consistent assignment to one can be extended to the other

- K-Consistency: For each K nodes, any consistent assignment to k-1 can be extended to the kth node.

#### Strong K-Consistency

Strong n-consistency means we can solve without backtracking.

- Choose any assignment to any variable

- choose a new variabe

- by 2-consistency, there is a choice consistent with the first

- Choose a new variable

- By 3-consistency, there is a choice consistent with the first 2

- ...

#### Structure

Suppose a graph of n variables can be broken into subproblems of only c variables:

- worst case solution cost is O((n/c)*d^c), linear in n 

Goal: change structure into nearly Tree-structured CSPs by cutting some sets

#### Tree-Structured CSPs

- Algorithm:

	- Order: Choose a root variable, order variables so that parents precede children

	- Remove backward: For i = n:2, apply RemoveInconsistent(Parent(Xi), Xi)

	- Assign forward: For i=1:n, assign Xi consistently with Parent(Xi)

	- Runtime: O(n*d^2)

- Claim 1: After backward pass, all root-to-leaf arcs are consistent

	- Proof: Each X -> Y was made consistent at one point and Y's domain could not have been reduced thereafter (because Y's children were processed before Y)

- Claim2: If root-to-leaf arcs are consistent, forward assignment will not backtrack

	- Proof: Induction on position

#### Nearly Tree-Structured CSPs

- Conditioning: instantiate a variable, prune its neighbors' domains

- Cusset conditioning: instantiate (in all ways) a set of variables such that the remaining constraint graph is a tree

- Cutset size c gives runtime O((d^c)(n-c)d^2), very fast for small c

#### Tree Decomposition

Subproblems overlap to ensure consistent solutions

runtime: O(n*d^2)

### Iterative Improvement

It's a local search method, typically work with "complete" states.

To apply to CSPs:

- Take an assignment with unsatisfied constraints

- Operators reassign variable values

- No fringe! Live on the edge

Algorithm: while not solved,

- Variable selection: randomly select any conflicted variable

- Value selection: min-conflicts heuristic:

	- Choose a value that violates the fewest constraints

	- I.e., hill climb with h(n) = total number of violated constraints

#### Performance of Min-Conflicts

Given random initial state, can solve n-queens in almost constant time for arbitrary n with high probability. The same appears to be true for any randomly-generated CSP except in a narrow range of the ratio: R = number of constraints / number of variables

E.g.

#### 4-Queens

- States: 4 queens in 4 columns (4^4 = 256 states)

- Operators: move queen in column

- Goal test: no attacks

- Evaluation: c(n) = number of attacks

### Local Search

Generally much faster and memory saved, but suboptimal and not complete.

#### Hill climbing

- start whereever

- Repeat: move to the best neighboring state

- If no neighbors better than current, quit

#### Simulated Annealing

Escape local maximal by allowing downhill moves.

#### Genetic Algorithms

Natural selection metaphor.

- Keep best N hypotheses at each step (selection) based on a fitness function

- Also have pairwise crossover operators, with optional mutation to give variety