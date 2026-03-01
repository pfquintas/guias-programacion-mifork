<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

En C, al carecer de un mecanismo de excepciones, el control de errores debe gestionarse mediante convenciones explícitas que no interrumpan el flujo del programa de forma automática. Para el caso de una función que calcula la raíz cuadrada de un número, y considerando que el error debe ser notificado al llamante (quien debe estar informado pero desde fuera de la función), se pueden emplear, entre otras, las siguientes dos estrategias de diseño ampliamente utilizadas.

- Opción 1: devolver un código de error y pasar el resultado por referencia.

Esta es una de las estrategias más comunes en C. Consiste en que la función no devuelva directamente el resultado, sino un valor entero que actúa como indicador de éxito o fracaso (por ejemplo, 0 para éxito y -1 para error). El resultado de la operación, si es exitosa, se almacena en una dirección de memoria que el llamante proporciona a través de un puntero. De esta forma, la función principal puede consultar primero el código de retorno para saber si la operación fue exitosa y, en caso afirmativo, utilizar el valor calculado.

#include <stdio.h>
#include <math.h>   // Para la función sqrt

// Versión que retorna un código de error y usa un puntero para el resultado
int raiz(double numero, double *resultado) {
    if (numero < 0.0) {
        return -1;   // Código de error: número negativo
    }
    *resultado = sqrt(numero);
    return 0;        // Código de éxito
}

int main() {
    double valor = -9.0;
    double res;
    int error;

    error = raiz(valor, &res);
    if (error != 0) {
        printf("Error: No se puede calcular la raiz de un numero negativo.\n");
    } else {
        printf("La raiz cuadrada de %.2f es %.2f\n", valor, res);
    }

    return 0;
}

En este diseño, el flujo de control permanece completamente en manos del programador. La función main debe verificar explícitamente el valor de retorno de raiz antes de usar el contenido de la variable res. Esto hace que el manejo de errores sea visible y obligatorio, aunque también puede ser ignorado si el programador no realiza la comprobación, lo cual es una desventaja potencial.

- Opción 2: Utilizar una variable global para el error y devolver un valor por defecto.

Otra alternativa, menos segura pero también empleada en ciertos contextos, es el uso de una variable global (por ejemplo, errno en la biblioteca estándar de C). En esta estrategia, la función devuelve un valor "especial" o "centinela" que indica un posible error (como -1.0 en una función que normalmente solo retorna positivos). Además, se modifica una variable global para almacenar un código específico que detalla la naturaleza del error. El llamante debe comprobar esta variable global inmediatamente después de la llamada a la función para determinar si el valor devuelto es válido o es consecuencia de un error.

#include <stdio.h>
#include <math.h>

// Variable global para indicar el estado del error
int error_raiz = 0;   // 0 significa "sin error"

// Versión que usa un valor centinela y una variable global de error
double raiz(double numero) {
    if (numero < 0.0) {
        error_raiz = -1;   // Se actualiza la variable global
        return -1.0;       // Valor centinela que no es una raíz válida
    }
    error_raiz = 0;         // Se restablece el indicador de éxito
    return sqrt(numero);
}

int main() {
    double valor = -9.0;
    double res;

    res = raiz(valor);
    if (error_raiz != 0) {
        printf("Error: No se puede calcular la raiz de un numero negativo.\n");
    } else {
        printf("La raiz cuadrada de %.2f es %.2f\n", valor, res);
    }

    return 0;
}

Este enfoque tiene la ventaja de que la firma de la función es más simple, ya que retorna directamente el tipo esperado. Sin embargo, presenta problemas conocidos: no es seguro en entornos con múltiples hilos de ejecución (la variable global es compartida), y el programador podría olvidarse de comprobar error_raiz antes de usar el resultado, asumiendo erróneamente que el valor devuelto es correcto. La biblioteca estándar de C utiliza un patrón similar con la variable errno para algunas funciones matemáticas y de entrada/salida.


## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una excepción es un evento inesperado que ocurre durante la ejecución de un programa y que interrumpe el flujo normal de las instrucciones. En lenguajes como Java, una excepción es un objeto que encapsula información sobre esa condición anómala, como el tipo de error, el punto del programa donde se originó y un mensaje descriptivo. Cuando se produce una situación excepcional (por ejemplo, división por cero, archivo no encontrado), el método donde ocurre crea ese objeto y lo lanza al sistema, que buscará un manejador adecuado para procesarlo.

El programador utiliza las excepciones con un objetivo principal: separar la lógica del algoritmo principal de la lógica de tratamiento de errores. Al implementar una función, en lugar de devolver códigos de error que el llamante debe verificar explícitamente (como se hacía en C), se puede lanzar una excepción cuando algo falla. Esto permite que el código que realiza el trabajo normal (calcular una raíz, leer un archivo) sea más limpio y legible, ya que no está entremezclado con comprobaciones constantes de valores de retorno. La función simplemente se concentra en su tarea y, si no puede completarla por una condición extraordinaria, lanza una excepción para notificarlo.

Al llamar a una función que puede lanzar excepciones, el programador utiliza el bloque try-catch para rodear la llamada y manejar los posibles fallos de manera centralizada. El objetivo aquí es construir programas más robustos y tolerantes a fallos. En lugar de que el programa termine abruptamente cuando ocurre un error (lo que sucedería si una excepción no se captura), se puede capturar la excepción, tomar una acción correctiva (como pedir un nuevo valor al usuario, liberar recursos o registrar el error en un archivo de log) y, si es posible, continuar con la ejecución. Esto proporciona una forma estructurada y controlada de responder a condiciones de error, mejorando la fiabilidad del software.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

A continuación se presenta la implementación del ejemplo de la raíz cuadrada en Java, encapsulando el método en una clase Calculadora y mostrando cómo el método main controla el error desde fuera mediante el mecanismo de excepciones.

- Paso 1: Definir una excepción personalizada (opcional pero recomendable).

Para que el error sea semánticamente rico y específico de la situación, se puede crear una clase de excepción propia que herede de Exception (excepción verificada) o de RuntimeException (excepción no verificada). En este caso, se opta por una excepción verificada para obligar al llamante a tratarla.

// Excepcion personalizada para errores de dominio en la raiz cuadrada
class DominioException extends Exception {
    public DominioException(String mensaje) {
        super(mensaje);
    }
}

- Paso 2: Implementar la clase Calculadora con el método raíz.

