SEMANA 2 : LISTAS ENLAZADAS Y NODOS
Lista enlazada : Los valores se separan en nodos separados y el orden se reconstruye siguiendo referencias  
Nodo : Es una unidad estructural que contiene un valor y una referencia a otro nodo(como una caja que contiene un dato y una direccion)
SLList : Tipo especial de lista que usa nodos con referencias unicamente al siguiente nodo (next , solo avanza no retrocede)
Head : Referencia que apunta al primer nodo de la lista
Tail : Referencia que apunta al ultimo nodo de la lista
Limitacion : Al no tener referencias hacia nodos anteriores en el caso de querer eliminar el ultimo nodo (tail) se debera recorrer todo  el arreglo hasta 1 antes que el tail para que este sea el nuevo tail , elevando el costo hasta O(n)
Operaciones y costos:
push(x) y pop : O(1) pues en ambos casos lo unico que se debe hacer es cambiar referencias , en caso del push , cambiar la referencia del primer nodo al antiguo head y el caso del pop simplemente limpiar el nuevo tail para cambiar su referencia a null
add(x) : O(1) pues para añadir un elemento al final de la lista solo se debe cambiar referencias para el nuevo tail 
size() : O(1) pues el tamaño por lo general es constante
remove() : O(n) pues se debe cambiar varias referencias , ademas de recorrer la lista hasta el elemento q se quiera borrar 
getnode() : O(n) pues se debe recorrer el arreglo hasta el elemento , en el peor de los casos recorre todo el arreglo
DLList : Tipo especial  de lista que usa nodos con dos referencias al siguiente y anterior nodo (prev y next , avanza y retrocede) a su vez presenta nodos centinela llamados dummy a los extremos fuera de la lista de n elementos para asegurar que cada nodo dentro de la lista tenga referencia a dos nodos , en caso del head referencia al dummy y al next por ejemplo
Operaciones y costos:
getnode() : O(1 + min(i,n-i)) pues debes recorrer los datos hasta el que deseas pero puedes hacerlo desde el inicio o desde el final segun mas te convenga 
addBefore() : O(1) pues de manera similar a SLList solo se debe cambiar referencias
remove() : O(1) pues de igual manera al add solo se cambia referencias

