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

.center[![]({{site.baseurl}}/presentation/queue/queue_interface.png)]

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