El método raiz recibe un número double, verifica si es negativo y, en tal caso, lanza la excepción personalizada. Si el número es válido, delega el cálculo en Math.sqrt y devuelve el resultado.

public class Calculadora {

    /**
     * Calcula la raiz cuadrada de un número.
     * @param numero El número del cual se desea la raiz (debe ser >= 0)
     * @return El resultado de la raiz cuadrada
     * @throws DominioException Si el número es negativo
     */
    public double raiz(double numero) throws DominioException {
        if (numero < 0) {
            throw new DominioException("No se puede calcular la raiz de un numero negativo: " + numero);
        }
        return Math.sqrt(numero);
    }
}

- Paso 3: Llamar al método desde el main y controlar la excepción desde fuera.

En el método main se crea una instancia de Calculadora y se invoca raiz dentro de un bloque try-catch. Si ocurre una DominioException, se captura y se informa al usuario; si no, se muestra el resultado con normalidad.

public class Principal {
    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        double valor = -9.0;  // Valor de prueba que provocará la excepción

        try {
            double resultado = calc.raiz(valor);
            System.out.println("La raiz cuadrada de " + valor + " es " + resultado);
        } catch (DominioException e) {
            // Se captura la excepción lanzada desde el método raiz
            System.out.println("Error: " + e.getMessage());
            // Opcionalmente se podría registrar el error o tomar otra acción
        }

        // El programa continúa su ejecución normalmente después del catch
        System.out.println("Programa finalizado.");
    }
}

Explicación del flujo de control:

1. Declaración de la excepción: El método raiz declara en su firma throws DominioException. Esto informa a quien lo invoque que es posible que se produzca ese tipo de error.
2. Lanzamiento de la excepción: Cuando se llama a raiz con un argumento negativo, la instrucción throw new DominioException(...) crea un objeto excepción y lo lanza. Esto interrumpe inmediatamente la ejecución del método raiz.
3. Captura y manejo: El método main tiene un bloque try que envuelve la llamada problemática. El flujo salta al bloque catch correspondiente al tipo de excepción lanzada. Allí se ejecuta el código de manejo (en este caso, imprimir el mensaje de error).
4. Continuación del programa: Después de ejecutar el bloque catch, el programa continúa con la siguiente línea después del bloque try-catch. El error ha sido controlado externamente sin necesidad de que raiz devuelva códigos especiales ni de que main verifique variables globales.
5. Esta implementación muestra cómo Java proporciona un flujo separado para el manejo de errores, haciendo el código más limpio y obligando al programador a considerar las situaciones excepcionales.


## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

Lanzar una excepción es el acto de crear un objeto de tipo Throwable (o una de sus subclases) y entregarlo al entorno de ejecución de Java, interrumpiendo inmediatamente el flujo normal del método donde ocurre. En el ejemplo de la calculadora, cuando el método raiz recibe un número negativo, ejecuta la instrucción throw new DominioException("..."). En ese momento, la ejecución dentro de raiz se detiene: ninguna línea posterior a ese throw se ejecutará. El objeto excepción contiene información sobre el error, como el mensaje y la traza de la pila, y comienza a ser devuelto "hacia arriba" por la máquina virtual.

Controlar o capturar una excepción es el proceso mediante el cual una porción de código (el bloque catch) intercepta la excepción lanzada y ejecuta una rutina alternativa para manejar el error. En el ejemplo, el método main contiene un bloque try que envuelve la llamada a calc.raiz(valor). Inmediatamente después, hay un bloque catch (DominioException e). Cuando se lanza DominioException desde raiz, la JVM busca en la pila de llamadas un manejador que coincida con ese tipo. Al encontrarlo en main, el control pasa a ese bloque catch, donde se ejecuta el código que imprime el mensaje de error. Capturar la excepción permite que el programa se recupere y continúe su ejecución, en lugar de terminar abruptamente.

La propagación de una excepción es el mecanismo por el cual la excepción viaja hacia atrás a través de la pila de llamadas, desde el método que la lanzó hasta que encuentra un bloque catch que la maneje, o hasta llegar al método main y, si no se captura, terminar el programa. Supóngase una variante del ejemplo donde el método main no capturara la excepción, sino que llamara a otro método intermedio, y este a raiz. Al lanzarse la excepción en raiz, esta se propagaría al método que invocó a raiz. Si ese método tampoco la captura, la excepción se propaga a main. Durante esta propagación, cada método de la pila se va abortando secuencialmente.

En cuanto al comportamiento de las funciones en la pila de llamadas, cuando una excepción se propaga a través de ellas, no se reanudan en ningún momento. Una vez que una función lanza una excepción (o recibe una propagada desde una función a la que llamó), su ejecución queda terminada de forma definitiva para ese flujo. El control no regresa nunca al punto donde se produjo el lanzamiento dentro de esa función. Por ejemplo, si raiz lanza la excepción, cualquier línea de código en raiz posterior al throw jamás se ejecutará. Si el método que llamó a raiz (por ejemplo, un método intermedio()) no captura la excepción y esta se propaga, intermedio() también termina inmediatamente después de la línea que invocó a raiz, sin ejecutar el resto de su código. La pila se va "desenrollando" (stack unwinding), descartando los marcos de pila de los métodos afectados, hasta que se encuentra un catch o se llega al final.


## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

La principal ventaja de la propagación natural de excepciones frente a los mecanismos de C radica en la automatización y centralización del manejo de errores. En C, como se vio en el ejemplo de la raíz cuadrada, cada función debe verificar explícitamente los códigos de retorno de las funciones que llama y, si no puede manejar el error localmente, debe devolver a su vez un código de error a su propia llamante. Esto implica que el programador debe escribir código de comprobación en cada nivel de la jerarquía de llamadas, lo que resulta tedioso, propenso a errores y ensombrece la lógica principal del algoritmo. Con las excepciones de Java, cuando una función lanza una excepción, esta asciende automáticamente por la pila sin necesidad de código adicional en los niveles intermedios. Estos niveles simplemente ignoran la situación excepcional si no están preparados para manejarla, y la excepción sigue su camino hasta llegar a un bloque catch diseñado para ello. Esta automatización libera al programador de la carga de tener que "retransmitir" manualmente el error a través de múltiples capas de funciones.

