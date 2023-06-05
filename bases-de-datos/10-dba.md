
## 1 Tareas básicas del DBA

- Instalación inicial y de nuevos componentes del software
- Interacción con el administrador del sistema
- Garantizar la seguridad del sistema
- Monitoreo y ajustes de rendimiento
- Soporte y recuperación de datos
- Prevención de riesgos y mantenimiento
- Análisis y recomendaciones de mejora
- Apoyo en diseño y optimización de modelos de datos
- Apoyo a los desarrolladores
- Definición de estándares de diseño y nomenclatura de objetos
- Documentación y registro periódico
- Valoración y salario

## 2 Estructura básica de Oracle

- Instancia:
  - SGA (System Global Area)
  - Procesos en segundo plano

- Base de datos:
  - Archivos donde se guarda la información

- Usuarios:
  - Procesos y áreas de memoria PGA (Program Global Area)

La arquitectura de Oracle se compone de instancias y bases de datos. La instancia incluye la SGA y los procesos en segundo plano, mientras que la base de datos almacena los archivos de información. Los usuarios tienen sus propios procesos y áreas de memoria PGA.

Oracle permite diferentes configuraciones de arquitectura, como Standalone con una máquina y múltiples instancias independientes, o RAC con varias máquinas y sus instancias accediendo a una única base de datos.

### 2.1 Estructura de la memoria en Oracle

- Instancia:
  - SGA (System Global Area)
    - Shared pool
      - Shared SQL Area
      - Data Dictionary Cache
      - Result Cache
      - Otros fragmentos de memoria
    - Buffer cache
      - Keep Pool
      - Recycle Pool
      - Buffers Cache
    - Large pool
    - Java pool
    - Stream pools
    - Fixed pool
    - Redo log buffer

- Usuarios:
  - PGA (Program Global Area)
    - UGA (User Global Area)
    - Stack
    - Zonas de Sort, Hash, Mapa de bits, opciones de sesión y cursores

La instancia de Oracle tiene una SGA que incluye diferentes componentes. El Shared pool almacena SQL ejecutadas y librerías del diccionario de datos. El Buffer cache guarda los datos y tiene diferentes zonas. También hay otros componentes como Large pool, Java pool, Stream pools, Fixed pool y Redo log buffer.

Cada proceso de usuario tiene una PGA que contiene la UGA y un Stack, así como zonas adicionales para Sort, Hash, Mapa de bits, opciones de sesión y cursores.

Oracle intenta mantener la mayor parte de la información en memoria para evitar el acceso a disco y mejorar el rendimiento.

### 2.2 Memoria

- Instancia:
  - Procesos background:
    - DBWn (Database Writer)
    - LGWR (Log Writer)
    - CKPT (Checkpoint)
    - SMON (System Monitor)
    - PMON (Process Monitor)
    - RECO (Recoverer)
    - ARCH (Archiver)
    - MMON (Manageability Monitor)
    - MMNL (Manageability Monitor Light)
    - MMON Slave
    - MMNL Slave
    - RVWR (Recovery Writer)
    - DIA0 (Dispatcher)
    - DIAG (Diagnostic)
    - LCK0 (Lock)
    - LREG (Listener Registration)
    - LSNR (Listener)
    - PZ99 (Parallel Query Slave)
    - Wnnn (Job Queue Slave)
    - ASMB (ASM Background)
    - MMAN (Memory Manager)
    - CJQ0 (Job Coordinator)
    - MMON J000
    - MMON J001
    - SMCO (Space Manager Coordinator)
    - LMD0 (Global Enqueue Service Daemon)
    - QMN0 (Queue Monitor)
    - MMON J002
    - MMON J003
    - CJQ0 Jnnn (Job Queue Slave)
    - MMAN Slave
    - MMAN Jnnn (Memory Manager Slave)
    - Dnnn (Dispatcher Slave)
    - CTWR (Client Result Cache Writer)
    - CTWR Slave

- Listener

Los procesos background forman parte de la instancia de Oracle y se encargan de mantener la estructura del sistema. Los usuarios se conectan a la base de datos a través del Listener, que crea procesos de servidor con su correspondiente PGA para atender a los usuarios.

### 2.3 Ficheros

- Archivos de datos:
  - Archivos de datos (data files)
  - Archivos de control (control files)
  - Archivos de registro (redo log files)
  - Archivos de parámetros (parameter files)

