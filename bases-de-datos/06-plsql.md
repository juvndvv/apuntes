
# PL/SQL

## Bloque PL/SQL

Un bloque PL/SQL consta de tres zona:

- ***Declaraciones***: Zona donde se declaran variables, cursores y excepciones de usuario que se necesitan

- ***Procesos***: Zona donde se realiza el proceso en si.

- ***Excepciones***: Zona donde se define el control de errores

Asi quedaria la estructura basica:

```SQL
DECLARE
    -- Declaracion de variables
    -- Declaracion de cursores
    -- Declaracion de excepciones

BEGIN
    -- Sentencias ejecutables

EXCEPTION
    -- Control de excepciones

END;
```

## Declaracion de variables

```SQL
NOMBRE_VARIABLE <TIPO>;
```

### Mapeo de variables a tipos de columnas de tablas:

```SQL
NOMBRE_VARIABLE TABLA.CAMPO%TYPE;
```

## Estructuras de control

### IF

```sql
IF (condicion) THEN
...
ELSIF THEN
...
ELSE
...
END IF;
```

### CASE

```sql
CASE
...
WHEN CONDICION1 THEN
...
WHEN CONDICION2 THEN
...
ELSE
...
END CASE;
```

### LOOP

```sql
LOOP
EXIT WHEN CONDICION
...
END LOOP;
```

### WHILE

```sql
WHILE CONDICION LOOP
...
END LOOP;
```

### FOR

```sql
FOR VARIABLE IN INICI...FINAL LOOP
...
END LOOP;
```

## Registros (structs)

```sql
TYPE <NOMBRE> IS RECORD (
    CAMPO1 <TIPO>,
    CAMPO2 <TIPO>
    ...
)
```

Uso:

```sql
DECLARE
    TYPE PERSONA IS RECORD (
        NOMBRE VARCHAR2(50),
        EDAD NUMBER(3)
    );

    ENRIQUE PERSONA;

BEGIN
    ENRIQUE.NOMBRE := 'ENRIQUE';
    ENRIQUE.EDAD := 85;

END;
/
```

## Cursores

Un cursor basicamente es una lista generada a partir de un query y hay que recorrerla, esto se puede hacer de diferentes maneras.

1. Manual

```sql
DECLARE
    CURSOR NOMBRE_CURSOR IS <QUERY>;

BEGIN
    OPEN NOMBRE_CURSOR;
    LOOP
        FETCH NOMBRE_CURSOR INTO VARIABLE;
        EXIT WHEN NOMBRE_CURSOR%NOTFOUND;
    END LOOP;
    CLOSE NOMBRE_CURSOR;

END;
/
```

2. Bucle for

```sql
DECLARE
    CURSOR NOMBRE_CURSOR IS <QUERY>;

BEGIN
    FOR C IN NOMBRE_CURSOR LOOP
        -- do something
    END LOOP;

END;
/
```

### Cursor con parametros

Permite crear un cursor que maneja parametros, de manera que el mismo cursor puede utilizarlo en diversas ocasiones reemplazando el valor del parametro.

```sql
DECLARE
    CURSOR MICURSOR(VARIABLE NUMBER) IS SELECT * FROM EMPLOYEES WHERE SALARY = VARIABLE;

BEGIN
    FOR C IN MICURSOR(100) LOOP
        -- do something
    END LOOP;

END;
/
```

## Sentencias DML

Oracle abre un cursor implicito por cada sentencia SQL que se haya de procesar. PL/SQL nos permite referirnos al cursor implicito mas reciente con ***SQL%***. Esto nos servira para recopilar informacion sobre la ultima sentencia SQL procesada.

Atributos definidos:

- ***%NOTFOUND***: devuelve TRUE si en un insert, update o delete no se ha procesado ninguna fila o si la select no ha recuperado nada. En este ultimo se provoca la excepcion NO_DATA_FOUND

- ***%FOUND***: devuelve true si un insert, update o delete ha procesado o si una select recupera algo.

- ***%ROWCOUNT***:  devuelve el numero de filas procesadas por un insert, update o delete o las recuperadas por una select.

## Excepciones

- ***NO_DATA_FOUND***: si una consulta SQL no devuelve nada. ORA-10403. -1403

- ***TOO_MANY_ROWS***: si una consulta SQL devuelve mas de una fila. ORA-01427. -1422

*Continuacion en pdf...*


### Manejo de excepciones

Las excepciones se controlan dentro de su propio bloque, EXCEPTION.

```sql
DECLARE
    -- Declaracions
BEGIN
    -- Execució
EXCEPTION
WHEN E1 THEN
    -- manejo excepcion
WHEN E2 THEN
    -- manejo excepcion
-- mas excepciones
WHEN OTHERS THEN
    -- manejo excepcion
END;
/
```

La excepcion ***OTHERS*** nos permite controlar errores imprevistos. Hay que ponerlo como ultimo manejador de excepciones

### Excepciones definidas por el usuario

PLSQL permite al usuario definir sus propias excepciones, las que tendran que ser declaradas y lanzadas explicitamente utilizando la sentencia RAISE.

Se tienen que declarar en el segmento DECLARE:

```sql
DECLARE
    <NOMBRE> EXCEPTION;

BEGIN
    IF <CONDICION> THEN
        RAISE <NOMBRE>;
    END IF;

EXCEPTION
WHEN <NOMBRE> THEN
    -- do something
END;
/
```

### Excepciones propias

Podemos definir nuestros propios errores:

```sql
DECLARE
    E1 EXCEPTION;
    PRAGMA EXCEPTION_INIT(E1, <ERROR>);

BEGIN
    -- do something

EXCEPTION 
    WHEN E1 THEN
        -- do something

END;
/
```
