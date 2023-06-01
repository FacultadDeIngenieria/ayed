class: center, middle, inverse

# Balanced Trees

---

# Balanced Trees

* A large, perfectly balanced, binary search tree

.center[![]({{site.baseurl}}/presentation/balancedtrees/balanced.png)]

???

The external nodes in this BST all fall on one of two levels, and the number of comparisons for any search is the same as the number of comparisons that would be used by binary search for the same key (if the items were in an ordered array). The goal of a balanced-tree algorithm is to keep a BST as close as possible to being as well balanced as this one, while still supporting efficient dynamic insertion, deletion, and other dictionary ADT operations.

---

# Balanced Trees

* BST algorithms are widely used in various applications, but they do have the problem of bad worst-case performance

--

* Worst-case scenarios for standard BST algorithms, are likely to occur in real-world situations
  * Files that are already sorted
  * Files containing a large number of duplicate keys
  * Files arranged in reverse order

--

* Quadratic construction times and linear search times!

---

# Balancing

* Periodic explicit rebalancing

--

  * We can balance most BSTs completely in linear time using a recursive algorithm, but without guarantees
  * Eventual rebalancing: insertion time for a sequence of keys between rebalancing operations can grow quadratic in the length of the sequence
  * Frequent rebalancing: each rebalancing operation costs at least linear time in the size of the tree.

--

* Difficult to use global rebalancing to guarantee fast performance in BSTs

--

* All the algorithms that we will consider, as they walk through the tree, do incremental, local operations that collectively improve the balance of the whole tree


---

# Balancing

* Let's provide _guaranteed performance_ for symbol-table implementations based on BSTs


* What we mean when we ask for performance guarantees?

---

# Performance guarantees in algorithm design

General approaches to providing performance guarantees in algorithm design
* Randomize
* Amortize
* Optimize

---

# Performance guarantees in algorithm design

## Randomize

A randomized algorithm introduces random decision making into the algorithm itself in order to reduce dramatically the chance of a worst-case scenario (no matter what the input)

--

* Randomized BSTs 
* Skip lists
* These algorithms are simple and broadly applicable but went undiscovered for decades

---

# Performance guarantees in algorithm design

## Amortize

An amortization approach does extra work at one time to avoid more work later in order to be able to provide guaranteed upper bounds on the average per-operation cost (the total cost of all operations divided by the number of operations)

--

* Splay BSTs
* The algorithm is a straightforward extension of the root insertion method

---

# Performance guarantees in algorithm design

## Optimize

An optimization approach takes the trouble to provide performance guarantees for every operation.

--

* These methods require that we maintain some structural information in the trees
* Programmers typically find the algorithms cumbersome to implement  

---

# Randomized BSTs

* To analyze the average-case performance costs for binary search trees, we made the assumption that the items are inserted in random order

* Property: each node in the tree is equally likely to be the one at the root

* It is possible to introduce _randomness_ into the insertion algorithm so that this property holds without any assumptions about the order in which the items are inserted

---

# Root Insertion

In the standard implementation of BSTs, every new node inserted goes somewhere at the bottom of the tree, replacing some external node

**Root Insertion:** each new item to be inserted at the root so that recently inserted nodes are at the top of the tree

* How to handle previous root children?
* Solution to this problem: use rotations

???

Suppose that the key of the item to be inserted is larger than the key at the root. We might start to make a new tree by putting the new item into a new root node, with the old root as the left subtree and the right subtree of the old root as the right subtree. However, the right subtree may contain some smaller keys, so we need to do more work to complete the insertion. Similarly, if the key of the item to be inserted is smaller than the key at the root and is larger than all the keys in the left subtree of the root, we can again make a new tree with the new item at the root, but more work is needed if the left subtree contains some larger keys. To move all nodes with smaller keys to the left subtree and all nodes with larger keys to the right subtree seems a complicated transformation in general, since the nodes that have to be moved can be scattered along the search path for the node to be inserted.

---

# Rotations

## Rotate Right

```java
private fun rotateRight(node : Node<K, V>) : Node<K, V> {
    val result = node.left!!
    node.left = result.right
    result.right = node
    return result
}
```

--

.center[![]({{site.baseurl}}/presentation/balancedtrees/right-rotation.png)]

---

# Rotations

## Rotate Left

```java
private fun rotateLeft(node : Node<K, V>) : Node<K, V> {
    val result = node.right!!
    node.right = result.left
    result.left = node
    return result
}
```

--

.center[![]({{site.baseurl}}/presentation/balancedtrees/left-rotation.png)]

---

# Root Insertion

.center[![]({{site.baseurl}}/presentation/balancedtrees/root-insertion.png)]

---

# Root Insertion

This sequence depicts the result of inserting the keys A S E R C H I into an initially empty BST, using the root insertion method.

.center[![]({{site.baseurl}}/presentation/balancedtrees/root-insertion-multiple.png)]

--

In practice, an advantage of the root insertion method is that recently inserted keys are near the top. The cost for search hits on recently inserted keys therefore is likely to be lower than that for the standard method.

---

# Root Insertion