- Archivos de configuración:
  - Archivos de contraseñas (password files)
  - Archivos de auditoría (audit files)
  - Archivos de alerta (alert log files)
  - Archivos de respaldo (backup files)
  - Archivos de traza (trace files)
  - Archivos de script (script files)
  - Archivos de exportación e importación (export/import files)

- Archivos de recuperación:
  - Archivos de respaldo (backup files)
  - Archivos de registro de recuperación (recovery catalog files)
  - Archivos de registro de control (control file records)

- Archivos de red:
  - Archivos de configuración de red (network configuration files)
  - Archivos de registro de red (network log files)

- Otros archivos:
  - Archivos de archivos temporales (temp files)
  - Archivos de intercambio (swap files)
  - Archivos de texto plano (plain text files)
  - Archivos de resultados (result files)
  - Archivos de datos externos (external data files)
  - Archivos de indexado (index files)
  - Archivos de archivo (archive files)

Los archivos en Oracle son fundamentales para almacenar datos, configuraciones, registros y otros elementos necesarios para el funcionamiento y la seguridad de la base de datos.

### 2.4 Vision general de la transaccion

1. Usuario realiza una consulta SELECT:
   - Consulta SQL se guarda en Shared Pool.
   - Se busca el valor en Buffer Cache.
   - Si no está en Buffer Cache, se carga desde los archivos de datos.

2. Actualización (UPDATE) del valor:
   - Cambio se realiza en el Buffer Cache.
   - No se modifica en los archivos de datos.
   - Cambios se guardan en Redo Log Buffer.

3. Usuario realiza COMMIT:
   - Datos se escriben en disco y se guardan en archivo de Redo Log.
   - Memoria tiene el valor actualizado, archivos de datos aún tienen el valor anterior.

4. Seguridad y recuperación:
   - Redo Log permite recuperar transacciones no guardadas en archivos de datos en caso de fallos.
   - Checkpoint se realiza para sincronizar memoria y archivos de datos.
   - Cambios se descargan masivamente en disco.

Este esquema simplificado ilustra el funcionamiento básico de Oracle, donde se utilizan diferentes componentes como Shared Pool, Buffer Cache y Redo Log Buffer. La separación entre memoria y archivos garantiza la seguridad y el rendimiento del sistema.

## 3 Arranque y parada de la base de datos:
1. Fase Nomount:
   - Acceso a los archivos de parámetros de la BD.
   - Arranque de la SGA (Shared Global Area).
   - Arranque de los procesos background.
   - Apertura de los logs.

2. Fase Mount:
   - Apertura de los archivos de control.
   - Localización y comprobación de los archivos de datos y redo log.

3. Fase Open:
   - Apertura de los archivos de datos.
   - Apertura de los redo log en línea.
   - Apertura del acceso a la base de datos.

Apagado de la base de datos:
1. SHUTDOWN NORMAL (o simplemente SHUTDOWN):
   - Espera a que los usuarios finalicen sus sesiones antes de cerrar todo.

2. SHUTDOWN IMMEDIATE:
   - Finaliza las transacciones pendientes y cierra la base de datos inmediatamente.

3. SHUTDOWN TRANSACTIONAL:
   - Finaliza las transacciones en curso sin realizar un rollback, pero no permite nuevas transacciones.

4. ABORT:
   - Detiene la base de datos abruptamente, similar a un fallo de energía.

Otros comandos relacionados:
- STARTUP: Inicia la base de datos.
- ALTER DATABASE: Permite cambiar de fase durante el arranque.
- ALTER DATABASE OPEN: Abre la base de datos desde la fase Mount.

Estado "Administrativo":
- Permite restringir el acceso a la base de datos a solo usuarios administradores (DBA).