Una segunda ventaja significativa es la separación nítida entre el código normal y el código de tratamiento de errores, que se vuelve más patente gracias a la propagación. En C, el código de error y el código de éxito suelen estar entremezclados, ya que cada llamada a una función debe ir seguida de una comprobación de su valor de retorno. Esto puede llevar a estructuras de código profundamente anidadas y difíciles de seguir. En Java, una función que actúa como mero intermediario (por ejemplo, una que solo delega en otra) no necesita contener ninguna lógica de error. Simplemente declara en su firma throws la excepción que puede propagar. El flujo normal de la función se escribe de manera limpia, asumiendo que todo funcionará correctamente. Si ocurre un error, la propagación se encarga de llevar la excepción a un lugar donde se pueda manejar de forma coherente, típicamente en un nivel superior de la aplicación. Esto permite que cada función se concentre en su responsabilidad principal, mejorando la modularidad y la legibilidad del código.

Finalmente, la propagación natural garantiza una mayor robustez al evitar que los errores pasen desapercibidos. En C, es posible que un programador olvide comprobar el código de retorno de una función, lo que podría llevar a que el programa continúe su ejecución con datos inconsistentes o inválidos, provocando fallos impredecibles más adelante. Las excepciones verificadas de Java obligan a quien llama a una función a ser consciente de las excepciones que esta puede lanzar, ya sea capturándolas o declarando que las propaga. Si no se hace ninguna de las dos cosas, el código no compila. Incluso con las excepciones no verificadas, aunque el compilador no fuerce su manejo, la propagación asegura que, si ocurren, interrumpirán el programa en el punto del error si no se capturan, en lugar de permitir que la ejecución continúe silenciosamente con un estado erróneo. Este comportamiento por defecto hace que los errores de programación sean más evidentes y fáciles de diagnosticar durante el desarrollo.


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

Sí, en la orientación a objetos, y particularmente en Java, las excepciones son objetos. Esto significa que una excepción es una instancia de una clase, la cual debe ser una subclase de la clase Throwable (directamente o a través de Exception o RuntimeException). Al ser objetos, las excepciones pueden contener tanto datos (atributos) como comportamientos (métodos). En su forma más básica, una excepción ya incorpora, gracias a su clase base, información como un mensaje descriptivo y la traza de la pila (stack trace). Sin embargo, al ser objetos, esta capacidad de almacenar información se puede extender enormemente.

La principal ventaja de que las excepciones sean objetos en términos de encapsulación es que permiten empaquetar toda la información relevante sobre un error en una única entidad cohesiva. Un objeto excepción puede encapsular no solo un mensaje de texto, sino también códigos de error específicos, el estado de ciertas variables en el momento del fallo, o incluso referencias a otros objetos relacionados con el error. Esta información viaja con la excepción a medida que se propaga por la pila de llamadas. Quien captura la excepción (el bloque catch) puede entonces utilizar los métodos públicos de ese objeto para obtener información detallada y estructurada sobre lo que sucedió, sin necesidad de acceder a variables globales o depender de convenciones frágiles. Esto hace que el manejo de errores sea más robusto y la comunicación sobre el fallo sea más rica y precisa.

Además, el hecho de que las excepciones sean objetos abre la puerta a la creación de excepciones personalizadas. El programador puede definir sus propias clases de excepción que hereden de Exception (para verificadas) o de RuntimeException (para no verificadas). Esto es una práctica común y muy recomendable porque permite crear una jerarquía de excepciones semánticamente significativa para el dominio del problema. Por ejemplo, en una aplicación bancaria se podrían crear SaldoInsuficienteException, CuentaBloqueadaException o LimiteDiarioExcedidoException. Cada una de estas clases personalizadas puede incluir atributos específicos, como el saldo actual o el límite diario, encapsulando así toda la información de contexto necesaria para que el manejador de la excepción pueda tomar una decisión informada (por ejemplo, sugerir al usuario un retiro menor). Esto eleva el manejo de errores de un simple control de códigos a un modelo de dominio rico y expresivo.


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

La información esencial que lleva cualquier objeto excepción en Java, y que constituye una ventaja crucial de encapsulación frente a los mecanismos de C, es la traza de la pila (stack trace). Esta traza es un conjunto de datos que detalla, paso a paso, la secuencia de llamadas a métodos que conducían al punto exacto donde se lanzó la excepción. Cuando se crea un objeto excepción, automáticamente captura una instantánea del estado de la pila de ejecución en ese momento, incluyendo los nombres de las clases, los métodos y los números de línea de cada llamada anidada. Al llegar al manejador (el bloque catch), esta información está disponible a través de métodos como printStackTrace() o mediante acceso programático a los elementos de la traza.

Esta capacidad de proporcionar la traza de la pila resuelve una de las limitaciones más significativas del manejo de errores en C. En los ejemplos anteriores en C, si una función raiz devolvía un código de error, el llamante solo recibía un valor numérico (por ejemplo, -1) y quizás una variable global como errno que indicaba la naturaleza del error. Sin embargo, no había manera automática de saber cuál era la secuencia exacta de llamadas que llevó a ese error. Si el programa era grande y la función era invocada desde múltiples lugares, el programador solo veía el código de error, pero no podía determinar de forma inmediata desde qué punto del código se había originado la llamada fallida. Con la traza de la pila de Java, el desarrollador o el sistema de logging puede reconstruir instantáneamente el camino de ejecución, lo que facilita enormemente la depuración y el diagnóstico de problemas en producción.

Además de la traza de la pila, todo objeto excepción encapsula un mensaje descriptivo, que se establece normalmente a través de su constructor. Este mensaje es una cadena de texto que explica la razón del error, como "No se puede calcular la raiz de un numero negativo: -9.0". A diferencia de un código numérico en C, que requiere una tabla de interpretación o un conjunto de #define, el mensaje de la excepción es autodescriptivo y puede ser mostrado directamente al usuario o registrado en un archivo de log sin necesidad de traducción adicional. Junto con la traza, esta información forma un paquete completo y autocontenido que viaja con la excepción, demostrando claramente las ventajas de la encapsulación: toda la información relevante sobre el error está unificada en un solo objeto, accesible desde cualquier punto donde se capture la excepción, sin depender de convenciones externas, variables globales o comprobaciones dispersas.


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

