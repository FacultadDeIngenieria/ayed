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
* Documentación exhaustiva
* Poco flexible ante cambios

## Agile
* Iterativo e incremental
* Entregas frecuentes y pequeñas
* Adaptable a cambios
* Colaboración continua con stakeholders

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

## Estructura típica en Java (nuestro proyecto):

```
anaydis/
├── src/
│   ├── main/java/anaydis/
│   │   ├── sort/
│   │   ├── search/
│   │   ├── compression/
│   │   └── immutable/
│   └── test/java/anaydis/
│       ├── sort/
│       └── search/
├── gradle/
├── informes/
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

## build.gradle (nuestro proyecto):
```groovy
apply plugin: 'idea'
apply plugin: 'java'

repositories {
    mavenCentral()
    maven {
        url = uri("https://maven.pkg.github.com/FacultadDeIngenieria/*")
    }
}

dependencies {
    implementation 'ar.edu.austral.fi.anaydis:anaydis-base:1.3.14'
    testImplementation 'junit:junit:4.13'
    testImplementation 'org.assertj:assertj-core:3.20.2'
}
```

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
* Eclipse
* NetBeans
* Visual Studio Code (con extensiones)

---

# Características clave de un IDE

## Code Completion (Autocompletado)
* Sugiere métodos, clases, variables
* Reduce errores de tipeo
* Aumenta productividad

## Refactoring
* Renombrar variables/métodos/clases
* Extraer métodos
* Mover clases
* Cambiar signatures

## Navigation
* Ir a definición
* Buscar usos
* Jerarquía de clases
* Structure view

## Debugging
* Breakpoints
* Step through code
* Inspect variables
* Evaluate expressions

---

# Shortcuts importantes (IntelliJ)

| Acción | Shortcut (Mac) | Shortcut (Windows/Linux) |
|--------|----------------|--------------------------|
| Buscar archivo | Cmd+Shift+O | Ctrl+Shift+N |
| Buscar en todo | Cmd+Shift+F | Ctrl+Shift+F |
| Ir a definición | Cmd+B | Ctrl+B |
| Refactor rename | Shift+F6 | Shift+F6 |
| Auto-format | Cmd+Alt+L | Ctrl+Alt+L |
| Run | Ctrl+R | Shift+F10 |
| Debug | Ctrl+D | Shift+F9 |

???

Aprender shortcuts es crucial para ser productivo. Al principio cuesta pero después no podrás vivir sin ellos.

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

## build.gradle (nuestro proyecto):

```groovy
apply plugin: 'idea'
apply plugin: 'java'

repositories {
    mavenCentral()
    maven {
        url = uri("https://maven.pkg.github.com/FacultadDeIngenieria/*")
    }
}