```java
private fun rootPut(node: Node<K, V>?, key: K, value: V): Node<K, V>? {
    return when (node) {
        null -> {
            size++
            Node(key, value)
        }
        else -> {
            val cmp = comparator.compare(key, node.key)
            when {
                cmp < 0 -> {
                    node.left = rootPut(node.left, key, value)
                    rotateRight(node)
                }
                cmp > 0 -> {
                    node.right = rootPut(node.right, key, value)
                    rotateLeft(node)
                }
                else -> {
                    node.value = value
                    node
                }
            }
        }
    }
}
```

---

# Randomized BSTs

When we insert a new node into a tree of N nodes, the new node should appear at the root with probability 1/(N + 1), so we simply make a randomized decision to use root insertion with that probability

--

.center[![]({{site.baseurl}}/presentation/balancedtrees/randomized-insertion.png)]

---

# Randomized BSTs

```java
private fun randomizePut(node: Node<K, V>?, key: K, value: V): Node<K, V>? {
    return when (node) {
        null -> {
            size++
            Node(key, value)
        }

        else -> {
            val accept = Random().nextInt(size) == 0
            if (accept) rootPut(node, key, value)
            else {
                val cmp = comparator.compare(key, node.key)
                when {
                    cmp < 0 -> node.left = randomizePut(node.left, key, value)
                    cmp > 0 -> node.right = randomizePut(node.right, key, value)
                    else -> node.value = value
                }
                node
            }
        }
    }
}
```

---

# Balancing

Randomized BSTs guarantees performance in a probabilistic sense, but still admit the possibility that a particular search operation could take linear time.

### Is there a type of BST for which we can guarantee that each and every insert and search operation will be logarithmic in the size of the tree?

--

* To guarantee that our BSTs will be balanced, we need flexibility in the tree structures that we use.
* To get this flexibility, let us assume that the nodes in our trees can hold more than one key
* We will allow 3-nodes and 4-nodes, which can hold two and three keys, respectively

---

# 2, 3, 4 Trees

A 2-3-4 search tree is a tree that either is empty or comprises three types of nodes: 

* **2-nodes**, with one key, a left link to a tree with smaller keys, and a right link to a tree with larger keys
* **3-nodes**, with two keys, a left link to a tree with smaller keys, a middle link to a tree with key values between the node’s keys and a right link to a tree with larger keys
* **4-nodes**, with three keys and four links to trees with key values defined by the ranges subtended by the node’s keys

--

A balanced 2-3-4 search tree is a 2-3-4 search tree with all links to empty trees at the same distance from the root.

.center[![]({{site.baseurl}}/presentation/balancedtrees/234.png)]

---

# 2, 3, 4 Insertion

.center[![]({{site.baseurl}}/presentation/balancedtrees/234-insertion-basic.png)]

---

# 2, 3, 4 Insertion

.center[![]({{site.baseurl}}/presentation/balancedtrees/234-insertion.png)]

---

# 2, 3, 4 Searches

### Searches in N-node 2-3-4 trees visit at most lg N + 1 nodes.

Every external node is the same distance from the root: The transformations that we perform have no effect on the distance from any node to the root, except when we split the root (in this case the distance from all nodes to the root is increased by 1). If all the nodes are 2-nodes, the stated result holds, since the tree is like a full binary tree; if there are 3-nodes and 4-nodes, the height can only be lower.

---

# Red-Black Trees

The 2-3-4 insertion algorithm is easy to understand, but implementing it directly is cumbersome because of all the different cases that can arise.

### The basic idea is to represent 2-3-4 trees as standard BSTs (2-nodes only) but to add one extra bit of information per node to encode 3-nodes and 4-nodes.

---

# Red-Black Trees

We think of the links as being of two different types: 
* red links, which bind together small binary trees comprising 3-nodes and 4-nodes
* black links, which bind together the 2-3-4 tree.

.center[![]({{site.baseurl}}/presentation/balancedtrees/234-to-rb.png)]

We represent 4-nodes as three 2-nodes connected by red links, and 3-nodes as two 2-nodes connected by a single red link. The red link in a 3-node may be a left link or a right link, so there are two ways to represent each 3-node.

### In any tree, each node is pointed to by one link, so coloring the links is equivalent to coloring the nodes.

---

# Red-Black Trees

.center[![]({{site.baseurl}}/presentation/balancedtrees/rb.png)]

Any path from the root to an external node in this tree has three black links

--

### Properties

1. Any path from the root to an external node in this tree has three black links
2. They correspond directly to 2-3-4 trees, so we can implement the balanced 2-3-4 tree algorithm by maintaining the correspondence

We get the best of both worlds: the simple search method from the standard BST and the simple insertion–balancing method from the 2-3-4 search tree.

---

# Red-Black Trees

### Search overhead

The search method never examines the field that represents node color, so the balancing mechanism adds no overhead to the time taken by the fundamental search procedure. 

--

### Insertion overhead

The overhead for insertion is small: we have to take action for balancing only when we see 4-nodes. If a node has two red children, it is a part of a 4-node.

---

# Red-Black Trees

### Splitting

.center[![]({{site.baseurl}}/presentation/balancedtrees/rb-splitting.png)]


---

# Red-Black Trees

### Insertion

.center[![]({{site.baseurl}}/presentation/balancedtrees/rb-insertion.png)]

---

# Red-Black Trees

### Insertion with rotation

.center[![]({{site.baseurl}}/presentation/balancedtrees/rb-insertion-rotation.png)]

---

# Red-Black Trees

.center[![]({{site.baseurl}}/presentation/balancedtrees/rb-construction.png)]
