# SQL ACTUALIZACIONES

## INSERT
    INSERT INTO tabla [(listaDeColumnas)]
    VALUES (valor1 [,valor2 ...]);

Esta sentencia inserta un registro en una tabla, o bien con el orden establecido en la tabla, o por el orden que nosotros pongamos en *listaDeColumnas*. Podemos no incluir algun campo en *listaDeColumnas* y este automaticamente sera *NULL*.

- Se ha de tener en cuenta que algunos campos no admiten *NULL*
- Tendra que ser del tipo especificado del campo: gastar *TO_DATE()...*

## SEQUENCE
Para crear una secuencia:

    CREATE SEQUENCE nombre
    [INCREMENT BY n]
    [START WITH n]
    [{MAXVALUE n | NOMAXVALUE}]
    [{MINVALUE n | NOMINVALUE}]
    [{CYCLE | NOCYCLE}]
    [CACHE n | NOCACHE];

- ***INCREMENT BY***: indica cuanto se incrementa la secuencia cada vez que se usa, por defecto en 1.
- ***START WITH***: indica el valor inicial de la secuencia, por defecto 1.
- ***MAXVALUE***: valor maximo que puede llegar la secuencia, por defecto ***NOMAXVALUE*** = 1027.
- ***MINVALUE***: valor minimo que puede llegar la secuencia, por defecto ***NOMINVALUE*** = -1026.
- ***CYCLE***: hace que la secuencia vuelva a empezar si ha llegado al valor maximo.
- ***CACHE***: indica cuantos valores de la secuencia Oracle dejara prealojados en memoria, por defecto 20. Si se elige ***NOCACHE*** no se guarda ninguno en memoria.

### Uso de una secuencia
Tenemos ***NEXTVAL***, que nos dara el siguiente valor de la secuencia y actualizara al siguiente; y ***CURRVAL***, que nos devuelve el valor actual, es decir, el ultimo que seha gastado. 

    SELECT DEPARTMENTS_SEQ.NEXTVAL FROM DUAL;

    SELECT DEPARTMENTS_SEQ.CURRVAL FROM DUAL;

## UPDATE
La instruccion ***UPDATE*** modifica los registros de una tabla

    UPDATE tabla
    SET columna1=valor1 [,columna2=valor2...]
    [WHERE condicion];

## DELETE
Esta instruccion elimina los registres de una tabla:

    DELETE [FROM] tabla [WHERE condicion]; 

## Estado de los datos durante la transaccion
El manejo de las transacciones es una de las cuestiones mas complejas para un *SGBD*. Los mas poderosos son capaces de gestionar las transacciones en cumplimiento de la norma que asegura estos cuatro aspectos (*ACID*):
1. **Atomicidad**: que ninguna instruccion se quede a medio hacer
2. **Consistencia**: asegura que una transaccion simepre mantenga la integridad de los datos, se anule o se haga finalmente la transaccion. Hasta en cualquier momento intermedio de la transaccion.
3. **Aislamiento**: asegura que las transacciones simultanias no afecten entre si. Es decir que una transaccion sea independiente a la otra.
4. **Durabilidad**: asegura que cuando se confirme la transaccion, los efectos de sus instrucciones sean definitivos, independientemente de que el sistema se apague o se cierre por un error grave.

Oracle cumple estrictamente esta norma. Si haces dos transacciones a la vez hasta que no se confirme la primero en ejecutarse la segunda no se realiza.

### SAVEPOINT (Punto de salvaguardado)
Realizamos las siguientes instrucciones:

    UPDATE...;
    SAVEPOINT PEREZ;
    UPDATE...;
    COMMIT;

En este caso se realizan las dos sentencias *UPDATE*, pero si hacemos lo siguiente:

    UPDATE...;
    SAVEPOINT PEREZ;
    UPDATE...;
    ROLLBACK TO PEREZ;

solo se ejecutaria la primera *UPDATE*, pero si ejecutamos:

    UPDATE...;
    SAVEPOINT PEREZ;
    UPDATE...;
    ROLLBACK;

no se ejecutaria nada.