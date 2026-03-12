class: center, middle, inverse

# Nivelación: Herramientas y Conceptos de Desarrollo

### Algoritmos y Estructuras de Datos

???

Esta presentación cubre los conceptos fundamentales y herramientas que usaremos durante todo el semestre.

---

# Agenda

1. SDLC - Software Development Life Cycle
2. Proyectos de Software
3. IDE - Integrated Development Environment
4. Build Systems
5. Version Control System (VCS)
6. Convention over Configuration
7. Testing
8. Best Practices
9. Continuous Integration
10. Code Coverage
11. GIT en profundidad

---
class: center, middle, inverse

# SDLC
## Software Development Life Cycle

---

# ¿Qué es el SDLC?

* El **Ciclo de Vida del Desarrollo de Software** es un proceso estructurado para planificar, crear, probar y desplegar software
* Define las fases que atraviesa un proyecto de software desde su concepción hasta su retiro

## Fases principales:

1. **Planificación**: Definir objetivos, alcance y viabilidad
2. **Análisis**: Requerimientos funcionales y no funcionales
3. **Diseño**: Arquitectura, componentes, interfaces
4. **Implementación**: Desarrollo del código
5. **Testing**: Verificación y validación
6. **Deployment**: Puesta en producción
7. **Mantenimiento**: Corrección de errores y mejoras

---

# Modelos de SDLC

## Waterfall (Cascada)
* Secuencial, cada fase debe completarse antes de la siguiente
* Documentación exhaustiva, poco flexible ante cambios

## Agile
* Iterativo e incremental
* Entregas frecuentes y pequeñas
* Adaptable a cambios, colaboración continua con stakeholders

## DevOps
* Integración de desarrollo y operaciones
* Automatización y continuous delivery
* Feedback rápido

---
class: center, middle, inverse

# Proyectos de Software

---

# Estructura de un Proyecto

* Un proyecto de software es más que código
* Incluye:
  * Código fuente
  * Dependencias
  * Configuración
  * Documentación
  * Tests
  * Scripts de build
  * Control de versiones

---

## Estructura típica en Java (nuestro proyecto):

```
algorithms-student/
├── src/
│   ├── main/java/algorithms/
│   │   ├── stack/
│   │   ├── queue/
│   │   └── tree/
│   └── test/java/algorithms/
│       ├── stack/
│       ├── queue/
│       └── tree/
├── gradle/
├── README.md
├── build.gradle
└── settings.gradle
```

---

# Gestión de Proyectos

* **Maven** y **Gradle** son las herramientas más populares en Java
* Gestionan:
  * Estructura del proyecto
  * Dependencias
  * Build process
  * Testing
  * Packaging

---

## build.gradle (nuestro proyecto):
```groovy
apply plugin: 'idea'
apply plugin: 'java'

java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(8)
    }
}

...

repositories {
    mavenCentral()
    maven {
        url = uri("https://maven.pkg.github.com/FacultadDeIngenieria/*")
        ...
    }
}

dependencies {
    implementation 'ar.edu.austral.fi.algorithms:algorithms-base:1.0.7'
    implementation 'org.jetbrains:annotations:22.0.0'
    testImplementation 'junit:junit:4.13'
    testImplementation 'org.assertj:assertj-core:3.20.2'
}
```

---
class: center, middle, inverse

# Convention over Configuration

---

# Convention over Configuration

* **Filosofía de diseño**: Asumir defaults sensatos
* Reducir decisiones que debe tomar el desarrollador
* Configurar solo lo que difiere del estándar

## Ejemplo: Estructura de Maven/Gradle

**Convention:**
```
src/main/java       → Código fuente
src/test/java       → Tests
src/main/resources  → Recursos
build/              → Output
```

**Sin convention:**
```
¿Dónde va el código? ¿Tests? ¿Recursos?
→ Cada proyecto decide → Inconsistencia
```

---

# Ejemplos en Java

## Naming Conventions:

* **Clases**: `PascalCase` → `BinarySearchTree`
* **Métodos**: `camelCase` → `isEmpty()`
* **Constantes**: `UPPER_SNAKE_CASE` → `MAX_SIZE`
* **Paquetes**: `lowercase` → `com.austral.ayed`

