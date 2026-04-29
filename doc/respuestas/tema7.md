<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

Un puntero a función en C es una variable que almacena la dirección de memoria de una función, permitiendo invocar dicha función indirectamente a través del puntero. Este mecanismo posibilita pasar funciones como argumentos a otras funciones, almacenarlas en estructuras o arrays, y decidir en tiempo de ejecución qué función ejecutar. La sintaxis para declarar un puntero a función incluye el tipo de retorno, el nombre del puntero entre paréntesis seguido de asterisco, y la lista de tipos de parámetros entre paréntesis.

A continuación se muestra un ejemplo en C que define una función aMayusculasLocal (nótese que el nombre aMayusculas se usará para el puntero, no para la función, para evitar conflicto semántico). La función recibe una cadena de caracteres y la convierte a mayúsculas in-place (modificando el arreglo original), devolviendo el puntero a la misma cadena. Luego se declara un puntero a función llamado aMayusculas que apunta a dicha función, y se invoca mediante el puntero.

#include <stdio.h>
#include <ctype.h>

// Función que convierte una cadena a mayúsculas (devuelve el mismo puntero)
char* convertirAMayusculas(char* cadena) {
    for (int i = 0; cadena[i] != '\0'; i++) {
        cadena[i] = toupper(cadena[i]);
    }
    return cadena;
}

int main() {
    // Declaración de puntero a función:
    // tipoRetorno (*nombrePuntero)(tipoParam1, ...)
    char* (*aMayusculas)(char*) = convertirAMayusculas;
    
    char texto[] = "hola mundo";
    
    // Invocación a través del puntero (equivalente a llamar a convertirAMayusculas)
    aMayusculas(texto);
    
    printf("%s\n", texto);  // Imprime "HOLA MUNDO"
    
    return 0;
}

En el código anterior, aMayusculas es una variable local que almacena la dirección de la función convertirAMayusculas. La asignación = convertirAMayusculas no utiliza paréntesis, pues se toma la dirección implícita de la función. Posteriormente, la línea aMayusculas(texto); invoca la función apuntada, pasando la cadena como argumento. Es importante notar que el nombre de la función sin paréntesis ya es implícitamente un puntero a ella, por lo que el operador & es opcional. Este patrón resulta útil para implementar estrategias variables, callbacks o tablas de dispatch.


## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

Una función lambda es una función anónima y concisa que puede definirse en el lugar donde se necesita, sin necesidad de declararla como un método separado. Este concepto, proveniente de la programación funcional, permite tratar las funciones como ciudadanos de primera clase: se pueden asignar a variables, pasar como argumentos o devolver como resultados. Las lambdas suelen tener una sintaxis ligera que omite el nombre de la función, los tipos de retorno explícitos (por inferencia) y, en muchos lenguajes, las llaves cuando el cuerpo es una sola expresión.

En JavaScript, una lambda (función flecha) se define con (parámetros) => expresión o (parámetros) => { bloque }. En Java, las lambdas se introdujeron en Java 8 y requieren una interfaz funcional (una interfaz con un único método abstracto, como Function<T,R>). A continuación se muestran ambos ejemplos, donde se convierte una cadena a mayúsculas utilizando una variable local aMayusculas que almacena la lambda.

- Ejemplo en JavaScript:

// Lambda que convierte una cadena a mayúsculas
const aMayusculas = (cadena) => cadena.toUpperCase();

// Uso de la lambda
let texto = "hola mundo";
let resultado = aMayusculas(texto);
console.log(resultado);  // Imprime "HOLA MUNDO"

- Ejemplo en Java:

import java.util.function.Function;

public class EjemploLambda {
    public static void main(String[] args) {
        // Variable local que referencia una lambda
        Function<String, String> aMayusculas = (cadena) -> cadena.toUpperCase();
        
        String texto = "hola mundo";
        String resultado = aMayusculas.apply(texto);
        System.out.println(resultado);  // Imprime "HOLA MUNDO"
    }
}

En ambos casos, la variable aMayusculas apunta a la lambda, que recibe un String y devuelve su versión en mayúsculas. En JavaScript, la lambda se asigna directamente a una constante y se invoca como una función normal. En Java, al ser un lenguaje con tipos estáticos, se declara con Function<String, String>, y la invocación se realiza mediante el método apply(). Nótese que, a diferencia del puntero a función en C (que almacena direcciones de funciones nombradas), las lambdas son objetos funcionales que capturan el contexto léxico y pueden acceder a variables efectivamente finales de su entorno.


## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

El paradigma funcional es un estilo de programación que trata la computación como la evaluación de funciones matemáticas, evitando cambios de estado y datos mutables. Sus pilares fundamentales incluyen: transparencia referencial (una función produce siempre el mismo resultado para los mismos argumentos), inmutabilidad de datos, funciones de orden superior (que pueden recibir o devolver otras funciones), y evaluación perezosa. A diferencia del paradigma imperativo, donde el programa se expresa como una secuencia de instrucciones que modifican un estado, el estilo funcional se centra en qué se debe calcular, no en cómo hacerlo paso a paso.

Java 8 se considera un lenguaje multi-paradigma porque, aunque su base es orientada a objetos, incorporó características del paradigma funcional como lambdas, interfaces funcionales, streams y operaciones agregadas (map, filter, reduce). Esto permite a los desarrolladores elegir el enfoque más adecuado según el problema: usar objetos y encapsulación para modelar entidades del dominio, o usar funciones puras y composición para procesar colecciones de forma declarativa y paralelizable. Otros lenguajes multi-paradigma son Scala, Python y C++ (con sus lambdas y std::function). La combinación de paradigmas no es excluyente; más bien enriquece las herramientas disponibles.

