class: center, middle, inverse

# Tree

---

# Agenda

* Tree
  * Usages and importance
  * Definition(s)
  * Binary tree
    * M-ary tree
  * Mathematical Properties of Binary Trees
  * Tree traversal
    * Preorder
    * Inorder
    * Postorder
  
---

# Usages and importance

* Trees are a mathematical abstraction that play a central role in the design and analysis of algorithms because
  * Algorithms dynamic sometimes behave like a tree (UnionFind)
  * Build and use explicit data structures that are concrete realizations of trees (recursive algorithms call structure)
* Present in everyday life
  * Keep track of ancestors or descendants with a family tree (terminology)
  * Sports tournaments
  * Organizational chart in a large corporation
  * Parse tree of an English sentence (language processing)
  * File systems

---

# Definition(s)

* A *tree* is a nonempty collection of *vertices* and *edges* that satisfies certain
requirements
* A *vertex* is a simple object (also referred to as a *node*) that can have a name and can carry other associated information
* An *edge* is a connection between two vertices
* A *path* in a tree is a list of distinct vertices in which successive vertices are connected by edges in the tree
* The defining property of a *tree* is that there is precisely one *path* connecting any two nodes
* If there is more than one path between some pair of nodes, or if there is no path between some pair of nodes, then we have a *graph*
* A disjoint set of trees is called a *forest*

---

# Definition(s)

* A *rooted tree* is one where we designate one node as the *root* of a tree
  * In computer science, we normally reserve the term *tree* to refer to *rooted trees*
  * In a *rooted tree*, any node is the *root* of a *subtree* consisting of it and the nodes below it
* Each node (except the *root*) has exactly one node above it, which is called its *parent*
* The nodes directly below a node are called its *children*
* Nodes with no children are called *leaves*, or *terminal* nodes
  * nodes with at least one child are sometimes called *nonterminal* nodes.

---

# Definition(s)

.center[![]({{site.baseurl}}/presentation/tree/type_of_trees.png)]

---

# Definition(s)

* Trees
* Rooted trees
* Ordered trees
  * Rooted tree in which the order of the children at every node is specified
* M-ary trees
  * If each node must have a specific number of children appearing in a specific
order, then we have an M-ary tree
* Binary trees
  * Simplest type of M-ary tree is the binary tree
  * Is an ordered tree consisting of two types of nodes: external nodes with no children and internal nodes with exactly two children
  * Since the two children of each internal node are ordered, we refer to the *left child* and the *right child* of internal nodes: every internal node must have both a left and a right child, although one or both of them might be an external node

---

# Binary tree

* A **binary tree** is either an external node or an internal node connected to a pair of binary trees, which are called the left subtree and the right subtree of that node
* The concrete representation that we use most often when implementing programs that use and manipulate binary trees is a structure with two links (a left link and a right link) for internal nodes
* These structures are similar to *linked lists*, but they have two links per node, rather than one

```java
class Node<E> { 
  E item; 
  Node<E> l; 
  Node<E> r;
  
  Node(Item v, Node l, Node r) { 
    this.item = v; this.l = l; this.r = r; 
  }
}
```

---

# Binary tree

.center[![]({{site.baseurl}}/presentation/tree/binary_trees.png)]

* This standard representation allows for efficient implementation of operations that call for moving down the tree from the root, but not for operations that call for moving up the tree from a child to its parent
* For algorithms that require such operations, we might add a third link to each node, pointing to the parent
  * This alternative is analogous to a *doubly linked list*

---

# M-ary tree

* An *M-ary tree* is either an *external node* or an *internal node* connected to an ordered sequence of M trees that are also M-ary trees
* After binary trees, the other most common one is 3-ary (ternary) trees
  * we use structures with three named links (*left*, *middle*, and *right*) each of which has specific meaning for associated algorithms

---

# Mathematical Properties of Binary Trees

* *A binary tree with N internal nodes has N + 1 external nodes*
* *A binary tree with N internal nodes has 2N links: N - 1 links to internal nodes and N + 1 links to external nodes*