En Java, un bloque try puede ir acompañado de múltiples bloques catch. Esta es una característica fundamental del manejo de excepciones, ya que permite al programador preparar respuestas diferenciadas para distintos tipos de errores que puedan ocurrir dentro del mismo bloque de código vigilado. Cada bloque catch se especializa en un tipo de excepción (o una familia de ellas, gracias al polimorfismo), y se colocan de forma consecutiva inmediatamente después del bloque try. La sintaxis exige que estos bloques se ordenen de la excepción más específica a la más general, ya que de lo contrario el código no compilaría; por ejemplo, no se puede colocar un catch (Exception e) antes de un catch (NullPointerException e), porque el primero, al ser más general, atraparía todas las excepciones y el segundo nunca tendría oportunidad de ejecutarse.

A pesar de que haya múltiples bloques catch definidos, solamente se ejecuta uno de ellos cuando se produce una excepción. El sistema de ejecución de Java evalúa los bloques catch en el orden en que aparecen en el código y busca el primero cuyo tipo de parámetro sea compatible con el tipo de la excepción lanzada. Una vez que encuentra una coincidencia, ejecuta el código contenido en ese bloque catch específico y, al terminar, el control salta fuera de toda la estructura try-catch, continuando con la siguiente instrucción después del último bloque catch (o del bloque finally, si lo hubiera). Los demás bloques catch definidos para ese mismo try son ignorados por completo, ya que la excepción ya ha sido manejada.

Este comportamiento es análogo a una estructura de decisión múltiple, pero basada en tipos de excepción en lugar de valores. Por ejemplo, supóngase un bloque try que realiza operaciones de archivo y de cálculo. Podría tener un primer catch para FileNotFoundException que muestre un mensaje de "Archivo no encontrado", y un segundo catch para ArithmeticException que indique "Error en cálculo". Si durante la ejecución del try se lanza una FileNotFoundException, el primer catch la capturará y se ejecutará, mostrando el mensaje correspondiente. El segundo catch no se ejecutará. Si, por el contrario, ocurre un error aritmético, el primer catch no coincidirá con el tipo de la excepción, por lo que se evaluará el segundo, que sí coincidirá y se ejecutará. En cualquier caso, solo un bloque catch entra en acción por cada excepción lanzada.


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

Para garantizar la ejecución de código de limpieza o liberación de recursos independientemente de si se produce o no una excepción, Java proporciona el bloque finally. Este bloque se coloca después del bloque try (y opcionalmente después de los bloques catch) y tiene la característica fundamental de que su código se ejecuta siempre, en todas las circunstancias: tanto si el bloque try se completa con normalidad, como si se lanza una excepción que es capturada por un catch, como incluso si se lanza una excepción que no es capturada y provoca la propagación. La única manera de que un bloque finally no se ejecute es si la JVM se detiene antes (por ejemplo, con una llamada a System.exit()) o si se produce un error catastrófico.

Ejemplo con try-catch-finally

En este ejemplo, se simula la apertura de un recurso (como un archivo) y se garantiza su cierre en el bloque finally, independientemente de que la operación tenga éxito o falle.

public class EjemploFinallyConCatch {
    public static void main(String[] args) {
        RecursoSimulado recurso = null;

        try {
            recurso = new RecursoSimulado("datos.txt");
            recurso.abrir();
            
            // Simulación de una operación que puede fallar
            double valor = -9.0;
            if (valor < 0) {
                throw new IllegalArgumentException("Valor negativo no permitido");
            }
            
            System.out.println("Operación completada con éxito.");
            
        } catch (IllegalArgumentException e) {
            System.out.println("Error capturado: " + e.getMessage());
            // Aquí se podría registrar el error o tomar otras medidas
            
        } finally {
            // Este bloque se ejecuta SIEMPRE, haya o no excepción, haya o no catch
            System.out.println("Ejecutando bloque finally: limpieza de recursos.");
            if (recurso != null) {
                recurso.cerrar();
            }
        }
        
        System.out.println("Programa continúa después del try-catch-finally.");
    }
}

// Clase auxiliar para simular un recurso
class RecursoSimulado {
    private String nombre;
    
    public RecursoSimulado(String nombre) {
        this.nombre = nombre;
    }
    
    public void abrir() {
        System.out.println("Recurso '" + nombre + "' abierto.");
    }
    
    public void cerrar() {
        System.out.println("Recurso '" + nombre + "' cerrado.");
    }
}

En este código, tanto si se lanza la excepción como si no, el bloque finally se ejecutará y se llamará a recurso.cerrar(), liberando así el recurso. Primero se ejecuta el catch correspondiente (si lo hay) y después el finally. Si no hubiera excepción, se ejecuta el try completamente y luego el finally.

Ejemplo con try-finally (sin catch)

En ocasiones, puede interesar no capturar la excepción en el método actual, sino dejar que se propague, pero aun así se necesita liberar recursos antes de que la excepción siga su camino. En estos casos, se utiliza un bloque try-finally sin catch.

public class EjemploTryFinally {
    
    public void metodoConRecurso() throws IllegalArgumentException {
        RecursoSimulado recurso = null;
        
        try {
            recurso = new RecursoSimulado("config.xml");
            recurso.abrir();
            
            // Operación que lanza una excepción
            double valor = -5.0;
            if (valor < 0) {
                throw new IllegalArgumentException("Error: valor negativo en método interno");
            }
            
            System.out.println("Esta línea no se ejecutará si hay excepción.");
            
        } finally {
            // Este bloque se ejecuta incluso antes de que la excepción se propague
            System.out.println("Liberando recurso en finally antes de propagar la excepción.");
            if (recurso != null) {
                recurso.cerrar();
            }
        }
        
        // Esta línea solo se ejecuta si NO hubo excepción
        System.out.println("Fin del método (sin excepción).");
    }
    
    public static void main(String[] args) {
        EjemploTryFinally ejemplo = new EjemploTryFinally();
        
        try {
            ejemplo.metodoConRecurso();
        } catch (IllegalArgumentException e) {
            System.out.println("Capturada excepción propagada: " + e.getMessage());
        }
    }
}

En este segundo ejemplo, el método metodoConRecurso no captura la excepción, sino que la deja propagar (declara throws). Sin embargo, el bloque finally se ejecuta inmediatamente después de producirse la excepción, cerrando el recurso, y luego la excepción continúa propagándose hacia el main. El orden de ejecución cuando ocurre la excepción es:

1. Se lanza la excepción dentro del try.
2. Se ejecuta el bloque finally (liberando el recurso).
3. La excepción se propaga al método llamante (main), donde es capturada.
4. Se ejecuta el catch en main.