Que las funciones sean "ciudadanos de primera clase" significa que pueden ser tratadas como cualquier otro valor del lenguaje: se pueden asignar a variables, almacenar en estructuras de datos, pasar como argumentos a otras funciones, y devolverse como resultados de funciones. Por ejemplo, en Java 8 una lambda asignada a Function<String,String> se puede enviar a un método map de un Stream, o en JavaScript una función puede ser elemento de un array. Esto contrasta con C/C++ sin orientación a objetos, donde las funciones existen solo en el nivel global o estático, y aunque se usan punteros a función para simular cierto grado de indirección, no soportan captura de contexto léxico (cierres) ni son objetos con estado asociado. El tratamiento de funciones como ciudadanos de primera clase es la base técnica que habilita el estilo funcional dentro de lenguajes que no nacieron puramente funcionales.


## 4. Explica la sintaxis básica de una función lambda en Java.

En Java, una función lambda se expresa con la sintaxis (parámetros) -> { cuerpo }, donde la flecha -> separa la lista de parámetros del cuerpo de la función. Los paréntesis son obligatorios si hay cero o más de un parámetro, pero opcionales cuando hay un solo parámetro y su tipo se puede inferir. El tipo de los parámetros puede declararse explícitamente u omitirse, porque el compilador lo deduce a partir de la interfaz funcional objetivo. El cuerpo puede ser una sola expresión (sin llaves y con retorno implícito) o un bloque de sentencias entre llaves, donde si devuelve un valor debe usar return explícitamente.

Existen variaciones sintácticas según el caso: si la lambda no recibe parámetros, se escriben paréntesis vacíos () -> expresión. Con un solo parámetro, se puede omitir el paréntesis: param -> expresión. Con varios parámetros, van entre paréntesis: (a, b) -> a + b. Si el cuerpo es un bloque, se usan llaves y punto y coma: (x, y) -> { int suma = x + y; return suma; }. Una lambda debe ser asignable a una variable de tipo interfaz funcional (una interfaz con un único método abstracto), como Function<T,R>, Predicate<T>, Consumer<T> o Runnable. Por ejemplo, Function<String, Integer> longitud = s -> s.length(); es válida porque el compilador infiere que s es String.

Las lambdas en Java no son funciones independientes; son instancias de interfaces funcionales generadas por el compilador. No pueden declarar su propio tipo genérico (el tipo se infiere del contexto), y solo pueden acceder a variables locales que sean efectivamente finales (no modificadas después de la definición) o atributos de instancia/estáticos. Esta sintaxis ligera contrasta con la necesaria para definir clases anónimas internas previas a Java 8, donde se requería escribir new Interfaz() { public tipo metodo(...) { ... } }. Las lambdas simplifican enormemente el uso de APIs funcionales como Stream, permitiendo escribir código más declarativo y legible.


## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

A continuación se amplían los ejemplos anteriores para mostrar cómo pasar una función lambda como parámetro a un método. En ambos lenguajes se define un método o función llamada transformar, que recibe una cadena y una función transformadora, y luego aplica dicha función a la cadena, devolviendo el resultado. Esto ilustra el concepto de funciones de orden superior, donde una función puede tomar otra función como argumento.

- Ejemplo en JavaScript:

// Función de orden superior que recibe una cadena y una función transformadora
function transformar(cadena, transformador) {
    return transformador(cadena);
}

// Lambda que convierte a mayúsculas
const aMayusculas = (s) => s.toUpperCase();

// Uso: se pasa la lambda como argumento
let texto = "hola mundo";
let resultado = transformar(texto, aMayusculas);
console.log(resultado);  // Imprime "HOLA MUNDO"

// También se puede pasar la lambda directamente sin asignarla a una variable
console.log(transformar("adiós", s => s.toUpperCase())); // "ADIÓS"

- Ejemplo en Java:

import java.util.function.Function;

public class EjemploParametroFuncion {
    
    // Método que recibe una cadena y una función transformadora
    public static String transformar(String cadena, Function<String, String> transformador) {
        return transformador.apply(cadena);
    }
    
    public static void main(String[] args) {
        // Lambda asignada a variable
        Function<String, String> aMayusculas = s -> s.toUpperCase();
        
        String texto = "hola mundo";
        String resultado = transformar(texto, aMayusculas);
        System.out.println(resultado);  // Imprime "HOLA MUNDO"
        
        // También se puede pasar la lambda directamente
        System.out.println(transformar("adiós", s -> s.toUpperCase())); // "ADIÓS"
    }
}

En ambos casos, el método transformar actúa como una función de orden superior al recibir un comportamiento (la transformación) como parámetro. En Java, el tipo Function<String, String> es una interfaz funcional que representa una función que recibe un String y devuelve otro String. Dentro del método transformar, se invoca al transformador mediante apply(cadena). En JavaScript, al no haber tipos estáticos, simplemente se recibe la función como parámetro y se invoca directamente con la cadena. Este patrón permite separar la estructura del algoritmo (transformar una cadena) del comportamiento específico (cómo transformarla), favoreciendo la reutilización y la composición. Nótese que, a diferencia de los punteros a función en C, aquí las lambdas pueden capturar variables del contexto y se pasan con una sintaxis mucho más limpia.


## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

A continuación se muestra cómo invocar transformar pasando directamente una función lambda que invierte la cadena, sin asignarla previamente a una variable. Esta práctica, conocida como "lambda inline", es muy común en programación funcional porque hace el código más conciso y revela la intención exactamente en el lugar donde se utiliza.

- Ejemplo en JavaScript:

function transformar(cadena, transformador) {
    return transformador(cadena);
}

// Inversión de cadena mediante lambda directa
let texto = "hola mundo";
let resultadoInverso = transformar(texto, s => s.split('').reverse().join(''));
console.log(resultadoInverso);  // Imprime "odnum aloh"

