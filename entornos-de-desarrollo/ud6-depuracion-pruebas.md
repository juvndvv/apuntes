
# 6. Depuracion y pruebas

## Depuracion

### Depuracion mediante logger en Java

Java contiene la Java Logging API. Esta API de logging nos permite configurar los mensajes de que tipo son escritos. Clases individuales pueden usar este logger para escribir mensajes en los archivos de log configurados.

El paquete ***java.util.logging*** nos proporciona las capacidad de hacer logging mediante la clase Logger.

#### 1. **Creacion del Logger**

```java
private final static Logger LOGGER = Logger.getLogger(Logger.GLOBAL_LOGGER_NAME);
```

#### 2. Niveles del logger

El log define la gravedad del mensaje. La clase ***Level*** se usa para definir que mensajes deben ser escritos en el log. El orden es el siguiente:


- ALL
- SEVERE (mas preocupante)
- WARNING
- INFO
- CONFIG
- FINE
- FINER
- FINEST
- OFF

El nivel se establece de la siguiente manera:

```java
LOGGER.setLevel(Level.INFO);
```

Esta sentencia establece el nivel en INFO, lo que hace que solo se muestren los mensajes de INFO, WARNING y SEVERE.

#### 3. Handler

Cada **Logger** puede tener acceso a varios **Handler**. Estos lo que hacen es recibir el mensaje de log y lo exportan con cierto formato a cierto archivo. Es importante saber que cuando hacemos log y este nos sale por consola esto es por que el log tiene por defecto lo que se llama **ConsoleHandler**, que como su propio nombre indica es el **FileHandler** sobre el 'archivo' consola.

Asi se crea un handler:

```java
FileHandler handler = new FileHandler(salida, append);
```

Donde:
- **salida**: archivo al que se redirije la salida.
- **append**: booleano, indica si sobreescribe o no.

#### 4. Formatter

El formatter como su propio nombre indica es la clase que le da formato a la salida del logging. Java nos proporciona la clase **java.util.logging.SimpleFormatter**, la cual hace un formateo simple.

*Voguella* nos proporciona un Formatter el cual nos da la salida en formato html, el **MyHtmlFormatter**:

```java
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.logging.Formatter;
import java.util.logging.Handler;
import java.util.logging.Level;
import java.util.logging.LogRecord;

// this custom formatter formats parts of a log record to a single line
class MyHtmlFormatter extends Formatter {
    // this method is called for every log records
    public String format(LogRecord rec) {
        StringBuffer buf = new StringBuffer(1000);
        buf.append("<tr>\n");

        // colorize any levels >= WARNING in red
        if (rec.getLevel().intValue() >= Level.WARNING.intValue()) {
            buf.append("\t<td style=\"color:red\">");
            buf.append("<b>");
            buf.append(rec.getLevel());
            buf.append("</b>");
        } else {
            buf.append("\t<td>");
            buf.append(rec.getLevel());
        }

        buf.append("</td>\n");
        buf.append("\t<td>");
        buf.append(calcDate(rec.getMillis()));
        buf.append("</td>\n");
        buf.append("\t<td>");
        buf.append(formatMessage(rec));
        buf.append("</td>\n");
        buf.append("</tr>\n");

        return buf.toString();
    }

    private String calcDate(long millisecs) {
        SimpleDateFormat date_format = new SimpleDateFormat("MMM dd,yyyy HH:mm");
        Date resultdate = new Date(millisecs);
        return date_format.format(resultdate);
    }

    // this method is called just after the handler using this
    // formatter is created
    public String getHead(Handler h) {
        return "<!DOCTYPE html>\n<head>\n<style>\n"
                + "table { width: 100% }\n"
                + "th { font:bold 10pt Tahoma; }\n"
                + "td { font:normal 10pt Tahoma; }\n"
                + "h1 {font:normal 11pt Tahoma;}\n"
                + "</style>\n"
                + "</head>\n"
                + "<body>\n"
                + "<h1>" + (new Date()) + "</h1>\n"
                + "<table border=\"0\" cellpadding=\"5\" cellspacing=\"3\">\n"
                + "<tr align=\"left\">\n"
                + "\t<th style=\"width:10%\">Loglevel</th>\n"
                + "\t<th style=\"width:15%\">Time</th>\n"
                + "\t<th style=\"width:75%\">Log Message</th>\n"
                + "</tr>\n";
    }

    // this method is called just after the handler using this
    // formatter is closed
    public String getTail(Handler h) {
        return "</table>\n</body>\n</html>";
    }
}
```

#### 5. Asociar Formatter a Handler

Lo proximo es asociar el **Formatter** al **Handler**, se hace de la siguiente manera:

