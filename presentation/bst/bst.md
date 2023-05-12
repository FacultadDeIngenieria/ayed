class: center, middle, inverse

# BST

---

# ADT Map

* Arreglo asociativo / Tabla símbolos
* Operaciones
    * Buscar una entrada existente
```java
/** Returns the value to which the specified key is mapped */
public V get(K key);
```
    * Agregar / Modificar una entrada
```java
/** Associates the specified value with the specified key in this map */
public void put(K key, V value);
```
    * Remover una entrada
```java
/** Removes the mapping for a key from this map if it is present */
public remove(K key);
```

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
    public remove(K key);

    /** Returns the number of key-value mappings in this map */
    public Int size();

    /** Returns true if this map contains no key-value mappings */
    public boolean isEmpty() { return size() == 0; }

    /** Returns true if this map contains a mapping for the specified key */
    public boolean contains(K key);
}
```

---

# List based implementation

[java.util.List](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)

```java
public class ArrayMap<K,V> implements Map<K,V> {
{
    @NotNull private final List<K> keys;
    @NotNull private final List<V> values;

    private int size = 0;

    @Override public int size() { return size; }

    @Override public boolean contains(@NotNull K key) { 
        return indexOf(key) >= 0; 
    }

    private int indexOf(K key) {
        for (int i = 0; i < size-1; i++)
        {
            if(key.equals(keys.get(i))) return index;
        }
        return -1;
    }

    @Override public V get(@NotNull K key) {
        final int index = indexOf(key);
        return index >= 0 ? values.get(index) : null;
    }
```

---

# List based implementation

```java
    @Override public V put(@NotNull K key, V value) {
        final int index = indexOf(key);
        if(index == -1) {
            index = size++;
            keys.add(index, key);
        }
        values.add(index, value);
    }

    @Override public void remove(K key) {
        final int index = indexOf(key)
        if(index != -1) {
            for(int i = index + 1; i < size - 1; i++) {
                keys.set(i - 1, keys.get(i));
                values.set(i - 1, values.get(i));
            }
            size--;
        }
    }
```

---

# List based with binary search

```java
    private int indexOf(K key) {
        final int index = find(key, 0, size-1)
        return index < 0 ? -1 : index;
    }

    private int find(K key, int low, int high) {
        if(low > high) return -(low + 1);

        final int middle = (low + high) / 2;
        final int cmp = comparator.compare(key, keys.get(middle));

        return cmp == 0 ? middle :
            cmp < 0 ? find(key, 0, middle - 1) :
                find(key, middle + 1, high);
    }
```

---
# List based with binary search

```java
    @Override public V put(@NotNull K key, V value) {
        int index = find(key, 0, size-1);
        if(index < 0) {
            index = (-index) -1
            for(int i = size; i > index+1; i--) {
                keys.set(i, keys.get(i-1));
                values.set(i, values.get(i-1));
            }
            size++;
            keys.set(index, key);
        }
        values.set(index, value);
    }
```

---

# Análisis

* Búsqueda: O(log<sub>2</sub>N)
* Inserción: O(N)
* Ideal para pocas actualizaciones

---

# Búsqueda por Interpolación

* Idea: usar el **valor** de la clave para estimar la posición

.center[![]({{site.baseurl}}/presentation/bst/interpolacion.png)]

* Orden inserción (para distribución uniforme): O(log<sub>2</sub> log<sub>2</sub> N)

---

# Binary Trees

.center[![]({{site.baseurl}}/presentation/bst/arbol_binario.gif)]

---

# Tree Map

```java
/**
 * Tree map
 */
abstract class TreeMap<K, V> extends Map<K, V> {

    @Nullable protected Node<K, V> head = null;
    protected int size = 0;

    @Override public int size() { return size; }

    class Node<K, V> {
        K key;
        V value;
        Node<K, V> left = null;
        Node<K, V> right = null;
    
    
        public String toString() { return String.format("%s [%s]", key, value); }
    }
}
```

---

# Binary Search Tree

```java
@Override public V get(K key) { return find(head, key).value; }

@Override public boolean contains(key: K) { return find(head, key) != null; }

private Node<K, V> find(Node<K, V> node, K key) {
    if(node == null) return null;
    
    int cmp = comparator.compare(key, node.key)
    if (cmp == 0) return node;
    else if (cmp < 0) find(node.left, key);
    else find(node.right, key);
}
```

---

# Binary Search Tree


```java
@Override public void put(K key, V value) { put(head, key, value); }

private Node<K, V> put(Node<K, V> node, K key, V value) {
    if (node == null) {
        
        size++;
        return new Node(key, value);

    } else {
        
        int cmp = comparator.compare(key, node.key);
        
        if (cmp < 0) node.left = put(node.left, key, value);
        else if (cmp > 0) node.right = put(node.right, key, value);
        else node.value = value;
        
        return node;
    }
}
```

---

```java
@Override public void remove(K key) {
    head = remove(head, key);
}

private Node<K, V> remove(Node<K, V> node, K key) {
    if(node == null) return null;

    int cmp = comparator.compare(key, node.key);
    
    if (cmp < 0) {
        node.left = remove(node.left, key);
        return node;
    } else if (cmp > 0) {
        node.right = remove(node.right, key);
        return node;
    } else {
        size--;
        
        if(node.left == null) return node.right;
        else if (node.right == null) return node.left;
        else {
            
            Node<K, V> next = first(node.right);
            node.key = next.key;
            node.value = next.value;
            next.key = key;
            node.right = remove(node.right, key);
            return node;

        }
    }
}
```
