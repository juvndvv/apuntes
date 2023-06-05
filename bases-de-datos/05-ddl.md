
# DDL (Data Definition Language) de Oracle

La sintaxis DDL (Data Definition Language) de Oracle se utiliza para crear, modificar y eliminar estructuras de base de datos, como tablas, índices, vistas, restricciones, entre otros.

## Tipos de datos

Tipo de dato  |	Descripción
| -- | --
CHAR(n) |	Cadena de caracteres de longitud fija. El tamaño máximo se especifica como n.
VARCHAR2(n) |	Cadena de caracteres de longitud variable. El tamaño máximo se especifica como n.
NUMBER(p,s) |	Número de precisión fija. Se especifica la precisión (p) y la escala (s).
DATE |	Valor de fecha y hora en formato 'DD-MON-YYYY HH:MI:SS'.
TIMESTAMP |	Valor de fecha y hora con mayor precisión que DATE.
BLOB |	Objeto binario grande, utilizado para almacenar datos binarios de gran tamaño.
CLOB |	Objeto de caracteres grandes, utilizado para almacenar texto de gran tamaño.
BINARY_INTEGER | Número entero binario con un rango de -2,147,483,648 a 2,147,483,647.
BOOLEAN |	Valor lógico que puede ser TRUE o FALSE.
RAW(n) |	Secuencia de bytes de longitud fija. El tamaño máximo se especifica como n.
LONG |	Cadena de caracteres de longitud variable con un tamaño máximo de 2 GB.
LONG RAW |Secuencia de bytes de longitud variable con un tamaño máximo de 2 GB.

## Tablas

La sintaxis para la creacion de tablas es:

```sql
CREATE TABLE nombre_table (
    columna1 tipo_dato,
    columna2 tipo_dato,
    -- [RESTRICCIONES]
);
```

Una vez creadas las tablas podemos modificar su contenido con la sentencia ALTER TABLE:

```sql
ALTER TABLE nombre_table
    [ADD columna tipo_dato]
    [DROP COLUMN columna]
    [RENAME TO nuevo_nombre_tabla]
    [RENAME COLUMN columna TO nuevo_nombre_columna]
    [MODIFY columna tipo_dato{(longitud_nueva)}]
    [ADD CONSTRAINT nombre_constr ...]
    [DROP CONTRAINT nombre_constr];
```

Tambien se pueden eliminar las tablas con la siguiente sentencia:

```sql
DROP TABLE nombre_tabla;
```

### Constraints (restricciones)

Algunos tipos comunes de restricciones son:

- ***Primary Key (Clave primaria)***: Es una restricción que identifica de forma única cada fila en una tabla. Se utiliza para garantizar que no haya duplicados y que cada fila tenga un valor válido para la clave primaria. Solo se permite una clave primaria por tabla.

```sql
CONSTRAINT pk_nombre_tabla
    PRIMARY KEY (nombre_columna1[, nombre_columna2...])
```

- ***Foreign Key (Clave foránea)***: Es una restricción que establece una relación entre dos tablas basada en una columna común. La clave foránea garantiza que los valores en la columna de la tabla secundaria coincidan con los valores existentes en la columna referenciada de la tabla principal.

```sql
CONSTRAINT fk_nombre_tabla
    FOREIGN KEY (columna1)
    REFERENCES tabla_principal (columna_referenciada)
```

- ***Unique Key (Clave única)***: Es una restricción que asegura que los valores en una columna o conjunto de columnas sean únicos en una tabla. A diferencia de la clave primaria, se permite tener múltiples claves únicas en una tabla.

```sql
CONSTRAINT uk_nombre_tabla UNIQUE (columna1)
```

- ***Check Constraint (Restricción de comprobación)***: Es una restricción que verifica que los valores en una columna cumplan una condición especificada. Puedes definir una expresión lógica personalizada para validar los datos ingresados en la columna.

```sql
CONSTRAINT ck_nombre_tabla CHECK (condicion)
```

- ***Not Null Constraint (Restricción de no nulo)***: Es una restricción que asegura que una columna no acepte valores nulos. Obliga a que todos los registros tengan un valor válido en esa columna

```sql
CREATE TABLE nombre_tabla (
    columna tipo_dato NOT NULL,
    ...
);
```

## Indices

Los índices se utilizan para mejorar el rendimiento de las consultas, ya que reducen el tiempo necesario para encontrar los datos requeridos. Al utilizar un índice, la base de datos puede realizar una búsqueda más rápida y eficiente, ya que tiene una guía rápida de los valores y ubicaciones asociadas.

Es importante tener en cuenta que si bien los índices mejoran el rendimiento de las consultas, también tienen un impacto en las operaciones de inserción, actualización y eliminación de datos, ya que la base de datos debe mantener los índices actualizados. Por lo tanto, es necesario equilibrar la cantidad y el tipo de índices utilizados en una base de datos según las necesidades específicas de la aplicación y la carga de trabajo.

En resumen, un índice es una estructura adicional en una base de datos que permite una búsqueda y acceso más rápido a los datos almacenados en una tabla, mejorando así el rendimiento de las consultas.

Sintaxis de creacion de un indice:

```sql
CREATE INDEX nombre_indice
ON nombre_tabla (columna1[, columna2,...
])
```

Es recomendable crear un índice en una columna cuando hay consultas frecuentes que buscan o filtran datos en esa columna. Los índices mejoran el rendimiento de las consultas al permitir una búsqueda más rápida y eficiente de los valores deseados. También se recomienda crear índices en columnas utilizadas para unir tablas o para ordenar resultados de consultas. Sin embargo, debes tener en cuenta que los índices tienen un impacto en las operaciones de inserción, actualización y eliminación, por lo que es necesario evaluar cuidadosamente y equilibrar el uso de índices según tus necesidades y patrones de acceso a los datos.

## Secuencias

Una secuencia en Oracle es un objeto de base de datos que genera valores únicos en orden secuencial. Se utiliza comúnmente para generar claves primarias automáticamente o para asignar valores únicos a columnas en una tabla. Una secuencia es independiente de cualquier tabla y puede ser compartida por múltiples tablas en la base de datos. Cuando se crea una secuencia, se especifica un valor inicial y un incremento. Cada vez que se llama a la secuencia, genera un nuevo número secuencial y lo devuelve. Las secuencias son especialmente útiles cuando se necesita garantizar valores únicos y en aplicaciones donde se requiere generar identificadores únicos de manera eficiente.

Se puede crear una secuencia de la siguiente manera:

```sql
CREATE SEQUENCE nombre_secuencia
    [START WITH valor_inicial]
    [INCREMENT BY incremento]
    [MINVALUE valor_minimo]
    [MAXVALUE valor_maximo]
    [CYCLE | NOCYCLE]
    [CACHE cantidad_cache]
    [ORDER | NOORDER];
```

## Sinonimos

En Oracle, un sinónimo es un objeto de base de datos que proporciona un nombre alternativo o alias para otro objeto existente, como una tabla, una vista, una secuencia o incluso otro sinónimo. El sinónimo permite acceder y referenciar al objeto original utilizando un nombre más sencillo y fácil de recordar, en lugar del nombre completo del objeto.

```sql
CREATE [PUBLIC] SYNONYM nombre_sinonimo
    FOR nombre_objeto
```
