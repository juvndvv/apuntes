# Estudio y analisis de un proyecto de software

- [Estudio previo](#id1)
    - [Ambito del proyecto](#id11)
    - [Estudio de viabilidad](#id12)
    - [Analisis de riesgos](#id13)
    - [Estimacion de costes](#id14)
    - [Planificacion](#id15)
- [Analisis](#id2)
    - [Analisis de requisitos](#id21)
    - [UML](#id22)

## Estudio previo <a name="id1" />
El estudio previo es la etapa mas temprana del desarrollo. En esta fase todo gira en torno a las necesidades del cliente y las capacidades del equipo de desarrollo. Es la fase mas critica, ya que cualquier error que se cometa aqui tendra consecuencias en el futuro desarrollo del proyecto

### Ambito del proyecto <a name="id11" />
En esta etapa debemos ***contextualizar el proyecto***, es importante hacer un analisis de las distintas tecnologias que nos permitan llevar a cabo el proyecto, asi como estudiar otras alternativas que sean similares a la que se quiere desarrollar. Tambien es conveniente hacer un pequeño estudio sobre la empresa cliente y el modelo de negocio en que se basa.

En esta etapa debemos de ser capaces de delimitar el ***alcance de nuestro proyecto***. que tareas sera posible realizar con nuestra aplicacion y que tareas se dejarian para un futuro desarrollo. Debemos definir tambien cuales son los ***objetivos principales y secundarios*** del proyecto.

La redaccion de esta seccion debe superar ***la prueba del ascensor***, consistente en ser capaces de describir en que consiste nuestro proyecto sin emplear ningun lenguaje tecnico, como si de una conversacion informal se tratase.

### Estudio de viabilidad <a name="id12" />
En esta etapa debemos abordar la viabilidad del proyecto desde tres perspectivas:

- ***Viabilidad tecnica***: debemos analizar cuales seran las exigencias y requisitos tecnicos para poder desarrollar nuestro proyecto. Es importante saber que tecnologias existen que nos permiten llevar a cabo el proyecto y si hay requisitos u objetivos que no sera posible llevar a cabo.

- ***Viabilidad economica***: se debe analizar los costes a nivel de hardware, software y humanos que supondra el desarrollo del proyecto. Comenzar a evaliar el tiempo que sera necesario para el desarrollo, la cantidad de personal necesaria, los equipos que habra que adquirir para el despliegue del proyecto y las licencias de software que haya que comprar

- ***Viabilidad legal***: debemos estudiar si es posible realizar el proyecto desde un punto de vista legal. Aqui debemos tener en cuenta que leyes o normativas existen que entren en conflicto con nuestro proyecto para poder adaptarnos a la legalidad, asi como los requisitos que pueda haber, *PE*: formularios de consentimiento para el tratamiento de datos personales, derechos digitales, normativa respecto al uso de cookies, etc.

### Analisis de riesgos <a name="id13" />
Debemos evaluar cuales son los riesgos a los que nos podemos enfrentar durante el desarrollo del proyecto y preparar bien un plan de accion para subsanarlos. La gestion de riesgos se realiza mediante dos herramientas:

- ***Evaluacion de riesgos***: identificar que riesgos pueden afectar a nuestro proyecto, la probabilidad de que ocurran y el nivel de impacto en el proyecto.

- ***Control de riesgos***: preparar planes de contingencia, bien sea para evitar que esos riesgos se produzcan o para estar preparados ante ellos y minimizar los daños. Debemos preparar estos planes principalmente para los riesgos que sean mas criticos y que puedan ocurrir con mas probabilidad.

### Estimacion de costes <a name="id14" />
Se distinguen dos tipos de costes en todo desarrollo de software:

- ***Costes economicos***
- ***Costes temporales***

Si bien la estimacion de costes economicos puede resultar bastante sencilla, la de costes temporales siempre es compleja. No existe una formula o tecnica para estimar los costes y solo la experiencia y un buen conocimiento tecnico de las herramientos a emplear pueden garantizar una estimacion de costes acertada.

Para realizar la estimacion de costes temporales debemos empezar a dividir el proyecto en tareas y subtareas, de forma que sea mas sencillo estimar el coste de realizar una tarea. Esta estimacion se puede realizar a nivel de horas, de dias o semanas.

### Planificacion <a name="id15" />
Esta tarea consiste en desglosar el proyecto en distintas etapas, tareas y subtareas, estudiar las dependencias entre ellas, la cantidad de personal que se debe destinar a cada una y el tiempo necesario para desarrollarlas.

Lo habitual es ayudarnos de herramientas, como los diagramas de Gantt, que nos permiten definir, pada cada tarea las fechas de inicio y fin, su distribucion temporal, duracion, dependencias, etc...

La metrica hora-persona nos indica la cantidad de trabajo ininiterrumpido requerido para realizar una tarea, lo habitual es expresar cada tarea en base a esta unidad de medida y pre-asignar el personal disponible para cada una de ellas.

## Analisis <a name="id2" />
Una vez realizado el estudio previo debemos entrar en materia en cuanto al desarrollo del proyecto. Lo primero es realizar una fase de analisis, que se divide en dos tareas:

- Analisis de requisitos: funcionales y no funcionales
- UML: casos de uso

#### Historias de usuario
Para realizar esta tarea cobra gran importancia las historias de usuario. Una historia de usuario se define como el uso que el usuario final ha de realizar del sistema. De pueden obtener mediante *entrevistas con el cliente y usuarios finales*

Ejemplos:

- *Historia 1*: como usuario, quiero poder ver el historico de las compras realizadas y poder ordenarlas y filtrarlas.
- *Historia 2*: como dueño, quiero que los usuarios tengan que registrarse y me indiquen sus datos personales.

Es importante que no nos quedemos solo con lo que nos dice el cliente y que le hagamos preguntas para obtener datos con mayor precision. En la *Historia 1* debemos averiguar criterios de ordenacion que se desean y tipos de filtros a aplicar. En la *Historia 2* debemos obtener un listado de los datos personales que se desean obtener asi como plantearnos la forma de almacenar esa informacion.

### Analisis de requisitos <a name="id21" />
Los requisitos de nuestra aplicacion nacen de las historias de usuario y definen las funcionalidades que debe presentar nuestra aplicacion. Esta es una tarea clave en el desarrollo del proyecto, ya que nos permitira tambien definir el ***modelo de datos*** a utilizar.

Se distinguen dos categorias de requisitos:

- Requisitos funcionales
- Requisitos no funcionales

Aunque se deben de cumplir todos los requisitos por igual, en ocasiones, tambien es conveniente asignar una prioridad a cada uno de los requisitos, de forma que se muestren primero al cliente los resultados mas relevantes.

#### Requisitos funcionales
Son las tareas que el usuario desea realizar en el sistema y las funciones que el usuario espera del mismo. Algunos ejemplos de requisitos funcionales son:

- *RF1*: el usuario tiene que poder loguearse en el sistem
- *RF2*: el administrador puede añadir articulos a la tienda
- *RF3*: el comercial tiene que poder asignar descuentos a los clientes
- *RF4*: el usuario tiene que recibir un mail con la factura en PDF

#### Requisitos no funcionales
Definen los requerimientos de nuestra aplicacion a nivel tecnico, que son transparentes al usuario. Hablariamos pues de ciertos tipos de tecnologiass o tecnicas a emplear en el desarrollo de la aplicacion. Ejemplos:

- *RNF1*: el sistema debe comprobar que la tarjeta tiene fondos antes de realizar la compra
- *RNF1*: el sistema debe almacenar la informacion en bases de datos MySQL
- *RNF1*: la aplicacion web debe ser usable en distintos navegadores y dispositivos moviles
- *RNF1*: la aplicacion debe utilizar interfaces de usuario desarrolladas mediante SceneBuilder
- *RNF1*: debemos guardar un numero de referencia para los pedidos realizados

#### Modelo de datos
Aqui juega un papel importante nuestra capacidad para comunicarnos con el cliente y extraer informacion relevante. En ocasiones deberemos tambien aportar nuestra experiencia para resolver ambigüedades o añadir informacion que el cliente no sepa expresar.

Por ejemplo:

- *"Quiero que se almacene la direccion para los envios"*: que elementos componen la direccion. Direccion fisica, CP, poblacion, provincia, pais, etc.

- *"Los usuarios tienen que poder comprar articulos a traves de la tienda online"*: que metodos de pago admitimos.

Para obtener el modelo de datos deberemos de ser capaces de identificar las *entidades* y sus *atributos*, como se relacionan entre ellas y que tipo de acciones pueden llevar a cabo.

### UML: Casos de uso <a name="id22" />
Los ***casos de uso*** son una traduccion formal de los requisitos y modelo de datos definido anteriormente. Atendiendo a UML, los casos de uso se representan mediante un digrama en el que intervienen actores(usuarios) y el sistema que contiene sus funcionalidades(casos de uso).

Ejemplo de diagrama de casos de uso:

<image src="https://d3n817fwly711g.cloudfront.net/uploads/2015/02/A-use-case-template-for-an-ATM-system.png" alt="Descripción de la imagen" height="450">

Ejemplo de documentacion de casos de uso:

<image src="https://www.juntadeandalucia.es/servicios/madeja/sites/default/files/imagecache/wysiwyg_imageupload_lightbox_preset/wysiwyg_imageupload/10/MadejaIR-EjemploCdU_0.png" alt="Descripción de la imagen" height="450">