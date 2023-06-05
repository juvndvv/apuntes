# Desarrollo de software

## Fundamentos del desarrollo de software
El ***software*** el conjunto de los programas de computo, procedimientos, reglas, documentacion y datos asociados, que forman parte de la operacion de un sistema de computacion.

Es el conjunto de ***componentes logicos*** de un ordenador que permiten la realizacion de tareas especificas.

### Tipos de software
- ***Software de sistema***: controladores, servidores, herramientas de diagnostico, etc.
- ***Software de programacion***: editores de texto, compiladores, interpretes, depuradores, IDEs, etc.
- ***Software de aplicacion***: aplicaciones ofimaticas, bases de datos, videojuegos, software empresarial, software de diseño, etc.

### Como se relaciona con el hardware
El software se ejecuta sobre dispositivos fisico.

***Arquitectura Von Neumann***:
- Sistemas de entrada/salida: para recibir y comunicar informacion
- Memoria principal: almacenan la instrucciones a ejecutar y datos
- Unidad de control: decodifica las instrucciones y su orden de ejecucion
- Unidad aritmetico-logica: ejecuta las instrucciones

El sistema operativo actua de intermediario, coordinando el hardware y las aplicaciones en funcionamiento.

Las aplicaciones consumen recursos(hardware):
- *Tiempo de CPU para su ejecucion*
- *Espacio de almacenamiento en memoria RAM*
- *Gestion de interrupciones*
- *Gestion de dispositivos de E/S*

## Tipos de codigo

#### Codigo fuente
Es el conjunto de lineas de texto que indica los pasos (instrucciones) a seguir por el ordenador para realizar una tarea.

- Esta escrito en un lenguaje de programacion especifico
- No es directamente ejecutable por el ordenador
- Debe ser compilado a lenguaje maquina o codigo objeto

#### Codigo objeto
Es el codigo resultado de una compilacion del codigo fuente

- Conjunto de instrucciones y datos que el ordenador entiende directamente
- Puede estar escrito en lenguaje maquina o bytecode
- Puede estar distribuido en diversos archivos
- Un enlazador (Linker) se encarga de juntar el codigo objeto y obtener el programa ejecutable

#### Codigo ejecutable
Es el conjunto de instrucciones compilada y enlazadas, listas para ser ejecutadas por una maquina.

- Depende del SO: .exe (Windows), permiso x (Linux)

## Maquinas virtuales
#### Objetivos
- Ser lo bastante simple para ejecutar codigo de manera sencilla pero con la suficiente riqueza para poder expresar las construcciones complejas.

- Estar lo suficientemente proximas a los procesadores reales para que el paso del codigo intermedio al codigo maquina sea directo.

#### Caracteristicas
- Pocos modos de direccionamiento
- Numero limitado de registros
- Juego de instrucciones simple
- Independiente del lenguaje de programacion

#### Ventajas
- Facilitan la escritura de compiladores para distintas maquinas
- Permiten utilizar un mismo binario en maquinas con distintas arquitecturas y SSOO
- Añaden una capa de optimizacion al proceso de traduccion de codigo fuente al codigo objeto

## Lenguajes de programacion
Un lenguaje de programacion es un ***lenguaje formal*** que proporciona al programador la capacidad de escribir una serie de instrucciones en forma de ***algoritmos***, con el fin de controlar el comportamiento logico de un sistema informatico

#### Clasificacion segun nivel
- ***Lenguaje maquina***: codigo binario
- ***Lenguaje ensamblador***: lenguaje maquina entendible por humanos. Requiere de un ensamblador
- ***Lenguajes de medio nivel***: permiten realizar tareas mas complejas. Siguen necesitando un ensamblador
- ***Lenguajes de alto nivel***: dependen de traductores y compiladores.

#### Clasificacion segun el paradigma
- ***Imperativo o por procedimientos***: se basan en dar instrucciones
- ***Orientada a objetos***: se basan en el imperativo pero encapsulan los elementos en objetos.
- ***Por eventos***: la estructura y ejecucion de los programas dependen de los sucesos que ocurren en el sistema
- ***Declarativos***: describen el problema, declaran propiedades y reglas que deben cumplirse
- ***Funcional***: se basan en la definicion de predicados, estan orientados hacia las matematicas
- ***Logicos***: se basan en la definicion de relaciones logicas
- ***Con restricciones***: similar a los logicos pero usando ecuaciones
- ***Multiparadigma***: uso de dos o mas paradigmas dentro de un programa
- ***Reactiva***: declaran objetos emisores de eventos asincronos y otros objetos subscriptores que reaccionan a los primeros
- ***Lenguajes de dominio***: desarrollados para resolver un problema en especifico

## Generacion de codigo
Como hemos mencionado, la tarea de programar implica pasar de un codigo fuente entendible por humanos a un codigo maquina.
Esta transformacion se realiza mediante un ***compilador*** o un ***interprete***, segun el lenguaje de programacion empleado.

En un *lenguaje compilado*, el codigo fuente es transformado a lenguaje maquina y posteriormente ejecutado en la maquina.

En un *lenguaje interpretado* el codigo fuente es transformado a un lenguaje maquina a medida que es ejecutado.

## Ingenieria del software
Programar es el proceso de crear un software fiable median te la escritura, prueba, depuracion, compilacion (o interpretacion) y mantenimiento del codigo fuente de dicho programa informatico

- Desarrollo logico de un prograama para resolver un problema en particular
- Escritura logica del programa empleando un lenguaje de programacion especifico
- Compilacion o interpretacion del programa
- Prueba y depuracion
- Desarrollo de la documentacion

La ***ingenireia del software*** es la rama de la ciencia de la computacion que estudia la creacion del software fiable y de calidad.

El desarrollo del software se divide en 7 fases:

- Analisis de requisitos
- Diseño de la solucion
- Implementacion
- Pruebas, depuracion y optimizacion
- Documentacion
- Explotacion
- Mantenimiento

#### Analisis
- Estudiar las necesidades del cliente
- Definir las funcionalidades que debe tener la aplicacion final
- Describir las interfaces de usuario necesarias
- Definir los casos de uso

Para realizar un correcto analisis de requisitos es vital una buena comunicacion entre cliente y desarrollador

#### Diseño
- Definir las tecnologias (software y hardware) a utilizar
- Diseñar los componentes funcionales del sistema
- Describir el funcionamiento del sistema
    1. Diagrama de Clases
    2. Diagramas de Secuencia
    3. Diagramas de Interaccion
    4. Diagramas de Actividades
    5. Diagramas de Estado

#### Codificacion
En esta fase se realiza la implementacion del programa.

Depende del lenguaje de programacion.

Debe cumplir los requisitos definidos en la fase de analisis y las funcionalidades y diagramas descritos en la fase de diseño

Esta directamente relacionada con la fase de pruebas

#### Pruebas
En todo desarrollo de software se deben definir una serie de pruebas para el correcto funcionamiento del sistema:

- Funcionales
- Estructurales
- De regresion
- Pruebas de codigo
- Prueba unitarias

Tambien se incluiria en esta fase la optimizacion del codigo

#### Documentacion
La fase de documentacion debe llevarse a cabo a lo largo de la vida del proyecto. En esta fase se incluirian:

- Control de versiones
- Uso de comentarios
- Documentacion de clases
- Documentacion de campos

#### Explotacion y mantenimiento
- Despliegue del software en el sistema cliente
- Gestion de incidencias
- Actualizacion y adaptacion del codigo a nuevas funcionalidades
- Revision y correccion de errores