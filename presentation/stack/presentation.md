class: center, middle, inverse

# Stack

---

# Agenda

* Stack
  * Definition
  * Usages
  * ADT Stack Interface
  * StringStack
  * Generics in Java
  * <Item> Stack
  * Iterable & Iterators

---

# What is an Stack?

* Stacks (the same as queues) are fundamental data structure types
* A *pushdown stack* (or just a *stack*) is a collection that is based on the last-in-first-out (LIFO) policy
* Operations: insert (push), remove (pop), iterate, test if empty.

.center[![]({{site.baseurl}}/presentation/stack/stack.png)]

---

# Usages 

* When you keep your mail in a pile on your desk, you are using a stack. 
  * You pile pieces of new mail on the top when they arrive and take each piece of mail from the top when you are ready to read it
* Email inbox strategy
  * Advantages / disadvantages
* Surfing the web
* StackOverflowException
* Arithmetic expression evaluation
  * `( 1 + ( ( 2 + 3 ) * ( 4 * 5 ) ) )`

---

# Client, implementation, interface

* Definitions: 
  * *Client:* program using operations defined in interface.
  * *Implementation:* actual code implementing operations.
  * *Interface:* description of data type, basic operations.

* Separate interface and implementation.
  * Ex: stack, queue, bag, priority queue, symbol table, union-find,...

* Benefits:
  * Client can't know details of implementation ⇒ client has many implementation from which to choose.
  * Implementation can't know details of client needs ⇒ many clients can re-use the same implementation.
  * **Design:** creates modular, reusable libraries.
  * **Performance:** use optimized implementation where it matters.

---

# Warmup ADT Stack Interface

* *Warmup API.* Stack of strings data type

.center[![]({{site.baseurl}}/presentation/stack/stack_warmup.png)]

* *Warmup client.* Reverse sequence of strings from standard input.

---

# Parameterized stack

* Previous class is `StackOfStrings`
  * We also want: `StackOfURLs`, `StackOfInts`, `StackOfVans`, ...

* **Attempt 1.** Implement a separate stack class for each type.
  * Rewriting code is tedious and error-prone.
  * Maintaining cut-and-pasted code is tedious and error-prone.

* **@#$!** most reasonable approach until Java 1.5

---

# Parameterized stack

* Previous class is `StackOfStrings`
  * We also want: `StackOfURLs`, `StackOfInts`, `StackOfVans`, ...

* **Attempt 2.** Implement a stack with items of type Object.
  * Casting is required in client.
  * Casting is error-prone: run-time error if types mismatch.

.center[![]({{site.baseurl}}/presentation/stack/stack_of_strings_take2.png)]

---

# Parameterized stack

* Previous class is `StackOfStrings`
  * We also want: `StackOfURLs`, `StackOfInts`, `StackOfVans`, ...

* **Attempt 3.** Java generics.
  * Avoid casting in client.
  * Discover type mismatch errors at *compile-time* instead of *run-time*.

.center[![]({{site.baseurl}}/presentation/stack/stack_of_strings_take3.png)]


* **Guiding principles.** Welcome compile-time errors; avoid run-time errors.

---

# Generic ADT Stack Interface

.center[![]({{site.baseurl}}/presentation/stack/stack_interface.png)]

---

# <Item> Stack Implementation

* We have to implementations available:
  * Fixed-capacity stack: array implementation
  * linked-list implementation (we will cover this one in future lessons) 

* Use array s[] to store N items on stack.
  * `push()`: add new item at s[N].
  * `pop()`: remove item from s[N-1]

.center[![]({{site.baseurl}}/presentation/stack/array_stack.png)]

* **Defect.** Stack overflows when N exceeds capacity. *[stay tuned]*

---

# <Item> Stack Implementation

.center[![]({{site.baseurl}}/presentation/stack/stack_of_strings.png)]

---

# Stack considerations

* **Overflow and underflow.**
  * *Underflow:* throw exception if pop from an empty stack.
  * *Overflow:* use resizing array for array implementation. [stay tuned]

* **Null items.** We allow null items to be inserted.

* **Loitering.** Holding a reference to an object when it is no longer needed. 

```java
  public Item pop() 
  { 
    return a[--N]; 
  }

vs

  public Item pop() 
  { 
    Item item = a[--N];
    a[N] = null;
    return item; 
  }
```

---

# Stack: resizing-array implementation

* **Problem.** Requiring client to provide capacity does not implement API!
* **Q.** How to grow and shrink array?