---

## Package Structure (nuestro proyecto):

```java
algorithms
├── stack             → Implementaciones de Stack
├── queue             → Implementaciones de Queue
└── tree              → Árboles binarios de búsqueda
```

Cada paquete contiene múltiples implementaciones:
* `stack/`: `ArrayStack`, `LinkedListStack`
* `queue/`: `ArrayQueue`, `LinkedListQueue`
* `tree/`: `BinarySearchTree`, `RedBlackBinarySearchTree`, etc.

## Benefits:
* Código predecible
* Fácil de entender para nuevos desarrolladores
* Herramientas funcionan mejor

---
class: center, middle, inverse

# IDE
## Integrated Development Environment

---

# ¿Qué es un IDE?

* Entorno integrado que combina múltiples herramientas de desarrollo:
  * Editor de código con syntax highlighting
  * Compilador
  * Debugger
  * Refactoring tools
  * Integración con VCS
  * Build automation

## IDEs populares para Java:

* **IntelliJ IDEA** (recomendado para este curso)
* Visual Studio Code (con extensiones)

---

# Características clave de un IDE

## Code Completion

* Sugiere métodos, clases, variables mientras escribes
* Reduce errores de tipeo
* Aumenta productividad significativamente

**Ejemplo en IntelliJ:**
```java
Stack<Integer> stack = new ArrayStack<>();
stack.pu  // Ctrl + Space muestra push()
```

* Muestra la documentación del método
* Sugiere parámetros requeridos
* Completa imports automáticamente

---

# Características clave de un IDE

## Refactoring

* Modificar estructura del código sin cambiar comportamiento
* El IDE actualiza todas las referencias automáticamente

**Operaciones comunes:**
* **Rename**: Renombrar variables/métodos/clases en todo el proyecto
* **Extract Method**: Convertir código seleccionado en un método
* **Inline**: Reemplazar llamada a método con su contenido
* **Change Signature**: Modificar parámetros de un método

**Ejemplo:**
```java
// Seleccionar código → Refactor → Extract Method
if (size == array.length) {
    resize(array.length * 2);
}
// → extractMethod: checkAndResize()
```

---

# Características clave de un IDE

## Navigation

* Navegar rápidamente por el código sin usar el mouse

**Funciones principales:**

* **Ir a definición**: Saltar a donde se define una clase/método
* **Buscar usos**: Ver dónde se usa una clase/método
* **Jerarquía de clases**: Ver herencia y implementaciones
* **Structure view**: Ver estructura de la clase actual
* **Buscar archivo**: Abrir archivo por nombre

**Ejemplo:**
```java
stack.push(item);  // Click derecho → "Go to Definition"
```

---

# Características clave de un IDE

## Debugging

* Ejecutar código paso a paso para encontrar errores
* Inspeccionar el estado del programa en tiempo real

**Herramientas principales:**

* **Breakpoints**: Pausar ejecución en una línea específica
* **Step Over**: Ejecutar línea actual
* **Step Into**: Entrar en método llamado
* **Step Out**: Salir del método actual
* **Evaluate Expression**: Ejecutar código Java mientras está pausado
* **Watch variables**: Monitorear valores de variables

**Uso típico:**
1. Poner breakpoint en línea sospechosa
2. Run en modo Debug
3. Inspeccionar valores de variables
4. Step through para ver flujo de ejecución

---
class: center, middle, inverse

# Build Systems

---

# ¿Qué es un Build System?

* Automatiza el proceso de compilación, testing y packaging
* Gestiona dependencias
* Ejecuta tareas en el orden correcto
* Garantiza builds reproducibles

## Beneficios:
* **Automatización**: No hacer pasos manuales
* **Consistencia**: Todos compilan igual
* **Eficiencia**: Compilación incremental
* **Integración**: Con CI/CD pipelines

---

# Gradle

* Build system moderno para JVM
* Basado en Groovy/Kotlin DSL
* Más flexible que Maven
* Compilación incremental
* Build cache

---

