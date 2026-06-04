class: center, middle, inverse

# Graphs

---

# Agenda

- What is a graph?
- Terminology
- Types of graphs
- ADT Graph
- Graph representations
    - Adjacency matrix
    - Adjacency list
- Graph traversals
    - Depth-First Search (DFS)
    - Breadth-First Search (BFS)
- Applications and beyond

---

# What is a graph?

* A **graph** is a collection of **vertices** (or *nodes*) connected by **edges**

--

* Formally, a graph $G = (V, E)$ where:
    * $V$ is a set of vertices
    * $E$ is a set of edges, each a pair of vertices

--

.center[<img src="{{site.baseurl}}/presentation/graphs/graph-example.svg" width="45%">]

???

A graph captures pairwise relationships between objects. Vertices are the objects; edges are the relationships.

---

# Why graphs?

Graphs model relationships between objects:

* **Social networks**: people connected by friendships
* **Road maps**: cities connected by roads
* **The Web**: pages connected by hyperlinks
* **Dependency graphs**: tasks connected by precedence
* **Networks**: routers, switches, links
* **Compilers**: control-flow graphs, call graphs

--

If you can draw circles connected by lines, you have a graph problem.

---

# Graphs vs. Trees

* A **tree** is a special kind of graph:
    * Connected
    * Acyclic
    * $|E| = |V| - 1$

--

* Graphs are **more general**:
    * Can have **cycles**
    * Can be **disconnected**
    * Can have **multiple edges** between the same vertices

--

.center[<img src="{{site.baseurl}}/presentation/graphs/graph-vs-tree.svg" width="65%">]

---

# Terminology

* **Adjacent vertices**: two vertices connected by an edge
* **Degree** of a vertex: number of edges incident to it
* **Path**: sequence of vertices connected by edges
* **Simple path**: a path with no repeated vertices
* **Cycle**: a path that starts and ends at the same vertex
* **Connected graph**: there is a path between every pair of vertices
* **Connected component**: a maximal connected subgraph
* **Subgraph**: a subset of the vertices and edges of a graph

--

.center[<img src="{{site.baseurl}}/presentation/graphs/terminology.svg" width="50%">]

---

# Directed vs. Undirected

* **Undirected graph**: edges have no direction
    * If $(u, v) \in E$, then $u$ and $v$ are mutually connected
    * Friendship in a social network

--

* **Directed graph** (digraph): edges have direction
    * $(u, v)$ goes from $u$ to $v$, but not necessarily the reverse
    * Followers on Twitter, web links, dependencies

--

.center[<img src="{{site.baseurl}}/presentation/graphs/directed-vs-undirected.svg" width="70%">]

---

# Weighted Graphs

* Each edge has an associated **weight** (or *cost*)

--

* Examples:
    * Distance between cities on a road map
    * Cost of a flight between airports
    * Bandwidth on a network link
    * Latency between servers

--

.center[<img src="{{site.baseurl}}/presentation/graphs/weighted-graph.svg" width="45%">]

---

# Special graphs

* **Tree**: connected and acyclic
* **DAG** (Directed Acyclic Graph): directed and no cycles
    * Build systems, package dependencies, scheduling
* **Bipartite**: vertices split into two sets, edges only across sets
    * Job assignments, matching problems
* **Complete graph** $K_n$: every pair of vertices is connected
* **Sparse graph**: $|E| \approx |V|$
* **Dense graph**: $|E| \approx |V|^2$

--

The choice of representation often depends on whether the graph is sparse or dense.

???

Rule of thumb: if the average vertex is connected to a **fixed small fraction** of all other vertices (or all of them), the graph is dense. If it's connected to a **constant number** of others regardless of graph size, it's sparse.

Examples:
- Dense: round-robin tournament (every team plays every other), flight network among major hub airports.
- Sparse: Facebook's friendship graph — billions of users, but each person has only a few hundred friends, so |E| ≈ c · |V|, not |V|².