## 4 Diccionario de Datos
  - Conjunto de tablas y vistas
  - Información sobre la base de datos
    - Definiciones de objetos de la BD
    - Espacio asignado y utilizado por los objetos
    - Datos de seguridad (usuarios, privilegios, roles)
  - Parte central de la gestión del administrador
    - Consulta de información con comandos SQL
  - Tablas básicas (solo lectura para usuarios)
    - Almacenan información sobre la BD
    - Accedidas principalmente por Oracle
  - Vistas (lectura para usuarios)
    - Presentan información legible para administradores y desarrolladores
    - Divididas en tres grupos principales:
      1. Vistas del Diccionario de Datos
      2. Vistas del Diccionario de Datos Ampliado
      3. Vistas Dinámicas (Performance Views)
    - Las vistas dinámicas proporcionan información en tiempo real y se actualizan continuamente
  - Vistas del Diccionario de Datos
    - Ejemplo: Dictionary (índice de vistas y su información)
    - Información detallada sobre objetos de usuario: USER_TABLES
  - Vistas Dinámicas (Performance Views)
    - Actualizadas continuamente con información del rendimiento y los componentes en ejecución
    - Ejemplo: V$FIXED_TABLE (tablas y vistas dinámicas)
    - Ejemplo: V$DATAFILE (información sobre archivos de datos)
    - Se utilizan para consultar detalles específicos, como la memoria en V$SGA

## 5 Parametros de la base de datos

### Parámetros de Oracle
- Definen el comportamiento de la base de datos.
- Pueden modificarse y cambiarse.
- Se pueden ver utilizando el comando SQLPlus o desde SQLDeveloper.

### Vista V$PARAMETER
- Proporciona información detallada sobre los parámetros.
- Permite buscar parámetros relacionados con la SGA.

### Archivos de parámetros
- SPFILE<nom_BD>.ORA: archivo utilizado al iniciar Oracle.
- SPFILE.ORA: archivo utilizado si no se encuentra el SPFILE<nom_BD>.ORA.
- INIT.ORA (opcional): archivo de texto utilizado en versiones anteriores de Oracle.

### Niveles de parámetros
- Parámetros actuales de la sesión: mostrados por SHOW PARAMETER o en V$PARAMETER.
- Valores originales del archivo de parámetros: mostrados en V$SPPARAMETER.
- Parámetros a nivel de instancia: mostrados en V$SYSTEM_PARAMETER.

### Modificación de parámetros
- ALTER SESSION: para cambiar parámetros a nivel de sesión.
- ALTER SYSTEM: para cambiar parámetros a nivel de sistema.
- Algunos parámetros no pueden cambiarse en caliente y requieren reiniciar la base de datos.
- Hay parámetros que solo se pueden modificar en el archivo de parámetros (SPFILE).

### Vista V$PARAMETER (campos IS)
- Proporciona información sobre los valores por defecto y si se pueden modificar a nivel de sesión o sistema.

## 6 Conexiones y redes

Oracle utiliza archivos de configuración tanto en el cliente como en el servidor para definir el comportamiento de las conexiones de usuario, propiedades y protocolos de comunicación.

### Tipos de conexiones

1. **BEQ**: conexión directa cuando el cliente y el servidor están en la misma máquina.
2. **Listener**: conexión remota donde el cliente solicita una conexión al listener, que luego lo comunica a la base de datos y abre un proceso dedicado.

### Arquitectura SQLNet

- Se utiliza para gestionar las conexiones en Oracle.
- Se abre un proceso dedicado con una zona de memoria particular (PGA) independientemente de la forma de conexión.

### Servicio y Listener

- Un mismo listener puede atender a más de una base de datos.
- Cada base de datos tiene un servicio (service_name) que consta del nombre de la base de datos y el dominio.
- El listener escucha las solicitudes para estos servicios.
- No es necesario tener dos listeners, aunque se pueden tener si es necesario.

### Configuración de conexiones

- **BEQ**: se especifica el usuario y contraseña en la conexión.
- **Listener**: se especifica la máquina, el puerto y el servicio al que se desea acceder.

### Archivos de configuración

- **TNSNAMES.ORA**: archivo en el lado del cliente que define cómo acceder al servidor.
- **LISTENER.ORA**: archivo de configuración del listener en el servidor.
- **SQLNET.ORA**: archivo de configuración del cliente.

### Configuración básica de archivos

- **LISTENER.ORA**: contiene parámetros de configuración del lado del servidor y servicios a atender.
- **TNSNAMES.ORA**: contiene parámetros de configuración del lado del cliente.
- **SQLNET.ORA**: contiene propiedades para la conexión, como el orden de búsqueda de servicios.

### Utilidades y herramientas

- **lsnrctl**: utilidad para administrar el listener.
- **Asistente de Configuración de Xarxa**: herramienta gráfica para configurar el listener, TNSNAMES.ORA y SQLNET.ORA.