```groovy
apply plugin: 'idea'
apply plugin: 'java'

idea {
    project {
        languageLevel = '17'
    }
}

java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(8)
    }
}

repositories {
    mavenCentral()
    maven {
        url = uri("https://maven.pkg.github.com/FacultadDeIngenieria/*")
        credentials { ... }
    }
}

dependencies {
    implementation 'ar.edu.austral.fi.algorithms:algorithms-base:1.0.7'
    implementation 'org.jetbrains:annotations:22.0.0'
    testImplementation 'junit:junit:4.13'
    testImplementation 'org.assertj:assertj-core:3.20.2'
}
```

---

# Gradle Tasks

* Las tareas (tasks) son la unidad de trabajo en Gradle

### Comando principal para este curso:

```bash
./gradlew clean test
```

Este comando limpia archivos compilados previos y ejecuta todos los tests.

### Otras tasks útiles:

```bash
./gradlew build      # Compila y ejecuta tests
./gradlew test       # Solo ejecuta tests
./gradlew clean      # Limpia archivos compilados
./gradlew check      # Ejecuta todas las verificaciones
```

**Importante**: Siempre ejecutar `./gradlew clean test` antes de hacer push para verificar que todo funciona correctamente.

---
class: center, middle, inverse

# Version Control System
## VCS

---

# ¿Por qué necesitamos VCS?

* **Historial completo** de cambios
* **Colaboración** entre múltiples desarrolladores
* **Branches** para features/fixes independientes
* **Rollback** a versiones anteriores
* **Backup** distribuido del código
* **Tracking** de quién hizo qué y cuándo

### Sin VCS:
```
proyecto_final.java
proyecto_final_v2.java
proyecto_final_v2_FINAL.java
proyecto_final_v2_FINAL_ahora_si.java
```

### Con VCS:
```
git log
commit 1a2b3c4d
commit 5e6f7g8h
commit 9i0j1k2l
```

---

# Git

* Sistema de control de versiones **distribuido**
* Creado por Linus Torvalds en 2005
* Cada desarrollador tiene una copia completa del repositorio
* Branches son baratos y rápidos
* Used por:
  * Linux kernel
  * Android
  * Prácticamente toda la industria del software

## Conceptos clave:
* **Repository**: Base de datos de versiones
* **Commit**: Snapshot del proyecto
* **Branch**: Línea independiente de desarrollo
* **Remote**: Repositorio remoto (GitHub, GitLab, etc.)

---
class: center, middle, inverse

# Testing

---

# ¿Por qué testear?

* **Verificar** que el código hace lo que debe hacer
* **Detectar bugs** temprano (más baratos de arreglar)
* **Documentación** ejecutable
* **Refactoring** seguro
* **Diseño** mejor (código testeable es buen código)

## Tipos de tests:

* **Unit Tests**: Prueban una unidad aislada (clase/método)
* **Integration Tests**: Prueban interacción entre componentes
* **End-to-End Tests**: Prueban el sistema completo

---

# Unit Testing con JUnit (ejemplo del curso)

```java
package algorithms.stack;

import org.junit.Test;

public class TestPractice02 implements StackTests {

    @Test
    public void testStackSmallSize() {
        testStackIntegers(new ArrayStack<>(), 10);
    }

    @Test
    public void testStackLargeSize() {
        testStackIntegers(new ArrayStack<>(), 1000);
    }

    @Test
    public void testStackUnderflow() {
        testUnderflow(new ArrayStack<>(5));
    }

    @Test
    public void testIterator() {
        testIterator(new ArrayStack<>(5));
    }
}
```

---

# Test-Driven Development (TDD)

## Ciclo Red-Green-Refactor:

1. **Red**: Escribir un test que falla
2. **Green**: Escribir el código mínimo para que pase
3. **Refactor**: Mejorar el código manteniendo tests verdes

## Beneficios:

* Especificación clara antes de implementar
* Cobertura alta
* Código más simple y enfocado
* Menos bugs

## Ejemplo:

```java
// 1. Red - Test primero
@Test
public void testSize() {
    Stack<Integer> stack = new Stack<>();
    assertEquals(0, stack.size());
    stack.push(1);
    assertEquals(1, stack.size());
}

// 2. Green - Implementar
public int size() { return elements.size(); }

// 3. Refactor si es necesario
```

