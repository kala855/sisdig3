# Problema de Asignación Concurrente

Uno de los problemas que la mayoría de diseñadores enfrentan cuando usan __Sequential Signal Assigment__ tiene que ver  con el hecho de que los valores asignados no aparecen inmediatamente en las señales. Esto puede causar comportamiento erróneo en el modelo si el diseñador depende de la actualización de este valor. Un ejemplo de este problema puede verse a continuación:

```vhdl
LIBRARY IEEE;
USE IEEE.std_logic_1164.ALL;

ENTITY mux IS
  PORT (i0, i1, i2, i3, a, b : IN std_logic;
    q : OUT std_logic);
END mux;


ARCHITECTURE mux_behave OF mux IS
  SIGNAL sel : INTEGER RANGE 0 TO 3;
BEGIN
  B : PROCESS (a, b, i0, i1, i2, i3)
  BEGIN
    sel <= 0;

    IF ( a = '1') THEN 
      sel <= sel + 1;
    END IF;

    IF ( b = '1') THEN 
      sel <= sel + 2;
    END IF;

    CASE sel IS
      WHEN 0 => 
        q <= i0;
      WHEN 1 => 
        q <= i1;
      WHEN 2 => 
        q <= i2;
      WHEN 3 => 
        q <= i3;
      WHEN OTHERS =>
        q <= NULL;
    END CASE;
  END PROCESS;
END mux_behave;
```

Como vimos previamente, este es un diseño de un multiplexor de 4 a 1. Dependiendo de los valores de __a__ y __b__, una de las cuatro entradas __i0 to i3__ es seleccionada y asignada a la salida __q__.

Esta arquitectura inicia inicia asignando el valor de __0__ a la señal __sel__. Luego, dependiendo de los valores de __a__ y __b__, los valores de __1__ o __2__ son adicionados a la señal __sel__ para seleccionar alguna entrada. Finalmente, el __CASE__ dependiendo del valor de la señal __sel__ selecciona alguna de las entradas de la __i0__ a la __i3__ y la asigna a la salida __q__.

Sin embargo esta arquitectura, como está definida en el __snippet__ de código anterior, no funciona de manera correcta. Esto debido a que el valor de la señal __sel__ nunca será inicializado por la primer línea en la arquitectura:

```vhdl
sel <= 0;
```

