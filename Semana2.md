# 🚀 SEMANA 2: Listas Enlazadas y Nodos

## 🧠 Conceptos Básicos
* **Lista enlazada**: Los valores se separan en nodos separados y el orden se reconstruye siguiendo referencias.
* **Nodo**: Es una unidad estructural que contiene un valor y una referencia a otro nodo (como una caja que contiene un dato y una dirección).
* **Head**: Referencia que apunta al *primer* nodo de la lista.
* **Tail**: Referencia que apunta al *último* nodo de la lista.

---

## 🔗 Singly Linked List (SLList)
Tipo especial de lista que usa nodos con referencias **únicamente al siguiente nodo** (`next`). Es decir, solo avanza, no retrocede.

> **⚠️ Limitación Principal:** 
> Al no tener referencias hacia nodos anteriores, en el caso de querer eliminar el último nodo (`tail`), se deberá recorrer toda la lista hasta 1 antes que el `tail` para que este sea el nuevo `tail`, elevando el costo hasta `O(n)`.

### ⚡ Operaciones y Costos (SLList)

| Operación | Costo | Justificación |
| :--- | :---: | :--- |
| **`push(x)` / `pop()`** | `O(1)` | Solo se cambian referencias. En el `push`, cambiar la referencia del primer nodo al antiguo `head`. En el `pop`, simplemente limpiar el nuevo `tail` para cambiar su referencia a `null`. |
| **`add(x)`** | `O(1)` | Para añadir un elemento al final de la lista solo se deben cambiar referencias para el nuevo `tail`. |
| **`size()`** | `O(1)` | El tamaño por lo general se mantiene en una variable constante. |
| **`remove()`** | `O(n)` | Se debe cambiar varias referencias, además de recorrer la lista hasta encontrar el elemento que se quiera borrar. |
| **`getnode()`** | `O(n)` | Se debe recorrer el arreglo hasta el elemento. En el peor de los casos, se recorre toda la lista. |

---

## 🔁 Doubly Linked List (DLList)
Tipo especial de lista que usa nodos con **dos referencias**: al siguiente y anterior nodo (`prev` y `next`, avanza y retrocede). 

A su vez, presenta **nodos centinela llamados *dummy*** a los extremos (fuera de la lista de `n` elementos) para asegurar que cada nodo dentro de la lista tenga referencia a dos nodos. En el caso del `head`, por ejemplo, hace referencia al dummy y al `next`.

### ⚡ Operaciones y Costos (DLList)

| Operación | Costo | Justificación |
| :--- | :---: | :--- |
| **`getnode(i)`** | `O(1 + min(i, n-i))`| Debes recorrer los datos hasta el que deseas, pero puedes hacerlo desde el inicio o desde el final según más te convenga (lo que esté más cerca). |
| **`addBefore()`** | `O(1)` | De manera similar a SLList, una vez en la posición solo se deben cambiar las referencias. |
| **`remove()`** | `O(1)` | De igual manera que en el `add`, al tener acceso a ambos lados del nodo solo se cambian las referencias. |