Este patrón es muy común en código que maneja recursos, como archivos, conexiones de red o bases de datos, garantizando que no queden recursos abiertos aunque se produzcan errores y la excepción deba ser manejada en un nivel superior.


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

Sí, en Java es perfectamente válido tener un bloque finally sin un bloque catch asociado. La sintaxis permite dos combinaciones principales: try-catch (con uno o varios catch) y try-finally. También es posible usar try-catch-finally con todas las partes. El bloque try-finally sin catch es especialmente útil cuando se desea garantizar la liberación de recursos o la ejecución de código de limpieza, pero no se quiere manejar la excepción en el método actual, sino que se prefiere que esta se propague hacia el llamante después de haber realizado las tareas de limpieza necesarias.

En cuanto a su ejecución, el bloque finally se ejecuta siempre, independientemente de que ocurra o no una excepción dentro del bloque try. Esta es su característica definitoria y la razón principal de su existencia. Si el bloque try se completa sin lanzar ninguna excepción, el bloque finally se ejecuta inmediatamente después, antes de que el control pase a la siguiente instrucción fuera de la estructura try-finally. Si se lanza una excepción dentro del try, el bloque finally se ejecuta igualmente, justo después de que la excepción haya sido lanzada pero antes de que la excepción se propague al método llamante. Esto garantiza que el código de limpieza se ejecute en cualquier circunstancia, protegiendo al programa de fugas de recursos.

La pregunta sobre el return dentro del bloque try es especialmente interesante porque muestra la prioridad absoluta que tiene el bloque finally. Incluso si existe una instrucción return dentro del bloque try, el bloque finally se ejecuta antes de que el valor sea realmente devuelto y el método termine. Es decir, cuando se encuentra un return en el try, el valor de retorno se calcula y se "prepara" para ser devuelto, pero la ejecución se interrumpe temporalmente para ejecutar todo el bloque finally. Una vez finalizado el finally, se completa la operación de retorno. Si además el bloque finally contiene su propia instrucción return (algo posible pero generalmente desaconsejado por confuso), esta sobrescribiría el valor de retorno original. Este comportamiento demuestra que el bloque finally tiene una prioridad ejecutiva máxima dentro de la estructura de manejo de excepciones.


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

En Java, las excepciones se clasifican en dos grandes categorías: controladas (checked exceptions) y no controladas (unchecked exceptions). La diferencia fundamental entre ambas radica en el momento en que el compilador verifica su manejo. Las excepciones controladas son aquellas que el compilador obliga a manejar explícitamente: si un método puede lanzar una excepción de este tipo, debe declararlo en su firma con throws o capturarla con un bloque try-catch. De no hacerlo, el código no compila. Por otro lado, las excepciones no controladas no están sujetas a esta verificación en tiempo de compilación; el compilador no fuerza al programador a declararlas o capturarlas, aunque opcionalmente se puede hacer.

La clase RuntimeException juega un papel central en esta clasificación, ya que actúa como la clase base para todas las excepciones no controladas. Cualquier excepción que herede directa o indirectamente de RuntimeException se considera no controlada. Por el contrario, las excepciones que heredan de Exception pero no de RuntimeException son controladas. Esta distinción refleja una filosofía de diseño: las excepciones controladas se utilizan para condiciones anómalas que son previsibles y de las cuales un programa bien estructurado debería poder recuperarse (por ejemplo, errores de entrada/salida), mientras que las no controladas suelen indicar errores de programación o condiciones que escapan al control del programa y que normalmente no deberían ser capturadas para intentar una recuperación, sino para registrar el fallo y terminar.

- Ejemplos típicos de excepciones controladas (checked):

1. IOException: Se lanza cuando ocurre un error en operaciones de entrada/salida, como al intentar leer un archivo que no existe o al producirse un fallo en una conexión de red. Es una de las excepciones controladas más comunes.

2. SQLException: Proporciona información sobre errores ocurridos al acceder a una base de datos, como problemas de conexión o errores en sentencias SQL.

3. ClassNotFoundException: Se produce cuando se intenta cargar una clase mediante su nombre en tiempo de ejecución (por ejemplo, con Class.forName()) y la clase no se encuentra en el classpath.

4. FileNotFoundException: Una subclase de IOException que se lanza específicamente cuando no se puede encontrar el archivo que se intenta abrir.

- Ejemplos típicos de excepciones no controladas (unchecked):

1. NullPointerException: Ocurre al intentar acceder a un método o atributo de una referencia que no apunta a ningún objeto (es decir, que vale null). Generalmente indica un error de programación.

2. ArrayIndexOutOfBoundsException: Se lanza al intentar acceder a una posición de un array que está fuera de sus límites (índice negativo o mayor o igual que la longitud del array).

3. IllegalArgumentException: Se utiliza para indicar que un método ha recibido un argumento inapropiado o inválido según su contrato. Es común lanzarla en validaciones de parámetros.

4. ArithmeticException: Se produce cuando ocurre una condición aritmética excepcional, como la división por cero.

- Lista de situaciones donde se suele preferir una excepción controlada:

1. Operaciones de entrada/salida con recursos externos: Al trabajar con archivos, conexiones de red o bases de datos, donde los fallos son esperables por causas ajenas al programa (disco lleno, archivo borrado, servidor caído). Se obliga al programador a considerar estos fallos y tomar medidas como reintentar la operación o informar al usuario.

2. Problemas de configuración o de entorno: Cuando el programa depende de la presencia de ciertos archivos de configuración, variables de entorno o bibliotecas, y su ausencia impide que la aplicación funcione correctamente.

3. Fallo en la carga dinámica de componentes: En sistemas que utilizan reflexión o cargan clases dinámicamente, es preferible usar excepciones controladas para que quien invoca sea consciente de que la clase solicitada podría no estar disponible.

4. Validación de entrada en constructores de objetos complejos: Cuando se construye un objeto y los parámetros proporcionados no cumplen las condiciones necesarias para crear una instancia válida, se puede lanzar una excepción controlada para que el llamante decida cómo manejar el fallo de construcción.

- Lista de situaciones donde se suele preferir una excepción no controlada:

1. Errores de programación: Condiciones que, en un programa correctamente escrito, no deberían ocurrir, como acceder a un array con un índice fuera de rango o invocar métodos sobre una referencia nula. Indican que hay que corregir el código.

2. Precondiciones de métodos (validación de parámetros): Cuando un método recibe un argumento que viola su contrato (por ejemplo, un null donde no se permite), se suele lanzar IllegalArgumentException o NullPointerException. Son errores del llamante.