// También se puede hacer con una función anónima tradicional (menos concisa)
let otroResultado = transformar("abc", s => s.split('').reverse().join(''));
console.log(otroResultado);  // "cba"

- Ejemplo en Java:

import java.util.function.Function;

public class EjemploLambdaInline {
    
    public static String transformar(String cadena, Function<String, String> transformador) {
        return transformador.apply(cadena);
    }
    
    public static void main(String[] args) {
        String texto = "hola mundo";
        
        // Lambda inline para invertir la cadena
        String resultadoInverso = transformar(texto, s -> new StringBuilder(s).reverse().toString());
        System.out.println(resultadoInverso);  // Imprime "odnum aloh"
        
        // Otro ejemplo con una cadena diferente
        String otroResultado = transformar("abc", s -> new StringBuilder(s).reverse().toString());
        System.out.println(otroResultado);  // "cba"
    }
}

En JavaScript, la inversión se logra convirtiendo la cadena en un array de caracteres con split(''), invirtiendo el array con reverse(), y uniéndolo nuevamente con join(''). En Java, se utiliza la clase StringBuilder, que posee un método reverse() mutable y eficiente, convirtiendo luego el resultado a String con toString(). En ambos casos, la lambda aparece directamente dentro de los paréntesis de transformar, sin ser almacenada en una variable intermedia. Obsérvese que la sintaxis es muy similar a la del ejemplo anterior, cambiando únicamente la implementación de la transformación. Esta capacidad de definir comportamientos en el lugar de uso es una de las ventajas más notables de las funciones lambda frente a los punteros a función de C, donde la función debe estar definida con nombre previamente, salvo que se usen extensiones no estándar.


## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

Un cierre o "closure" es una función lambda que captura y recuerda el entorno léxico en el que fue definida, incluso después de que ese entorno haya dejado de ejecutarse. Esto significa que la lambda puede acceder a variables locales del ámbito circundante (siempre que sean efectivamente finales o inmutables), a parámetros del método contenedor y a atributos de la clase. En lenguajes funcionales, los cierres permiten crear funciones con estado privado sin necesidad de definir clases explícitas. En Java, las lambdas pueden formar cierres sobre variables locales siempre que dichas variables no cambien su valor después de la definición de la lambda (es decir, sean "efectivamente finales").

A continuación se modifica el ejemplo anterior para demostrar un cierre en Java. Se define una variable local prefijo fuera de la lambda, y dentro de la lambda se concatena dicha variable con la cadena de entrada. La lambda captura automáticamente el valor de prefijo en el momento de su definición.

import java.util.function.Function;

public class EjemploClosure {
    
    public static String transformar(String cadena, Function<String, String> transformador) {
        return transformador.apply(cadena);
    }
    
    public static void main(String[] args) {
        // Variable local que será capturada por el cierre
        String prefijo = "Prefijo: ";
        
        // Lambda que accede a la variable local 'prefijo' (closure)
        Function<String, String> agregarPrefijo = s -> prefijo + s;
        
        String texto = "hola mundo";
        String resultado = transformar(texto, agregarPrefijo);
        System.out.println(resultado);  // Imprime "Prefijo: hola mundo"
        
        // También se puede definir la lambda inline capturando 'prefijo'
        String otroResultado = transformar("adiós", s -> prefijo + s);
        System.out.println(otroResultado);  // Imprime "Prefijo: adiós"
        
        // Nota: si se intenta modificar 'prefijo' después, el compilador mostraría error
        // porque la variable debe ser efectivamente final. Por ejemplo:
        // prefijo = "Otro: ";  // Error de compilación si la lambda ya la capturó
    }
}

En este ejemplo, la variable prefijo está definida fuera de la lambda, pero dentro del mismo método main. La lambda agregarPrefijo forma un cierre sobre prefijo, accediendo a su valor cuando se invoca posteriormente. Este comportamiento es fundamental para la programación funcional, pues permite parametrizar comportamientos con valores del contexto sin necesidad de pasar explícitamente todos los datos como argumentos. La restricción de que la variable capturada sea efectivamente final garantiza la seguridad en entornos concurrentes, evitando problemas de sincronización por mutación compartida. Compárese con los punteros a función en C, que no soportan cierres porque no pueden acceder al entorno local de su definición (solo a variables globales o estáticas).


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

La diferencia fundamental entre una función lambda (con cierre) en lenguajes como Java o JavaScript y un puntero a función en C radica en la capacidad de capturar el entorno léxico. Un puntero a función en C almacena únicamente la dirección de una función global o estática, sin ningún contexto asociado. Cuando se invoca a través del puntero, la función ejecutada solo tiene acceso a sus parámetros, a variables globales y a variables estáticas, pero no puede acceder a variables locales del ámbito desde donde el puntero fue obtenido. En contraste, una función lambda puede formar un cierre que incluye variables locales del ámbito circundante, siempre que sean efectivamente finales, arrastrando consigo esos valores o referencias al momento de su definición.

En términos de implementación, un puntero a función en C es simplemente una dirección de memoria (por ejemplo, void (*func)(int) ocupa típicamente 4 u 8 bytes). Una lambda en Java o JavaScript es un objeto más complejo: en Java, el compilador genera una instancia de una interfaz funcional que contiene los valores capturados como campos implícitos; en JavaScript, la lambda es un objeto de función que guarda el entorno léxico en su cadena de alcance. Otra diferencia importante es la seguridad de tipos y la sintaxis: los punteros a función en C requieren declaraciones explícitas de tipos de retorno y parámetros (con sintaxis compleja como int (*)(char*, int)), mientras que las lambdas en Java se apoyan en la inferencia de tipos y en interfaces funcionales como Function<T,R>, ofreciendo un manejo más limpio y menos propenso a errores.

