# CS188 Artificial Intelligence



My notes of UC Berkeley online course [Intro to Artificial Intelligence](https://www.bilibili.com/video/av15628609/?p=5)

### Search

#### Two type of search

**Planning: sequences of actions**

* The path to the goal is the important thing
* Paths have various costs, depths
* Heuristics give problem-specific guidance

**Identification: assignments to variables**

* The goal itself is important, not the path
* All paths at the same depth \(for some formulations\)
* CSPs are specialized for identification problems

#### Uninformed and Informed Search \(Planning\)

* All search algorithms are the same except for fringe strategies.
* **Informed Search**: Introduced heuristic to estimate of distance to nearest goal for each state and therefore speed up search \(Solve performance problem of UCS\).

**Abstraction**

* A state space.
* A successor function \(with actions, costs\).
* A start state and a goal test.

**Example**

* Traveling in Romania \(Map with weighted edge\)
* Pac-man game planning
* Pancake Problem

**Properties**

* Complete: Guaranteed to find a solution if one exists?
* Optimal: Guaranteed to find the least cost path?
* Time and space complexity.

**Depth-First Search \(DFS\)**

* Complete if we prevent cycles
* Not optimal
* Time O\(b^m\)
* Space O\(bm\)

**Breadth-First Search \(BFS\)**

* Complete
* Optimal only if costs are all 1
* Time O\(b^s\)
* Space O\(b^s\)

**Uniform Cost Search \(UCS\)**

* Complete and optimal
* Time O\(b^\(C\*/E\)\)
* Space O\(b^\(C\*/E\)\)
* **Problem**: Explores options in every “direction” because no information about goal location

**Greedy Search**

* Expand the node that seems closest
* **Can be wrong** because of this \(not optimal\)

**A\*: Combining UCS and Greedy**

Uniform-cost orders by path cost, or backward cost **g\(n\)**. Greedy orders by goal proximity, or forward cost **h\(n\)**. A\* Search orders by the sum: **f\(n\) = g\(n\) + h\(n\)**.

**Admissibility**

* Inadmissible \(pessimistic\) heuristics break optimality by trapping good plans on the fringe
* Admissible \(optimistic\) heuristics slow down bad plans but never outweigh true costs
* A heuristic h is admissible \(optimistic\) if: 0 &lt;= h\(n\) &lt;= h\*\(n\) where h\*\(n\) is the true cost to a nearest goal.

**Consistency**

Heuristic “arc” cost ≤ actual cost for each arc: h\(A\) – h\(C\) ≤ cost\(A to C\)

**Heuristic design**

A\* is **optimal** with admissible/consistent heuristics. In general, most natural admissible heuristics tend to be consistent, especially if from relaxed problems so often use relaxed problems.

**Comparison**

[![](https://camo.githubusercontent.com/afda9f403db0c91a1a49af9f71d491ccb41c1f3502dd54bca3daa0c42d8027ce/68747470733a2f2f7773312e73696e61696d672e636e2f6c617267652f303036744e633739677931667371317175616133766a333161323065327767362e6a7067)](https://camo.githubusercontent.com/afda9f403db0c91a1a49af9f71d491ccb41c1f3502dd54bca3daa0c42d8027ce/68747470733a2f2f7773312e73696e61696d672e636e2f6c617267652f303036744e633739677931667371317175616133766a333161323065327767362e6a7067)

#### Constraint Satisfaction Problems \(Identification\)

**Abstraction**

* A special subset of search problems
* State is defined by variables Xi with values from a domain D \(sometimes D depends on i\)
* Goal test is a set of constraints specifying allowable combinations of values for subsets of variables

**Example**

* Map Coloring
* N-Queens
* Cryptarithmetic
* Sudoku

**Backtracking Search**

Backtracking search is the basic uninformed algorithm for solving CSPs

* **idea 1: One variable at a time**: Variable assignments are commutative, so only need to consider assignments to a single variable at each step
* **Idea 2: Check constraints as you go**: Consider only values which do not conflict previous assignments. \(Might have to do some computation to check the constraints “Incremental goal test”\)

**Improving Backtracking**

**Filtering**

Detect inevitable failure early: Keep track of domains for unassigned variables and cross off bad options.

**Forward Checking**

Cross off values that violate a constraint when added to the existing assignment.

**Disadvantage**: doesn't provide early detection for all failures.

**Constraint Propagation**

Forward checking propagates information from assigned to unassigned variables, but doesn't provide early detection for all failures.

So introduce **Arc consistent**: An arc X-&gt;Y is consistent iff for every x in the tail there is some y in the head which could be assigned without violating a constraint.

A simple form of propagation makes sure all arcs are consistent.

**Disadvantage**: Running slow and can not detect all failures.

**K-Consistency**

* 1-Consistency \(Node Consistency\): Each single node’s domain has a value which meets that node’s unary constraints
* 2-Consistency \(Arc Consistency\): For each pair of nodes, any consistent assignment to one can be extended to the other
* K-Consistency: For each k nodes, any consistent assignment to k-1 can be extended to the kth node.

**Strong K-Consistency**

k, k-1...1 Consistency which means we can solve without backtracking.

**Ordering**

**Minimum remaining values \(MRV\)**

Choose the variable with the fewest legal left values in its domain \(“Fail-fast” ordering\)

**Least Constraining Value \(LCV\)**

Given a choice of variable, choose the least constraining value which leave more choices for future. I.e., the one that rules out the fewest values in the remaining variables. Note that it may take some computation to determine this! \(E.g., rerunning filtering\)

**Structure**

**Subproblems**

Independent subproblems are identifiable as connected components of constraint graph. Solve subproblems Independently will speed things up based on \# of variables of subproblems.

**Tree-Structured CSPs**

If the constraint graph has no loops, the CSP can be solved in **O\(n d^2\)** time compared to general CSPs, where worst-case time is **O\(d^n\)**

**Cutset**

Instantiate \(in all ways\) a set of variables such that the remaining constraint graph is a **tree structure CSP**, which reduce runtime to **O\(\(d^c\)\*\(n-c\)\*d^2\)**.

**Local search: Iterative Algorithms for CSPs**

Take an assignment with unsatisfied constraints, operators reassign variable values until problem solved.

**Performance**

The same appears to be true for any randomly-generated CSP except in a narrow range of the ratio [![](https://camo.githubusercontent.com/d8155da5791d2ef78e1b91f5f03058df5c9d016d6dae5e44e8d87109fff66ac6/68747470733a2f2f7773312e73696e61696d672e636e2f6c617267652f303036744b6654636c7931667372363533756666336a3330687530656d6161352e6a7067)](https://camo.githubusercontent.com/d8155da5791d2ef78e1b91f5f03058df5c9d016d6dae5e44e8d87109fff66ac6/68747470733a2f2f7773312e73696e61696d672e636e2f6c617267652f303036744b6654636c7931667372363533756666336a3330687530656d6161352e6a7067)

#### Adversarial Search \(Minimax Tree\)

* Just like DFS: Time: O\(b^m\), Space: O\(bm\).
* Optimal against a perfect player but do not reasoning against possible outcome.

**Resource Limits**

Hard to search all leaves of tree \(bound by time\).

**Solution1: Depth limited search**

Search only to a limited depth in the tree, replace terminal utilities with an evaluation function for non-terminal positions.

**Properties**

* Guarantee of optimal play is gone
* The deeper in the tree the evaluation function is buried, the less the quality of the evaluation function matters
* Tradeoff between complexity of features and complexity of computation

**Evaluation Functions**

* Typically weighted linear sum of features
* Have danger of replanning

**Solution2: Alpha-Beta Pruning**

Reduce unnecessary compute if possible.

**General steps**

\(MIN version, MAX version is symmetric\)

1. We’re computing the MIN-VALUE at some node n
2. We’re looping over n’s children
3. n’s estimate of the children’s min is dropping
4. Who cares about n’s value? MAX
5. Let a be the best value that MAX can get at any choice point along the current path from the root
6. If n becomes worse than a, MAX will avoid it, so we can stop considering n’s other children \(it’s already bad enough that it won’t be played\)

**Properties**

* No effect on minimax value computed for the root!
* Values of intermediate nodes might be wrong
* Good child ordering improves effectiveness of pruning
* A simple example of meta-reasoning: computing about what to compute

#### Uncertainty and Utilities

Uncertain outcomes controlled by chance \(EXP node\), not an adversary \(MIN node\).

**Expectimax Search**

Replace MIN node with EXP node

* MAX nodes as in minimax search.
* Chance nodes are like MIN nodes but the outcome is uncertain.
* Calculate their **expected utilities**. I.e. take weighted average \(expectation\) of children.
* Hard to prune if using average.
* For evaluation, magnitude become important other than ordering.

**Expectiminmax Search \(Mixed Layer\)**

Ad EXP node between MAX and MIN node.

* Environment is an extra “random agent” player that moves after each min/max agent
* Each node computes the appropriate combination of its children

**Properties**

As depth increases, probability of reaching a given search node shrinks

* So usefulness of search is diminished
* So limiting depth is less damaging
* But **pruning** is trickier

**Example**

Historic AI: TDGammon uses depth-2 search + very good evaluation function + reinforcement learning: world-champion level play.

**Generalization of minimax**

* Terminals have utility tuples.
* Node values are also utility tuples.
* Each player maximizes its own component.
* Can give rise to cooperation and competition dynamically and naturally.

**Maximum Expected Utility Principle**

* A rational agent should chose the action that **maximizes its expected utility, given its knowledge.**
* Note: an agent can be entirely rational \(consistent with MEU\) without ever representing or manipulating utilities and probabilities. E.g., a lookup table for perfect tic-tac-toe, a reflex vacuum cleaner

### Markov Decision Processes \(MDP\)

�Agent do not have fully control over future \(action\). “Markov” generally means that given the present state, the future and the past are independent.

#### Abstraction

* A set of states s
* A set of actions a
* A transition function T\(s, a, s’\)
  * Probability that a from s leads to s’, i.e., P\(s’\| s, a\)
  * Also called the model or the dynamics
* A reward function R\(s, a, s’\)
  * Sometimes just R\(s\) or R\(s’\)
* A start state
* Maybe a terminal state

#### Quantities

* Policy = map of states to actions
* Utility = sum of discounted rewards
* Values = expected future utility from a state \(max node\)
* Q-Values = expected future utility from a q-state \(chance node\)
* The value \(utility\) of a state s: V\*\(s\) = expected utility starting in s and acting optimally
* The value \(utility\) of a q-state \(s,a\): Q\*\(s,a\) = expected utility starting out having taken action a from state s and \(thereafter\) acting optimally
* The optimal policy: t\*\(s\) = optimal action from state s

#### Examples

* Noisy movement
* Racing car states

#### Utilities of Sequences

**Discounting**

* It’s reasonable to maximize the sum of rewards
* It’s also reasonable to prefer rewards now to rewards later
* So use discounting: values of rewards decay exponentially

**Example: discount of 0.5**

* U\(\[1,2,3\]\) = 1_1 + 0.5_2 + 0.25\*3
* U\(\[1,2,3\]\) &lt; U\(\[3,2,1\]\)

**Infinite Utilities**

What if the game lasts forever? Do we get infinite rewards?

**Solution**

1. Finite horizon: \(similar to depth-limited search\)
   * Terminate episodes after a fixed T steps \(e.g. life\)
   * Gives nonstationary policies \( depends on time left\)
2. Discounting: use 0 &lt; y &lt; 1
   * Smaller means smaller “horizon” – shorter term focus
3. Absorbing state: guarantee that for every policy, a terminal state will eventually be reached \(like “overheated” for racing\)

#### The Bellman Equations

[![](https://camo.githubusercontent.com/b8633489ceca599eb0a38dc7055669f5f2068949c4dba70ee4c774038293d8ce/68747470733a2f2f7773332e73696e61696d672e636e2f6c617267652f303036744e6337396c79316673766c6430763267776a33313279306c6b7766692e6a7067)](https://camo.githubusercontent.com/b8633489ceca599eb0a38dc7055669f5f2068949c4dba70ee4c774038293d8ce/68747470733a2f2f7773332e73696e61696d672e636e2f6c617267652f303036744e6337396c79316673766c6430763267776a33313279306c6b7766692e6a7067)

#### Solution1: Value Iteration

Based on Bellman Equations, **fixed depth k** and compute the actual value by **max over all actions** to compute the optimal values: [![](https://camo.githubusercontent.com/5ac3c1c9f63fd6fd86d5dd7d1c4cb57bac51bd327bd97466c5a85f8ca5408955/68747470733a2f2f7773342e73696e61696d672e636e2f6c617267652f303036744e6337396c79316673766c6d736265696d6a333071383032696466742e6a7067)](https://camo.githubusercontent.com/5ac3c1c9f63fd6fd86d5dd7d1c4cb57bac51bd327bd97466c5a85f8ca5408955/68747470733a2f2f7773342e73696e61696d672e636e2f6c617267652f303036744e6337396c79316673766c6d736265696d6a333071383032696466742e6a7067)

**Problems**

1. It’s slow – O\(A\*S^2\) per iteration
2. The “max” at each state rarely changes
3. The policy often converges long before the values

**Example**

[![](https://camo.githubusercontent.com/190f9fc92ad221424ceb8ad2b287089c2034479139157b9bbfba405179964956/68747470733a2f2f7773332e73696e61696d672e636e2f6c617267652f303036744e6337396c79316673766c6f34353774346a33316338306f697766672e6a7067)](https://camo.githubusercontent.com/190f9fc92ad221424ceb8ad2b287089c2034479139157b9bbfba405179964956/68747470733a2f2f7773332e73696e61696d672e636e2f6c617267652f303036744e6337396c79316673766c6f34353774346a33316338306f697766672e6a7067)

#### Solution2: Policy Iteration

Because of the problem if value iteration, think of using another way to solve MDP.

**Policy Evaluation**

Find a way calculate the V’s for a fixed policy.

**Fixed Policies**

If we fixed some policy \(s\), then the tree would be simpler: only **one action per state** instead of all actions.

**Calculation**

Idea 1: Turn recursive Bellman equations into updates \(like value iteration\)

* Efficiency: O\(S2\) per iteration

Idea 2: Without the maxes, the Bellman equations are just a linear system

* Good for parallel

**Policy Extraction**

"Reversed" Value Iteration: Find a way to calculate the policy given values.

**Computing Actions from Values**

Do a mini-expectimax \(one step\) [![](https://camo.githubusercontent.com/525c6023f4af81acb5affb5b51e9401195dc91466a2cb2e7724919934872a437/68747470733a2f2f7773332e73696e61696d672e636e2f6c617267652f303036744e6337396c79316673766c6a7134356f6f6a3330726d3033346466752e6a7067)](https://camo.githubusercontent.com/525c6023f4af81acb5affb5b51e9401195dc91466a2cb2e7724919934872a437/68747470733a2f2f7773332e73696e61696d672e636e2f6c617267652f303036744e6337396c79316673766c6a7134356f6f6a3330726d3033346466752e6a7067)

**Computing Actions from Q-Values**

Completely trivial: just choose the max Q-Values action!

**Conclusion**

Actions are easier to select from Q-Values than values

**Steps**

1. Policy evaluation: calculate utilities for some fixed policy \(not optimal utilities!\) until convergence
2. Policy improvement: update policy using one-step look-ahead with resulting converged \(but not optimal!\) utilities as future values
3. Repeat steps until policy converges

**Properties**

* It’s still optimal!
* Can converge \(much\) faster under some conditions

#### Look in depth: These all look the same!

* They basically are – they are all variations of Bellman updates
* They all use one-step lookahead expectimax fragments
* They differ only in whether we plug in a fixed policy or max over actions

#### From MDP to learning

Specifically, reinforcement learning is an MDP, but you couldn’t solve it with just computation \(probability unknown\). So you needed to actually act to figure it out.

* Exploration: you have to try unknown actions to get information
* Exploitation: eventually, you have to use what you know
* Regret: even if you learn intelligently, you make mistakes
* Sampling: because of chance, you have to try things repeatedly
* Difficulty: learning can be much harder than solving a known MDP

### Reinforcement Learning

#### Abstraction

* Still assume a Markov decision process \(MDP\):
* A set of states s S
* A set of actions \(per state\) A
* A model T\(s,a,s’\)
* A reward function R\(s,a,s’\)
* Still looking for a policy \(s\)
* New twist: don’t know T or R, must actually try actions and states out to learn

#### Model-Based Learning

**Model-Based Idea**

Learn an approximate model based on experiences。 Solve for values as if the learned model were correct

**Steps**

1. Learn empirical MDP model
   * Count outcomes s’ for each s, a
   * Normalize to give an estimate of T
   * Discover each R when we experience \(s, a, s’\)
2. Solve the learned MDP
   * For example, use value iteration, as before

**Example**

[![](https://camo.githubusercontent.com/b33664eba88b2de591b3d81050f3f5aa3a4edfadce2cf4a0e5b0ad4e2c64484c/68747470733a2f2f7773312e73696e61696d672e636e2f6c617267652f303036744b66546367793166737a766273397066756a3331696b307338676e342e6a7067)](https://camo.githubusercontent.com/b33664eba88b2de591b3d81050f3f5aa3a4edfadce2cf4a0e5b0ad4e2c64484c/68747470733a2f2f7773312e73696e61696d672e636e2f6c617267652f303036744b66546367793166737a766273397066756a3331696b307338676e342e6a7067)

#### Model-Free Learning

**Passive Reinforcement Learning**

* Simplified task: policy evaluation
* Input: a fixed policy \(s\)
* You don’t know the transitions T\(s,a,s’\)
* You don’t know the rewards R\(s,a,s’\)
* Goal: learn the state values

**Active Reinforcement Learning**

* Full reinforcement learning: optimal policies \(like value iteration\)
* You don’t know the transitions T\(s,a,s’\)
* You don’t know the rewards R\(s,a,s’\)
* You choose the actions now
* Goal: learn the optimal policy / values
* In this case:
  * Learner makes choices!
  * Fundamental tradeoff: exploration vs. exploitation
  * This is NOT offline planning! You actually take actions in the world and find out what happens…

**Direct Evaluation**

**Goal**

Compute values for each state under

**Idea**

Average together observed sample values

* Act according to
* Every time you visit a state, write down what the sum of discounted rewards turned out to be
* Average those samples

**Example**

[![](https://camo.githubusercontent.com/93e9dbb435798f19254f77973a027fcd2965cc2f8aab47629871110751242629/68747470733a2f2f7773332e73696e61696d672e636e2f6c617267652f303036744b66546367793166743030393868346c656a33316a7730736f7133792e6a7067)](https://camo.githubusercontent.com/93e9dbb435798f19254f77973a027fcd2965cc2f8aab47629871110751242629/68747470733a2f2f7773332e73696e61696d672e636e2f6c617267652f303036744b66546367793166743030393868346c656a33316a7730736f7133792e6a7067)

**Problems**

Advantage

* It’s easy to understand
* It doesn’t require any knowledge of T, R
* It eventually computes the correct average values, using just sample transitions

Disadvantage

* It wastes information about state connections
* Each state must be learned separately
* So, it takes a long time to learn

**Temporal Difference Learning**

**Idea**

Sample-Based Policy Evaluation. Learn from every experience.

* Update V\(s\) each time we experience a transition \(s, a, s’, r\)
* Likely outcomes s’ will contribute updates more often

**Example**

[![](https://camo.githubusercontent.com/35b2b93e1e05c81c5ba6fd4aeab302dd13e5e8dd5f206e0a75f1882371831356/68747470733a2f2f7773322e73696e61696d672e636e2f6c617267652f303036744b665463677931667430306365343278656a33316a323073776a73392e6a7067)](https://camo.githubusercontent.com/35b2b93e1e05c81c5ba6fd4aeab302dd13e5e8dd5f206e0a75f1882371831356/68747470733a2f2f7773322e73696e61696d672e636e2f6c617267652f303036744b665463677931667430306365343278656a33316a323073776a73392e6a7067)

**Problems**

TD value leaning is a model-free way to do policy evaluation, mimicking Bellman updates with running sample averages. However, if we want to turn values into a \(new\) policy, we’re sunk. Solution: learn Q-values, not values.

**Q-Learning**

**Idea & Steps**

[![](https://camo.githubusercontent.com/a53308b616e02b5938bad1256dc0e35d7112fd1d6dbc536559a350876bf516d3/68747470733a2f2f7773342e73696e61696d672e636e2f6c617267652f303036744b66546367793166743030686a7838346f6a33316a61306e6964687a2e6a7067)](https://camo.githubusercontent.com/a53308b616e02b5938bad1256dc0e35d7112fd1d6dbc536559a350876bf516d3/68747470733a2f2f7773342e73696e61696d672e636e2f6c617267652f303036744b66546367793166743030686a7838346f6a33316a61306e6964687a2e6a7067)

**Properties**

* Converges to optimal policy -- even if you’re acting suboptimally

**Caveats**

* Have to explore enough.
* Have to eventually make the learning rate small enough but not decrease it too quickly.
* Basically, in the limit, it doesn’t matter how you select actions.

