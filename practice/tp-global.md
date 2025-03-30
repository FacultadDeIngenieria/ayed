---
title: Práctica Global
layout: practice
permalink: /practice/global
---

# Práctica Global: 

* Entrega: Fin de cursada

## Ejercicio 1

* Implementar la clase `algorithms.stack.DoublyLinkedStack` que implemente la interfaz `algorithms.stack.Stack`.
  * Debe ser Iterable
  * Debe utilizar una Double Linked List para su implementación
  * Debe detectar y soportar underflow (arrojar una NoSuchElementException exception)

## Ejercicio 2

* Implementar la clase `algorithms.queue.DoubleStackQueue` que implemente la interfaz `algorithms.queue.Queue`
  * Debe ser Iterable
  * Debe utilizar dos stacks para su implementación, donde uno almacena los elementos de entrada (in) y el otro los de salida (out)
  * Debe detectar y soportar underflow (arrojar una NoSuchElementException exception)


## Ejercicio 3
* Implementar la clase `algorithms.queue.CircularLinkedListQueue` que implemente la interfaz `algorithms.queue.Queue`
  * Debe utilizar un único puntero para representar la cola
  * Dicho puntero (last) referencia al último nodo de la lista
  * A diferencia de la LinkedListQueue, el último nodo no tiene un next nulo, sino que siempre referencia al primer nodo dentro de la cola
  * Es por ésto que se genera una lista circular, en donde tanto el enqueue como el dequeue tienen una complejidad de orden constante O(1)

## Ejercicio 4
* Implementar la clase `algorithms.tree.NonRecursiveBST` que implemente la interfaz `algorithms.tree.TreeMap`
  * Debe implementar un BST no recursivo, es decir iterativo
  * Debe utilizar la clase algorithms.tree.Node como nodos del árbol
  * Debe tener un constructor que reciba un Comparator<K> para poder comparar las keys

## Ejercicio 5
* Implementar el método `public K ceiling(K key)` en la clase `algorithms.tree.BinarySearchTree`
  * El método ceiling debe devolver la menor clave en el árbol, que sea igual o mayor a la clave especificada
  * Debe arrojar NoSuchElementException en el caso que no exista una clave que cumpla con la condición

## Ejercicio 6
* Implementar el método `public K floor(K key)` en la clase `algorithms.tree.BinarySearchTree`
  * El método floor debe devolver la mayor clave en el árbol, que sea igual o menor a la clave especificada
  * Debe arrojar NoSuchElementException en el caso que no exista una clave que cumpla con la condición

## Ejercicio 7
* Implementar los métodos `remove`, `removeMin` y `removeMax` en la clase `algorithms.tree.RedBlackBinarySearchTree`
  * Dichos métodos deben eliminar el nodo dada una clave `K key`, remover el nodo con la menor clave del árbol, y remover el nodo con la mayor clave del árbol respectivamente
