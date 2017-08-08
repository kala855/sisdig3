# Arquitecturas y Entidades

A continuación continuaremos con la explicación de algunas
características presentes en las __Architectures and Entities__.

## Asignación Concurrente de Señales

Veamos un ejemplo a continuación:


```vhdl
select <= 0 WHEN s0 = '0' AND s1 = '0' ELSE
        1 WHEN s0 = '1' AND s1 = '0' ELSE
        2 WHEN s0 = '0' AND s1 = '1' ELSE
        3;
```

En un lenguaje como __C__ o __C++__, cada asignación se ejecuta de
manera totalmente secuencial, una después de la otra en un orden
especificado. Básicamente puede decirse que el orden de ejecución es
determinado por el orden de las sentencias en el archivo fuente. En
__VHDL__, no hay un orden específico de las sentencias de asignación.
El orden de ejecución es totalmente especificado por los eventos que
ocurren sobre las señales a las cuales una sentencia de asignación es
sensible.

Por ejemplo una asignación es identificada por el símbolo __<=__. De
manera tal en el ejemplo anterior, la señal llamada __select__ obtendrá
un valor numérico que será asignado de acuerdo a los valores __s0__ y
__s1__. Ésta sentencia será ejecutado cuando la señal __s0__ o __s1__
cambien. Normalmente un sentencia de asignación de señales es sensible a
los cambios en cualquier señal que se encuentra a la derecha del símbolo
__<=__. En este caso se puede decir que la señal __select__ es sensible
a las señales __s0__ y __s1__.
