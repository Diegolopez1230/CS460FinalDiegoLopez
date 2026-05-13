# The Torchbearer

**Student Name:** Diego Lopez
**Student ID:** 827527188
**Course:** CS 460 – Algorithms | Spring 2026

> This README is your project documentation. Write it the way a developer would document
> their design decisions , bullet points, brief justifications, and concrete examples where
> required. You are not writing an essay. You are explaining what you built and why you built
> it that way. Delete all blockquotes like this one before submitting.

---

## Part 1: Problem Analysis

> Document why this problem is not just a shortest-path problem. Three bullet points, one
> per question. Each bullet should be 1-2 sentences max.

- **Why a single shortest-path run from S is not enough:**
  A single shortest-path run from S is not sufficient because it can't decide the best order to visit the relic chambers before ending at T. 

- **What decision remains after all inter-location costs are known:**
  The structural decision that remains after all inter-location travel costs are known is picking the visitation order of the relic chambers. Different orders make different total costs even when every pairwise shorest path is already known. 

- **Why this requires a search over orders (one sentence):**
  The problem is a search over orders, not a single computation because the algorithm has to compare many possible combinations of relic visits to pick which order makes the minimum total fuel cost. 

---

## Part 2: Precomputation Design

### Part 2a: Source Selection

> List the source node types as a bullet list. For each, one-line reason.

| Source Node Type | Why it is a source |
|---|---|
| Entrance | For the entrance node, S, we need the shortest travel costs from starting to each first relic that is possible. |
| Exit | For the exit node, T, it’s here for completeness and duplicate source selection, even though no route happens after T. |

### Part 2b: Distance Storage

> Fill in the table. No prose required.

| Property | Your answer |
|---|---|
| Data structure name | The data structure name is a dictionary of dictionaries. |
| What the keys represent | The keys represent source nodes and destination nodes. |
| What the values represent | The values represent the min fuel cost from source to destination. |
| Lookup time complexity | O(1) |
| Why O(1) lookup is possible | Dictionary lookup is O(1) average case for both keys becuase of hashing. |

### Part 2c: Precomputation Complexity

> State the total complexity and show the arithmetic. Two to three lines max.

- **Number of Dijkstra runs:** The Dijkstra runs from the entrance and from every relic chamber, so there are k + 1 total runs.
- **Cost per run:** One Dijkstra run takes O(m log n).
- **Total complexity:** O((k + 1)m log n), which simplifies to O(km log n).
- **Justification (one line):** There are k + 1 selected source nodes, and every source needs one shortest path calculation.

---

## Part 3: Algorithm Correctness

> Document your understanding of why Dijkstra produces correct distances.
> Bullet points and short sentences throughout. No paragraphs.

### Part 3a: What the Invariant Means

> Two bullets: one for finalized nodes, one for non-finalized nodes.
> Do not copy the invariant text from the spec.

- **For nodes already finalized (in S):**
  The invariant for nodes already finalized in S means that their stored distance is the shortest distance from the source and won’t change.

- **For nodes not yet finalized (not in S):**
  The invariant for nodes not yet finalized, not in S, means that their current distance is the shortest path found so far, but a cheaper path can still be found after.

### Part 3b: Why Each Phase Holds

> One to two bullets per phase. Maintenance must mention nonnegative edge weights.

- **Initialization : why the invariant holds before iteration 1:**
  Before the first cycle, the source node has a distance of 0, and all other nodes are set to infinity, so the invariant starts right.

- **Maintenance : why finalizing the min-dist node is always correct:**
  Finalizing the node with the min distance is right because all edge weights are nonnegative, so no path after can make a smaller distance.

- **Termination : what the invariant guarantees when the algorithm ends:**
  When the algorithm is done, each finalized node has its shortest path from the source.

### Part 3c: Why This Matters for the Route Planner

> One sentence connecting correct distances to correct routing decisions.

This is important for the route planner because correct shortest path distances lets the route planner to compare relic visitation orders with the right travel costs.

---

## Part 4: Search Design

### Why Greedy Fails

> State the failure mode. Then give a concrete counter-example using specific node names
> or costs (you may use the illustration example from the spec). Three to five bullets.

- **The failure mode:** Greedy only picks the cheapest next relic, but that local choice could take you to a worse total route.
- **Counter-example setup:** The costs are S -> B = 1, S -> C = 2, and S -> D = 2 from S.
- **What greedy picks:** Greedy would pick B first because it has the smallest cost from S.
- **What optimal picks:** The optimal route is S -> B -> D -> C -> T with total fuel cost = 4.
- **Why greedy loses:** A cheap next step does not guarantee the best full route because the rest of the relic order still affects the total cost.

### What the Algorithm Must Explore

> One bullet. Must use the word "order."

- The algorithm has to look at different relic visitation orders and compare their total costs to find the best order.

---

## Part 5: State and Search Space

### Part 5a: State Representation

> Document the three components of your search state as a table.
> Variable names here must match exactly what you use in torchbearer.py.

| Component | Variable name in code | Data type | Description |
|---|---|---|---|
| Current location | current_loc | node | Where Torchbearer is at |
| Relics already collected | relics_remaining | set | Not visited relics |
| Fuel cost so far | cost_so_far | float | Fuel total spent in order to get to the current state |

### Part 5b: Data Structure for Visited Relics

> Fill in the table.

| Property | Your answer |
|---|---|
| Data structure chosen | Set |
| Operation: check if relic already collected | Time complexity: O(1) |
| Operation: mark a relic as collected | Time complexity: O(1) |
| Operation: unmark a relic (backtrack) | Time complexity: O(1) |
| Why this structure fits | The set makes it fast to track which relics are still left |

### Part 5c: Worst-Case Search Space

> Two bullets.

- **Worst-case number of orders considered:** k!
- **Why:** With k relics, the algorithm might need to try every possible relic visit order.

---

## Part 6: Pruning

### Part 6a: Best-So-Far Tracking

> Three bullets.

- **What is tracked:** _Your answer here._
- **When it is used:** _Your answer here._
- **What it allows the algorithm to skip:** _Your answer here._

### Part 6b: Lower Bound Estimation

> Three bullets.

- **What information is available at the current state:** _Your answer here._
- **What the lower bound accounts for:** _Your answer here._
- **Why it never overestimates:** _Your answer here._

### Part 6c: Pruning Correctness

> One to two bullets. Explain why pruning is safe.

- _Your answer here._

---

## References

> Bullet list. If none beyond lecture notes, write that.
- Lecture Notes
- https://coderraj07.medium.com/dijkstras-algorithm-templates-and-problem-solving-9b858912e747 
