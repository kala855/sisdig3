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