dependencies {
    implementation 'ar.edu.austral.fi.anaydis:anaydis-base:1.3.14'
    testImplementation 'junit:junit:4.13'
}
```

---

# Gradle Tasks

* Las tareas (tasks) son la unidad de trabajo en Gradle

## Comando principal para este curso:

```bash
./gradlew clean test
```

Este comando limpia archivos compilados previos y ejecuta todos los tests.

## Otras tasks útiles:

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

## Sin VCS:
```
proyecto_final.java
proyecto_final_v2.java
proyecto_final_v2_FINAL.java
proyecto_final_v2_FINAL_ahora_si.java
```

## Con VCS:
```
git log
commit 1a2b3c4d
commit 5e6f7g8h
commit 9i0j1k2l
```

---

# Git: El estándar de facto

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

## Package Structure (nuestro proyecto):

```java
anaydis
├── sort              → Algoritmos de ordenamiento
├── search            → Estructuras de búsqueda
├── compression       → Algoritmos de compresión
├── immutable         → Estructuras inmutables
└── string            → Búsqueda en strings
```

## Benefits:
* Código predecible
* Fácil de entender para nuevos desarrolladores
* Herramientas funcionan mejor

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
package anaydis.sort;

import org.junit.Test;

public class TestPractice02 extends SorterTest {

    /** Test BubbleSorter with String generator. */
    @Test
    public void testBubbleWithStringGenerator() {
        testSorter(createStringDataSetGenerator(),
                   SorterType.BUBBLE, 10);
        testSorter(createStringDataSetGenerator(),
                   SorterType.BUBBLE, 50);
        testSorter(createStringDataSetGenerator(),
                   SorterType.BUBBLE, 100);
    }

    /** Test InsertionSorter with Integer generator. */
    @Test
    public void testInsertionWithIntegerGenerator() {
        testSorter(createIntegerDataSetGenerator(),
                   SorterType.INSERTION, 10);
        testSorter(createIntegerDataSetGenerator(),
                   SorterType.INSERTION, 100);
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

❌ **Mal:**
```java
public void s(List<T> l, Comparator<T> c) {
    for(int i=0; i<l.size()-1; i++) {
        for(int j=i+1; j<l.size(); j++) {
            if(c.compare(l.get(i), l.get(j)) > 0) {
                swap(l, i, j);
            }
        }
    }
}
```

✅ **Bien:**
```java
public void sort(List<T> list, Comparator<T> comparator) {
    for(int i = 0; i < list.size() - 1; i++) {
        for(int j = i + 1; j < list.size(); j++) {
            if(greater(comparator, list, i, j)) {
                swap(list, i, j);
            }
        }
    }
}
```

## 2. Funciones pequeñas

* Una función debe hacer **una cosa**
* Máximo 20-30 líneas
* Si tiene múltiples niveles de indentación → extraer métodos

---

# Best Practices: Diseño

## 3. DRY - Don't Repeat Yourself

* No duplicar código
* Extraer código común a métodos/clases

## 4. SOLID Principles

* **S**ingle Responsibility: Una clase, una responsabilidad
* **O**pen/Closed: Abierto a extensión, cerrado a modificación
* **L**iskov Substitution: Subtipos deben ser sustituibles
* **I**nterface Segregation: Interfaces específicas
* **D**ependency Inversion: Depender de abstracciones

## 5. Fail Fast

* Validar entradas temprano
* Lanzar exceptions claras
* No dejar el sistema en estado inconsistente

```java
public void push(T element) {
    if (element == null) {
        throw new IllegalArgumentException("Cannot push null");
    }
    // ...
}
```

---

# Best Practices: Ejemplo del curso

## AbstractSorter - Herencia bien aplicada

```java
public abstract class AbstractSorter {
    // Métodos comunes a TODOS los sorters
    protected <T> boolean greater(Comparator<T> comparator,
                                  List<T> list, int i, int j) {
        return comparator.compare(list.get(i), list.get(j)) > 0;
    }

    protected <T> void swap(List<T> list, int i, int j) {
        list.set(j, list.set(i, list.get(j)));
    }
}

public class BubbleSorter extends AbstractSorter {
    // Solo implementa el algoritmo específico
    public <T> void sort(List<T> list, Comparator<T> comparator) {
        // Usa greater() y swap() del padre
        // No duplica código
    }
}
```

---

# Best Practices: Testing

## 6. Tests deben ser:

* **Fast**: Ejecutarse rápido
* **Independent**: No depender de otros tests
* **Repeatable**: Mismo resultado siempre
* **Self-Validating**: Pass o fail, sin inspección manual
* **Timely**: Escritos antes o junto con el código

## 7. Un assert por test (idealmente)

* Tests específicos
* Mensajes de error claros
* Fácil identificar qué falló

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

## Flujo típico:

1. Developer hace commit y push
2. CI server detecta cambio
3. CI ejecuta build y tests
4. CI reporta resultado (✅ o ❌)
5. Si falla, equipo lo arregla inmediatamente

## Beneficios:

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

## Regla importante:

> **Si el build está en rojo, prioridad #1 es arreglarlo**

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

## Reporte JaCoCo:

```
Class              Line Coverage    Branch Coverage
Stack              95% (38/40)      90% (18/20)
Queue              87% (35/40)      75% (15/20)
BST                78% (45/58)      65% (26/40)
```

## ¿Qué buscar?

* **Alto coverage** (>80%) es bueno, pero no suficiente
* **Líneas no cubiertas**: ¿Son edge cases importantes?
* **Branches no cubiertos**: ¿Faltan tests de casos if/else?

## Importante:

> **100% coverage no significa código perfecto**
>
> Puede tener tests malos que ejecutan código pero no verifican nada útil

---
class: center, middle, inverse

# GIT en Profundidad

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
feature:      C --- D
Result:  A --- B --- C --- D (main)
```

### Three-way merge:
```
main:    A --- B --- E
              ↓
feature:      C --- D
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

# Workflow típico con Git (este curso)

```bash
# 1. Clonar tu repositorio (una vez)
git clone https://github.com/FacultadDeIngenieria/algoritmos-tunombre.git
cd algoritmos-tunombre

# 2. Hacer cambios en tu código
# ... implementar BubbleSorter, etc ...

