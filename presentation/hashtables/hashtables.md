class: center, middle, inverse

# Hash Tables

---

# Agenda

- Hashing
- Hash functions
- Separate chaining
- Linear probing

---

# ADT Map

```java
/**
 * Map interface. Defines an object that maps keys to values.
 */
interface Map<K, V> {

    /** Returns the value to which the specified key is mapped */
    public V get(K key);

    /** Associates the specified value with the specified key in this map */
    public void put(K key, V value);

    /** Removes the mapping for a key from this map if it is present */
    public void remove(K key);

    /** Returns the number of key-value mappings in this map */
    public Int size();

    /** Returns true if this map contains no key-value mappings */
    public boolean isEmpty() { return size() == 0; }

    /** Returns true if this map contains a mapping for the specified key */
    public boolean contains(K key);
}
```

---

# Hashing

- Basic plan:
    - Save items in a **key-indexed** table
    - index is a function of the key
- Basic implementation:
    - Unordered symbol table
    - One array for keys, one array for values
    - Store the value associated with key i in array entry i
    - Reference key-value pairs using arrays by doing arithmetic operations to transform keys into array indices
- Search algorithms that use **hashing**, two parts:
    - First, compute a hash function that transforms the search key into an array index
        - Ideally, different keys would map to different indices
        - This is generally beyond our reach, so we have to face the possibility that two or more different keys may hash to the same array index
    - Second part of a hashing search is a *collision-resolution*:
        - *Separate chaining*
        - *Linear probing*

---

# Hashing: the problem

.center[![]({{site.baseurl}}/presentation/hashtables/hash_collision.png)]

---

# Hashing: time-space tradeoff

- If there were **no memory limitation**:
    - we could do any search with only one memory access by simply using the key as an index in a **(potentially huge)** array
- If there were **no time limitation**:
    - we can get by with only a minimum amount of memory by using sequential search in an unordered array
- Hashing provides a way to use a reasonable amount of both memory and time to strike a balance between these two extremes
- We can trade off time and memory in hashing algorithms by adjusting parameters, not by rewriting code
- To help choose values of the parameters, we use classical results from probability theory

---

# Hash functions

- Computation that transforms keys into array indices
- Problem domain:
    - We have an array that can hold **M key-value** pairs
    - We need a hash function that can transform any given key into an index into that array: an integer in the range **[0, M–1]**
    - We seek a hash function that both:
        - easy to compute
        - uniformly distributes the keys: for each key, every integer between **0** and **M** should be equally likely (independently for every key)
- The hash function depends on the key type
    - we need a different hash function for each key type that we use
- For many common types of keys, we can make use of default implementations provided by Java

---

# Hash functions: some examples

- **Positive integers:** modular hashing, we choose the array size **M** to be **prime** and, for any positive integer key k, compute the remainder when dividing k by M (`k % M`, in Java)
- **Floating-point numbers**: If the keys are real numbers between *0* and *1*, we might just multiply by *M* and round off to the nearest integer to get an index between *0* and *M – 1*
- **Strings:** Modular hashing works for long keys such as strings, too: we simply treat them as huge integers.

```java
    int hash = 0;
    for (int i = 0; i < s.length(); i++)
        hash = (R * hash + s.charAt(i)) % M;
```

---

# Hashing functions: user defined types

- **Java conventions**: Java helps us address the basic problem that every type of data needs a hash function by ensuring that every data type inherits a method called `hashCode()` that returns a 32-bit integer
    - The implementation of `hashCode()` for a data type must be consistent with `equals()` 
- **Requirement.** 
    - If `x.equals(y)`, then `(x.hashCode() == y.hashCode())`
- **Highly desirable.** 
    - If `!x.equals(y)`, then `(x.hashCode() != y.hashCode())`
- Converting a `hashCode()` to an array index
    - combine `hashCode()` with modular hashing in our implementations to produce integers between **0** and **M – 1**

```java
    private int hash(K key)
    {
        return (key.hashCode() & 0x7fffffff) % M;
    }
```

- This code masks off the sign bit (to turn the 32-bit number into a 31-bit nonnegative integer) and then computes the remainder when dividing by M

---

# `hashCode() design`

- "Standard" recipe for user-defined types
    - Combine each significant field using the 31x + y rule
    - If field is a primitive type, use wrapper type hashCode()
    - If field is null, return 0
    - If field is a reference type, use hashCode()
    - If field is an array, apply to each entry

```java
public class Transaction
{
    ...
    private final String who;
    private final Date when;
    private final double amount;
    public int hashCode() {
        int hash = 17;
        hash = 31 * hash + who.hashCode();
        hash = 31 * hash + when.hashCode();
        hash = 31 * hash + ((Double) amount).hashCode();
        return hash;
    }
}
```

- How the `equals()` should be?

---

# Hashing with separate chaining

- A hash function converts keys into array indices
- The second component of a hashing algorithm is *collision resolution*: a strategy for handling the case when two or more keys to be inserted hash to the same index
- A straightforward and general approach to collision resolution is to build, for each of the M array indices, a linked list of the key-value pairs whose keys hash to that index
- This method is known as *separate chaining* because items that collide are chained together in separate linked lists
- The basic idea is to choose **M** to be sufficiently large that the lists are sufficiently short to enable efficient search through a two-step process:
    - hash to find the list that could contain the key
    - then sequentially search through that list for the key

.center[![]({{site.baseurl}}/presentation/hashtables/collisions.png)]

---

# Hashing with separate chaining

.center[![]({{site.baseurl}}/presentation/hashtables/separate_chaining.png)]

---

# Hashing with separate chaining

- **Consequence.** For M lists and N keys, number of compares (`equals()` and `hashCode()`) for search / insert is proportional to N / M
    - **M** too large ⇒ too many empty chains
    - **M** too small ⇒ chains too long
    - **Typical choice:** M ~ N / 4 ⇒ constant-time ops
- **Deletion.** To delete a key-value pair, simply hash to find the linked list containing the key, then invoke the `delete()` method
    - Reusing code in this way is preferable to reimplementing this basic operation on linked lists

---

# Hashing with linear probing

- Another approach to implementing hashing is to store N key-value pairs in a hash table of size M > N, relying on empty entries in the table to help with collision resolution
- Such methods are called *open-addressing* hashing methods
- The simplest, linear probing:
    - When a new key collides, find next empty slot, and put it there
- Linear probing is characterized by identifying three possible outcomes:
    - Key equal to search key: search hit
    - Empty position (null key at indexed position): search miss
    - Key not equal to search key: try next entry

---

# Hashing with linear probing

.center[![]({{site.baseurl}}/presentation/hashtables/linear_probing.png)]

---

# Hashing with linear probing: Analysis

- **Clustering.** The average cost of linear probing depends on the way in which the entries clump together into contiguous groups of occupied table entries, called *clusters*, when they are inserted
- 