3. Operaciones matemáticas inválidas: Como la división por cero o el cálculo de la raíz cuadrada de un número negativo en una función que no admite complejos. Reflejan un uso incorrecto de la función.

4. Condiciones de las que no se puede recuperar localmente: Situaciones en las que, aunque ocurra el error, no tiene sentido que el método actual intente una recuperación, y la única opción razonable es que la excepción ascienda hasta un manejador general que registre el fallo y termine la operación.


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

La cláusula throws es una declaración que se incluye en la firma de un método en Java para indicar que ese método puede lanzar uno o más tipos de excepciones durante su ejecución. Su sintaxis consiste en la palabra reservada throws seguida de una lista separada por comas de los tipos de excepción que el método podría propagar hacia su llamante. Por ejemplo: public void leerArchivo(String nombre) throws IOException, FileNotFoundException. Esta declaración no ejecuta ninguna acción por sí misma, sino que actúa como una especificación formal del comportamiento del método, formando parte de su contrato con quienes lo invocan.

El propósito fundamental de throws es informar al compilador y a los programadores que usan el método sobre las excepciones controladas que deben ser consideradas. Cuando un método incluye throws para una excepción controlada, está delegando la responsabilidad de manejar esa excepción en el método que lo llama. En esencia, el método está diciendo: "Yo puedo encontrarme con esta situación excepcional, pero no sé cómo manejarla apropiadamente en mi contexto; por tanto, prefiero que quien me ha invocado decida qué hacer con ella". Esta declaración es obligatoria para las excepciones controladas que el método no captura internamente, ya que el compilador exige que toda excepción controlada posible sea declarada o manejada.

La cláusula throws constituye una alternativa directa a capturar una excepción controlada mediante un bloque try-catch. Ante una excepción controlada que puede ocurrir dentro de un método, el programador tiene dos opciones: asumir la responsabilidad de manejar el error localmente (capturándolo con try-catch) o declinar esa responsabilidad y pasarla al método llamante (declarando la excepción con throws). Ambas opciones satisfacen la exigencia del compilador de que la excepción controlada sea atendida. La elección entre una u otra depende del contexto y de dónde tenga más sentido tomar una decisión sobre el error. Por ejemplo, un método de bajo nivel que lee un byte de un archivo no debería decidir qué hacer si el archivo no existe; esa decisión corresponde al método de más alto nivel que sabe qué archivo se espera y qué acción tomar ante su ausencia. Por ello, el método de bajo nivel declarará throws IOException, permitiendo que la excepción se propague hacia arriba. En cambio, un método que sabe exactamente cómo responder ante un error específico (por ejemplo, usar un valor por defecto) capturará la excepción y aplicará esa lógica.


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

A continuación se presenta un ejemplo completo en Java que ilustra una función que abre un fichero, declara mediante throws que no manejará la posible excepción de archivo no encontrado, pero utiliza un bloque finally para garantizar la liberación del recurso, permitiendo que la excepción se propague hacia el método llamante.

import java.io.FileReader;
import java.io.IOException;

public class LectorArchivo {

    /**
     * Método que intenta abrir y leer un archivo, pero no maneja las excepciones de E/S.
     * Declara throws IOException para que la excepción se propague al llamante.
     * Sin embargo, utiliza un bloque finally para asegurar el cierre del recurso.
     *
     * @param nombreArchivo El nombre del archivo a abrir
     * @return El contenido del archivo como String (simplificado)
     * @throws IOException Si ocurre un error de entrada/salida (incluyendo archivo no encontrado)
     */
    public String leerContenidoArchivo(String nombreArchivo) throws IOException {
        FileReader lector = null;
        StringBuilder contenido = new StringBuilder();

        try {
            // Esta línea puede lanzar FileNotFoundException (subclase de IOException)
            lector = new FileReader(nombreArchivo);

            // Simulación de lectura: se lee carácter por carácter
            int caracter;
            while ((caracter = lector.read()) != -1) {
                contenido.append((char) caracter);
            }

            System.out.println("Archivo leído correctamente dentro del método.");
            return contenido.toString();

        } finally {
            // El bloque finally se ejecuta SIEMPRE:
            // - Si no hay excepción, después del try y antes del return.
            // - Si hay excepción, inmediatamente después de lanzarse y antes de propagarla.
            System.out.println("Ejecutando finally: cerrando el recurso.");

            if (lector != null) {
                try {
                    lector.close(); // close también puede lanzar IOException, pero se ignora en este ejemplo
                } catch (IOException e) {
                    System.err.println("Error al cerrar el archivo: " + e.getMessage());
                }
            }
        }
    }

    /**
     * Método principal que llama a la función anterior y maneja la excepción.
     */
    public static void main(String[] args) {
        LectorArchivo lector = new LectorArchivo();
        String nombre = "archivo_inexistente.txt";

        try {
            String resultado = lector.leerContenidoArchivo(nombre);
            System.out.println("Contenido del archivo: " + resultado);
        } catch (IOException e) {
            System.err.println("Error capturado en main: " + e.getMessage());
            e.printStackTrace(); // Muestra la traza completa para depuración
        }
    }
}

Explicación detallada del ejemplo:

1. Declaración throws en la firma: El método leerContenidoArchivo declara throws IOException. Esto indica explícitamente que no capturará las excepciones de entrada/salida que puedan ocurrir, particularmente al crear el FileReader (que puede lanzar FileNotFoundException) o durante la lectura con read(). Al hacer esto, el método transfiere la responsabilidad de manejar el error al código que lo invoca, en este caso, el método main.

2. Uso del bloque finally para liberación de recursos: A pesar de que el método no maneja la excepción, es crucial asegurar que el recurso FileReader se cierre correctamente para evitar fugas de recursos. El bloque finally garantiza que el código de cierre se ejecute siempre:

- Si no hay excepción: Se ejecuta el try completamente, se prepara el return, pero antes de devolver el control se ejecuta el finally, cerrando el archivo. Luego se completa el return.

- Si hay excepción (por ejemplo, archivo no encontrado): La excepción se lanza al crear el FileReader. Inmediatamente, antes de que la excepción abandone el método, se ejecuta el bloque finally. Allí se cierra el recurso (aunque en este caso lector será null porque la excepción ocurrió antes de asignarlo). Después de ejecutar el finally, la excepción se propaga al método main.

