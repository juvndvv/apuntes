
# Procedimientos, paquetes, funciones y triggers

## Procedimientos y funciones

### Funciones

Las ***funciones*** devuelven obligatoriamente un dato.

```PL/SQL
CREATE [OR REPLACE] FUNCTION <nombre> RETURN <tipo> IS|AS
    -- declaracion de variables

    BEGIN

    EXCEPTION

    END;
```

### Procedimientos

Un ***procedimiento*** no devuelven nada, hacen operaciones y reciben la informacion necesaria mediante los paramteros y pueden utilizar esos parametros para devolver informaciones.

```PLSQL
CREATE [OR REPLACE] PROCEDURE <nombre> IS|AS
    -- declaracion de variables

    BEGIN

    EXCEPTION

    END;
```

### Parametros de entrada y de salida

En las funciones los parametros siempre seran de entrada, en cambio en los procedimientos podran ser de entrada, de salida y de entrada/salida.

La sintaxis de los parametros es la siguiente:

```PLSQL
CREATE OR REPLACE PROCEDURE(A IN NUMBER, B OUT VARCHAR2, C IN OUT NUMBER)
    ...
```

### Uso de funciones y procedimientos

Para poder usar las funciones y los procedimientos primero se tiene que **compilar** el archivo con el bloque PL/SQL que los contiene. Para ello usamos:

```PLSQL
START <archivo>
```

Si hay algun **error de compilacion** lo podremos ver usando:

```PLSQL
SHOW ERRORS
```

Una vez compilado podremos hacer uso desde cualquier lugar de nuestra base de datos. Los procedimientos que no devuelven ningun valor se pueden utilizar con la sentencia:

```PLSQL
EXECUTE <procedimiento>
```

Si lo que queremos es usar el procedimiento o funcion dentro de un bloque PL/SQL se invoca como cualquier otro predeterminado de Oracle. Por ejemplo, para el siguiente procedimiento:

```PLSQL
CREATE OR REPLACE PROCEDURE INFO_RECTANGULO(L1 IN NUMBER, L2 IN NUMBER, AREA OUT NUMBER, PERIMETRO OUT NUMBER) IS
BEGIN
    AREA := L1 * L2;
    PERIMETRO := (L1 + L2) * 2;
END;
/
```

Su uso seria el siguiente:

```PLSQL
[EJEMPLO DE LLAMADA AL PROCEDIMIENTO]
DECLARE
    AREA      NUMBER;
    PERIMETRO NUMBER;
    L1        NUMBER;
    L2        NUMBER;
BEGIN
    L1 := 2;
    L2 := 4;

    INFO_RECTANGULO(
        L1 => L1 /*IN NUMBER*/,
        L2 => L2 /*IN NUMBER*/,
        AREA => AREA /*OUT NUMBER*/,
        PERIMETRO => PERIMETRO /*OUT NUMBER*/
    );
    DBMS_OUTPUT.PUT_LINE(
        A => PERIMETRO /*IN VARCHAR2*/
    );
END;
/
```

Si queremos *borrar* un procedimiento o funcion la siguiente instruccion nos permite hacerlo:

```PLSQL
DROP [PROCEDURE|FUNCTION] <nombre>
```

### Ver funciones y procedimientos

Existen dos vistas que nos permiten ver el contenido de las funciones y de los procedimientos las cuales son:

- ***USER_OBJECTS***: contiene la informacion o *cabeceras* de las funciones y procedimientos. Si le haces un *desc user_objects* muestra que guarda de ellos.

- ***USER_SOURCE***: contiene el contenido de las funciones y procedimientos. Aqui podemos ver la implementacion de los mismos. Podemos ver tambien que guarda ejecutando *desc user_source*.

## Paquetes

Un ***paquete*** es una estructura *PL/SQL* que permite almacenar juntas una serie de objetos relacionados. Dentro de un paquete se pueden incluir procedimientos, funciones, cursores, tipos y variables.

Para crear paquetes primero se debe crear la cabecera del paquete, que indica el contenido del mismo, con la siguiente sintaxis:

```PLSQL
CREATE OR REPLACE PACKAGE <nombre> IS|AS
    --cabeceras del contenido
    FUNCTION <nombre1>(<parametros>) RETURN <tipo>;
    PROCEDURE <nombre2>(<parametros>);
    ...
END <nombre>;
```

Luego deberemos implementar lo que se ha indicado que contendra el paquete y lo haremos de la siguiente manera:

```PLSQL
CREATE OR REPLACE PACKAGE BODY <nombre> IS|AS
    FUNCTION <nombre1>(<parametros>) RETURN <tipo> IS|AS
        ...
    END <nombre1>;
    PROCEDURE <nombre2>(<parametros>) IS|AS
        ...
    END <nombre2>;
END <nombre>;
```

### Uso de paquetes

Para utilizar los paquetes tambien deberemos de compilarlos, tanto la cabecera como el cuerpo con *START*. Para ejecutar con *EXECUTE* o utilizarlo dentro de un bloque PL/SQL se utiliza la siguiente sintaxis:

```PLSQL
PAQUETE.PROCEDIMIENTO(params)
```

Para **eliminar** con el siguiente:

```PLSQL
DROP PACKAGE <nombre>
```

## Triggers

Un ***trigger*** o disparador proporciona una tecnica procedural para especificar y mantener las restricciones de integridad. Permiten a los usuarios especificar las condiciones de integridad mas complejas, ya que un disparador es esencialmente un *procedimiento* *PL/SQL*.

Estos son objetos de una base de datos que se utiliza para definir una accion automatizada que se ejecuta en respuesta a un evento especifico en una tabla o vista.

Los triggers pueden ser utilizados para realizar una amplia variedad de tareas, como mantener la integridad de los datos, auditar cambios en la tabla, automatizar calculos y realizar acciones personalizadas en funcion de ciertos eventos.

```PLSQL
CREATE [OR REPLACE] TRIGGER <nombre>
BEFORE|AFTER|INSTEAD OF
[DELETE] [OR INSERT] [OR UPDATE [OF columna [,columna]]]
ON tabla|vista
[REFERENDING [OLD [AS] <old>] [NEW [AS] new]]
[FOR EACH {ROW|STATEMENT}]
[WHEN condicion]
    -- bloque pl/sql
```

## Transacciones

## BLOB

## REF CURSOR

## PL/SQL dinamico
