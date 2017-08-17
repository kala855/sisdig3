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