3. Propagación y manejo externo: En main, la llamada a leerContenidoArchivo está envuelta en un bloque try-catch. Cuando se produce la excepción en el método interno, esta se propaga hasta main (después de pasar por el finally del método llamado). El bloque catch (IOException e) en main la captura y ejecuta el código de manejo del error, que en este caso imprime un mensaje y la traza de la pila. De esta forma, se logra un diseño limpio: la responsabilidad de manejar el error recae en quien tiene el contexto adecuado (main), mientras que el método de bajo nivel solo se ocupa de su tarea específica y de liberar los recursos que adquirió.


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

Sí, es perfectamente posible incluir excepciones no controladas, como RuntimeException o cualquiera de sus subclases, en la cláusula throws de un método. La sintaxis de Java lo permite sin restricciones. Un método podría declarar, por ejemplo: public void metodo() throws NullPointerException, IllegalArgumentException. Sin embargo, a diferencia de las excepciones controladas, el compilador no exige esta declaración. Incluso si un método lanza internamente una NullPointerException y no la declara con throws, el código compilará sin problemas. La inclusión de excepciones no controladas en throws es, por tanto, opcional y tiene un propósito fundamentalmente documental.

En cuanto a si el método llamante debería poner un try-catch para estas excepciones no controladas declaradas, no existe ninguna obligación por parte del compilador. El llamante puede invocar el método sin ningún tipo de protección y el código compilará igualmente. La decisión de capturar o no una excepción no controlada, incluso si está declarada en throws, es una decisión de diseño del programador, no un requisito del lenguaje. Si el llamante decide no capturarla, la excepción, al ser no controlada, se propagará naturalmente hacia arriba en la pila de llamadas igual que cualquier otra excepción, pudiendo llegar a terminar el programa si no es manejada en ningún nivel.

El sentido de declarar excepciones no controladas en throws es principalmente documentar el contrato del método de forma más explícita. Aunque el compilador no fuerce su manejo, incluirlas en la firma informa a quienes utilicen el método sobre las posibles condiciones excepcionales que puede encontrar, incluso si estas se deben a errores de programación. Por ejemplo, un método que espera un índice dentro de un rango podría documentar que lanza IndexOutOfBoundsException. Esto ayuda a los programadores a escribir código más robusto, ya que, aunque no estén obligados, pueden decidir capturar específicamente esas excepciones si tienen una estrategia de recuperación adecuada. También es útil para herramientas de documentación como Javadoc, que pueden generar una lista completa de las excepciones que un método puede lanzar, independientemente de su tipo. En resumen, declarar excepciones no controladas en throws es una buena práctica de documentación, pero no altera las obligaciones de manejo por parte del llamante.


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

La recomendación general para elegir entre excepciones controladas y no controladas en Java se basa en la naturaleza del error y en quién se espera que lo maneje. Las excepciones controladas (checked) se recomiendan para situaciones en las que el llamante del método puede razonablemente esperar una recuperación del error y tomar una acción alternativa. Típicamente se utilizan para condiciones anómalas que son externas al control del programa, como errores de entrada/salida (IOException), problemas de conexión a bases de datos (SQLException) o ausencia de archivos (FileNotFoundException). En estos casos, el programador que invoca el método tiene la oportunidad de, por ejemplo, solicitar otro nombre de archivo, reintentar la conexión o informar al usuario de forma educada. El compilador obliga a manejar estas excepciones, lo que garantiza que estas condiciones de error previsibles no sean ignoradas accidentalmente.

Por otro lado, las excepciones no controladas (unchecked) se recomiendan para errores que generalmente indican problemas en la lógica del programa, condiciones que violan el contrato del método o situaciones de las que no tiene sentido intentar una recuperación local. IllegalArgumentException es un ejemplo paradigmático: si un método recibe un argumento que no cumple con sus precondiciones (por ejemplo, un valor negativo para una función que solo acepta positivos), lanzar esta excepción indica al llamante que ha usado el método incorrectamente. Otros ejemplos son NullPointerException o IndexOutOfBoundsException. En estos casos, la solución no es un bloque try-catch en el llamante, sino corregir el código para que no se produzca la condición ilegal. El compilador no obliga a capturarlas porque se asume que un programa correctamente escrito debería prevenirlas, no manejarlas en tiempo de ejecución.

