class: center, middle, inverse

# Queue

---

# Agenda

* Queue
  * Definition
  * Generic ADT Queue Interface
  * Fixed capacity array implementation
  * Resizing array implementation
  
---

# Definition

* A FIFO queue (or just a queue) is a collection that is based on the first-in-first-out (FIFO) policy
* The policy of doing tasks in the same order that they arrive is one that we encounter frequently in everyday life:
  * people waiting in line at a theater
  * cars waiting in line at a toll booth
  * tasks waiting to be serviced by an application on your computer 
* One bedrock principle of any service policy is the perception of **fairness**

---

# Example

.center[![]({{site.baseurl}}/presentation/queue/queue_example.png)]

---

# Generic ADT Queue Interface

```java
public interface Queue<E> extends Iterable<E> {
    /**
     * Adds a new item to this queue.
     * @param  item the item to add
     */
    void enqueue(@NotNull E item);

    /**
     * Removes and returns the item on this queue that was least recently added.
     * @return the item on this queue that was least recently added
     * @throws NoSuchElementException if this queue is empty
     */
    @NotNull E dequeue();

    /**
     * Returns true if this queue is empty.
     * @return {@code true} if this queue is empty; {@code false} otherwise
     */
    boolean isEmpty();

    /**
     * Returns the number of items in the queue
     * @return  size
     */
    int size();
}
```

---

# How to implement a fixed-capacity queue with an array?

* Can't be done efficiently with an array.

.center[![]({{site.baseurl}}/presentation/queue/queue_array.png)]

---

# Queue: resizing-array implementation

* Use array `q[]` to store items in `queue`
  * `enqueue()`: add new item at `q[tail]`
  * `dequeue()`: remove item from `q[head]`
  * Update `head` and `tail` modulo the `capacity`
  * Add resizing array

.center[![]({{site.baseurl}}/presentation/queue/queue_resizing_array.png)]

* **Q.** How to resize?

---

# Queue: resizing-array trace

.center[![]({{site.baseurl}}/presentation/queue/resizing_array_queue_test.png)]