Adicionalmente, los punteros a función en C no pueden ser genéricos sin recurrir a void* con casting inseguro, mientras que las lambdas en Java se benefician de la genericidad del lenguaje. Por último, el ciclo de vida es diferente: un puntero a función en C solo es válido mientras la función apuntada permanezca en memoria (normalmente toda la ejecución), mientras que una lambda puede ser asignada a una variable, almacenada en colecciones, e incluso sobrevivir al ámbito donde se definió (como en el caso de un callback registrado que se ejecuta más tarde), gracias a que los valores capturados se copian o referencian adecuadamente. Esta capacidad de "recordar" el contexto es lo que hace a las lambdas mucho más expresivas y útiles para programación funcional y orientada a eventos, a costa de una pequeña sobrecarga en memoria y tiempo de ejecución.


## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

Una función que devuelve otra función es un claro ejemplo de función de orden superior. En este caso, crearDescuento recibe un porcentaje y devuelve una lambda que, a su vez, recibe un precio original y aplica el descuento capturado. La lambda interna forma un cierre sobre el parámetro porcentaje, recordando ese valor incluso después de que crearDescuento haya terminado su ejecución. Esto permite generar múltiples funciones descuento reutilizables, cada una con su propio porcentaje fijo, sin necesidad de pasar el porcentaje cada vez que se calcula un descuento.

A continuación se muestra la implementación en Java, con su correspondiente prueba:

import java.util.function.Function;

public class EjemploDevolverFuncion {
    
    // Función que crea y devuelve una función de descuento
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        // La lambda captura 'porcentaje' del entorno (closure)
        return precioOriginal -> precioOriginal * (1 - porcentaje / 100.0);
    }
    
    public static void main(String[] args) {
        // Crear dos funciones descuento distintas
        Function<Double, Double> descuento10 = crearDescuento(10);  // 10% de descuento
        Function<Double, Double> descuento25 = crearDescuento(25);  // 25% de descuento
        
        double precio = 100.0;
        
        double precioConDescuento10 = descuento10.apply(precio);
        double precioConDescuento25 = descuento25.apply(precio);
        
        System.out.println("Original: " + precio);
        System.out.println("Con 10% descuento: " + precioConDescuento10);  // 90.0
        System.out.println("Con 25% descuento: " + precioConDescuento25);  // 75.0
    }
}

En este código, crearDescuento devuelve la lambda precioOriginal -> precioOriginal * (1 - porcentaje / 100.0). La variable porcentaje es un parámetro del método crearDescuento y, al ser capturada por la lambda, se convierte en parte del cierre. Cuando se invoca crearDescuento(10), la lambda resultante lleva consigo el valor 10; cuando se invoca crearDescuento(25), la lambda lleva el valor 25. Cada lambda tiene su propio cierre independiente, de modo que descuento10 y descuento25 son funciones distintas con comportamientos diferentes. Este patrón es muy útil en configuración y estrategias variables: se puede generar familias de funciones paramétricas sin necesidad de definir múltiples métodos o clases. Además, nótese que la lambda capturada accede a porcentaje como si estuviera en su ámbito local, pero en realidad es una variable del método contenedor, lo que ejemplifica perfectamente el concepto de cierre. Sin los cierres, sería necesario pasar explícitamente el porcentaje como segundo parámetro en cada llamada, perdiendo la comodidad de tener funciones "preconfiguradas".


## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

Una interfaz funcional en Java es una interfaz que contiene exactamente un único método abstracto (sin implementar). Aunque puede contener múltiples métodos default o static (con implementación), la presencia de un solo método abstracto es lo que la califica como funcional. Este único método abstracto define el "contrato" que debe cumplir una expresión lambda o una referencia a método asignable a esa interfaz. Por ejemplo, Function<T,R>, Predicate<T>, Consumer<T> y Runnable son interfaces funcionales estándar. Java 8 introdujo la anotación opcional @FunctionalInterface, que hace que el compilador verifique que la interfaz cumple el requisito de un único método abstracto, generando un error en caso contrario.

Los requisitos para que una interfaz sea considerada funcional son: (1) Tener exactamente un método abstracto, sin contar los métodos que sobrescriben métodos públicos de Object (como equals, toString o hashCode), ya que estos no cuentan porque toda clase los hereda de Object. (2) Puede tener cualquier número de métodos default (con cuerpo) y métodos static. (3) Puede declarar métodos abstractos heredados de superinterfaces, siempre que el conjunto total de métodos abstractos (eliminando duplicados por herencia) sea exactamente uno. Por ejemplo, si una interfaz extiende otra interfaz funcional y no añade nuevos métodos abstractos, sigue siendo funcional.

La existencia de las interfaces funcionales es la clave que permite que las lambdas en Java sean fuertemente tipadas. Una expresión lambda no tiene un tipo por sí misma; su tipo se infiere a partir del contexto de asignación o invocación, que debe ser una interfaz funcional. Por ejemplo, la lambda s -> s.toUpperCase() puede ser tanto un Function<String,String> como un UnaryOperator<String> (que extiende a Function), dependiendo de la variable a la que se asigne. Esta comprobación estática evita errores de tipo en tiempo de compilación y garantiza que la lambda cumpla el contrato definido por la interfaz. En contraste, los punteros a función en C no tienen esta capa de abstracción basada en interfaces, sino que verifican directamente la compatibilidad de firmas, pero no permiten añadir comportamiento por defecto ni métodos adicionales como sí lo hacen las interfaces funcionales.


## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