---

# ADT Graph

```java
/**
 * Graph interface. Defines a set of vertices connected by edges.
 */
interface Graph<V> {

    /** Adds a vertex to the graph */
    public void addVertex(V vertex);

    /** Adds an edge between two vertices */
    public void addEdge(V from, V to);

    /** Returns the vertices adjacent to a given vertex */
    public Iterable<V> adjacent(V vertex);

    /** Returns the number of vertices */
    public int vertexCount();

    /** Returns the number of edges */
    public int edgeCount();

    /** Returns true if there is an edge between two vertices */
    public boolean hasEdge(V from, V to);
}
```

---

# Representation: Adjacency Matrix

* A $V \times V$ matrix
* `matrix[i][j] = true` if there is an edge from $i$ to $j$
    * Or the edge weight, for weighted graphs

--

.center[<img src="{{site.baseurl}}/presentation/graphs/adjacency-matrix.svg" width="75%">]

???

The matrix is symmetric for undirected graphs.

---

# Representation: Adjacency Matrix

### Pros

* $O(1)$ to check if an edge exists
* Simple to implement
* Good for **dense** graphs

### Cons

* $O(V^2)$ space — wasteful for sparse graphs
* $O(V)$ to iterate over the neighbors of a vertex
* Adding a vertex is $O(V^2)$

---

# Representation: Adjacency List

* An array (or map) of lists, one per vertex
* `adj[v]` is the list of vertices adjacent to `v`

--

.center[<img src="{{site.baseurl}}/presentation/graphs/adjacency-list.svg" width="75%">]

---

# Representation: Adjacency List

### Pros

* $O(V + E)$ space — efficient for **sparse** graphs
* Easy to iterate over neighbors
* Adding a vertex or edge is $O(1)$ amortized

### Cons

* $O(\text{degree}(v))$ to check if a specific edge exists
* Slightly more complex than a matrix

--

### Most graphs in practice are sparse, so adjacency lists are the **default choice**.

---

# Comparison

| Operation              | Adjacency Matrix | Adjacency List      |
|------------------------|------------------|---------------------|
| Space                  | $O(V^2)$         | $O(V + E)$          |
| Add edge               | $O(1)$           | $O(1)$              |
| Remove edge            | $O(1)$           | $O(\text{degree})$  |
| Check if edge exists   | $O(1)$           | $O(\text{degree})$  |
| Iterate over neighbors | $O(V)$           | $O(\text{degree})$  |

--

* Use **matrix** for dense graphs or when checking edge existence is the dominant operation
* Use **list** for sparse graphs or when iterating over neighbors is the dominant operation

---

# Implementation

```java
public class AdjacencyListGraph<V> implements Graph<V> {

    private final Map<V, List<V>> adj = new HashMap<>();

    public void addVertex(V v) {
        adj.putIfAbsent(v, new LinkedList<>());
    }

    public void addEdge(V from, V to) {
        addVertex(from);
        addVertex(to);
        adj.get(from).add(to);
        adj.get(to).add(from); // undirected
    }

    public Iterable<V> adjacent(V v) {
        return adj.getOrDefault(v, Collections.emptyList());
    }
}
```

---

# Graph Traversals

* **Goal**: visit every vertex reachable from a starting vertex

--

* Two fundamental strategies:
    * **Depth-First Search (DFS)**: go as deep as possible before backtracking
    * **Breadth-First Search (BFS)**: explore all neighbors at the current level before moving deeper

--

* Both run in $O(V + E)$ with adjacency lists
* Both require us to **mark visited vertices** to avoid cycles

---

# Depth-First Search (DFS)

* Use a **stack** (often the call stack via recursion)
* Mark vertices as visited to avoid revisiting them

```java
public void dfs(V vertex, Set<V> visited) {
    visited.add(vertex);
    process(vertex);
    for (V neighbor : graph.adjacent(vertex)) {
        if (!visited.contains(neighbor)) {
            dfs(neighbor, visited);
        }
    }
}
```