---

# More definitions

* The **level** of a node in a tree is one higher than the level of its parent (with the root at level 0)
* The **height** of a tree is the maximum of the levels of the tree's nodes
* The **path length** of a tree is the sum of the levels of all the tree's nodes
* The **internal path length** of a binary tree is the sum of the levels of all the tree's internal nodes
* The **external path length** of a binary tree is the sum of the levels of all the tree's external nodes

---

# Mathematical Properties of Binary Trees

* The external path length of any binary tree with N internal nodes is 2N greater than the internal path length
* The height of a binary tree with N internal nodes is at least lg N and at most N - 1
* The internal path length of a binary tree with N internal nodes is at least N
lg(N/4) and at most N(N - 1)/2
* Binary trees appear extensively in computer applications, and performance is best when the binary trees are fully balanced (or nearly so)

---

# Tree traversal

* Before considering algorithms that construct binary trees and trees, we consider algorithms for the most basic tree-processing function: *tree traversal*
  * Given a (reference to a) tree, we want to process every node in the tree systematically
  * In a linked list, we move from one node to the next by following the single link; for trees, however, we have decisions to make, because there may be multiple links to follow
* We begin by considering the process for binary trees
  * For linked lists, we had two basic options: 
    * process the node and then follow the link (in which case we would visit the nodes in order), or 
    * follow the link and then process the node (in which case we would visit the nodes in reverse order)

---

# Tree traversal

* For binary trees, we have two links, and we therefore have three basic orders in which we might visit the nodes:
  * *Preorder*, where we visit the node, then visit the left and right subtrees 
  * *Inorder*, where we visit the left subtree, then visit the node, then visit the right subtree
  * *Postorder*, where we visit the left and right subtrees, then visit the node
* The recursive implementation is pretty strightforward

---

# Recursive tree traversal

```java
private void recursiveTraverse(Node h) {
  if (h == null) return; 
  h.item.visit(); 
  recursiveTraverse(h.l); 
  recursiveTraverse(h.r);
}

void traverse() { 
  recursiveTraverse(root); 
}
```

* As is, the code implements a *preorder traversal*; 
* if we move the call to `visit` between the recursive calls, we have an *inorder traversal*;
* and if we move the call to `visit` after the recursive calls, we have a *postorder traversal*

---

# Nonrecursive tree traversal

* We use an stack
* For simplicity, we begin by considering an abstract stack
  * holds items or trees
  * initialized with the tree to be treversed
* We enter into a loop, where we pop and process the top entry on the stack, continuing until the stack is empty
  * If the popped entity is an item, we visit it;
  * if the popped entity is a tree, then we perform a sequence of push operations that depends on the desired ordering:
  * For *preorder*, we push the right subtree, then the left subtree, and then the node.
  * For *inorder*, we push the right subtree, then the node, and then the left subtree.
  * For *postorder*, we push the node, then the right subtree, and then the left subtree.

---

# Nonrecursive tree traversal

* Stack dynamic for *preorder*, *inorder*, *postorder*

.center[![]({{site.baseurl}}/presentation/tree/stack_dynamic.png)]

---

# Implementation

```java
private void stackTraverse(Node h)
{ 
  Stack<Node> s = new Stack<Node>(max);
  s.push(h);
  
  while (!s.empty()) {
    h = s.pop(); 
    
    h.item.visit();
    
    if (h.r != null) 
      s.push(h.r); 
    
    if (h.l != null) 
      s.push(h.l);
  }

}

void stackTraverse()
{ 
  stackTraverse(root); 
}
```

---

# Level-order traversal

* A fourth natural traversal strategy is simply to visit the nodes in a tree as they appear on the page, reading down from top to bottom and from left to right
* This method is called *level-order* traversal because all the nodes on each level appear together, in order

.center[![]({{site.baseurl}}/presentation/tree/level_traversal.png)]

---

# Implementation

* ?