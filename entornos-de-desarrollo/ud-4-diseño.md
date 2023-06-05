
# Diseño

## Diagramas de clases

En ingenieria de software, un diagrama de clases en *UML* es un tipo de diagrama de estructura estatica que describe la estructura de un sistema mostrando las clases del sistema, sus atributos, operaciones (o metodos) y las relaciones entre los objetos.

Las clases se representan de la siguiente manera:

| Clase |
| -- |
| [v] nombreAtributo: tipoDato |
| [v] nombreMetodo(parametros): tipoRetorno |

### Visibilidad [v]

| Caracter | Visibilidad | Descripcion |
| :--: | :--: | :--: |
| - | Privado | Solo la misma clase puede acceder a ellos |
| # | Protegido | Solo la clase o las subclases |
| + | Publico | Todas las demas clases pueden acceder |
| ~ | Paquete | Solo las clases del mismo paquete |

### Relaciones entre clases

#### Asociaciones

Las asociaciones representan las relaciones mas generales entre clses, es decir, las relaciones con menor contenido semantico. Para *UML* una asociacion va a describir un conjunto denvínculos entre las instancias de las clases.

Las relaciones pueden ser binarias o n-arias, aunque lo mas normal es utilizar solo relaciones binarias, y sin entrar en detalles, se puede afirmar que una relacion n-aria puede modelarse mediante un conjunto finito de relaciones binarias.

<image src="https://github.com/juvndvv/study/blob/main/entornos-de-desarrollo/img/asociacion.png" alt="herencia" height="90">

    Clase1 -- Clase2


#### Agregación

La agregación es una asociacion con unas connotaciones semanticas mas definidas: es la relación parte-de, que representa una entidad como agregado de partes.

<image src="https://github.com/juvndvv/study/blob/main/entornos-de-desarrollo/img/agregacion.png" alt="herencia" height="90">

    Clase1 o-- Clase2

#### Herencia

La herencia es la tipica relación de generalización/especialización entre clases. En *UML* la herencia se representa mediante una flecha, cuya punta es un triangulo vacio. La flecha que representa a la herencia va orientada desde la subclase a la superclase. Puede ocurrir herencia multiple.

<image src="https://github.com/juvndvv/study/blob/main/entornos-de-desarrollo/img/herencia.png" alt="herencia" height="90">

    Clase1 <|-- Clase2