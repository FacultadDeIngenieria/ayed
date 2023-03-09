class: center, middle, inverse

# Caso de estudio: Union-Find

---

# Conceptos

* Pasos para desarrollar un algoritmo usable:
  * Modelar el problema.
  * Encontrar un algoritmo para resolverlo.
  * ¿Es lo suficientemente rápido? ¿es una solución in-memory?
  * Si no lo es, encontrar el problema por qué.
  * Solucionar es problema.
  * Iterar hasta que uno esté satisfecho :-).

---

# Problema: Dynamic Connectivity

* We start with the following problem specification: The input is a sequence of pairs of integers, where each integer represents an object of some type and we are to interpret the pair p q as meaning “p is connected to q.”

.center[![]({{site.baseurl}}/presentation/unionfind/example1.png)]

---

# Huge Dynamic Connectivity Example

* **Q:** Is there a path connecting `p` and `q`?

.center[![]({{site.baseurl}}/presentation/unionfind/huge_dynamic_connections.png)]

* **A:** Yes.

---

# Modelado del problema

* We assume "is connected to" is an equivalence relation:
  * **Reflexive**: p is connected to p.
  * **Symmetric**: if p is connected to q, then q is connected to p.
  * **Transitive**: if p is connected to q and q is connected to r, then p is connected to r.

* **Connected component**: Maximal `set` of objects that are mutually connected.

.center[![]({{site.baseurl}}/presentation/unionfind/connected_components.png)]

---

layout: true

# Operations

---

* `Find.` In which component is object p ?
* `Connected.` Are objects p and q in the same component?
* `Union.` Replace components containing objects p and q with their union.

--

.center[![]({{site.baseurl}}/presentation/unionfind/operations.png)]

---

layout: false

# UnionFind Class

```java
public class UF
{
  private int[] id;   // access to component id (site indexed)
  private int count;  // number of components
  public UF(int N)    // Initialize component id array.
  {  
    count = N;
    id = new int[N];
    for (int i = 0; i < N; i++)
      id[i] = i;
  }

  public int count() {  return count;  }
  
  public boolean connected(int p, int q) {  return find(p) == find(q);  }
  
  public int find(int p)

  public void union(int p, int q)

}
```

---

# Quick-find [eager approach]

* Data structure.
  * Integer array `id[]` of length `N`.
  * Interpretation: `id[p]` is the id of the component containing `p`

.center[![]({{site.baseurl}}/presentation/unionfind/qf_data_structure1.png)]

* `Find.` What is the `id` of `p`?
* `Connected.` Do `p` and `q` have the same `id`?
* `Union.` To merge components containing `p` and `q`, change all entries whose `id` equals `id[p]` to `id[q]`

.center[![]({{site.baseurl}}/presentation/unionfind/qf_data_structure2.png)]

---

# Quick-find: `find()` & `union()`

```java
  public int find(int p)
    {  return id[p];  }
  
  public void union(int p, int q) // Put p and q into the same component.
  {  
    int pID = find(p);
    int qID = find(q);
    
    // Nothing to do if p and q are already in the same component.
    if (pID == qID) return;
     
    // Rename p’s component to q’s name.
    for (int i = 0; i < id.length; i++)
      if (id[i] == pID) id[i] = qID;
    
    count--; 
  }
```

---

# Quick-find overview

.center[![]({{site.baseurl}}/presentation/unionfind/quick_find.png)]

---

# Quick-find trace

.center[![]({{site.baseurl}}/presentation/unionfind/quick_find_trace.png)]

---

# Quick-find is too slow

* **Cost model.** Number of array accesses (for read or write).

.center[![]({{site.baseurl}}/presentation/unionfind/qf_too_slow.png)]

* **Union is too expensive.** It takes `N^2` array accesses to process a sequence of `N` union operations on `N` objects.

---

# Quick-union [lazy approach]

* Data structure.
  * Integer array `id[]` of length `N`.
  * Interpretation: `id[i]` is parent of `i`.
  * Root of `i` is `id[id[id[...id[i]...]]]`.

.center[![]({{site.baseurl}}/presentation/unionfind/qu_data_structure1.png)]
.center[![]({{site.baseurl}}/presentation/unionfind/qu_data_structure2.png)]

---

# Quick-union [lazy approach]

* `Find.` What is the `id` of `p`?
* `Connected.` Do `p` and `q` have the same `id`?
* `Union.` To merge components containing `p` and `q`, change all entries whose `id` equals `id[p]` to `id[q]`

.center[![]({{site.baseurl}}/presentation/unionfind/qu_data_structure3.png)]
.center[![]({{site.baseurl}}/presentation/unionfind/qu_data_structure4.png)]

---

# Quick-union: `find()` & `union()`

```java
  private int find(int p)
  {  // Find component name.
    while (p != id[p]) p = id[p];
    
    return p; 
  }
  
  public void union(int p, int q) // Give p and q the same root.
  {  
    int pRoot = find(p);
    int qRoot = find(q);
    
    if (pRoot == qRoot) return;
    
    id[pRoot] = qRoot;
    
    count--; 
  }
```

---

# Quick-union overview

.center[![]({{site.baseurl}}/presentation/unionfind/quick_union_overview.png)]

---

# Quick-union trace

.center[![]({{site.baseurl}}/presentation/unionfind/quick_union_trace1.png)]

---

# Quick-union trace

.center[![]({{site.baseurl}}/presentation/unionfind/quick_union_trace2.png)]

---

# Quick-union Worst Case

.center[![]({{site.baseurl}}/presentation/unionfind/quick_union_wc.png)]

---

# Quick-union is also too slow

* **Cost model.** Number of array accesses (for read or write).

.center[![]({{site.baseurl}}/presentation/unionfind/qu_too_slow.png)]

* **Quick-find defect.**
  * Union too expensive (N array accesses).
  * Trees are flat, but too expensive to keep them flat.
* **Quick-union defect.**
  * Trees can get tall.
  * Find/connected too expensive (could be N array accesses).

---

# Weighted Quick-union

```java
public class WeightedQuickUnionUF
{
  private int[] id;   // parent link (site indexed)
  private int[] sz;   // size of component for roots (site indexed)
  private int count;  // number of components

  public WeightedQuickUnionUF(int N) {
    count = N;
    id = new int[N];
    for (int i = 0; i < N; i++) id[i] = i;
    
    sz = new int[N];
    for (int i = 0; i < N; i++) sz[i] = 1;
  }

  public int count() {  return count;  }
  public boolean connected(int p, int q)  {  return find(p) == find(q);  }

  private int find(int p) // Follow links to find a root.
  {  
    while (p != id[p]) p = id[p];
    return p;
  }
```

---

# Weighted Quick-union

```java
  public void union(int p, int q)
  {
    int i = find(p);
    int j = find(q);
    if (i == j) return; // Make smaller root point to larger one.
    
    if (sz[i] < sz[j]) { 
      id[i] = j; 
      sz[j] = sz[j] + sz[i]; 
    } else { 
      id[j] = i; 
      sz[i] = sz[i] + sz[j]; 
    }
    count--; 
  }
```

---

# Weighted Quick-union trace

.center[![]({{site.baseurl}}/presentation/unionfind/wquick_union_trace.png)]
 
---

# Quick-union vs Weighted Quick-union

.center[![]({{site.baseurl}}/presentation/unionfind/qu_vs_wqu.png)]
 
---

# Performance characteristics

.center[![]({{site.baseurl}}/presentation/unionfind/perf_table.png)]
 
---