---

# DFS — Iterative version

```java
public void dfs(V source) {
    Stack<V> stack = new LinkedStack<>();
    Set<V> visited = new HashSet<>();
    stack.push(source);
    while (!stack.isEmpty()) {
        V vertex = stack.pop();
        if (visited.contains(vertex)) continue;
        visited.add(vertex);
        process(vertex);
        for (V neighbor : graph.adjacent(vertex)) {
            if (!visited.contains(neighbor)) {
                stack.push(neighbor);
            }
        }
    }
}
```

---

# DFS — Visit order

.center[<img src="{{site.baseurl}}/presentation/graphs/dfs-trace.svg" width="55%">]

DFS dives down one branch all the way before backing up — like exploring a maze by always taking the next door before turning back. The order at each step depends on how neighbors are listed in the adjacency list.

???

Live trace from a tiny Python DFS program on this same graph.

Adjacency lists:
A: [B, D]
B: [A, C, E]
C: [B, E]
D: [A, E]
E: [B, C, D, F]
F: [E]

```
DFS from A

visit A
  -> follow edge A-B
  visit B
    -- skip A (already visited)
    -> follow edge B-C
    visit C
      -- skip B (already visited)
      -> follow edge C-E
      visit E
        -- skip B (already visited)
        -- skip C (already visited)
        -> follow edge E-D
        visit D
          -- skip A (already visited)
          -- skip E (already visited)
        backtrack from D
        -> follow edge E-F
        visit F
          -- skip E (already visited)
        backtrack from F
      backtrack from E
    backtrack from C
    -- skip E (already visited)
  backtrack from B
  -- skip D (already visited)
backtrack from A
```

Final visit order: A → B → C → E → D → F

Indentation = recursion depth. Every `backtrack` line corresponds to popping a frame off the call stack — in an iterative implementation those are literally `stack.pop()` calls.

---

# DFS Applications

* Detect **cycles** in a graph
* Find **connected components**
* **Topological sorting** in DAGs
* Solve **mazes** and puzzles
* Find **paths** between two vertices
* Classify edges (tree, back, forward, cross)
* Strongly connected components (Tarjan, Kosaraju)

???

**Strongly connected components (SCCs)** apply to *directed* graphs.

A **strongly connected component** is a maximal set of vertices where every vertex can reach every other one *following edge directions*. The "strongly" part matters — in a digraph, A → B doesn't imply B → A. An SCC is a group where the round trip always works.

Example — directed edges A→B, B→C, C→A, C→D, D→E, E→D:
- {A, B, C} is one SCC (they cycle)
- {D, E} is another SCC (they reach each other)
- C reaches D but D can't reach C → different SCCs

**Why care?**
- Detect cyclic dependencies in build/import graphs
- Compress a digraph: collapse each SCC into a super-node → result is always a DAG
- Find groups in social networks where information flows both ways

**Two classic algorithms — both O(V + E):**

- **Kosaraju**: DFS on the original graph recording finish times → reverse all edges → DFS again in reverse finish-time order. Each DFS tree in pass 2 is an SCC. *Two passes, simple to explain.*
- **Tarjan**: single DFS tracking each vertex's discovery time and "low-link" (lowest discovery time reachable). When low-link == discovery time, you've found an SCC root. *One pass, more elegant, trickier.*

In practice Tarjan is used more (single pass, cache-friendly). Kosaraju is taught more (easier to explain).

---

# Breadth-First Search (BFS)

* Use a **queue** to process vertices in order of distance from the source
* Vertices are visited in **layers** (level by level)

```java
public void bfs(V source) {
    Queue<V> queue = new LinkedQueue<>();
    Set<V> visited = new HashSet<>();
    queue.enqueue(source);
    visited.add(source);
    while (!queue.isEmpty()) {
        V vertex = queue.dequeue();
        process(vertex);
        for (V neighbor : graph.adjacent(vertex)) {
            if (!visited.contains(neighbor)) {
                visited.add(neighbor);
                queue.enqueue(neighbor);
            }
        }
    }
}
```