# 3. Verificar que compila y tests pasan
./gradlew clean test

# 4. Ver qué cambió
git status
git diff

# 5. Agregar cambios al staging
git add src/main/java/anaydis/sort/BubbleSorter.java
# o agregar todo:
git add .

# 6. Crear commit con mensaje descriptivo
git commit -m "Implementar BubbleSorter"

# 7. ANTES DE PUSH: Verificar nuevamente
./gradlew clean test

# 8. Push a GitHub (esto actualiza TeamCity)
git push origin master

# 9. Verificar TeamCity que el build esté ✅ verde
```

---

# Git Good Practices

## Commits:

* **Pequeños y frecuentes**
* **Mensajes descriptivos**:
  * ❌ "fix"
  * ❌ "cambios"
  * ❌ "tp2"
  * ✅ "Implementar BubbleSorter"
  * ✅ "Agregar tests para InsertionSorter"
  * ✅ "Fix: Corregir comparación en SelectionSorter"

## Mensajes formato:

```
<tipo>: <descripción breve>

<descripción detallada (opcional)>

<referencias (opcional)>
```

Ejemplo:
```
feat: Implementar QuickSort con median-of-three

Implementa QuickSort usando estrategia median-of-three
para selección de pivot, mejorando el caso promedio.

TP4 - Ejercicio 3
```

---

# Git Comandos útiles

```bash
# Ver diferencias
git diff HEAD~1 HEAD        # Último commit vs anterior
git diff main..feature-x    # Entre branches

# Deshacer cambios
git checkout -- <file>      # Descartar cambios working dir
git reset HEAD <file>       # Unstage archivo
git reset --hard HEAD       # ⚠️ Descartar todo (peligroso)

# Modificar último commit
git commit --amend          # Cambiar mensaje o agregar cambios

# Ver quién modificó cada línea
git blame <file>

# Buscar en historial
git log --grep="bug"
git log -S"función"         # Buscar cambios en código

# Guardar temporalmente
git stash                   # Guardar cambios
git stash pop               # Recuperar cambios
```

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

## ⚠️ REGLAS CRÍTICAS:

* ❌ **Si TeamCity está en rojo el viernes siguiente a la entrega → TP DESAPROBADO**
* ❌ **NO hacer commit de archivos compilados** (`build/`, `.gradle/`, etc.)
* ❌ **NO hacer commit de archivos de IDE** (`.idea/`, `*.iml`)
* ✅ **SIEMPRE ejecutar `./gradlew clean test` antes de push**
* ✅ **Verificar TeamCity después de cada push**
* ✅ **El `.gitignore` ya está configurado correctamente**

---

# .gitignore (nuestro proyecto)

* Archivo que especifica qué archivos/directorios Git debe ignorar

```bash
# Build output
build
out
target/
bin/

# IDE
.idea/
.vscode/
.idea_modules/
*.iws
*.iml
*.ipr

# OS
.DS_Store

# Gradle
.gradle
gradle.properties

# Libs
libs/

# History
.history
```

---
class: center, middle, inverse

# Resumen

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

---

# Herramientas que usaremos

* **IntelliJ IDEA**: IDE
* **Java 8+**: Lenguaje (configurado en build.gradle)
* **Gradle**: Build system
* **JUnit 4**: Testing framework
* **Git**: Version control
* **GitHub**: Remote repository (individual)
* **TeamCity**: Continuous Integration
* **JaCoCo**: Code coverage (opcional)

## Setup inicial:

1. Instalar JDK 8 o superior
2. Instalar IntelliJ IDEA
3. Instalar Git
4. Configurar Git:
   ```bash
   git config --global user.name "Tu Nombre"
   git config --global user.email "tu@email.austral.edu.ar"
   ```
5. Clonar tu repositorio personal (te será asignado)

---

# Para la próxima clase

## Leer:

* Revisar la práctica asignada
* Familiarizarse con el repositorio del curso

## Preparar:

* Tener instalado IntelliJ IDEA
* Tener configurado Git
* Haber clonado tu repositorio personal
* Poder ejecutar `./gradlew clean test` exitosamente
* Verificar acceso a TeamCity

## Practicar:

* Comandos básicos de Git (add, commit, push, status, diff)
* Ejecutar `./gradlew clean test` y ver resultado
* Navegar código en IntelliJ
* Hacer un commit de prueba y verificar en TeamCity

---
class: center, middle, inverse

# ¿Preguntas?

### pedro.colunga@ing.austral.edu.ar
