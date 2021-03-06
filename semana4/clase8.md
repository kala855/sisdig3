# Tipos de datos

Todos los objetos que hemos visto hasta ahora - la señal, la variable y la constante - Pueden ser declarados usando una especificación de tipo que especifica las características del objeto. __VHDL__ contiene un amplio rango de tipos que pueden ser usados para crear objetos simples o complejos. 

Para definir un nuevo tipo, se debe crear una declaración de tipo. Una declaración de tipo define el nombre del tipo y el rango de el tipo. Las declaraciones de tipo son permitidas en las secciones de declaración de paquetes, entidades, arquitecturas, subprogramas y procesos. 

Una declaración de tipo se ve de la siguiente forma:

```vhdl
TYPE type_name IS type_mark;
```

La construcción __type_mark__ cubre un amplio rango de metodos para especificar el tipo. Puede ser cualquier tipo de enumeración de todos los valores pertenecientes al tipo. A continuación vamos a ver algunos __type_mark__. En la siguiente figura podemos ver un diagrama que muestra los tipos disponible en __VHDL__. Las cuatro categorías mas amplias son tipos escalares, compuestos, de acceso y tipos de archivo. Los tipos escalares incluyen todos los tipos simples tales como enteros y reales. Los tipos compuestos incluyen arreglos y registros. Los tipos de acceso son equivalentes a punteros en lenguajes de programación típicos. Finalmente, los tipos archivo le permiten al diseñador la habilidad de declarar objectos __file__.

![TiposDatos](./images/tipos.png)

## Tipos Escalares

Los tipos escalares describen objetos que pueden almacenar como máximo un valor a la vez. Este tipo en sí puede contener múltiples valores, pero un objeto que es declarado para ser de este tipo puede almacenar uno de los valores escalares en un punto específico.

Los tipos escalares comprenden estas cuatro clases:

* __Tipos Enteros__
* __Tipos Reales__
* __Tipos Enumerados__
* __Tipos Físicos__

### Tipos Enteros

Son exactamente como los enteros matemáticos. Todas las operaciones básicas como suma, resta, multiplicación y división aplica a éste tipo de dato. El manual de referencia de __VHDL__ no especifica un rango máximo para enteros, pero especifica un rango mínimo: desde -2.147.483.647 hasta 12.147.483.647. Este rango está definido en el paquete estándar contenido en la __Standard Library__
