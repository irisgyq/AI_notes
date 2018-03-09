## Informed Search —— have sense of the direction of goal state
---

### Search Heuristics
---

A heuristic is a function that **estimates** how close a state is to a goal, and designed for a particular search problem.

Heuristics 是否选取的优，会影响到用到 Heuristics 的搜索算法的优劣程度。

e.g.

- pac-man: mahattan distance

- travelling: straight-line distance from current city to Bucharest

- pancake-flip: the number of the largest pancake that is still out of place. 其实就是数当前的这个 pancake 里从下往上，哪一层如果错了，这是当前第几层，那 heuristic 就是几。

#### Optimality —— admissibility and consistency

Admissible (optimistic) heuristics slow down bad plans but never outweigh true costs.

- 0 <= h(n) <= h*(n) where h*(n) is the true cost to a nearest goal

Consistency: heuristic "arc" cost <= actual cost for each arc.

- h(A) - h(C) <= cost(A to C) 

- Consequences of consistency:

	- The f value along a path never decreases: h(A) <= cost(A to C) + h(C)

	- A* graph search is optimal

But inadmissible heuristics are often useful too.

e.g.

- 8 puzzle

	- states: all combinations of blocks
	
	- how many states? 8!

	- how many susseccors: 4

	- cost: a cost of one per action you take 

	- heuristic 1: number of tiles misplaced

		- h(start) = 8

		- This is a relaxed-problem heuristic
		
	- heuristic 2: total manhattan distance
		
		- h(start) = 3+1+2+...=18
	
	- heuristic 3: actual cost

		- admissible although, it needs to expand fewer nodes but usually do more work per node to compute heuristic itself

#### Trivial Heuristics, Dominance

- Dominance: ha >= hc if for all n: ha(n) >= hc(n)

- Heuristics from a semi-lattice

	- Max of admissible heuristics is admissible: h(n)=max(ha(n), hb(n))

- Trivial heuristics

	- Bottom of lattice is the zero heuristic

	- Top pf lattice is the exact heuristic h*(n)


### Greedy Search
---

Only guided by heuristics, each time choose the node which has the smallest heuristic (UCS 看的是每次累积的 actual costs，但是 Greedy 只是看当前的一个 heuristic).

h(n): forward costs.

Strategy: expand a node that you think is closest to a goal state.

**But** Best-first takes you straight to the (wrong) goal, and in the worst case, it is like a badly-guided DFS. 

Sometimes go wrong. Not global optimal.

### A* Search
---

**Heuristics + costs.**

f(n) = g(n) [backward costs] + h(n) [forward costs]

相当于是 UCS 和 Greedy 的结合。

We can't stop A* when we enqueue a goal, we need to wait that all nodes in fringe have exited. 也就是要检查完所有的情况之后，再选出最优解。

#### A* is optimal only if heuristics are optimistic!
#### When A* use graph search, sometime it may go wrong, because we travelse one node before the same node with optimal value! So we need to promise the consistency of heuristics.

A* tree search is optimal if heuristic is admissible, UCS is a special case (h=0).

A* graph search is optimal if heuristic is consistent, UCS is optimal (h=0 is consistent)

Consistency implies admissibility.

In general, most natural admissible heuristics tend to be consistent, especially if from relaxed problems.

#### Optimality of A* Tree Search

Assume:

- A is an optimal goal node

- B is a suboptimal goal node

- h is admissible

Claim: A will exit the fringe before B

Proof: 

- Imagine B is on the fringe

- Some ancestor n of A is on the fringe, too (maybe A!)

- claim: n will be expanded before B

	- f(n) is less or equal to f(A)
	
		- f(n) = g(n) + h(n) Definition of f-cost
		
		- f(n) <= g(A) Admissibility of h
		
		- g(A) = f(A) h=0 at  a goal
		
	- f(A) is less than f(B)
	
		- g(A) < g(B) B is suboptimal
		
		- f(A) < f(B) h=0 at a goal
	
	- n expands before B 

		- f(n) <= f(A) < f(B)

- All ancestors of A expand before B

- A expands before B

- A* search is optimal

#### UCS vs A*

Uniform-cost expands equally in all "directions"

A* expands mainly toward the goal, but does hedge its bets to ensure optimality


### Graph Search

Tree search will do a lot of extra work, so failure to detect repeated states can cause exponentially more work.

- Idea: never expand a state twice

- How to implement:

	- Tree search + set of expanded states ("closed set")
	
	- Before expanding a node, check to make sure its state has never been expanded before

	- If not new, skip it, if new add to closed set

**store the closed set as a set, not a list!!!**

It won't wreck the completeness.







 