---

# Assertions comunes

```java
// Igualdad
assertEquals(expected, actual);
assertNotEquals(unexpected, actual);

// Booleanos
assertTrue(condition);
assertFalse(condition);

// Nulls
assertNull(object);
assertNotNull(object);

// Mismo objeto
assertSame(expected, actual);

// Arrays
assertArrayEquals(expectedArray, actualArray);

// Exceptions
@Test(expected = IllegalArgumentException.class)
public void testException() {
    // código que debe lanzar exception
}
```

---
class: center, middle, inverse

# Best Practices

---

# Best Practices: Código

## 1. Nombres descriptivos

✅ **Bien:**
```java
public void push(E item) {
    if(size == array.length) {
        E[] copy = (E[]) new Object[array.length * 2];
        for(int i = 0; i < size; i++) {
            copy[i] = array[i];
        }
        array = copy;
    }
    array[size++] = item;
}
```
---

# Best Practices: Código

## 2. Funciones pequeñas

* Una función debe hacer **una cosa**
* Máximo 20-30 líneas
* Si tiene múltiples niveles de indentación → extraer métodos

---

# Best Practices: Diseño

## 3. DRY - Don't Repeat Yourself

* **Principio**: No duplicar código
* Si encuentras código repetido → extraer a método/clase común
* Facilita mantenimiento: cambio en un solo lugar

**❌ Mal:**
```java
// En ArrayStack
if (size == array.length) {
    E[] newArray = (E[]) new Object[array.length * 2];
    System.arraycopy(array, 0, newArray, 0, size);
    array = newArray;
}

// En ArrayQueue - mismo código duplicado
if (size == array.length) {
    E[] newArray = (E[]) new Object[array.length * 2];
    System.arraycopy(array, 0, newArray, 0, size);
    array = newArray;
}
```

---

# Best Practices: Diseño

## 3. DRY - Don't Repeat Yourself

* **Principio**: No duplicar código
* Si encuentras código repetido → extraer a método/clase común
* Facilita mantenimiento: cambio en un solo lugar

**✅ Bien:**
```java
// Extraer a método común
private void resize(int capacity) {
    E[] copy = (E[]) new Object[capacity];
    System.arraycopy(array, 0, copy, 0, size);
    array = copy;
}
```

---

# Best Practices: Diseño

## 4. SOLID Principles

### Single Responsibility Principle (SRP)
* Una clase debe tener **una única razón para cambiar**
* Cada clase tiene una responsabilidad bien definida

---

# Best Practices: Diseño

## 4. SOLID Principles

**Ejemplo:**
```java
// ❌ Mal: Stack que también escribe a archivo
public class Stack<E> {
    public void push(E item) { ... }
    public void saveToFile(String filename) { ... }  // No es responsabilidad del Stack
}

// ✅ Bien: Separar responsabilidades
public class Stack<E> {
    public void push(E item) { ... }
}

public class StackPersistence {
    public void saveToFile(Stack<?> stack, String filename) { ... }
}
```

---

# Best Practices: Diseño

## 4. SOLID Principles

### Open/Closed Principle (OCP)
* **Abierto para extensión, cerrado para modificación**
* Agregar funcionalidad sin cambiar código existente

**Ejemplo en nuestro proyecto:**
```java
// Interface permite extensión sin modificar código existente
public interface Stack<E> {
    void push(E item);
    E pop();
}

// Diferentes implementaciones sin cambiar la interfaz
public class ArrayStack<E> implements Stack<E> { ... }
public class LinkedListStack<E> implements Stack<E> { ... }
```

---

# Best Practices: Diseño

## 4. SOLID Principles

### Liskov Substitution Principle (LSP)
* Los subtipos deben ser sustituibles por sus tipos base
* `ArrayStack` y `LinkedListStack` deben comportarse igual desde la perspectiva de `Stack<E>`

---

# Best Practices: Diseño

## 4. SOLID Principles

### Interface Segregation Principle (ISP)
* Interfaces específicas mejor que interfaces generales
* Los clientes no deben depender de métodos que no usan

---

# Best Practices: Diseño