En cuanto a la existencia de ambos tipos en todos los lenguajes, la respuesta es negativa. La distinción entre excepciones controladas y no controladas es una característica particular de Java (y de algunos otros lenguajes como C# en sus inicios, aunque C# finalmente optó por no incorporar las checked exceptions). La mayoría de los lenguajes modernos con manejo de excepciones, como Python, JavaScript, Ruby, C++ (en su uso más común) o C#, solo disponen de un tipo de excepción, que sería equivalente a las no controladas de Java. En estos lenguajes, todas las excepciones son no controladas: no existe la obligación por parte del compilador de declararlas o capturarlas. El programador puede elegir capturarlas o no, pero el lenguaje no impone ninguna verificación en tiempo de compilación. Esto simplifica el lenguaje y evita la proliferación de bloques try-catch forzosos, pero traslada al programador la responsabilidad de documentar adecuadamente las excepciones que pueden lanzar sus funciones y de decidir cuándo es necesario capturarlas para la robustez de la aplicación.


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Sí, tiene perfecto sentido lanzar excepciones dentro de un bloque catch. Esta práctica, conocida como relanzamiento de excepciones, es una técnica común y útil en el manejo estructurado de errores. Al estar dentro de un bloque catch, se tiene acceso al objeto excepción que se acaba de capturar, y se puede decidir lanzarlo nuevamente, ya sea el mismo objeto u otro diferente. Esto permite implementar patrones como el registro de errores (log) antes de propagar la excepción, o la transformación de una excepción de bajo nivel en una de más alto nivel que sea más significativa para el contexto de la aplicación.

Ejemplo de relanzamiento de la misma excepción:

Una situación común es querer registrar el error antes de que la excepción continúe propagándose. Esto es útil para dejar constancia del problema sin impedir que capas superiores de la aplicación lo manejen.

public void procesarArchivo(String nombre) throws IOException {
    try {
        FileReader fr = new FileReader(nombre);
        // Operaciones con el archivo...
    } catch (IOException e) {
        // Se registra el error para depuración
        System.err.println("Error al procesar archivo: " + e.getMessage());
        // Se relanza la misma excepción para que la maneje quien llamó
        throw e;
    }
}

En este código, el método captura la IOException, la registra y luego la vuelve a lanzar. El método llamante recibirá exactamente la misma excepción, con su mensaje y traza originales, pero ahora además se ha dejado constancia en el log del punto intermedio por donde pasó.

Ejemplo de lanzar una excepción diferente (encapsulación o traducción):

Otra situación muy frecuente es capturar una excepción de bajo nivel y lanzar una nueva excepción de más alto nivel que sea más apropiada para la abstracción de la capa actual. Esto se conoce como encapsulación de excepciones.

// Excepción propia de la capa de negocio
class ServicioException extends Exception {
    public ServicioException(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

public void realizarOperacionFinanciera(String cuentaId) throws ServicioException {
    try {
        // Código que puede lanzar SQLException al acceder a BD
        accesoBD.actualizarSaldo(cuentaId, 100);
    } catch (SQLException e) {
        // Se traduce la excepción técnica a una de negocio
        throw new ServicioException("Error al actualizar saldo para cuenta: " + cuentaId, e);
    }
}

Aquí, la excepción de bajo nivel SQLException se captura y se envuelve dentro de una nueva excepción ServicioException. Es crucial incluir la excepción original como causa (mediante el constructor que acepta Throwable) para no perder la traza original del problema. Esto permite que las capas superiores trabajen con excepciones acordes a su nivel de abstracción, sin depender de detalles de implementación como el acceso a base de datos.

¿Cuándo tiene sentido relanzar la misma excepción?

- Registro centralizado (logging): Se captura la excepción para registrarla en un archivo de log y luego se relanza para que otro manejador (quizás más cercano a la interfaz de usuario) decida cómo presentarla al usuario.

- Tareas de limpieza parcial: Se captura para realizar alguna acción de limpieza que no pudo hacerse en finally (por ejemplo, notificar a otro sistema) y luego se relanza para que el error no quede oculto.

- Filtrado o decisión condicional: Se captura, se evalúa alguna condición (por ejemplo, el código de error) y, dependiendo de ella, se decide si se maneja localmente o se relanza.

¿Cuándo tiene sentido lanzar una excepción diferente?

- Abstracción de capas: Una capa de negocio no debe exponer excepciones de bajo nivel como SQLException o IOException a sus clientes. Se encapsulan en excepciones propias del dominio.

- Unificación de errores: Varios tipos de excepciones técnicas se pueden agrupar en una única excepción de más alto nivel que represente un fallo genérico de la operación.

- Enriquecimiento semántico: Se añade información contextual relevante (como el identificador de la cuenta o el nombre del archivo) que no estaba presente en la excepción original.

En todos los casos, es fundamental preservar la excepción original como causa de la nueva para no perder la traza de depuración. Java proporciona constructores para ello en todas las clases de excepción estándar.


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

La "causa" de una excepción es un mecanismo que permite asociar una excepción original (de bajo nivel) con una nueva excepción que se lanza en su lugar, estableciendo una relación de causalidad entre ambas. Esto es fundamental cuando se realiza encapsulación o traducción de excepciones, ya que permite no perder la información original sobre el error que ocurrió realmente, aunque se esté lanzando una excepción diferente y de más alto nivel. La excepción original se guarda como un atributo dentro de la nueva excepción, y puede ser recuperada posteriormente mediante métodos como getCause().

- Ejemplo de encapsulación con causa en Java:

// Excepción personalizada de la capa de negocio
class ProcesamientoException extends Exception {
    public ProcesamientoException(String mensaje, Throwable causa) {
        super(mensaje, causa);  // Se llama al constructor de Exception que guarda la causa
    }
}

public class ServicioProcesamiento {
    
    public void procesarDatos(String archivo) throws ProcesamientoException {
        try {
            // Código que puede lanzar una excepción de bajo nivel
            leerArchivo(archivo);
        } catch (IOException e) {
            // Se encapsula la IOException dentro de ProcesamientoException
            throw new ProcesamientoException("Error al procesar el archivo: " + archivo, e);
        }
    }
    
    private void leerArchivo(String nombre) throws IOException {
        // Simulación de una excepción de archivo no encontrado
        if (nombre == null || nombre.isEmpty()) {
            throw new FileNotFoundException("El archivo no existe: " + nombre);
        }
        // Resto del código...
    }
}

En este ejemplo, cuando ocurre una IOException en el método leerArchivo, esta es capturada en procesarDatos. En lugar de simplemente lanzar una nueva ProcesamientoException con un mensaje, se utiliza el constructor que acepta un segundo parámetro de tipo Throwable. Este constructor almacena internamente la excepción original (e) como la causa de la nueva excepción. De esta forma, la nueva excepción contiene tanto el mensaje de alto nivel como la referencia a la excepción original que provocó el fallo.

- Visualización de la causa en la traza de pila

Cuando una excepción que tiene una causa se imprime por pantalla, ya sea mediante printStackTrace() o porque el programa termina y la máquina virtual la muestra, la causa se muestra de forma claramente visible. La traza incluye tanto la pila de la nueva excepción como la de la excepción original, indicando explícitamente la relación de causalidad.

Por ejemplo, si se ejecuta el siguiente código:

public class Principal {
    public static void main(String[] args) {
        ServicioProcesamiento servicio = new ServicioProcesamiento();
        try {
            servicio.procesarDatos(null);
        } catch (ProcesamientoException e) {
            e.printStackTrace();
        }
    }
}

La salida por consola sería similar a:

ProcesamientoException: Error al procesar el archivo: null
    at ServicioProcesamiento.procesarDatos(ServicioProcesamiento.java:12)
    at Principal.main(Principal.java:5)
Caused by: java.io.FileNotFoundException: El archivo no existe: null
    at ServicioProcesamiento.leerArchivo(ServicioProcesamiento.java:19)
    at ServicioProcesamiento.procesarDatos(ServicioProcesamiento.java:9)
    ... 1 more

Nótese la línea "Caused by:" que introduce la traza de la excepción original. Esto permite a quien depura el problema conocer no solo que hubo un error de procesamiento, sino que la causa raíz fue una FileNotFoundException originada en el método leerArchivo. La información incluye los números de línea y la secuencia completa de llamadas, lo que facilita enormemente el diagnóstico.

Este mecanismo de causalidad es esencial para mantener la trazabilidad del error a través de diferentes capas de abstracción, permitiendo que las excepciones de alto nivel no oculten los detalles técnicos que realmente originaron el problema.