---
title: Práctica 5
layout: practice
permalink: /practice/5
---

# Práctica 5: Trees

* Fecha: 11 de mayo, 2023
* Entrega: 18 de mayo, 2023

## Setup
Para poder llevar a cabo esta práctica, es necesario actualizar la dependencia `ar.edu.austral.fi.algorithms:algorithms-base` a la version `1.0.3`.

Para esto, en sus repositorios, en la linea 39 del file `build.gradle`, la linea debe contener:

```    implementation 'ar.edu.austral.fi.algorithms:algorithms-base:1.0.4'```

Luego, correr `./gradlew build` o dar `Reload projects` desde el gradle menu en sus IDEs.


## Ejercicio 1

* Implementar la clase algorithms.tree.BinarySearchTree que implemente la interfaz algorithms.tree.TreeMap.
  * Debe utilizar la clase algorithms.tree.Node como nodos del árbol
  * Debe tener un constructor que reciba un Comparator<K> para poder comparar las keys