```java
Formatter formatter = new SimpleFormatter();
fileHandler.setFormatter(formatter);
```

#### 6. Asociar Handler a Logger

Una vez tenemos configurado el **FileHandler** hace falta asociarlo al **Logger**:

```java
LOGGER.addHandler(fileHandler);
```

#### 7. Resultado

Aplicando todo lo anterior podemos crear una clase que lo que haga es coger el **Logger** global y nos lo configure como nosotros queramos, para que en el punto de entrada de nuestra aplicacion lo configuremos y ya nos olvidemos. Esto tambien nos permite crear metodos para que durante la ejecucion del programa podamos modificar la configuracion de este. Tambien se elimina el ConsoleHandler.

```java
import java.io.IOException;
import java.util.logging.ConsoleHandler;
import java.util.logging.FileHandler;
import java.util.logging.Formatter;
import java.util.logging.Handler;
import java.util.logging.Level;
import java.util.logging.Logger;
import java.util.logging.SimpleFormatter;

public class MyLogger {
    private static final Logger LOG = Logger.getLogger(Logger.GLOBAL_LOGGER_NAME);
    private static final String[] PERMITTED_LEVELS = {
        "INFO",
        "WARNING"
    };
    
    private static String filename;
    private static boolean isCustomFilename;
    
    private static FileHandler fileTxt;

    private static FileHandler fileHTML;

    private static void setFilename() throws IOException {
        filename = LOG.getLevel().getName().toLowerCase();
        LOG.info("Cambiando el nombre del archivo a " + filename);
        setupFiles();
    }

    public static void setFilename(String s) throws IOException {
        LOG.info("Cambiando el nombre del archivo a " + s);
        filename = s;
        isCustomFilename = true;
        setupFiles();
    }

    public static void setLevel(int level) throws IOException {
        try {
            LOG.warning("Cambiamos el nivel de LOG a " + PERMITTED_LEVELS[level]);
            Level newLevel = Level.parse(PERMITTED_LEVELS[level]);
            LOG.setLevel(newLevel);

            if (!isCustomFilename)
                setFilename();
        } catch (ArrayIndexOutOfBoundsException e) {
            LOG.warning("Var opcion = " + (level + 1) + " no valida");
            System.out.println("La opcion no es valida");
        }
    }

    public static void setup() throws IOException {
        // suppress the logging output to the console
        Logger rootLogger = Logger.getLogger("");
        Handler[] handlers = rootLogger.getHandlers();
        if (handlers[0] instanceof ConsoleHandler) {
            rootLogger.removeHandler(handlers[0]);
        }

        LOG.setLevel(Level.INFO);
        isCustomFilename = false;
        setFilename();      // to level name
    }

    private static void setupFiles() throws IOException {
        if (fileTxt != null){
            fileTxt.flush();
            fileTxt.close();
            LOG.removeHandler(fileTxt);
        }
        
        if (fileHTML != null){
            fileHTML.flush();
            fileHTML.close();
            LOG.removeHandler(fileHTML);
        }

        String output = "log/";

        fileTxt = new FileHandler(output + filename + ".txt", true);
        fileHTML = new FileHandler(output + filename + ".html", true);

        // create a TXT formatter
        Formatter formatterTxt = new SimpleFormatter();
        fileTxt.setFormatter(formatterTxt);
        LOG.addHandler(fileTxt);

        // create an HTML formatter
        Formatter formatterHTML = new MyHtmlFormatter();
        fileHTML.setFormatter(formatterHTML);
        LOG.addHandler(fileHTML);
    }
}
```

### Depuracion mediante Debugger



## Pruebas con JUnit Jupiter

Las pruebas unitarias con JUnit Jupiter son una forma de verificar el comportamiento individual y correcto de unidades de código a nivel de método o clase. JUnit Jupiter es un popular framework de pruebas unitarias para Java que ofrece un conjunto de anotaciones, aserciones y otras utilidades para facilitar la escritura y ejecución de pruebas automatizadas.

Las pruebas unitarias se centran en probar una "unidad" específica de código, como un método o una clase, de manera aislada y independiente del resto del sistema. El objetivo principal de las pruebas unitarias es asegurarse de que cada unidad de código funcione correctamente según sus especificaciones, lo que ayuda a garantizar un comportamiento correcto y predecible en el código.

### Importar libreria

En Visual Studio Code, cuando creamos nuestro proyecto de Java, tenemos que configurarlo para que admita tests. Se asume que el Extension Pack de Java esta instalado. Los pasos son los siguientes:

