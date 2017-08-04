# ¿ Qúe es una FPGA ?

Una __FPGA__ (__Field Programmable Gate Array__) es un circuito integrado
diseñado para ser configurado y adecuarlo a las necesidades que se
tengan.

La configuración de una FPGA es generalmente especificada usando un
lenguaje de descripción de hardware (Hardware Description Language).

Una FPGA contiene un arreglo de bloques lógicos programables, y una
jerarquía de interconexiones reconfigurables que permiten que los
bloques sean "cableados en conjunto", algo similar a lo que sucede
cuando se inter-conectan compuertas lógicas en muchas configuraciones.
Los blosques lógicos pueden ser configurados para realizar funciones
combinatorial complejas, o para simplemente construir compuertas AND y
XOR. En la mayoría de las FPGAs, los bloques lógicos también contienen
elmentos de memoria, los cuales pueden ser Flip-Flops sencillos o
bloques de memoria aún más complejos.

## ¿ Qué es un bloque lógico ?

In computación, un bloque lógico o bloque lógico configurable
(__Configurable Logic Block__) es un bloque de construcción fundamental
de una __FPGA__. Los bloques lógicos pueden ser configurados para
obtener compuertas lógicas configurables.

Los bloques lógicos son la parte más común en la arquitectura de una
__FPGA__, son usualmente dispuestos dentro de una matriz de bloques
lógicos. Estos bloques necesitan generalmente puertos de entrada/salida
(I/O) (para interconectarse con señales externas a la FPGA), y canales
de ruteo (para interconexión de bloques lógicos).

![Logic Cell](./images/FPGA_cell_example.png "Logic Cell")