* **First try.**
  * push(): increase size of array s[] by 1.
  * pop(): decrease size of array s[] by 1.

* **Too expensive.**
  * Need to copy all items to a new array, for each operation.
  * Array accesses to insert first N items = N + (2 + 4 + ... + 2(N – 1)) ~ N^2.
  * **N:** 1 array access per push
  * **2(N – 1):** array accesses to expand to size N (ignoring cost to create new array)

* **Challenge.** Ensure that array resizing happens infrequently

---

# Stack: resizing-array implementation

* **Q.** How to grow array?
* **A.** If array is full, create a new array of *twice* the size ("repeated doubling"), and copy items.

```java
private void resize(int max)
{ // Move stack of size N <= max to a new array of size max.
  Item[] temp = (Item[]) new Object[max];
  for (int i = 0; i < N; i++)
    temp[i] = a[i];
  a = temp;
}

public void push(Item item)
{  // Add item to top of stack.
  if (N == a.length) resize(2*a.length);
  a[N++] = item;
}
```

* **Array accesses to insert first N = 2^i items.** N + (2 + 4 + 8 + … + N) ~ 3N.
  * N: 1 array access per push
  * (2 + 4 + 8 + … + N): k array accesses to double to size k (ignoring cost to create new array)

---

# Stack: resizing-array implementation

* **Q.** How to shrink array?
* **First try.**
  * `push()`: double size of array s[] when array is full
  * `pop()`: halve size of array s[] when array is *one-half full*

* **Too expensive in worst case.**
 * Consider push - pop - push - pop - ... sequence when array is full
 * Each operation takes time proportional to N.

.center[![]({{site.baseurl}}/presentation/stack/shrink_array.png)]

---

# Stack: resizing-array implementation

* **Q.** How to shrink array?
* **Efficient solution.**
  * `push()`: double size of array s[] when array is full
  * `pop()`: halve size of array s[] when array is *one-quarter full*

```java
public Item pop()
{ // Remove item from top of stack.
  Item item = a[--N];
  a[N] = null;  // Avoid loitering.
  if (N > 0 && N == a.length / 4) resize(a.length / 2);
  return item;
}
```

---

# Stack resizing-array implementation: performance

* **Amortized analysis.** Starting from an empty data structure, average running time per operation over a worst-case sequence of operations.
* **Proposition.** Starting from an empty stack, any sequence of M push and pop operations takes time proportional to M.

.center[![]({{site.baseurl}}/presentation/stack/resizing_stack_perf.png)]

---

# Iteration

* **Design challenge.** Support iteration over stack items by client, without revealing the internal representation of the stack. 

.center[![]({{site.baseurl}}/presentation/stack/it_design_challenge.png)]

* **Java solution.** Make stack implement the `java.lang.Iterable` interface.

---

# Iterators

* **Q.** What is an `Iterable`?
* **A.** Has a method that returns an `Iterator`

```java
public interface Iterable<Item>
{
  Iterator<Item> iterator();
}
```

* **Q.** What is an `Iterator` ?
* **A.** Has methods `hasNext()` and `next()`

```java
public interface Iterator<Item>
{
  boolean hasNext();
  Item next();
  void remove(); //optional; use at your own risk
}
```

---

# Iterators

* **Q.** Why make data structures `Iterable`?
* **A.** Java supports elegant client code.

```java
//shorthand
for (Item i : stack)
 System.out.println(i);
```
```java
//longhand
Iterator<Item> i = stack.iterator();
while (i.hasNext())
{
  Item i = i.next();
  System.out.println(i);
}
```

---

# Stack iterator

```java
import java.util.Iterator;

public class Stack<Item> implements Iterable<Item>
{
  ...
  public Iterator<Item> iterator() { return new ReverseArrayIterator(); }
 
  private class ReverseArrayIterator implements Iterator<Item>
  {
    private int i = N;
    public boolean hasNext() { return i > 0; }
    public Item next() { return s[--i]; }
    public void remove() { /* not supported */ }
  }
}
```

---

# Iteration: Concurrent Modification

* **Q.** What if client modifies the data structure while iterating?
* **A.** A fail-fast iterator throws a `java.util.ConcurrentModificationException`.

```java
for (Item i : stack)
  stack.push(i);
```

* **Q.** How to detect?
* **A.**
  * Count total number of push() and pop() operations in Stack.
  * Save counts in Iterator subclass upon creation.
  * If, when calling next() and hasNext(), the current counts do not equal the saved counts, throw exception.
