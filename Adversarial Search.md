# Adversarial Search

## Zero-Sum games

- Agent have opposite utilities (values on outcomes)

- Let's think of a single value that one maximizes and the other minimizes

- Adversarial, pure competition

Single agent use max value to be final state value, while adversarial agents use minimax.

## Minimax

- Deterministric, zero-sum games:

	- Tic-tac-toe, chess, checkers

	- One player maximizes result

	- The other minimizes result

- Minimax search:

	- A state-space search tree

	- Players alternate turns

	- Compute each node's minimax value: the best achievable utility against a rational (optimal) adversary

- Time: O(b^m)

- Space: O(bm)

### Minimax Implementation

def value (state):

	if the state is a terminal state: return the state's utility
	
	if the next agent is MAX: return max-value (state)
	
	if the next agent is MIN: return min-value (state)

def max-value (state):
	
	initialize v = -infinity
	
	for each successor of state:
	
	       v = max(v, min-value(successor))
	
	return v

def min-value (state):

	initialize v = +infinity
	
	for each successor of state:
	
		v = min(v, max-value(successor))
	
	return v

### Minimax Efficiency

#### Resource limits

- Problem: In realistic games, cannot search to leaves.

- Solution: Depth-limited search

	- Instead, search only to a limited depth in the tree

	- Replace terminal utilities with an evaluation function for non-terminal positions

Guarantee of optimal play is gone!

#### Evaluation Function

Ideal function: returns the actual minimax value of the position

In practice: typically weighted linear sum of features:

- **Eval(s) = w1f1(s) + w2f2(s) + ... + wnfn(s)**

- e.g. N-queens: f1(s) = (num of white queens - num of black queens)

#### Alpha-Beta Pruning

- General configuration (MIN version)

	- We're computing the MIN-VALUE at some node n

	- We're looping over n's children

	- n's estimate of the children's min is dropping

	- Who cares about n's value? MAX

	- Let alpha be the best value that MAX can get at any choice point along the current path from the root

	- If n becomes worse than alpha, MAX will avoid it, so we can stop considering n's other children (it's already bad enough that it won't be played)

- MAX version is symmetric

#### Alpha-beta implementation

alpha: MAX's best option on path to root

beta: MIN's best option on path to root

def max-value (state, alpha, beta)

	initialize v = -infinity
	
	for each successor of state:
	
		v = max(v, value(successor, alpha, beta))
		
		if v >= beta return v
		
		alpha = max(alpha, v)
		
	return v
	
def min-value (state, alpha, beta):
	
	initialize v = +infinity
	
	for each successor of state:
	
		v = min(v, value(successor, alpha, beta))
		
		if v <= alpha return v
		
		beta = min(beta, v)
		
	return v

#### Alpha-beta with "perfect ordering"

Good child ordering improves effectiveness of pruning

- Time complexity drops to O(b^(m/2))

- Doubles solvable depth!

- Full search of, e.g. chess, is still hopeless..



