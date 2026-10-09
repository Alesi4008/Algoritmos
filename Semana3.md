# Semana 3: Pilas, Colas y Deques

## Conceptos Generales

*   **ADT (Tipo de Dato Abstracto):** Es la lógica detrás de la estructura. Básicamente, te dice **qué** puede hacer el programa y qué operaciones están disponibles (su "contrato"), sin decirte *cómo* están programadas. Por ejemplo, los ADT que veremos hoy: Pila, Cola, Deque y sus políticas (LIFO, FIFO).
*   **Implementación / Representación:** Es el código real que *implementa* este ADT; es decir, **cómo** se guardan los datos en la memoria del computador. Ejemplos: Arreglos estáticos o dinámicos, `SLList` (Listas simplemente enlazadas), `DLList` (Listas doblemente enlazadas), etc.
*   **ADT Restringido:** Básicamente es una secuencia de datos (arreglo dinámico, listas enlazadas, etc.) pero con el acceso restringido deliberadamente. No te deja acceder a cualquier posición (como al medio), sino que te obliga a interactuar con los datos de una manera específica (solo por los extremos) para seguir un patrón de uso claro.

---

## Pila (Stack)
Una estructura donde solo se puede interactuar por un extremo llamado **tope (top)**. Es como apilar libros: solo puedes poner o quitar libros desde arriba, no los del inicio o el medio.

*   **Política LIFO (Last In, First Out):** El último en entrar es el primero en salir. Por ejemplo, cuando acomodas bloques uno encima de otro, el último que pones es el primero que quitas, de lo contrario se cae la torre.

### Operaciones Fundamentales del Stack
*   `push(x)`: Guarda un elemento en el tope ($O(1)$).
*   `pop()`: Quita y te devuelve el elemento del tope ($O(1)$).
*   `peek()`: Solo mira qué hay en el tope sin quitarlo ($O(1)$).
*   `isEmpty()`: Pregunta si la pila está vacía (retorna `true` o `false`) ($O(1)$).
*   `size()`: Te dice cuántos elementos hay guardados ($O(1)$).

### Implementación Enlazada de una Pila (`LinkedStack`)
Básicamente se usa una `SLList` pero únicamente con una referencia al `head` (cabeza). Como todo se opera desde el tope (el `head`), no hace falta guardar referencias a otros lados de la lista (no necesitamos `tail`).

*   **Invariantes de LinkedStack:**
    *   Si `n == 0` $\rightarrow$ la pila está vacía y `head == null`.
    *   Si `n > 0` $\rightarrow$ la pila no está vacía y `head != null`.
    *   Siguiendo las referencias `next` desde el `head`, se alcanzan exactamente `n` nodos.

---

## Cola (Queue)
Estructura donde ingresas elementos por un extremo (el final / *rear*) y retiras por el extremo opuesto (el frente / *front*). 

*   **Política FIFO (First In, First Out):** El primero en entrar es el primero en salir. Funciona exactamente igual que la cola de un banco: el primero que llega es el primero en ser atendido.

### Operaciones Fundamentales de la Cola
*   `add(x)`: Inserta un elemento al final ($O(1)$ amortizado).
*   `remove()`: Quita y devuelve el elemento del frente ($O(1)$ amortizado).
*   `peek()`: Consulta el elemento del frente sin quitarlo ($O(1)$).
*   `isEmpty()` y `size()`: Comprueban estado y tamaño ($O(1)$).

---

## Cola Circular (`ArrayQueue`)
Es la implementación de una Cola usando un **arreglo**. Para no mover todos los datos al eliminar un elemento del frente (lo cual sería muy lento), el arreglo funciona de manera **circular**. 

**¿Cómo funciona?** 
Van ingresando datos. El primer elemento ingresado obtiene el índice del frente `j = 0` (es el primero en ser atendido). Los siguientes se ubican a su derecha.
Si el elemento del frente (`j=0`) es atendido y retirado, **no movemos a los demás**. Simplemente avanzamos el frente: ahora `j = 1`. 
A partir de ahí, los nuevos elementos siguen agregándose a la derecha. Si el arreglo llega a su límite derecho pero hay espacios vacíos al inicio (dejados por los que ya fueron atendidos), los nuevos elementos "dan la vuelta" y se ubican en esos espacios libres de la izquierda.

### Variables de Estado
*   `a[]`: El arreglo físico real en la memoria.
*   `j`: El **índice físico** donde está parado el frente actual de la fila.
*   `n`: El **tamaño lógico**, es decir, el número de personas/elementos que hay guardados en la cola ahora mismo.

### Índices: Físico vs. Lógico
*   **Índice Lógico ($k$):** El orden conceptual dentro de la cola (quién es el 1º en la fila, el 2º, el 3º...). Por ejemplo, $k=0$ es siempre el que está al frente esperando su turno.
*   **Índice Físico:** La posición real o "casilla de la memoria" del arreglo donde está guardado el dato (`a[0]`, `a[1]`, etc.), sin importar su turno de atención.

### Invariante Circular
Todo este mecanismo cumple una regla matemática fundamental. El elemento que ocupa la posición lógica $k$ en la fila, se ubica en la memoria usando la fórmula:
`a[(j + k) % a.length]`
*(El módulo `%` es lo que permite que el cálculo "dé la vuelta" cuando llega al final del arreglo).*

### Resize() (Redimensionamiento)
Cuando el arreglo físico se llena, hay que crear uno más grande y copiar los datos.
*   **Costo de la operación:** `resize()` en sí mismo cuesta **$O(n)$** porque debe copiar todos los elementos uno por uno. Sin embargo, como ocurre muy rara vez, hace que la operación `add(x)` mantenga un costo de **$O(1)$ amortizado**.
*   **¿Cómo se copian los datos?** No se pueden copiar los elementos "sin más" copiando las posiciones físicas, porque los datos podrían estar enroscados/divididos por la circularidad. Para arreglarlo y mantener el orden FIFO, los copiamos guiándonos por su **índice lógico ($k$)**, colocándolos ordenadamente en el nuevo arreglo comenzando desde la posición física `0`. De esta forma, "desenrollamos" la cola.

---

## Deque (Double-Ended Queue / Cola de Doble Extremo)
Es una estructura híbrida entre una Pila y una Cola. Permite insertar y retirar elementos por **ambos extremos** libremente (por el frente o por el final).

*   **Implementación ideal:** Generalmente se implementa usando una `DLList` (Lista Doblemente Enlazada). 
*   **Características de la DLList para el Deque:** Es una lista donde cada nodo tiene **dos referencias**: una al nodo siguiente (`next`) y otra al nodo anterior (`prev`). 
*   **Nodos Dummy (Centinelas):** Para evitar errores al insertar o eliminar en los extremos y hacer el código más limpio, se colocan nodos invisibles llamados *dummy* en los extremos, manteniendo el invariante de que todos los nodos reales tengan conexiones a ambos lados. El `dummy` inicial se conecta con el primer dato, y el `dummy` final con el último.