---

# BFS — Visit order

.center[<img src="{{site.baseurl}}/presentation/graphs/bfs-trace.svg" width="75%">]

BFS spreads out in concentric "rings" from the source — every vertex at distance $k$ is visited before any vertex at distance $k+1$.

---

# BFS Applications

* **Shortest path** in *unweighted* graphs
* Find connected components
* Test if a graph is **bipartite**
* **Web crawlers**
* Social network "**degrees of separation**"
* Garbage collection (mark phase)
* Broadcasting in networks

---

# DFS vs. BFS

| Aspect           | DFS                        | BFS                          |
|------------------|----------------------------|------------------------------|
| Data structure   | Stack (or recursion)       | Queue                        |
| Memory           | $O(h)$ — height            | $O(w)$ — width               |
| Order            | Deep first                 | Level by level               |
| Finds            | A path                     | **Shortest** path (unweighted) |
| Use case         | Connectivity, cycles, topo | Distances, layered traversal |

--

* If the graph has a wide branching factor, BFS can use a lot of memory
* If the graph is very deep, DFS can blow the stack

---

# Detecting cycles with DFS

```java
private boolean hasCycle(V vertex, V parent, Set<V> visited) {
    visited.add(vertex);
    for (V neighbor : graph.adjacent(vertex)) {
        if (!visited.contains(neighbor)) {
            if (hasCycle(neighbor, vertex, visited)) return true;
        } else if (!neighbor.equals(parent)) {
            return true; // visited and not the parent → cycle
        }
    }
    return false;
}
```

For undirected graphs: a back edge to a non-parent visited vertex means there's a cycle.

---

# Shortest path with BFS

In an **unweighted** graph, BFS finds the shortest path from a source to every other vertex.

```java
public Map<V, Integer> distances(V source) {
    Map<V, Integer> dist = new HashMap<>();
    Queue<V> queue = new LinkedQueue<>();
    dist.put(source, 0);
    queue.enqueue(source);
    while (!queue.isEmpty()) {
        V v = queue.dequeue();
        for (V w : graph.adjacent(v)) {
            if (!dist.containsKey(w)) {
                dist.put(w, dist.get(v) + 1);
                queue.enqueue(w);
            }
        }
    }
    return dist;
}
```

---

# Beyond DFS and BFS

When edges have weights or we need richer answers, more specialized algorithms come into play:

* **Shortest paths in weighted graphs**: Dijkstra, Bellman-Ford, Floyd-Warshall
* **Minimum spanning trees**: Prim, Kruskal
* **Strongly connected components**: Tarjan, Kosaraju
* **Maximum flow**: Ford-Fulkerson, Edmonds-Karp
* **Matching**: Hopcroft-Karp

--

Many of these build on DFS/BFS as primitives.

---

# Real-world use cases

* **Google Maps / Waze**: Dijkstra (and variants) for shortest routes
* **Social networks**: BFS for "people you may know"
* **Compilers**: DAGs for instruction scheduling, control-flow graphs
* **Package managers**: topological sort to install dependencies in order
* **Search engines**: PageRank on the graph of web pages
* **Recommendation systems**: graph embeddings, random walks
* **Logistics & operations research**: flow networks, matching, TSP

---

# Cost summary

| Operation                  | Adjacency Matrix | Adjacency List |
|----------------------------|------------------|----------------|
| Space                      | $O(V^2)$         | $O(V + E)$     |
| DFS / BFS traversal        | $O(V^2)$         | $O(V + E)$     |
| Detect cycle               | $O(V^2)$         | $O(V + E)$     |
| Shortest path (unweighted) | $O(V^2)$         | $O(V + E)$     |

--

For large, sparse graphs the difference is dramatic — a million-vertex sparse graph fits in an adjacency list but not in an adjacency matrix.
