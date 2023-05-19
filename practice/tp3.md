---
title: Práctica 3
layout: practice
permalink: /practice/3
---

# Práctica 3: Queue

* Fecha: 30 de marzo, 2023
* Entrega: 13 de abril, 2023

## Setup
Para poder llevar a cabo esta práctica, es necesario actualizar la dependencia `ar.edu.austral.fi.algorithms:algorithms-base` a la version `1.0.2`.

Para esto, en sus repositorios, en la linea 39 del file `build.gradle`, modifiquen la linea:

`    implementation 'ar.edu.austral.fi.algorithms:algorithms-base:1.0.0'`

por la siguiente linea:

`    implementation 'ar.edu.austral.fi.algorithms:algorithms-base:1.0.2'`

Luego, correr `./gradlew build` o dar `Reload projects` desde el gradle menu en sus IDEs.


## Ejercicio 1

* Implementar la clase algorithms.queue.ArrayQueue que implemente la interfaz algorithms.queue.Queue 
  * Debe ser Iterable
  * Debe soportar dos constructores
  * Debe utilizar un Array para su implementación
  * Debe detectar y soportar underflow (arrojar una NoSuchElementException exception)
  


