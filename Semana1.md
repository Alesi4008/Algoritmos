# 🚀 SEMANA 1: Arreglos Dinámicos, Invariantes y Costo

## 🧠 Conceptos Fundamentales
* **Estructura de datos**: Es una forma de organizar información junto con sus operaciones, propiedades de correctitud y costos. Es decir: Representación + Operaciones + Correctitud + Costo.
* **ADT (Tipo de Dato Abstracto):** Es el **plano arquitectónico** o el contrato. El ADT especifica el comportamiento observable. El plano dice: *"Esta casa debe tener una puerta y poder abrirse"*. No le importa el material, solo te dice **qué** debe hacer (como decir que existe una operación `add()`, sin escribir su código).
* **Implementación:** Es la **casa ya construida**. La implementación decide cómo representar el estado y ejecutar las operaciones. Aquí es donde el programador decide: *"Voy a construir la puerta con madera (usar un `Integer[]`) y ponerle esta chapa específica"*. Es el código real que hace que las reglas del ADT se cumplan.
* **Tamaño (`n`) frente a Capacidad (`a.length`)**: El tamaño es el número de elementos lógicos que realmente tienes guardados. La capacidad es el número de posiciones físicas disponibles en tu arreglo de respaldo.

---

## 📏 Arreglos Dinámicos y Redimensionamiento
Los arreglos en Java tienen una longitud física que se fija al crearlos y no cambia. Para almacenar más elementos cuando el arreglo se llena (`n == a.length`), usamos **Arreglos Dinámicos**.

* **Arreglo dinámico**: Estructura que crece reemplazando su arreglo de respaldo por uno nuevo de mayor capacidad.
* **El método `resize()`**: Su objetivo es aumentar la capacidad sin perder elementos ni cambiar el tamaño lógico. Requiere copiar los `n` elementos al nuevo arreglo.
* **Políticas de Crecimiento**:
  * **Crecimiento aditivo**: Aumentar la capacidad de 1 en 1. Produce expansiones demasiado frecuentes.
  * **Crecimiento geométrico**: Multiplicar la capacidad por un factor (por ejemplo, x2). Separa las expansiones costosas permitiendo muchas inserciones baratas.

---

## ⚡ Operaciones y Complejidad Asintótica
La **complejidad asintótica (Big-O)** describe cómo crece el trabajo de una operación cuando aumenta el tamaño del problema (`n`).

| Operación | Costo | Justificación |
| :--- | :---: | :--- |
| **`size()` / `capacity()`** | `O(1)` | Solo leen la variable `n` o `a.length` respectivamente. |
| **`get(i)` / `set(i, x)`** | `O(1)` | El arreglo permite acceso directo mediante el índice. No necesita recorrer las posiciones anteriores. |
| **`resize()`** | `O(n)` | Debe copiar los `n` elementos lógicos al nuevo arreglo. |
| **`add(x)` (al final)** | `O(1)` amortizado | Generalmente solo requiere escribir y actualizar `n` (costo `O(1)`). Ocasionalmente hace un `resize()` costoso, pero gracias al crecimiento geométrico, el costo "promedio" o amortizado se mantiene constante. |
| **`add(i, x)` / `remove(i)`** | `O(n)` (peor caso) | Puede requerir desplazar hasta `n-i` elementos hacia la derecha (para añadir) o hacia la izquierda (para borrar). |
| **`indexOf(x)`** | `O(n)` (peor caso) | Realiza una búsqueda secuencial, examinando elementos hasta encontrarlo o agotar la secuencia. |

---

## 🛡️ Correctitud, Invariantes y Pruebas
Para que nuestra estructura sea confiable, debemos respetar ciertas reglas lógicas:

* **Invariante de representación**: Es una propiedad que debe cumplirse en *todo* estado válido de la estructura. Por ejemplo: `0 <= n <= a.length`.
* **Precondición**: Lo que una operación exige que se cumpla antes de poder ejecutarse.
* **Postcondición**: El efecto o resultado que la operación promete cumplir al terminar.

> **⚠️ Diferencia entre Test y Benchmark:**
> * **Análisis:** Predice el crecimiento asintótico del trabajo.
> * **Test (Prueba):** Busca errores concretos y verifica propiedades lógicas (correctitud).
> * **Benchmark:** Mide el rendimiento o ejecución concreta (tiempo real en milisegundos/nanosegundos) en una máquina específica.