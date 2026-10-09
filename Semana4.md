# Semana 4: Árboles Binarios de Búsqueda (BST)

## 1. De Estructuras Lineales a Jerárquicas
Hasta ahora vimos listas y arreglos. Esas son **estructuras lineales**: una cosa va detrás de la otra (usando `next` o índices)[cite: 5]. 
Ahora pasamos a **estructuras jerárquicas**: en lugar de un solo camino, cada elemento puede bifurcarse en dos caminos distintos (`left` y `right`)[cite: 5].

### Vocabulario Básico de Árboles
*   **Árbol:** Estructura de nodos conectados que no tiene ciclos (no da vueltas en bucle)[cite: 5].
*   **Raíz (root):** El punto de inicio del árbol (el de más arriba)[cite: 5].
*   **Hoja (leaf):** Un nodo que no tiene hijos (el final del camino)[cite: 5].
*   **Nodo Interno:** Cualquier nodo que tenga al menos un hijo[cite: 5].
*   **Padre / Hijo:** Relación directa. Si A apunta a B, A es padre y B es hijo[cite: 5].
*   **Subárbol:** Un nodo cualquiera y TODOS los descendientes que cuelgan de él[cite: 5].
*   **Árbol Binario:** Un árbol donde cada nodo tiene **como máximo 2 hijos** (izquierdo y derecho)[cite: 5].

---

## 2. El BST (Binary Search Tree)
Un BST es un árbol binario especial usado como un **conjunto ordenado** (no admite elementos duplicados)[cite: 5]. Para que un árbol binario sea considerado "de búsqueda" (BST), debe cumplir una regla de oro estricta.

### El Invariante Global de Orden (¡MUY IMPORTANTE!)
Para CUALQUIER nodo del árbol:
*   Todas las claves en su **subárbol izquierdo** deben ser **MENORES** que el nodo[cite: 5].
*   Todas las claves en su **subárbol derecho** deben ser **MAYORES** que el nodo[cite: 5].

> **OJO al error común:** No basta con que el hijo izquierdo sea menor y el derecho mayor[cite: 5, 7]. TODO lo que cuelgue por la izquierda (hasta el último nieto) debe ser menor que la raíz de ese subárbol[cite: 5]. Gracias a esto, si buscas un número, puedes descartar mitades enteras del árbol rápidamente[cite: 5].

---

## 3. Representación del BST en Código
Usamos una clase `Node` que guarda 4 cosas[cite: 5]:
*   `int x`: La clave o valor.
*   `Node left, right`: Referencias a sus hijos.
*   `Node parent`: Referencia hacia arriba (quién es su papá).

Y el árbol en sí solo guarda[cite: 5]:
*   `root`: El nodo raíz.
*   `n`: El tamaño lógico (cuántos nodos hay en total).

### Invariantes de Representación
*   Si `n == 0`, entonces `root == null` (árbol vacío)[cite: 5].
*   El `parent` de la `root` siempre es `null`[cite: 5, 7].
*   Si un nodo baja a su hijo, el hijo debe apuntar de regreso a él como su `parent`[cite: 5, 7].

---

## 4. Operaciones Centrales del ADT

### A. La Búsqueda Base: `findLast(x)`
Es el "motor" del árbol. Intenta buscar el valor `x`[cite: 5]. 
*   **¿Cómo funciona?** Usa un nodo actual `w` para bajar por el árbol (si es menor va por `left`, si es mayor va por `right`) y guarda el nodo anterior en la variable `prev`[cite: 5].
*   **¿Qué retorna?** 
    *   Si encuentra el dato, retorna el nodo donde está[cite: 5].
    *   Si NO lo encuentra (`w` llega a `null`), **retorna `prev`**, que es el *último nodo real* que visitó antes de caerse del árbol[cite: 5].

### B. Consultar si existe: `contains(x)`
Usa `findLast(x)` como su sirviente[cite: 6]. 
Le dice: "Tráeme lo que encontraste". Luego, `contains` solo revisa: *"¿El nodo que me trajiste no es nulo y su valor es exactamente el `x` que pedí?"*[cite: 6]. Retorna `true` o `false` en $O(h)$[cite: 6, 7].

### C. Insertar: `add(x)`
Usa el mismo viaje que `findLast(x)`[cite: 6].
1.  Llama a `findLast(x)`. Si la clave ya existe, retorna `false` (no se admiten duplicados, `n` no cambia)[cite: 6, 7].
2.  Si no existe, el GPS (`findLast`) lo dejó parado exactamente en el nodo padre donde debe ir la nueva hoja[cite: 6].
3.  Crea el nuevo nodo, lo engancha al `left` o `right` del padre (según si es menor o mayor), establece el `parent` del nuevo nodo y suma `n++`[cite: 6, 7]. Costo: $O(h)$[cite: 7].

### D. Imprimir ordenado: `inorder()`
Recorre el árbol con la regla: **Izquierda $\rightarrow$ Nodo $\rightarrow$ Derecha**[cite: 6].
Como en un BST los menores están a la izquierda y los mayores a la derecha, imprimirlo en este orden te devuelve los números **perfectamente ordenados de menor a mayor**[cite: 6]. Costo: $O(n)$ porque visita a todos[cite: 7].

---

## 5. Altura ($h$) y Complejidad (Forma del Árbol)
La velocidad de un BST no depende de cuántos nodos tenga ($n$), sino de su **altura ($h$)**, que es el camino más largo desde la raíz hasta una hoja[cite: 6].

*   **Árbol de poca altura (Equilibrado):** Si insertamos los datos mezclados, el árbol se abre como un arbusto. Su altura es muy cortita ($h \approx \log n$). Las búsquedas son ultra rápidas: **$O(\log n)$**[cite: 6].
*   **Árbol Degenerado:** Si insertamos datos ya ordenados (ej. 10, 20, 30, 40), el árbol solo crece hacia la derecha como si fuera una lista lineal[cite: 6]. Su altura es igual a la cantidad de nodos ($h \approx n$). Buscar algo aquí es muy lento: **$O(n)$**[cite: 6].

> **Moraleja:** NUNCA digas que las operaciones de un BST cuestan $O(\log n)$ por defecto. Lo correcto es decir que **cuestan $O(h)$**, porque el costo depende totalmente de la forma del árbol[cite: 6, 7].

---

## 6. Testing (Errores comunes)
*   Ver que el `inorder()` imprime los números ordenados **NO** demuestra que el árbol esté perfecto[cite: 6, 7]. Podrías haber olvidado conectar la variable `parent` de los nodos de regreso hacia arriba, y el inorder no se daría cuenta[cite: 7].
*   Siempre debes verificar los invariantes: que `n` coincida con los nodos reales, que los `parent` apunten bien, y que se cumpla la regla de mayores/menores globalmente[cite: 7].
