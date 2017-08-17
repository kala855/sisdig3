# PROBLEMA DE ASIGNACIÓN CONCURRENTE

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
Esta sentencia dentro de un __process__ agenda un evento para la señal __sel__ en el siguiente delta de tiempo con el valor de __0__. Sin embargo, el procesamiento continúa dentro de la sentencia __process__ con la siguiente sentencia secuencial. El valor de __sel__ se mantiene en el valor que haya tenido al ingresar al __process__. Sólo cuando el __process__ es completado el delta de tiempo finaliza y el nuevo valor en __sel__ se ve reflejado. Ya cuando esto suceda, el resto del __process__ ha sido procesado usando un valor erróneo del valor de __sel__.

Existen 2 formas de solucionar este problema, el primero sería insertar una sentencia __WAIT__ después de cada asignación secuencial como se muestra a continuación:


```vhdl
ARCHITECTURE mux_fix1 OF mux IS
  SIGNAL sel : INTEGER RANGE 0 TO 3;
BEGIN
  PROCESS
  BEGIN
    sel <= 0;
    WAIT FOR 0 ns; 
    
    IF (a = '1') THEN
      sel <= sel + 1;
    END IF;
    WAIT FOR 0 ns;
    
    IF (b='1') THEN
      sel <= sel + 2;
    END IF;
    WAIT FOR 0 ns;
  
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
    WAIT ON a,b,i0,i1,i2,i3;
  END PROCESS;
END mux_fix1;
```
Las sentencias __WAIT__ después de cada asignación en las señales causan al proceso a esperar por un delta de tiempo antes de continuar con la ejecución. Esperar este delta ocasiona que el nuevo valor alcance a propagarse de la manera esperada.

Una consecuencia de las sentencias __WAIT__, sin embargo, es que el proceso no puede tener una lista de sensibilidad. Un proceso con sentencias __WAIT__ que se encuentren dentro de él o en un llamado a un subprograma llamado desde el proceso no puede tener una lista de sensibilidad. La lista de sensibilidad implica que la ejecución incie desde el inicio del procedimiento, mientras una sentencia __WAIT__ permite suspender un proceso particular en un punto específico. Las 2 son mutuamente exclusivas.

Dado que el proceso no puede tener la lista de sensibilidad, una sentencia __WAIT__ ha sido añadida al final del __process__, de manera tal que se imita el comportamiento exacto de una lista de sensibilidad. Ésta sentencia puede verse a continuación:

```vhdl
WAIN ON a, b, i0, i1, i2, i3;
```
La sentencia __WAIT__ permite continuar cuando alguna de las señales definidas despues de la palabra reservada __ON__ tiene algún tipo de evento sobre ellas.

Este método permite resolver el problema de asignación secuencial, pero una mejor solución es usar una variable interna, en lugar de una señal interna como se muestra a continuación:

```vhdl
ARCHITECTURE mux_fix2 OF mux IS
BEGIN
  PROCESS (a, b, i0, i1, i2, i3)
    VARIABLE sel : INTEGER RANGE 0 TO 3;
  BEGIN
    sel := 0;
    WAIT FOR 0 ns; 
    
    IF (a = '1') THEN
      sel := sel + 1;
    END IF;
    
    IF (b='1') THEN
      sel := sel + 2;
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
END mux_fix2;
```
En este caso, la señal __sel__ de el ejemplo anterior ha sido convertida a una variable entera interna. Esto fue realizado moviendo la declaración desde la arquitectura a antes del __BEGIN__ del __PROCESS__. Las variables sólo puede ser declaradas en la sección de el __PROCESS__ o de los __SUBPROGRAM__.

Ahora que se usan variables, las asignaciones __:=__ que funcionan sobre este tipo de declaraciones deben usarse. Las variables son actualizadas inmediatamente y permitirán que nuestro diseño funcionen de la manera esperada.

# TIPOS DE DATOS

Vamos ahora a ver los tipos de datos que se puede usar en __VHDL__, ya algunos hemos venido utilizándolos, ahora vamos a dar una explicación de los más comunes.

## Tipos de Objetos

Un objeto en __VHDL__ consiste de uno de los siguientes:

* __SIGNAL__, la cual representa una interconexión cableada que conecta puertos instanciados en los componentes.

* __VARIABLE__, la cual es usada para almacenamiento local de datos temporales, sólo es visible al interior del __PROCESS__.

* __CONSTANT__, la cual nombra valores específicos.

### SIGNAL

Los objetos de este tipo son usados para conectar entidades entre sí y formar modelos complejos. Las señales son los medios de comunicación de datos dinámicos entre entidades. Una declaración de una señal se ve de la siguiente manera:

```vhdl
SIGNAL signal_name : signal_type [:= initial_value];
```
La palabra clave __SIGNAL__ es seguida por una o más nombres de señale. Cada nombre de señal crea una nueva señal como tal. Cada nombre de señal debe ir separado con una coma. Los tipos de señal especifican el tipo de dato de la información que contendrá esa señal. Finalmente, la señal puede contener valores de inicialización.

Las señales deben ser declaradas en las secciones de declaración de la entidad, arquitectura o paquetes. Las señales definidas en paquetes son comúnmente referidas como señales globales debido a que pueden ser compartidad entre las entidades.

A continuación podemos ver un ejemplo de declaración de señales:

```vhdl
LIBRARY IEEE;
USE IEEE.std_logic_1164.ALL;
PACKAGE sigdecl IS
  TYPE bus_type IS ARRAY(0 TO 7) OF std_logic;
  
  SIGNAL vcc : std_logic := '1';
  SIGNAL ground : std_logic := '0';
  
  FUNCTION magic_function (a : IN bus_type) RETURN
    bus_type;
    
END sigdecl;

USE WORK.sigdecl.ALL;
LIBRARY IEEE;
USE IEEE.std_log_1164.ALL;
ENTITY board_design IS
  PORT (data_in : IN bus_type;
        data_out: OUT bus_type);
        
  SIGNAL sys_clk : std_logic := '1';
END board_design;

ARCHITECTURE data_flow OF board_design IS
  SIGNAL int_bus : bus_type;
  CONSTANT disconnect_value : bus_type := ('X','X','X','X','X','X','X','X');
BEGIN
  int_bus <= data_in WHEN sys_clk = '1'
    ELSE int_bus;
    
  data_out <= magic_function(int_bus) WHEN sys_clk = '0'
    ELSE disconnect_value;
    
  sys_clk <= NOT(sys_clk) after 50 NS;
END data_flow;

```
