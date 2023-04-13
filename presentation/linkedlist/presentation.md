class: center, middle, inverse

# Linked List

---

# Agenda

* Definition
* Building a linked list
* Insert at the beginning
* Remove from the beginning
* Insert at the end
* Insert/remove at other positions
* Traversing a linked list

---

# What is a Linked List?

* Fundamental data structure that is an appropriate choice for representing the data in a collection ADT implementation
* Not directly supported by the Java language (sort of)
* The implementation serves as a model for the code that we use for building more complex data structures
* **Definition:** A *linked list* is a recursive data structure that is either empty (*null*) or a reference to a node having a generic item and a reference to a linked list.

```java
private class Node<Item>
{
  Item item;
  Node<Item> next; 
}
```

---

# Building a linked list

* To build a linked list that contains the items **to**, **be**, and **or**, we create a **Node** for each item, set the item field in each of the nodes to the desired value, and set the **next** fields to build the linked list.

.center[![]({{site.baseurl}}/presentation/linkedlist/linked-list.png)]

---

# Building a linked list

* A linked list represents a *sequence* of items
* An array can represent also a *sequence* of items

```java

  String[] s = { "to", "be", "or" };

````

* The difference is that it is easier to *insert* items into the sequence and to *remove* items from the sequence with **linked lists**.

---

# Insert at the beginning

.center[![]({{site.baseurl}}/presentation/linkedlist/linked-list-insert-front.png)]

* This code for inserting a node at the beginning of a linked list involves just a few assignment statements, so the amount of time that it takes is independent of the length of the list

---

# Remove from the beginning

* This operation is even easier:
  * simply assign to `first` the value `first.next` 
* Normally, you would retrieve the value of the item before doing this assignment
  * because once you change the value of `first`, you may not have any access to the node to which it was referring
  * Typically, the node object becomes an orphan, and the Java memory management system eventually reclaims the memory it occupies
* Again, this operation just involves one assignment statement, so its running time is independent of the length of the list.

.center[![]({{site.baseurl}}/presentation/linkedlist/linked-list-remove-first.png)]

---

# Insert at the end

* How do we add a node to the end of a linked list? 
  * To do so, we need a link to the last node in the list
* Maintaining an extra link is not something that should be taken lightly in linked-list code
  * every method that modifies the list needs code to check whether that variable needs to be modifie

.center[![]({{site.baseurl}}/presentation/linkedlist/linked-list-insert-end.png)]

---

# Insert/remove at other positions

* In summary, we can implement the following operations on linked lists with just a few instructions (provided that we have access to both a link `first` to the first element in the list and a link `last` to the last element in the list):
  * Insert at the beginning
  * Remove from the beginning
  * Insert at the end
* Other operations, such as the following, are not so easily handled:
  * Remove a given node
  * Insert a new node before a given node
* In the absence of any other information, the only solution is to traverse the entire list looking for the node that links to last
  * Undesirable because it takes time proportional to the length of the list
* The standard solution to enable arbitrary insertions and deletions is to use a *doubly-linked list*, where each node has two links, one in each direction.

---

# Traversal

* To examine every item in an array, we use familiar code like the following loop for processing the items in an array a[]:

```java
for (int i = 0; i < N; i++)
{
  // Process a[i].
}
```

* There is a corresponding idiom for examining the items in a linked list:

```java
for (Node x = first; x != null; x = x.next)
{
  // Process x.item.
}
```