1. Pulsamos F1 y buscamos *Java Run Tests*
2. Nos saldra un matraz en la barra izquierda en el que tendremos que clickar
3. A continuacion le damos a *Enable Java Tests*
4. Seleccionamos *JUnit Jupiter*
5. Se nos descargaran las dependencias necesarias.

Ya podremos implementar nuestros tests

### Implementacion de los tests

Es buena practica crear una carpeta ***src/tests*** en la que incluyamos todos nuestros tests, para asi tenerlos separados del programa en si.

#### Preparacion de las pruebas

La estructura tipica de una prueba unitaria con JUnitJupiter sigue un patron Arrange-Act-Assert(Preparar-Actuar-Verificar), donde se establece el estado inicial, se ejecuta la unidad de código que se está probando y luego se verifican los resultados o comportamientos esperados.

#### Anotaciones

- ***@Test***: indica que un metodo es una prueba unitaria y se debe ejecutar como parte de la suite de pruebas
- ***@DisplayName***: simplemente le da nombre a la prueba
- ***@BeforeEach***: se debe ejecutar antes de cada prueba individual
- ***@AfterEach***: se debe ejecutar despues de cada prueba individual
- ***@BeforeAll***: se debe ejecutar antes de todas las pruebas individuales
- ***@AfterAll***: se debe ejecutar despues de todas las pruebas individuales

#### Assertions

Las mas basicas son:

- ***assertEquals(expected, actual)***: Compara si dos valores son iguales. Esta aserción verifica que el valor actual sea igual al valor expected. Se pueden utilizar versiones sobrecargadas de este método para especificar una tolerancia en comparaciones numéricas o proporcionar un mensaje de error personalizado.

- ***assertTrue(condition)***: Verifica si una condición es verdadera. Esta aserción verifica que la condición dada sea verdadera. Si la condición es falsa, la prueba fallará.

- ***assertFalse(condition)***: Verifica si una condición es falsa. Esta aserción verifica que la condición dada sea falsa. Si la condición es verdadera, la prueba fallará.