A continuación se define la interfaz funcional Transformador siguiendo los requisitos especificados. Esta interfaz contendrá un único método abstracto que recibe un String y devuelve otro String. Se añade la anotación @FunctionalInterface para que el compilador valide que cumple la condición de ser interfaz funcional, aunque dicha anotación no es obligatoria, sí es una buena práctica porque documenta la intención y previene errores accidentales (como agregar un segundo método abstracto sin querer).

@FunctionalInterface
public interface Transformador {
    
    // Único método abstracto: transforma una cadena en otra
    String transformar(String entrada);
    
    // Se pueden incluir métodos default o static sin afectar la condición de funcional
    default void mostrarEjemplo() {
        System.out.println("Ejemplo de transformador: " + transformar("ejemplo"));
    }
    
    static Transformador identidad() {
        return s -> s;
    }
}

Esta interfaz Transformador cumple perfectamente el papel que anteriormente se había representado con la interfaz genérica Function<String, String>. Al definirla manualmente, se hace explícito el propósito del transformador y puede resultar más legible en dominios específicos. El método transformar es el método abstracto que toda lambda asignada a Transformador debe implementar. Por ejemplo, se podría usar así: Transformador mayusculas = s -> s.toUpperCase(); y luego String resultado = mayusculas.transformar("hola");. Nótese que la interfaz incluye un método default (mostrarEjemplo) y un método static (identidad), lo cual demuestra que una interfaz funcional puede tener múltiples métodos implementados siempre que solo haya un método abstracto. Esta flexibilidad no existe en los punteros a función de C, donde no hay mecanismo para asociar comportamiento adicional o métodos de utilidad junto con el puntero. La creación manual de interfaces funcionales es especialmente útil cuando se desean nombres semánticos para el dominio del problema, mejorando la claridad del código sobre el uso de las interfaces genéricas estándar.


## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

A continuación se redefine la interfaz funcional Transformador con parámetros genéricos, de modo que pueda transformar un tipo de entrada T en un tipo de salida R. Esta versión genérica es análoga a la interfaz estándar Function<T,R>, pero con un nombre semántico más específico para transformaciones. El uso de genéricos permite reutilizar la misma interfaz para múltiples propósitos sin sacrificar la seguridad de tipos en tiempo de compilación.

@FunctionalInterface
public interface Transformador<T, R> {
    
    // Método abstracto que transforma un valor de tipo T en un valor de tipo R
    R transformar(T entrada);
    
    // Método default de ejemplo
    default void mostrarAplicacion(T valor) {
        System.out.println("Entrada: " + valor + " -> Salida: " + transformar(valor));
    }
}

A continuación se muestra un ejemplo concreto donde se utiliza esta interfaz genérica para crear un transformador que redondea un número Double (entrada) a un Integer (salida). Se emplean dos estrategias de redondeo: una con Math.round() y otra con redondeo hacia abajo (truncamiento). La lambda se asigna a una variable de tipo Transformador<Double, Integer>.

public class EjemploTransformadorGenerico {
    
    public static void main(String[] args) {
        // Transformador que redondea un Double al Integer más cercano
        Transformador<Double, Integer> redondear = num -> (int) Math.round(num);
        
        // Transformador que trunca un Double (redondea hacia abajo)
        Transformador<Double, Integer> truncar = num -> num.intValue();  // equivalente a (int) num.doubleValue()
        
        double valor = 3.67;
        
        Integer redondeado = redondear.transformar(valor);
        Integer truncado = truncar.transformar(valor);
        
        System.out.println("Original: " + valor);
        System.out.println("Redondeado: " + redondeado);  // 4
        System.out.println("Truncado: " + truncado);      // 3
        
        // Uso del método default
        redondear.mostrarAplicacion(5.49);   // Imprime: Entrada: 5.49 -> Salida: 5
    }
}

En este código, la interfaz Transformador<T,R> declara un único método abstracto transformar(T entrada): R. Al instanciar las lambdas redondear y truncar, el compilador infiere que T es Double y R es Integer por el contexto de la asignación (la variable de tipo Transformador<Double, Integer>). La lambda num -> (int) Math.round(num) captura el parámetro num como Double y devuelve un Integer; la lambda num -> num.intValue() hace uso del método de instancia intValue() de Double para truncar la parte decimal. Este diseño genérico hace que Transformador sea extremadamente versátil, aplicable a cualquier par de tipos, no solo a String y String. Compárese con los punteros a función en C, donde la genericidad solo puede lograrse mediante void* con casting explícito y pérdida de seguridad de tipos, mientras que aquí el sistema de tipos garantiza que la entrada y salida coincidan correctamente.


## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

Java 8 introdujo el paquete java.util.function que contiene una amplia colección de interfaces funcionales predefinidas, organizadas según el número y tipo de parámetros, así como el tipo de retorno. Estas interfaces cubren la mayoría de los casos de uso comunes, evitando tener que definir interfaces personalizadas. Las principales categorías son:

Funciones (Function): Function<T,R> recibe un argumento de tipo T y devuelve un resultado de tipo R. Su método es apply(). UnaryOperator<T> extiende Function<T,T> para cuando entrada y salida son del mismo tipo. DoubleFunction<R>, IntFunction<R>, etc., son versiones especializadas para tipos primitivos, evitando el auto-boxing.

Predicados (Predicate): Predicate<T> recibe un argumento T y devuelve un boolean. Su método es test(). Es útil para filtrado. Existen versiones para tipos primitivos como IntPredicate, DoublePredicate.

Consumidores (Consumer): Consumer<T> recibe un argumento T y no devuelve resultado (void). Su método es accept(). Se usa para operaciones que producen efectos laterales, como imprimir o guardar. BiConsumer<T,U> recibe dos argumentos.

Proveedores (Supplier): Supplier<T> no recibe argumentos y devuelve un valor de tipo T. Su método es get(). Se usa para generación o suministro perezoso de valores.

