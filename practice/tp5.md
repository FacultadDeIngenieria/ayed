---
title: Práctica 5
layout: practice
permalink: /practice/5
---

# Práctica 5: Tree Visitor

* Fecha: 9 de mayo, 2024
* Entrega: 16 de mayo, 2024

## Setup
Para poder llevar a cabo esta práctica, es necesario actualizar la dependencia `ar.edu.austral.fi.algorithms:algorithms-base` a la version `1.0.7`.

Para esto, en sus repositorios, en la linea 43 del file `build.gradle`, la linea debe contener:

```    implementation 'ar.edu.austral.fi.algorithms:algorithms-base:1.0.7'```

Luego, correr `./gradlew build` o dar `Reload projects` desde el gradle menu en sus IDEs.


## Ejercicio 1

* Implementar la clase `algorithms.tree.BinaryTreeVisitor` que implemente la interfaz `algorithms.tree.TreeVisitor`.
  * Debe utilizar la clase algorithms.tree.Node para representar los árboles a recorrer