- ***assertThrows(ExpectedException.class, () -> {// bloque de codigo que lanza excepcion})***: Verifica que un bloque de codigo lance una excepcion en cierto escenario.

Pero tambiene existen las siguientes:

- ***assertNull(actual)***: Verifica si un valor es nulo. Esta aserción verifica que el valor actual sea nulo. Si el valor no es nulo, la prueba fallará.

- ***assertNotNull(actual)***: Verifica si un valor no es nulo. Esta aserción verifica que el valor actual no sea nulo. Si el valor es nulo, la prueba fallará.

- ***assertArrayEquals(expectedArray, actualArray)***: Compara si dos arreglos son iguales. Esta aserción verifica si los arreglos expectedArray y actualArray tienen los mismos elementos en el mismo orden.

- ***assertSame(expected, actual)***: Compara si dos objetos son idénticos. Esta aserción verifica que los objetos expected y actual sean el mismo objeto (referencia). Es similar a assertEquals para objetos, pero en lugar de verificar la igualdad de valores, verifica si son la misma instancia.

- ***assertNotSame(unexpected, actual)***: Verifica si dos objetos no son idénticos. Esta aserción verifica que los objetos unexpected y actual no sean el mismo objeto (referencia).

- ***assertArrayEquals(expectedArray, actualArray, tolerance)***: Compara si dos arreglos numéricos son iguales dentro de una tolerancia. Esta aserción se utiliza para comparar arreglos numéricos teniendo en cuenta una tolerancia para valores de coma flotante. Puedes especificar la tolerancia como el tercer parámetro.

- ***assertIterableEquals(expectedIterable, actualIterable)***: Compara si dos iterables contienen los mismos elementos en el mismo orden. Esta aserción se utiliza para comparar iterables (como listas, conjuntos) y verifica si contienen los mismos elementos en el mismo orden.

- ***assertLinesMatch(expectedLines, actualLines)***: Compara si dos listas de cadenas tienen líneas que coinciden. Esta aserción se utiliza para comparar listas de cadenas línea por línea, permitiendo coincidencias parciales y no sensibles a espacios en blanco.

#### Creacion de tests

Con toda la teoria asimilada, un ejemplo de tests para la siguiente clase:

```java
public class Algoritmos {
    public static double calculaArea(int base, int altura, String figura) throws IllegalArgumentException {
        if (figura.equals("triangulo")) {
            return (double) (base * altura) / 2;        // Modificacion: casteado a double el divisor

        } else if (figura.equals("rectangulo")) {
            return base * altura;

        } else {
            throw new IllegalArgumentException("Figura incorrecta. Solo se acepta 'triangulo' o 'rectangulo'.");
        }
    }

    public static double resuelveEcuacion(int a, int b, int c, String tipo) throws IllegalArgumentException {
        // Modificacion: se ha implementado primero la validacion de la entrada
        if (!tipo.equals("sup") && !tipo.equals("inf")) {
            throw new IllegalArgumentException("Valor inválido. Se esperaba 'sup' o 'inf'.");
        }

        float raiz = b * b - 4 * a * c;

        if (raiz < 0) {
            throw new IllegalArgumentException("Raíz negativa. La ecuación no se puede resolver.");
        }

        if (tipo.equals("sup")) {
            return (-b + Math.sqrt(raiz)) / (2 * a);

        } else {
            return (-b - Math.sqrt(raiz)) / (2 * a);
        }
    }

}
```

Podria ser el siguiente:

```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.DisplayName;

public class AlgoritmosTest {
    @Test
    @DisplayName("Testeando calcularArea tipo triangulo con inputs validos")
    void testCalcularArea_ValidInputsTriangulo() {
        int base = 3;
        int altura = 5;
        String tipo = "triangulo";

        double expected = 7.5;
        double result = Algoritmos.calculaArea(base, altura, tipo);

        Assertions.assertEquals(expected, result);
    }

    @Test
    @DisplayName("Testeando calcularArea tipo rectangulo con inputs validos")
    void testCalcularArea_ValidInputsRectangulo() {
        int base = 3;
        int altura = 5;
        String tipo = "rectangulo";
        double expected = 15;

        double result = Algoritmos.calculaArea(base, altura, tipo);

        Assertions.assertEquals(expected, result);
    }

    @Test
    @DisplayName("Testeando calcularArea con figura invalida")
    void testCalcularArea_InvalidInputs() {
        int base = 1;
        int altura = 1;
        String figura = "circulo";

        Assertions.assertThrows(IllegalArgumentException.class, () -> Algoritmos.calculaArea(base, altura, figura));
    }

    @Test
    @DisplayName("Testeando resuelveEcuacion con inputs validos (tipo: sup)")
    public void testResuelveEcuacion_ValidInputSup() {
        int a = 1;
        int b = 4;
        int c = 3;
        String tipo = "sup";
        double expected = -1.0;

        double result = Algoritmos.resuelveEcuacion(a, b, c, tipo);

        Assertions.assertEquals(expected, result);
    }

    @Test
    @DisplayName("Testeando resuelveEcuacion con inputs validos (tipo: inf)")
    public void testResuelveEcuacion_ValidInputInf() {
        int a = 1;
        int b = 4;
        int c = 3;
        String tipo = "inf";
        double expected = -3.0;

        double result = Algoritmos.resuelveEcuacion(a, b, c, tipo);

        Assertions.assertEquals(expected, result);
    }

    @Test
    @DisplayName("Testeando resuelveEcuacion con tipo invalido")
    public void testResuelveEcuacion_InvalidTipo() {
        int a = 1;
        int b = 4;
        int c = 3;
        String tipo = "invalid";
        String expectedMessage = "Valor inválido. Se esperaba 'sup' o 'inf'.";

        IllegalArgumentException excepcion = Assertions.assertThrows(IllegalArgumentException.class, () -> {
            Algoritmos.resuelveEcuacion(a, b, c, tipo);
        });

        String mensajeError = excepcion.getMessage();
        Assertions.assertEquals(expectedMessage, mensajeError);
    }

    @Test
    @DisplayName("Testeando resuelveEcuacion con raiz negativa sup")
    public void testResuelveEcuacion_NegativeRaizSup() {
        int a = 1;
        int b = 1;
        int c = 1;
        String tipo = "sup";
        String expectedMessage = "Raíz negativa. La ecuación no se puede resolver.";

        IllegalArgumentException excepcion = Assertions.assertThrows(IllegalArgumentException.class, () ->
            Algoritmos.resuelveEcuacion(a, b, c, tipo)
        );

        String mensajeError = excepcion.getMessage();
        Assertions.assertEquals(expectedMessage, mensajeError);
    }

    @Test
    @DisplayName("Testeando resuelveEcuacion con raiz negativa inf")
    public void testResuelveEcuacion_NegativeRaizInf() {
        int a = 1;
        int b = 1;
        int c = 1;
        String tipo = "inf";
        String expectedMessage = "Raíz negativa. La ecuación no se puede resolver.";

        IllegalArgumentException excepcion = Assertions.assertThrows(IllegalArgumentException.class, () ->
            Algoritmos.resuelveEcuacion(a, b, c, tipo)
        );

        String mensajeError = excepcion.getMessage();
        Assertions.assertEquals(expectedMessage, mensajeError);
    }
}
```