Operadores (Operator): BinaryOperator<T> extiende BiFunction<T,T,T> para operaciones binarias donde ambos operandos y el resultado son del mismo tipo (por ejemplo, suma de números). UnaryOperator<T> es un caso especial de Function<T,T>.

Versiones con dos argumentos (Bi): BiFunction<T,U,R> recibe dos argumentos y devuelve un resultado. BiPredicate<T,U> recibe dos argumentos y devuelve boolean. BiConsumer<T,U> recibe dos argumentos y devuelve void.

Especializaciones para tipos primitivos: Para evitar el costo del auto-boxing, existen interfaces como IntToDoubleFunction, LongToIntFunction, ToIntFunction<T>, DoubleUnaryOperator, etc. Por ejemplo, IntFunction<R> recibe un int y devuelve un R; ToIntFunction<T> recibe un T y devuelve un int.

Usar estas interfaces predefinidas en lugar de crear interfaces propias es recomendable por consistencia y reutilización. Por ejemplo, el Transformador<T,R> personalizado definido antes es redundante con Function<T,R>. Sin embargo, en dominios muy específicos puede tener sentido crear una interfaz con nombre semántico (ej. Validator<T> extends Predicate<T>) para mejorar la legibilidad, pero extendiendo de las predefinidas. Conocer estas interfaces es fundamental para trabajar con Streams, lambdas y APIs funcionales en Java, ya que forman la base de la programación funcional dentro del lenguaje.


## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

El método forEach de la interfaz List (y en general de Iterable) es una operación terminal que recibe un Consumer<? super T> y aplica ese consumidor a cada elemento de la colección. A diferencia del bucle for tradicional, que es imperativo y enfatiza cómo recorrer (índices o iteradores), forEach adopta un estilo declarativo: se especifica qué hacer con cada elemento, y la iteración queda oculta. Esto permite escribir código más conciso y, en combinación con lambdas, muy expresivo.

A continuación se muestra un ejemplo donde se recorre una lista de Integer y se imprime un mensaje solo para aquellos números que sean positivos. Nótese que el Consumer se implementa mediante una lambda que contiene la lógica de comprobación y la salida por consola. Si se desea separar la condición, podría combinarse con un filter previo antes del forEach, pero aquí se mantiene dentro del mismo consumidor por simplicidad.

import java.util.Arrays;
import java.util.List;

public class EjemploForEach {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(-3, 5, 0, 8, -1, 10);
        
        // Versión funcional con forEach y lambda
        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println(n + " es positivo");
            }
        });
        
        // También se puede usar una referencia a método si la lógica está en un método aparte
        // numeros.forEach(EjemploForEach::mostrarSiPositivo);
    }
    
    private static void mostrarSiPositivo(Integer n) {
        if (n > 0) {
            System.out.println(n + " es positivo");
        }
    }
}

En este código, numeros.forEach(n -> { ... }); itera internamente sobre la lista. La lambda n -> { ... } cumple con la interfaz funcional Consumer<Integer>, ya que recibe un parámetro (n) y no devuelve resultado (void). Comparado con un bucle for (Integer n : numeros) { if (n > 0) System.out.println(...); }, la versión con forEach es igual de legible pero se presta mejor a composiciones funcionales. Por ejemplo, si además se quisiera filtrar antes de imprimir, se podría encadenar con numeros.stream().filter(n -> n > 0).forEach(System.out::println). Sin embargo, es importante señalar que forEach no está exento de críticas: no soporta break, continue o retorno temprano (excepto lanzando excepciones), y en streams paralelos el orden no está garantizado a menos que se use forEachOrdered. Por ello, para simples iteraciones con efectos laterales, forEach es adecuado; para transformaciones o reducciones, es preferible usar map, filter, reduce sin efectos laterales. En la práctica, conocer ambas aproximaciones (imperativa y funcional) permite elegir la más idónea según el contexto.


## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

La firma void forEach(Consumer<? super T> action) utiliza un comodín ? super T en lugar de Consumer<T> para permitir que el método acepte un Consumer que pueda manejar no solo elementos del tipo exacto T, sino también tipos superiores en la jerarquía. Esto es útil por el principio de covariancia/contravarianza en genéricos: dado que Consumer es un consumidor (toma un valor y hace algo con él, sin devolver nada), si se tiene un Consumer<Number> que sabe procesar números, también puede procesar Integer porque Integer es subtipo de Number. Es decir, un consumidor de una superclase puede consumir cualquier subtipo. Usar ? super T en un parámetro que recibe datos (consumidor) permite mayor flexibilidad en la llamada, cumpliendo el principio de PECS.

PECS es un acrónimo mnemotécnico que significa Producer extends, Consumer super. Fue acuñado por Joshua Bloch en "Effective Java". La regla es:

- Si una estructura produce valores de tipo T (los devuelve, los emite), se declara con ? extends T. Por ejemplo, un List<? extends Number> puede ser fuente de números (se puede leer como Number), pero no se pueden añadir elementos porque se desconoce el subtipo exacto.

- Si una estructura consume valores de tipo T (los recibe como argumento), se declara con ? super T. Por ejemplo, un Consumer<? super Integer> puede consumir enteros, y se le puede pasar un Consumer<Number> o Consumer<Object>.

- Si una estructura es ambas (produce y consume), no se usan comodines y se emplea el tipo exacto T.

Aplicando PECS al método transformar que se ha venido usando, supóngase que se quiere generalizar para que acepte un transformador más flexible que pueda trabajar con supertipos o subtipos. Por ejemplo, si se tiene una jerarquía class Animal y class Perro extends Animal, y se desea un transformador que convierta Perro en String pero que también pueda aceptar un transformador de Animal a String (pues todo Perro es Animal). En ese caso, el método que aplica la transformación debe declarar el parámetro como Transformador<? super T, R> cuando T es el tipo de entrada (que se consume, porque el transformador recibe un T) y R es el tipo de salida (producido, por lo que sería ? extends R si se quiere flexibilidad en el retorno). Pero en el caso simple, si transformar solo consume un T y produce un R, se aplica PECS parcialmente:

