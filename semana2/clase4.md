# Sequential Processing

Las sentencias secuenciales son aquellas que tienden a ejecutarse de manera serial en nuestro diseño, de manera diferente a los diseños totalmente comportamentales en los cuales es claro que existen muchas asignaciones concurrentes. La mayoría de lenguajes de programación como C/C++, soportan este tipo de comportamiento. De hecho, __VHDL__ toma la sintaxis de este tipo de diseño de [__ADA__](http://www.adacore.com/adaanswers/about/ada).

## Sentencia Process

En una arquitectura definida para una entidad, todas las sentencias son concurrentes. Nos preguntamos entonces ¿ donde existe secuencialidad en __VHDL__ ?. Existe una sentencia, o palabra reservada, conocida como _sentencia process_ la cual internamente sólo contendrá sentencias secuenciales. Un _process_ puede existir en una arquitectura y definir regiones en la arquitectura donde todas las sentencias son secuenciales.

Una sentencia *process* tiene una sección de declaraciones y una parte donde se definen las sentencias. En la seccion de declaraciones se deben declarar los tipos, variables, constantes, subprogramas entre otras. La parte de las sentencias contiene solo sentencias netamente secuenciales. Entre las sentencias de este tipo están __CASE__, __IF THEN ELSE__, __LOOP__, entre otras.