## 4. SOLID Principles

### Interface Segregation Principle (ISP)

**✅ Bien:**
```java
public interface Stack<E> {
    void push(E item);
    E pop();
}

public interface Queue<E> {
    void enqueue(E item);
    E dequeue();
}
```

???

The Single Responsibility Principle (SRP) and Interface Segregation Principle (ISP) both promote low coupling and high cohesion but differ in scope. SRP dictates a class should have one reason to change (focused responsibility). ISP mandates that clients should not depend on unused methods, favoring small, specific interfaces over large ones

---

# Best Practices: Diseño

## 4. SOLID Principles (continuación)

### Dependency Inversion Principle (DIP)
* Depender de abstracciones, no de implementaciones concretas
* Módulos de alto nivel no deben depender de módulos de bajo nivel

---

### Dependency Inversion Principle (DIP)

**❌ Mal:**
```java
public class StackProcessor {
    private ArrayStack<Integer> stack;  // Depende de implementación concreta

    public void process() {
        stack.push(1);
    }
}
```

**✅ Bien:**
```java
public class StackProcessor {
    private Stack<Integer> stack;  // Depende de abstracción

    public StackProcessor(Stack<Integer> stack) {
        this.stack = stack;
    }

    public void process() {
        stack.push(1);
    }
}
```

---

# Best Practices: Diseño

## 5. Fail Fast

* **Principio**: Detectar y reportar errores lo más temprano posible
* Validar entradas en el momento que se reciben
* Lanzar exceptions claras y descriptivas
* No dejar el sistema en estado inconsistente


---

**Ejemplo en ArrayStack:**
```java
@Override
public void push(@NotNull E item) {
    // Fail fast: validar input inmediatamente
    if (item == null) {
        throw new IllegalArgumentException("Cannot push null element");
    }

    if (isFull()) {
        resize(2 * array.length);
    }
    array[size++] = item;
}

@Override
public E pop() {
    // Fail fast: detectar underflow inmediatamente
    if (isEmpty()) {
        throw new NoSuchElementException("Stack is empty");
    }

    E item = array[--size];
    array[size] = null;
    return item;
}
```

**Beneficios**: Errores más fáciles de debuggear, código más robusto

---

# Best Practices: Testing

## 6. Tests deben ser:

* **Fast**: Ejecutarse rápido
* **Independent**: No depender de otros tests
* **Repeatable**: Mismo resultado siempre
* **Self-Validating**: Pass o fail, sin inspección manual
* **Timely**: Escritos antes o junto con el código

---

# Best Practices: Testing

## 7. Un assert por test (idealmente)

* Tests específicos
* Mensajes de error claros
* Fácil identificar qué falló

---

# Best Practices: Testing

## 8. Arrange-Act-Assert pattern

```java
@Test
public void testPush() {
    // Arrange
    Stack<Integer> stack = new Stack<>();

    // Act
    stack.push(5);

    // Assert
    assertEquals(1, stack.size());
}
```

---
class: center, middle, inverse

# Continuous Integration

---

# Continuous Integration (CI)

* Práctica de integrar código frecuentemente (varias veces al día)
* Cada integración es verificada por un build automatizado
* Detecta errores rápidamente

### Flujo típico:

1. Developer hace commit y push
2. CI server detecta cambio
3. CI ejecuta build y tests
4. CI reporta resultado (✅ o ❌)
5. Si falla, equipo lo arregla inmediatamente

### Beneficios:

* Reduce integration problems
* Feedback rápido
* Releases más frecuentes y confiables
* Mayor confianza en el código

---

# CI Tools

## TeamCity (usado en este curso)

* JetBrains - Servidor de CI
* Configuración visual
* Soporte excelente para Java/Gradle
* Build agents distribuidos
* **Cada push a GitHub actualiza automáticamente TeamCity**

### ⚠️ Regla CRÍTICA de TeamCity:

> Si el viernes siguiente a la entrega el TeamCity está en ❌ rojo,
> se considera práctica **DESAPROBADA** (incluso si pasa a verde después).

---

# CI Tools

## Otros populares:

* **Jenkins**: Open source, muy configurable
* **GitHub Actions**: Integrado con GitHub
* **GitLab CI**: Integrado con GitLab
* **Travis CI**: Popular en open source
* **CircleCI**: Cloud-native

---

# CI Pipeline Example

```yaml
# Ejemplo conceptual de un pipeline

build:
  - checkout code
  - run: ./gradlew clean build

test:
  - run: ./gradlew test
  - run: ./gradlew integrationTest

quality:
  - run: ./gradlew checkstyle
  - run: ./gradlew pmd
  - run: code coverage check

notify:
  - if success: notify team (green)
  - if failure: notify team (red)
```

**Si el build está en rojo, prioridad #1 es arreglarlo**

---
class: center, middle, inverse

# Code Coverage

---

# Code Coverage

* Métrica que mide qué porcentaje del código es ejecutado por los tests
* **No garantiza** calidad de tests, pero identifica código sin testear

## Tipos de cobertura:

* **Line Coverage**: % de líneas ejecutadas
* **Branch Coverage**: % de branches (if/else) ejecutados
* **Method Coverage**: % de métodos ejecutados

## Herramientas:

* **JaCoCo** (Java Code Coverage)
* Cobertura
* Emma

---

# JaCoCo con Gradle

```groovy
plugins {
    id 'jacoco'
}

jacoco {
    toolVersion = "0.8.8"
}

test {
    finalizedBy jacocoTestReport
}

jacocoTestReport {
    reports {
        xml.enabled true
        html.enabled true
    }
}

// Verificar cobertura mínima
jacocoTestCoverageVerification {
    violationRules {
        rule {
            limit {
                minimum = 0.80  // 80%
            }
        }
    }
}
```

---

# Interpretando Coverage

### Reporte:

```
Class              Line Coverage    Branch Coverage
Stack              95% (38/40)      90% (18/20)
Queue              87% (35/40)      75% (15/20)
BST                78% (45/58)      65% (26/40)
```

### ¿Qué buscar?

* **Alto coverage** (>80%) es bueno, pero no suficiente
* **Líneas no cubiertas**: ¿Son edge cases importantes?
* **Branches no cubiertos**: ¿Faltan tests de casos if/else?

### Importante:

> **100% coverage no significa código perfecto**
>
> Puede tener tests malos que ejecutan código pero no verifican nada útil

---
class: center, middle, inverse

# GIT

---

# Git: Conceptos Fundamentales

## Repository (Repositorio)

* Base de datos que almacena historial completo del proyecto
* `.git/` directorio en la raíz del proyecto

## Commit

* Snapshot del proyecto en un momento dado
* Identificado por hash SHA-1: `1a2b3c4d5e6f...`
* Contiene:
  * Cambios realizados
  * Autor y fecha
  * Mensaje descriptivo
  * Referencia al commit padre

## Working Directory, Staging Area, Repository

```
Working Directory  →  Staging Area  →  Repository
(tu código)           (git add)        (git commit)
```

---

# Estados de Git

```
                git add              git commit
Untracked  →  Staged (Index)  →  Committed (Repository)
  ↓               ↓
Modified    →  Staged
```

## Ver estado:

```bash
$ git status

On branch main
Changes not staged for commit:
  modified:   src/Stack.java

Untracked files:
  src/NewClass.java
```

---

# Comandos básicos de Git

```bash
# Inicializar repositorio
git init

# Clonar repositorio remoto
git clone <url>

# Ver estado
git status

# Agregar archivos al staging area
git add <file>
git add .                    # Todos los cambios

# Crear commit
git commit -m "mensaje"

# Ver historial
git log
git log --oneline           # Compacto
git log --graph             # Con grafo

# Ver cambios
git diff                    # Working vs Staging
git diff --staged           # Staging vs Committed
```

---

# Branching

* **Branch**: Línea independiente de desarrollo
* Extremadamente ligero en Git (solo un puntero)
* Permite trabajar en features sin afectar main

```bash
# Crear branch
git branch feature-x

# Cambiar a branch
git checkout feature-x
# o crear y cambiar en un comando
git checkout -b feature-x

# Listar branches
git branch
git branch -a              # Incluye remotos

# Eliminar branch
git branch -d feature-x
```