// Versión genérica del método transformar con regla PECS
public static <T, R> R transformar(T entrada, Function<? super T, ? extends R> transformador) {
    return transformador.apply(entrada);
}

En este ejemplo, ? super T en el primer parámetro de Function (tipo de entrada) permite pasar un Function<Animal, String> cuando se tiene un Perro (porque el transformador consume el perro tratándolo como animal). ? extends R en el tipo de retorno permite que el transformador devuelva un subtipo de R, flexibilizando la salida. Esta es la aplicación completa de PECS para una Function<T,R>: entrada como consumer (super), salida como producer (extends). De hecho, la interfaz Function<T,R> en Java sigue esta regla en sus métodos combinatorios (andThen, compose). Recordar PECS ayuda a escribir APIs genéricas más flexibles y a entender por qué ciertos comodines aparecen en firmas del JDK como Collections.copy(List<? super T> dest, List<? extends T> src) o Consumer<? super T>.


## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

Las referencias a métodos son una sintaxis abreviada para expresar lambdas que simplemente invocan un método existente. En lugar de escribir (args) -> objeto.metodo(args), se puede escribir objeto::metodo. Esto hace el código más conciso y legible, especialmente cuando la lambda solo redirige la llamada. Tanto Java como JavaScript soportan este concepto, aunque con diferencias importantes: en JavaScript se usa el operador bind para fijar el contexto this, mientras que en Java la referencia a método es una característica del lenguaje que se resuelve en tiempo de compilación.

- Ejemplo en Java:

public class Persona {
    private String nombre;
    
    public Persona(String nombre) {
        this.nombre = nombre;
    }
    
    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
    
    public static void main(String[] args) {
        Persona persona = new Persona("Ana");
        
        // Referencia a método de instancia enlazada a un objeto concreto
        Runnable referenciaSaludar = persona::saludar;
        
        // Invocación a través de la referencia
        referenciaSaludar.run();  // Imprime "Hola, soy Ana"
    }
}

- Ejemplo en JavaScript:

class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }
    
    saludar() {
        console.log(`Hola, soy ${this.nombre}`);
    }
}

const persona = new Persona("Ana");

// Referencia al método (sin bind, perdería el contexto this)
const referenciaSaludarSinBind = persona.saludar;
// referenciaSaludarSinBind(); // Error: this is undefined (en modo estricto)

// Forma correcta: usar bind para fijar el contexto
const referenciaSaludar = persona.saludar.bind(persona);
referenciaSaludar();  // Imprime "Hola, soy Ana"

// También se puede usar una lambda envolvente
const otraReferencia = () => persona.saludar();
otraReferencia();  // Imprime "Hola, soy Ana"

En Java, persona::saludar produce un objeto de tipo Runnable porque saludar tiene firma void() (sin parámetros y sin retorno). La referencia captura automáticamente el persona específico en el que se invocará el método, formando un cierre. En JavaScript, en cambio, pasar persona.saludar sin más pierde la vinculación con this, porque las funciones en JS no están ligadas al objeto original a menos que se use bind, call, apply o se definan como funciones flecha. La diferencia fundamental es que en Java el método saludar no es un ciudadano de primera clase independiente de su receptor; la referencia a método es una construcción sintáctica que el compilador traduce a una lambda. En JavaScript, las funciones son objetos de primera clase independientes, y el this se determina en tiempo de ejecución según cómo se invoque la función. Por eso, para obtener una referencia segura al método de una instancia, se requiere bind. Este contraste ayuda a comprender los distintos modelos de funciones en lenguajes orientados a objetos y funcionales.


## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

En Java existen cuatro tipos de referencias a métodos, que permiten reemplazar expresiones lambda simples por una sintaxis más compacta. Estas referencias se clasifican según el tipo de método al que apuntan y el contexto de su invocación. A continuación se detalla cada tipo con un ejemplo ilustrativo.

1. Referencia a método estático:
- Sintaxis: Clase::metodoEstatico
- Se utiliza cuando la lambda tiene la forma (args) -> Clase.metodoEstatico(args). El método estático recibe los mismos parámetros que la lambda.

import java.util.function.Function;

public class EjemploReferencias {
    public static Integer parsearEntero(String cadena) {
        return Integer.parseInt(cadena);
    }
    
    public static void main(String[] args) {
        // Lambda equivalente: s -> Integer.parseInt(s)
        Function<String, Integer> parser = EjemploReferencias::parsearEntero;
        Integer resultado = parser.apply("123");
        System.out.println(resultado);  // 123
    }
}

2. Referencia a constructor:
- Sintaxis: Clase::new
- Se emplea cuando la lambda simplemente invoca al constructor de una clase. Los parámetros de la lambda se corresponden con los argumentos del constructor.

import java.util.function.Supplier;
import java.util.function.Function;

class Persona {
    String nombre;
    Persona() { this.nombre = "Anónimo"; }
    Persona(String nombre) { this.nombre = nombre; }
}

public class EjemploConstructor {
    public static void main(String[] args) {
        // Referencia a constructor sin parámetros
        Supplier<Persona> creador = Persona::new;
        Persona p1 = creador.get();  // Llama a Persona()
        
        // Referencia a constructor con un parámetro
        Function<String, Persona> creadorConNombre = Persona::new;
        Persona p2 = creadorConNombre.apply("Ana");  // Llama a Persona(String)
    }
}

