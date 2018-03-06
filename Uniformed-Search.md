## Uninformed Search —— No idea of where is the goal, just explore and explore
---

### Agents that Plan
---

Reflex agents:

choose action based on current percept, do not consider the future consequences of their actions 就像条件反射一样，不用做判断

Planning agents:

Desions based on consequences of actions, must formulate a goal, consider how the world would be. 随时随地要进行判断，随机应变

而且还要考虑 optimal vs. complete planning, planning vs. replanning

### Search Problems
---

A search problem consists of:

- A state space

- A successor function (with actions, costs)

- A start state and a goal test


A **solution** is a sequence of actions (a plan) which transforms the start state to a goal state.

E.g.

#### Traveling in Romania

- state space: cities

- Successor function:  Roads: Go to adjacent city with cost = distance

- Start state: Arad

- Goal test: Is state == Bucharest?

- Solution: a path from Arad to Bucharest.

**A search state keeps only the details needed for planning.**

#### Problem: pac-man pathing

- States: (x,y) location

- Actions: NSEW

- Successor: Update location only

- Goal test: is (x,y) = END

#### Problem: pac-man Eat-all dots

- States: {(x,y), dot booleans }

- Actions: NSEW

- Successor: update location and possibly a dot boolean

- Goal test: dots all false

#### Problem: eat all dots while keeping the ghosts perma-scared

- state space: agent position, dot booleans, power pellet booleans, remaining scared time

#### World state of pac-man

- World state:
	- Agent positions: 120
	- Food count: 30
	- Ghost positions: 12
	- Agent facing: NSEW

- World Space size:
120 * 2^30 * 12^2 * 4

- Pathing space size:
120

- eat-all-dots space size:
120 * 2^30

### State space graph and tree
---

graph: each node appears only once.

trees: start state is root node, children correspond to successors, same state will occur many times.

**Lots of repeated structures in the search tree!**

### Depth-First Search
---

Stratefy: expand a deepest node first.

Implementation: Fringe is a LIFO stack

- It is not complete (which means it is not guaranteed to find a solution if one exists)

- It is not optimal (which means it is not guaranteed to find the least cost path)

- time complexity: O(b^m)

- space complexity: O(bm)

### Breadth-First search
---

Strategy: expand a shallowest node first

Implementation: Fringe is a FIFO queue

- It is complete

- It is optimal if costs are all 1

- Time complexity: O(b^s)

- Space complexity: O(b^s)

### Iterative Deepening
---

Idea: get DFS's space advantage with BFS's time / shallow-solution advantages

也就是限制深度的 DFS。

- It is complete

- It is optimal if costs are all 1

- Time complexity: O(b^s)

- Space complexity: O(bs)

### Uniform-cost search
---

Stategy: expand a cheapest node first

Implementation: Fringe is a priority queue (**priority: cumulative cost**)

- It is complete

- It is optimal

- Time complexity: O(b^(c*/e))

- Space complexity: O(b^(c*/e))

UCS also has some disadvantages:

Explores options in every "direction", and no information about goal location.

所以尽管 UCS 其实算是 Uninformed Search 算法里最好的一种了，但是有时候我们更需要 informed search 的一些特性。

E.g.

#### Pancake problem

state space graph with costs as weights.

这里的 cost 用的是从上一个状态到当前状态所需要 flip 的 pancakes 的层数， 比如2就是只只需翻转最上面两层， 3就是需要翻转上面3层，4就是整个都要翻转。