---

## Naming conventions:

* `feature/nueva-funcionalidad`
* `bugfix/corregir-error`
* `hotfix/fix-critico`

---

# Merging

* Integrar cambios de un branch a otro

```bash
# Merge simple (fast-forward)
git checkout main
git merge feature-x

# Merge con merge commit
git merge --no-ff feature-x
```

## Tipos de merge:

### Fast-Forward:
```
main:    A --- B
               ↓
feature:       C --- D
Result:  A --- B --- C --- D (main)
```

---

# Merging

* Integrar cambios de un branch a otro

```bash
# Merge simple (fast-forward)
git checkout main
git merge feature-x

# Merge con merge commit
git merge --no-ff feature-x
```

## Tipos de merge:

### Three-way merge:
```
main:    A --- B --- E
               ↓
feature:       C --- D
Result:  A --- B --- E --- M (main)
               ↑         ↗
                     C --- D
```

---

# Merge Conflicts

* Ocurren cuando Git no puede resolver automáticamente diferencias
* Debemos resolverlos manualmente

```bash
$ git merge feature-x
Auto-merging src/Stack.java
CONFLICT (content): Merge conflict in src/Stack.java
Automatic merge failed; fix conflicts and then commit.
```

## Archivo con conflicto:

```java
public void push(T element) {
<<<<<<< HEAD
    if (size >= capacity) {
        resize();
    }
=======
    if (isFull()) {
        throw new IllegalStateException("Stack is full");
    }
>>>>>>> feature-x
    elements[size++] = element;
}
```

## Resolver:

1. Editar archivo manualmente
2. `git add <file>`
3. `git commit`

---

# Remote Repositories

* Versión del proyecto alojada en internet o red
* **origin**: Nombre default del remote principal

```bash
# Ver remotes configurados
git remote -v

# Agregar remote
git remote add origin <url>

# Fetch: Descargar cambios sin merge
git fetch origin

# Pull: Fetch + Merge
git pull origin main

# Push: Subir commits locales
git push origin main
git push -u origin main    # Set upstream

# Branches remotos
git checkout -b feature-x origin/feature-x
```

---

# Git Good Practices

## Commits:

* **Pequeños y frecuentes**
* **Mensajes descriptivos**:
  * ❌ "fix"
  * ❌ "cambios"
  * ❌ "tp2"
  * ✅ "Implementar ArrayStack"
  * ✅ "Agregar iterator a LinkedListQueue"
  * ✅ "Fix: Corregir underflow en ArrayStack.pop()"

---

# Git Workflow en este curso

## Para las prácticas:

1. Cada alumno tiene su **repositorio personal** (ej: `algoritmos-tunombre`)
2. **Clone** a tu máquina local
3. Implementar los TPs trabajando en `master` branch
4. **Hacer commits frecuentes** con mensajes descriptivos
5. **Ejecutar `./gradlew clean test`** antes de cada push
6. **Push** a GitHub → esto actualiza TeamCity automáticamente
7. Verificar que build en TeamCity esté ✅ **verde**

## Algunas reglas básicas:

* ❌ NO hacer commit de archivos compilados (`build/`, `.gradle/`, etc.)
* ❌ NO hacer commit de archivos de IDE (`.idea/`, `*.iml`)
* ✅ SIEMPRE ejecutar `./gradlew clean test` antes de push
* ✅ Verificar TeamCity después de cada push
* ✅ El `.gitignore` ya está configurado correctamente, pero puede actualizarse

---

# Resumen: Lo que vimos

1. **SDLC**: Fases del desarrollo de software
2. **Proyectos**: Estructura y gestión con Maven/Gradle
3. **IDE**: IntelliJ y sus características
4. **Build Systems**: Automatización con Gradle
5. **VCS**: Control de versiones con Git
6. **Convention over Configuration**: Defaults sensatos
7. **Testing**: Unit tests con JUnit y TDD
8. **Best Practices**: Código limpio y mantenible
9. **Continuous Integration**: Build y tests automáticos
10. **Coverage**: Medición de cobertura con JaCoCo
11. **Git en profundidad**: Branches, merging, workflow