3. Referencia a método de instancia sobre una instancia concreta:
- Sintaxis: instancia::metodoInstancia
- La lambda captura una instancia específica y la función utiliza esa instancia para invocar el método. La lambda puede tener parámetros adicionales que se pasan al método.

class Calculadora {
    public int sumar(int a, int b) { return a + b; }
}

public class EjemploInstanciaConcreta {
    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        // Lambda equivalente: (a, b) -> calc.sumar(a, b)
        java.util.function.BinaryOperator<Integer> sumador = calc::sumar;
        int resultado = sumador.apply(3, 5);
        System.out.println(resultado);  // 8
    }
}

4. Referencia a método de instancia sobre cualquier instancia (al primer parámetro implícito):
- Sintaxis: Clase::metodoInstancia
- Este es el caso más sutil: la lambda toma como primer parámetro la instancia sobre la que se invoca el método, y los parámetros restantes se pasan al método. Es útil para métodos que no reciben parámetros adicionales.

import java.util.function.Function;

class Persona {
    private String nombre;
    public Persona(String nombre) { this.nombre = nombre; }
    public String getNombre() { return nombre; }
}

public class EjemploSobreCualquierInstancia {
    public static void main(String[] args) {
        // Lambda equivalente: (Persona p) -> p.getNombre()
        Function<Persona, String> obtieneNombre = Persona::getNombre;
        
        Persona ana = new Persona("Ana");
        Persona juan = new Persona("Juan");
        
        System.out.println(obtieneNombre.apply(ana));  // "Ana"
        System.out.println(obtieneNombre.apply(juan)); // "Juan"
    }
}

En el último caso, Persona::getNombre es una referencia a un método de instancia no vinculado (unbound). El primer parámetro de la función resultante es el receptor implícito (Persona), que luego se utiliza para invocar getNombre. Este tipo de referencia es muy usado con Streams, por ejemplo: lista.stream().map(Persona::getNombre).forEach(System.out::println). Cada tipo de referencia tiene su propósito y permite eliminar el ruido sintáctico de las lambdas cuando estas son meramente delegantes. Conocer estos cuatro casos es esencial para escribir código Java funcional de forma idiomática y concisa.


## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

A continuación se muestra cómo ordenar una lista de Persona utilizando Collections.sort con dos enfoques diferentes: uno implementando la lógica de comparación manualmente dentro de una lambda, y otro empleando los métodos estáticos y por defecto de la interfaz funcional Comparator. Ambos enfoques son funcionales, pero el segundo es más declarativo y expresivo, además de estar mejor preparado para encadenamientos complejos.

Primero, se define la clase Persona:

public class Persona {
    private String nombre;
    private int edad;
    
    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
    
    public String getNombre() { return nombre; }
    public int getEdad() { return edad; }
    
    @Override
    public String toString() {
        return nombre + " (" + edad + ")";
    }
}

- 1. Lambda manual implementando la lógica de comparación completa:
En este caso, la lambda recibe dos personas y devuelve un entero siguiendo el contrato de Comparator<T>: negativo si el primero es menor, cero si son iguales, positivo si el primero es mayor.

import java.util.*;

public class OrdenacionManual {
    public static void main(String[] args) {
        List<Persona> personas = Arrays.asList(
            new Persona("Ana", 25),
            new Persona("Luis", 30),
            new Persona("Ana", 20),
            new Persona("Carlos", 25)
        );
        
        // Ordenación con lambda manual
        Collections.sort(personas, (p1, p2) -> {
            int comparacionEdad = Integer.compare(p1.getEdad(), p2.getEdad());
            if (comparacionEdad != 0) {
                return comparacionEdad;
            } else {
                return p1.getNombre().compareTo(p2.getNombre());
            }
        });
        
        System.out.println(personas);
        // Salida esperada: [Ana (20), Ana (25), Carlos (25), Luis (30)]
    }
}

2. Empleando Comparator con sus métodos encadenables:
Aquí se usa Comparator.comparingInt para extraer la edad, luego thenComparing para añadir el criterio secundario sobre el nombre. Esta versión es más legible y menos propensa a errores.

import java.util.*;

public class OrdenacionComparator {
    public static void main(String[] args) {
        List<Persona> personas = Arrays.asList(
            new Persona("Ana", 25),
            new Persona("Luis", 30),
            new Persona("Ana", 20),
            new Persona("Carlos", 25)
        );
        
        // Ordenación con Comparator encadenado
        Comparator<Persona> comparador = Comparator
            .comparingInt(Persona::getEdad)
            .thenComparing(Persona::getNombre);
        
        Collections.sort(personas, comparador);
        
        // También se puede hacer directamente en el sort sin variable intermedia
        // personas.sort(Comparator.comparingInt(Persona::getEdad).thenComparing(Persona::getNombre));
        
        System.out.println(personas);
        // Salida esperada: [Ana (20), Ana (25), Carlos (25), Luis (30)]
    }
}

En la primera versión, se escribe explícitamente la lógica de comparación con condicionales, lo cual es funcional pero verboso y susceptible a errores en el orden de los parámetros. En la segunda versión, se utilizan los métodos estáticos y por defecto de Comparator (comparingInt y thenComparing), que internamente construyen comparadores compuestos de forma segura. Esta segunda aproximación es más declarativa: se indica qué extraer (getEdad, luego getNombre) sin especificar cómo realizar la comparación numérica o lexicográfica. Además, las referencias a método (Persona::getEdad) hacen el código aún más limpio. Nótese que Collections.sort también puede ser reemplazado por el método List.sort desde Java 8 (personas.sort(comparador)). Este ejemplo muestra cómo las lambdas y las interfaces funcionales predefinidas permiten escribir código más conciso y mantenible, acercándose al estilo de programación funcional sin perder la seguridad de tipos.