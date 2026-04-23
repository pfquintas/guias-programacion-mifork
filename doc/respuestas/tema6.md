<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

A continuación se presenta un ejemplo en Java (adaptado a los conocimientos previos) que emplea Object para crear una estructura de datos basada en un array primitivo capaz de almacenar cualquier tipo de dato, junto con su explicación correspondiente.

public class AlmacenadorObject {
    private Object[] elementos;
    private int tamaño;
    private static final int CAPACIDAD_INICIAL = 10;

    public AlmacenadorObject() {
        elementos = new Object[CAPACIDAD_INICIAL];
        tamaño = 0;
    }

    public void agregar(Object elemento) {
        if (tamaño == elementos.length) {
            Object[] nuevo = new Object[elementos.length * 2];
            for (int i = 0; i < elementos.length; i++) {
                nuevo[i] = elementos[i];
            }
            elementos = nuevo;
        }
        elementos[tamaño] = elemento;
        tamaño++;
    }

    public Object obtener(int indice) {
        if (indice < 0 || indice >= tamaño) {
            throw new IndexOutOfBoundsException("Índice inválido");
        }
        return elementos[indice];
    }

    public int tamaño() {
        return tamaño;
    }
}

Este AlmacenadorObject utiliza un array de tipo Object[] para contener cualquier tipo de referencia, ya que en Java todas las clases heredan directa o indirectamente de Object. Los tipos primitivos (como int, double) se pueden almacenar igualmente gracias al autoboxing (por ejemplo, int se convierte automáticamente a Integer, que a su vez es un Object). El método agregar recibe un Object y lo guarda en el array, redimensionándolo mediante la creación de un nuevo array y copia manual de elementos (similar a como se haría en C con realloc).

Al igual que en C con void*, esta aproximación pierde la información de tipo original. Al invocar obtener, el resultado es de tipo Object, por lo que se requiere una conversión explícita (cast) al tipo original para acceder a sus métodos específicos. Por ejemplo: String s = (String) almacenador.obtener(0). Si se realiza un cast incorrecto, se producirá una ClassCastException en tiempo de ejecución. Esta falta de seguridad de tipos es precisamente el problema que resuelve la genericidad en Java, permitiendo definir Almacenador<T> donde T se especifica al crear la instancia y el compilador verifica las operaciones.


## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

Programación genérica significa escribir código (clases, interfaces o métodos) que opera sobre tipos de datos que se especifican en el momento de su uso, en lugar de fijarlos anticipadamente. El objetivo es lograr reutilización y seguridad de tipos simultáneamente: una misma implementación puede funcionar con diferentes tipos concretos, pero el compilador puede verificar que dichos tipos sean coherentes y evitar conversiones incorrectas. En lenguajes como C++ (con plantillas) o Java (con genericidad desde la versión 5), la programación genérica permite abstraerse del tipo concreto manteniendo el chequeo estático.

El ejemplo anterior con Object no es un ejemplo de programación genérica en el sentido estricto del término, sino una simulación anterior a la existencia de genericidad. Aunque logra almacenar cualquier tipo de dato y reutiliza una única clase, sacrifica por completo la seguridad de tipos: el compilador no puede evitar que se mezclen tipos incompatibles ni que se realicen conversiones erróneas en tiempo de ejecución. La verdadera programación genérica requiere un mecanismo de parámetros de tipo (como <T> en Java) que permita al compilador conocer y restringir el tipo con el que se instancia la estructura. El ejemplo con Object es más bien una solución dinámica basada en el tipo raíz común, conocida como polimorfismo de inclusión, que resuelve la reutilización pero no la seguridad de tipos.


## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

Emplear void* en C o Object en Java para crear estructuras de datos genéricas introduce dos problemas fundamentales relacionados con el chequeo de tipos. El primero es la pérdida de información de tipo en tiempo de compilación: al almacenar un valor de cualquier tipo concreto (por ejemplo, un int o una struct Persona en C, o un String en Java) dentro de un contenedor tipado como void* u Object, el compilador olvida cuál era el tipo original. En consecuencia, toda operación de extracción requiere una conversión explícita (cast) hacia el tipo original, y el compilador no puede verificar si dicha conversión es correcta; simplemente confía en el programador. Esto traslada la responsabilidad del chequeo de tipos del compilador al programador.

El segundo problema es la aparición de errores en tiempo de ejecución cuando dichas conversiones fallan. En Java, si se extrae un Integer pero se intenta convertir a String, se produce una ClassCastException en tiempo de ejecución, no un error de compilación. En C, la situación es aún más peligrosa: convertir un void* a un tipo incorrecto no genera ninguna excepción, sino que puede provocar comportamiento indefinido (lecturas de memoria incorrectas, corrupción de datos o fallos de segmentación), que son extremadamente difíciles de depurar. Además, estas estructuras permiten mezclar tipos heterogéneos sin control: nada impide guardar un String, un Integer y un Double en el mismo contenedor, y luego recuperarlos con casts incorrectos. La genericidad real (como las plantillas de C++ o los genéricos de Java) resuelve ambos problemas al hacer que el tipo sea un parámetro que el compilador conoce y verifica en cada punto de uso, eliminando la necesidad de conversiones manuales peligrosas.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

Los parámetros de tipo son identificadores que se declaran entre los símbolos < > en la definición de una clase, interfaz o método genérico, y que actúan como marcadores para el tipo concreto que se proporcionará más adelante. Por ejemplo, en la declaración class Caja<T>, la T es un parámetro de tipo que representa un tipo desconocido en el momento de escribir la clase. Este parámetro puede utilizarse dentro de la definición para declarar atributos (private T contenido), parámetros de métodos (void guardar(T item)), tipos de retorno (T obtener()) e incluso para crear variables locales. Al instanciar la clase, se reemplaza el parámetro por un tipo concreto: Caja<String> reemplaza todas las T por String. Es importante destacar que los parámetros de tipo solo aceptan tipos de referencia, nunca tipos primitivos (aunque con autoboxing se puede simular).

La principal ventaja de los parámetros de tipo sobre soluciones como Object o void* es que el compilador mantiene un chequeo de tipos estático durante todo el desarrollo. Cuando se escribe Caja<String> caja = new Caja<>(); caja.guardar("texto");, el compilador sabe que guardar espera un String y rechazará intentos como caja.guardar(123). Del mismo modo, al invocar caja.obtener(), el compilador sabe que devuelve String sin necesidad de conversión explícita. Además, los parámetros de tipo pueden tener límites mediante la cláusula extends (ej. <T extends Number>), lo que restringe los tipos permitidos y permite invocar métodos del tipo límite (como intValue() de Number) dentro de la clase genérica. De este modo, los parámetros de tipo convierten la estructura de datos genérica en una construcción segura y expresiva, en lugar de una mera colección de objetos sin tipo.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

A continuación se presenta un ejemplo en Java (con ArrayList genérico) y otro en C++ (con std::vector mediante templates), que ilustran cómo instanciar una estructura dinámica que solo admite String (o std::string), añadir valores y recorrerlos con total seguridad de tipos, sin necesidad de conversiones explícitas.

- Ejemplo en Java (Generics):

import java.util.ArrayList;
import java.util.List;

public class EjemploGenerics {
    public static void main(String[] args) {
        // Instanciación de una lista genérica que solo admite String
        List<String> lista = new ArrayList<>();
        
        // Inserción de valores (el compilador verifica que sean String)
        lista.add("Primero");
        lista.add("Segundo");
        lista.add("Tercero");
        // lista.add(123); // Error de compilación: Integer no es String
        
        // Recorrido con for-each: cada elemento es automáticamente String
        for (String elemento : lista) {
            System.out.println("Elemento: " + elemento + ", longitud: " + elemento.length());
        }
        
        // También se puede acceder por índice sin conversión
        String primerElemento = lista.get(0); // No requiere cast
        System.out.println("Primer elemento: " + primerElemento.toUpperCase());
    }
}

Al declarar List<String>, el compilador conoce que el parámetro de tipo es String. El método add solo acepta String, y get devuelve directamente String. El recorrido con for (String elemento : lista) es posible porque el compilador sabe el tipo contenido. Cualquier intento de insertar un Integer o hacer una conversión incorrecta se detecta en compilación, no en ejecución.

- Ejemplo en C++ (Templates):

#include <iostream>
#include <vector>
#include <string>

int main() {
    // Instanciación de un vector que solo admite std::string
    std::vector<std::string> lista;
    
    // Inserción de valores (el compilador verifica que sean std::string)
    lista.push_back("Primero");
    lista.push_back("Segundo");
    lista.push_back("Tercero");
    // lista.push_back(123); // Error de compilación: int no se convierte a std::string
    
    // Recorrido con range-based for (C++11): cada elemento es std::string
    for (const std::string& elemento : lista) {
        std::cout << "Elemento: " << elemento 
                  << ", longitud: " << elemento.length() << std::endl;
    }
    
    // Acceso por índice sin conversión
    std::string primerElemento = lista[0]; // No requiere cast
    std::cout << "Primer elemento: " << primerElemento << std::endl;
    
    return 0;
}

std::vector<std::string> es una instanciación de la plantilla vector con el tipo concreto string. El compilador genera una versión especializada de vector que trabaja exclusivamente con string. Al igual que en Java, el código no contiene conversiones explícitas (cast), y cualquier error de tipo se detecta en tiempo de compilación. La diferencia fundamental es que C++ genera código separado para cada tipo con el que se instancia la plantilla (especialización en tiempo de compilación), mientras que Java usa el mismo código subyacente con type erasure.

Conclusión de ambos: En ambos lenguajes, la programación genérica permite crear estructuras de datos reutilizables con seguridad de tipos. El recorrido demuestra que cada elemento se maneja directamente como String (o std::string), sin riesgo de ClassCastException (Java) ni de comportamiento indefinido (C++). Esto contrasta con el uso de void* u Object, donde se requerirían conversiones manuales peligrosas.


## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

El compilador de Java y el de C++ procesan la programación genérica de manera radicalmente diferente debido a sus distintos objetivos de diseño. Cuando se instancia una clase con parámetros de tipo en Java, el compilador realiza un proceso llamado type erasure (borrado de tipos). Esto significa que verifica en tiempo de compilación que todos los usos de los tipos genéricos sean correctos (por ejemplo, que no se inserte un Integer en un List<String>), pero luego elimina toda la información de tipos genéricos del bytecode generado. En tiempo de ejecución, List<String> y List<Integer> son exactamente el mismo List de Object, y las conversiones necesarias se insertan automáticamente por el compilador donde corresponden. El JVM nunca sabe que existieron tipos genéricos.

En C++, el mecanismo es completamente opuesto mediante la instanciación de plantillas (template instantiation). Cuando el compilador de C++ encuentra std::vector<std::string>, genera una copia completa y separada del código de la plantilla vector, reemplazando el parámetro de tipo por std::string. Esto ocurre en tiempo de compilación para cada tipo concreto con el que se instancia la plantilla. Por tanto, std::vector<int> y std::vector<std::string> producen dos clases distintas y completamente independientes en el código objeto, cada una optimizada para su tipo específico. No hay pérdida de información de tipos en tiempo de ejecución, y se puede preguntar por el tipo real de los elementos.

Las consecuencias prácticas son importantes. En Java, el type erasure permite la compatibilidad binaria con código anterior a Java 5 (no se necesita recompilar las bibliotecas antiguas), pero impide ciertas operaciones como new T(), new T[n] o instanceof List<String>. En C++, la instanciación de plantillas genera código altamente eficiente y con información de tipos completa en tiempo de ejecución, pero puede aumentar el tamaño del código si se instancia con muchos tipos diferentes (code bloat) y hace más lenta la compilación. Además, C++ soporta tipos primitivos como parámetros de plantilla (vector<int> es válido), mientras que Java requiere usar sus wrappers (ArrayList<Integer>), confiando en el autoboxing para la conversión implícita.


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

A continuación se define la clase genérica Par<K, V> en Java, junto con un ejemplo de uso que devuelve la media y la desviación típica de un array de double utilizando un Par<Double, Double>.

- Definición de la clase Par<K, V>:

public class Par<K, V> {
    private final K primero;
    private final V segundo;

    public Par(K primero, V segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public K getPrimero() {
        return primero;
    }

    public V getSegundo() {
        return segundo;
    }
}

La clase declara dos parámetros de tipo, K y V, que representan respectivamente el tipo del primer valor y el tipo del segundo valor. El constructor recibe un valor de tipo K y otro de tipo V, y los almacena en atributos final (inmutables). Los métodos getPrimero() y getSegundo() devuelven cada valor con su tipo original, sin necesidad de conversión. Esta clase es análoga a la interfaz Pair de algunas bibliotecas, pero simplificada para propósitos didácticos.

- Ejemplo de uso:

public class Estadisticas {
    
    public static Par<Double, Double> calcularMediaYDesviacion(double[] datos) {
        if (datos == null || datos.length == 0) {
            return new Par<>(0.0, 0.0);
        }
        
        double suma = 0.0;
        for (double valor : datos) {
            suma += valor;
        }
        double media = suma / datos.length;
        
        double sumaCuadrados = 0.0;
        for (double valor : datos) {
            sumaCuadrados += Math.pow(valor - media, 2);
        }
        double desviacion = Math.sqrt(sumaCuadrados / datos.length);
        
        return new Par<>(media, desviacion);
    }
    
    public static void main(String[] args) {
        double[] valores = {10.0, 20.0, 30.0, 40.0, 50.0};
        Par<Double, Double> resultado = calcularMediaYDesviacion(valores);
        
        System.out.println("Media: " + resultado.getPrimero());
        System.out.println("Desviación típica: " + resultado.getSegundo());
        
        // El tipo es seguro: no requiere conversión
        double media = resultado.getPrimero();
        double desviacion = resultado.getSegundo();
        System.out.println("Doble de la media: " + (media * 2));
    }
}

La función calcularMediaYDesviacion realiza dos recorridos sobre el array (uno para la suma y otro para la suma de cuadrados), calcula la media y la desviación, y los empaqueta en un Par<Double, Double>. Al ser ambos valores del mismo tipo (Double), se usa el mismo parámetro para K y V, pero la clase permite que sean diferentes (por ejemplo, Par<String, Integer>). En el main, se extraen los valores mediante getPrimero() y getSegundo(), que devuelven directamente Double, no Object. Esto demuestra la seguridad de tipos: no se necesita ningún cast, y el compilador garantiza que media y desviacion son efectivamente Double. Si se intentara asignar el resultado a una variable de tipo String, el compilador lo rechazaría inmediatamente.


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

A continuación se presenta un ejemplo de un método genérico seleccionaUno en Java, junto con una comparación con una versión que usa Object, para ilustrar las ventajas en términos de seguridad de tipos y ausencia de conversiones explícitas.

- Método genérico con parámetro de tipo:

import java.util.Random;

public class Utilidades {
    private static final Random aleatorio = new Random();
    
    // Versión genérica: parámetro de tipo T
    public static <T> T seleccionaUno(T objeto1, T objeto2) {
        return aleatorio.nextBoolean() ? objeto1 : objeto2;
    }
    
    public static void main(String[] args) {
        String a = "Hola";
        String b = "Mundo";
        String resultado = seleccionaUno(a, b); // Sin cast
        System.out.println("Resultado: " + resultado);
        
        // Integer c = 10;
        // String d = "Texto";
        // String error = seleccionaUno(c, d); // Error de compilación: tipos incompatibles
    }
}

- Versión equivalente con Object (sin genericidad):

public class UtilidadesSinGenericos {
    private static final Random aleatorio = new Random();
    
    // Versión con Object: cualquier tipo, pero requiere cast al usar
    public static Object seleccionaUno(Object objeto1, Object objeto2) {
        return aleatorio.nextBoolean() ? objeto1 : objeto2;
    }
    
    public static void main(String[] args) {
        String a = "Hola";
        String b = "Mundo";
        Object temp = seleccionaUno(a, b);
        String resultado = (String) temp; // Necesario downcasting explícito
        System.out.println("Resultado: " + resultado);
        
        Integer c = 10;
        String d = "Texto";
        Object temp2 = seleccionaUno(c, d); // Compila, pero mezcla tipos diferentes
        String erroneo = (String) temp2; // Compila, pero causa ClassCastException en runtime
    }
}

- Diferencias explicadas:

1. Evitar downcasting: en la versión genérica, el método devuelve directamente T, que en el contexto de llamada con String se convierte en String. Por tanto, el resultado se asigna a String resultado sin necesidad de conversión explícita. En la versión con Object, el método devuelve Object, y el programador debe realizar un downcast manual con (String). Este cast es propenso a errores: si el objeto devuelto no es realmente un String, se produce una ClassCastException en tiempo de ejecución. La versión genérica elimina por completo la necesidad de casts en el código cliente.

2. Forzar que ambos objetos sean del mismo tipo: en la versión genérica, el parámetro T obliga a que los dos argumentos sean del mismo tipo concreto. Si se intenta pasar un Integer y un String, el compilador intenta inferir un tipo común (en este caso Object & Serializable & Comparable<?>), pero al asignar el resultado a String falla la compilación porque la inferencia no produce String. De hecho, la llamada seleccionaUno(c, d) no compila porque no existe un T que sea simultáneamente subtipo de Integer y de String (excepto Object, pero entonces el retorno sería Object y no se asignaría a String). En cambio, la versión con Object acepta cualquier combinación de tipos, permitiendo mezclar Integer con String silenciosamente, lo que solo se descubre cuando se produce el cast erróneo en tiempo de ejecución. Así, la genericidad traslada la detección del error del tiempo de ejecución al tiempo de compilación, cumpliendo el principio de detección temprana de errores.


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

Sí, en Java se pueden establecer restricciones en los parámetros de tipo mediante la cláusula extends. Esto permite definir un límite superior, indicando que el tipo genérico debe ser un subtipo de una clase determinada o implementar una interfaz específica. Por ejemplo, <T extends Number> restringe T a Number o a cualquiera de sus subclases (Integer, Double, Float, etc.). Dentro de la clase genérica, se pueden invocar todos los métodos declarados en Number, como doubleValue() o intValue(), lo que permite tratar los valores numéricos de forma uniforme.

A continuación se presentan dos soluciones para una clase Punto que trabaja con coordenadas numéricas. La primera usa Number directamente sin genericidad, y la segunda utiliza genéricos con restricción para reforzar el chequeo de tipos.

- Sin genericidad, usando Number directamente:

public class PuntoNumber {
    private final Number x;
    private final Number y;
    
    public PuntoNumber(Number x, Number y) {
        this.x = x;
        this.y = y;
    }
    
    public Number getX() { return x; }
    public Number getY() { return y; }
    
    public double calcularDistanciaA(PuntoNumber otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
}

- Con genericidad y restricción <T extends Number>:

public class Punto<T extends Number> {
    private final T x;
    private final T y;
    
    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }
    
    public T getX() { return x; }
    public T getY() { return y; }
    
    public double calcularDistanciaA(Punto<T> otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx*dx + dy*dy);
    }
    
    // Ejemplo de uso
    public static void main(String[] args) {
        Punto<Integer> p1 = new Punto<>(1, 2);
        Punto<Integer> p2 = new Punto<>(4, 6);
        System.out.println(p1.calcularDistanciaA(p2));
        
        Punto<Double> p3 = new Punto<>(1.5, 2.5);
        // Punto<Integer> p4 = new Punto<>(1, 2.5); // Error: tipos incompatibles
        
        // El compilador sabe que getX() devuelve Integer en p1 y Double en p3
        Integer x1 = p1.getX(); // Sin cast
        Double x3 = p3.getX(); // Sin cast
    }
}

- Comparación de ambas soluciones:

La solución con Number es más simple pero pierde información de tipo: getX() devuelve Number, obligando a quien llama a realizar conversiones si necesita el tipo concreto (Integer, Double, etc.) y no garantiza que ambas coordenadas sean del mismo subtipo numérico. La solución genérica <T extends Number> fuerza que las dos coordenadas sean del mismo tipo concreto T (por ejemplo, Integer o Double), y getX() devuelve exactamente T, eliminando la necesidad de conversiones. Además, en el método calcularDistanciaA, se exige que el otro punto tenga el mismo T, lo que evita mezclar accidentalmente tipos numéricos incompatibles.

- Respecto al type erasure, ¿cuál es el tipo final tras la compilación?

Debido al type erasure de Java, el compilador elimina el parámetro de tipo <T extends Number> y lo reemplaza por su límite superior, que en este caso es Number. Por tanto, tras la compilación, la clase Punto<T extends Number> se convierte efectivamente en una clase equivalente a PuntoNumber (la primera solución), donde todos los lugares donde aparecía T pasan a ser Number. El compilador inserta automáticamente las conversiones necesarias (verificación de tipos y casts implícitos) tanto en el código de Punto como en el código cliente. En tiempo de ejecución, Punto<Integer> y Punto<Double> son la misma clase Punto con campos de tipo Number. Esto demuestra que la genericidad en Java es una herramienta de verificación en compilación que no modifica el comportamiento en tiempo de ejecución respecto al uso directo de Number. Sin embargo, la ventaja es que el compilador proporciona un chequeo de tipos más estricto y elimina la necesidad de conversiones manuales en el código fuente.


## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

La solución sin genericidad (usando Number) sí permite crear un punto con coordenadas de tipos numéricos diferentes, por ejemplo new PuntoNumber(1, 2.5), donde 1 es un Integer y 2.5 es un Double. Esto se debe a que ambos son subtipos de Number, y el constructor acepta cualquier Number de forma independiente para x y para y. El compilador no impone ninguna restricción sobre la relación entre los tipos de x e y. Esta flexibilidad puede ser deseable en algunos contextos, pero también puede introducir sutiles problemas de precisión o comportamiento inesperado al realizar operaciones que asumen homogeneidad.

La solución con genericidad <T extends Number> no permite mezclar tipos distintos: new Punto<Integer, Double>(1, 2.5) es ilegal porque el parámetro T debe ser un único tipo concreto. Si se declara Punto<Integer>, ambas coordenadas deben ser Integer; si se declara Punto<Double>, ambas deben ser Double. Para mezclar tipos, se necesitaría una declaración diferente como Punto<Number> (que sería equivalente a la solución sin genéricos) o usar dos parámetros de tipo independientes (<X extends Number, Y extends Number>). La restricción con un solo T refuerza la homogeneidad, lo que es útil cuando se requiere que ambas coordenadas sean del mismo tipo numérico para preservar consistencia en operaciones internas.

En la solución sin generics (clase PuntoNumber), el método getX() devuelve Number. Esto significa que el código cliente recibe un objeto de tipo Number, y para operar con él como Integer o Double concreto se requiere una conversión explícita (downcast): Integer x = (Integer) punto.getX(). Si se realiza un cast incorrecto, se produce una ClassCastException en tiempo de ejecución. Además, el programador debe conocer o recordar cuál era el tipo concreto original.

En la solución con generics (clase Punto<T extends Number>), el método getX() devuelve el tipo T, que es el parámetro de tipo concreto con el que se instanció la clase. Por ejemplo, si se crea Punto<Integer>, getX() devuelve Integer; si se crea Punto<Double>, devuelve Double. No se necesita ninguna conversión explícita, y el compilador garantiza que el valor devuelto es exactamente del tipo declarado. El código cliente puede escribir directamente Integer x = punto.getX() o Double x = punto.getX() según corresponda, y cualquier error de asignación se detecta en tiempo de compilación.

La versión genérica refuerza el chequeo de tipos de dos maneras fundamentales: homogeneidad forzada (ambas coordenadas deben ser del mismo subtipo numérico) y eliminación de casts (el tipo de retorno es preciso y conocido en compilación). La versión sin genéricos es más flexible pero traslada la responsabilidad de la corrección de tipos al programador, con el consiguiente riesgo de errores en tiempo de ejecución. La elección entre una u otra depende del contexto: si se necesita máxima flexibilidad (coordenadas de tipos distintos), la solución con Number es adecuada; si se requiere consistencia y seguridad de tipos, la solución genérica es preferible. El type erasure hace que en tiempo de ejecución ambas sean equivalentes (ambas almacenan Number), pero la diferencia está en el contrato que el compilador impone al programador.


## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

A continuación se presenta una versión mejorada del código anterior utilizando genericidad para eliminar el uso de instanceof y los downcasts explícitos, forzando que el método distanciaA solo acepte puntos del mismo tipo que el receptor.

- Solución genérica con interfaz parametrizada:

// Interfaz genérica: el parámetro T representa el tipo concreto de punto
public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}

// Punto2D implementa Punto<Punto2D>
public class Punto2D implements Punto<Punto2D> {
    private final double x, y;
    
    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    @Override
    public double distanciaA(Punto2D p) {
        // No se necesita instanceof ni cast: p es directamente Punto2D
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2));
    }
}

// Punto3D implementa Punto<Punto3D>
public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;
    
    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }
    
    @Override
    public double distanciaA(Punto3D p) {
        // p es directamente Punto3D, sin necesidad de verificación
        return Math.sqrt(Math.pow(x - p.x, 2) + 
                         Math.pow(y - p.y, 2) + 
                         Math.pow(z - p.z, 2));
    }
}

- Ejemplo de uso y verificación de tipos:

public class Main {
    public static void main(String[] args) {
        Punto2D p1 = new Punto2D(0, 0);
        Punto2D p2 = new Punto2D(3, 4);
        System.out.println(p1.distanciaA(p2)); // Correcto: 5.0
        
        Punto3D q1 = new Punto3D(0, 0, 0);
        Punto3D q2 = new Punto3D(1, 2, 2);
        System.out.println(q1.distanciaA(q2)); // Correcto: 3.0
        
        // Error de compilación: no se puede mezclar Punto2D con Punto3D
        // p1.distanciaA(q2); // No compila: tipos incompatibles
    }
}

- Explicación del patrón utilizado: la declaración Punto<T extends Punto<T>> utiliza un patrón conocido como self-referential generics (genéricos auto-referenciales) o CRTP (Curiously Recurring Template Pattern) por su analogía con C++. La interfaz genérica define que el parámetro T debe ser un subtipo de Punto<T>. Cuando Punto2D implementa Punto<Punto2D>, se está diciendo que Punto2D es su propio parámetro de tipo. Esto permite que el método distanciaA reciba explícitamente T (es decir, Punto2D en la implementación) en lugar de la interfaz base Punto. Como resultado, dentro de Punto2D.distanciaA se sabe con certeza que el parámetro es otro Punto2D, y se puede acceder directamente a sus campos x e y.

- Ventajas sobre el original: el compilador garantiza que distanciaA solo se invoca con el tipo correcto, no existe la posibilidad de mezclar Punto2D con Punto3D accidentalmente, se elimina la necesidad de instanceof y de downcasts explícitos, y los errores de tipo se detectan en tiempo de compilación, no en tiempo de ejecución.

- Consideración sobre type erasure: debido al type erasure de Java, en tiempo de ejecución la interfaz Punto<T> se convierte en Punto (pérdida del parámetro genérico). Sin embargo, en cada clase concreta como Punto2D, el método distanciaA recibe efectivamente Punto2D porque la sobrescritura se resuelve mediante la firma exacta. El compilador inserta los puentes (bridge methods) necesarios para que la máquina virtual de Java enlace correctamente el método heredado de la interfaz con la implementación concreta. El resultado es un código seguro, limpio y sin comprobaciones dinámicas.


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

- ¿Es List<String> subtipo de List<Object>?

No. En Java, List<String> no es subtipo de List<Object>, aunque String sea subtipo de Object. Esto significa que no se puede asignar una List<String> a una variable de tipo List<Object>, ni pasar una List<String> a un método que espere List<Object>. La razón es que la genericidad en Java es invariante: el tipo parametrizado List<T> mantiene una relación de invariación respecto a su parámetro T. Si se permitiera dicha asignación, se podría añadir un Integer a una List<String> mediante la referencia de tipo List<Object>, violando la seguridad de tipos. El compilador impide esto directamente.

- ¿Es String[] subtipo de Object[]?

Sí. En Java, un array de String[] es subtipo de Object[], porque los arrays son covariantes. Esto significa que se puede asignar String[] a una variable Object[]. Sin embargo, esta covarianza es problemática en tiempo de ejecución: el array "recuerda" su tipo original. Si a través de la referencia Object[] se intenta almacenar un elemento que no sea String (por ejemplo, un Integer), el código compila pero lanza una excepción ArrayStoreException en tiempo de ejecución. Este problema se debe a que los arrays en Java tienen reificación de tipos (conocen su tipo en tiempo de ejecución), mientras que los genéricos usan type erasure (pierden la información de tipos en tiempo de ejecución).

- Problema en tiempo de ejecución con arrays:

String[] strings = new String[10];
Object[] objects = strings;          // Permitido por covarianza
objects[0] = 123;                    // Compila, pero lanza ArrayStoreException en runtime

El array strings sabe que fue creado como String[], por lo que rechaza almacenar un Integer. En cambio, si se usara una lista genérica, el error ocurriría en compilación.

- Definiciones de varianza:

1. Covariante: Un tipo genérico C<T> es covariante respecto a T si cuando A es subtipo de B, entonces C<A> es subtipo de C<B>. En Java, los arrays son covariantes (String[] subtipo de Object[]). Los wildcards con extends permiten covariancia de lectura: List<? extends Number> es covariante en el sentido de que se puede leer como Number, pero no se puede escribir.
2. Contravariante: Un tipo genérico C<T> es contravariante respecto a T si cuando A es subtipo de B, entonces C<B> es subtipo de C<A>. En Java, los wildcards con super permiten contravariancia de escritura: List<? super Integer> puede recibir Integer, pero lo que se lee es Object. No existe una construcción nativa contravariante en arrays.
3. Invariante: Un tipo genérico C<T> es invariante respecto a T si no hay relación de subtipado entre C<A> y C<B> aunque A y B la tengan. En Java, las clases genéricas sin wildcards (List<String> y List<Object>) son invariantes. Esta es la opción por defecto, y es la más segura, porque evita los problemas de los arrays y mantiene el sistema de tipos coherente en compilación.


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

Un wildcard es un comodín que se representa con el símbolo ? y se utiliza en declaraciones de tipos genéricos para indicar un tipo desconocido. Permite expresar relaciones de subtipado que no son posibles con la invariante por defecto de los genéricos. Los wildcards se emplean exclusivamente en contextos donde se usa un tipo parametrizado (como tipo de variable, parámetro de método o tipo de retorno), pero no en la definición de clases ni en la creación de instancias. Existen tres formas principales: ? (sin límite, cualquier tipo), ? extends T (límite superior: T o cualquier subtipo de T), y ? super T (límite inferior: T o cualquier supertipo de T). La introducción de wildcards permite que las estructuras genéricas sean covariantes (con extends) o contravariantes (con super) en el punto de uso, sin perder la seguridad de tipos en tiempo de compilación.

En List<? extends T>, no se puede añadir un elemento porque no se sabe exactamente de qué subtipo es la lista (podría ser List<Integer> o List<Double>), pero se puede leer con seguridad asignando a una variable de tipo T. En List<? super T>, se puede añadir cualquier subtipo de T, pero al leer solo se garantiza que es Object.

- Suma de números usando ? extends Number:

public static double sumarLista(List<? extends Number> lista) {
    double suma = 0.0;
    for (Number numero : lista) {
        suma += numero.doubleValue(); // Se lee como Number
    }
    return suma;
}

// Uso:
List<Integer> enteros = Arrays.asList(1, 2, 3);
List<Double> reales = Arrays.asList(1.5, 2.5, 3.5);
System.out.println(sumarLista(enteros)); // 6.0
System.out.println(sumarLista(reales));   // 7.5
// sumarLista(enteros) es válido porque List<Integer> es subtipo de List<? extends Number>

El método solo necesita leer los números y sumarlos. No escribe en la lista. ? extends Number permite pasar cualquier lista de un subtipo de Number (Integer, Double, etc.) y tratar cada elemento como Number.

- Añadir enteros a una lista usando ? super Integer:

public static void añadirEnteros(List<? super Integer> destino, int cantidad) {
    for (int i = 0; i < cantidad; i++) {
        destino.add(i); // Se pueden añadir Integer o sus subtipos
    }
}

// Uso:
List<Number> numeros = new ArrayList<>();
List<Object> objetos = new ArrayList<>();
List<Integer> soloEnteros = new ArrayList<>();

añadirEnteros(numeros, 5);   // Válido: Number es supertipo de Integer
añadirEnteros(objetos, 5);   // Válido: Object es supertipo de Integer
añadirEnteros(soloEnteros, 5); // Válido: Integer es supertipo de sí mismo

// Lo que no se puede hacer:
// Integer valor = destino.get(0); // Error: no se sabe si es Integer, solo se sabe que es Object
Object obj = destino.get(0); // Válido: siempre se puede leer como Object

El método necesita escribir enteros en la lista, pero no necesita leerlos con un tipo específico. ? super Integer acepta listas de Integer, Number, Object o cualquier supertipo de Integer, garantizando que sea seguro añadir valores de tipo Integer.

- Principio PECS (Producer Extends, Consumer Super):

1. Producer (productor): si un método obtiene elementos de una estructura, se declara con ? extends T → la estructura produce T.
2. Consumer (consumidor): si un método introduce elementos en una estructura, se declara con ? super T → la estructura consume T.
3. Si una estructura es a la vez productora y consumidora (por ejemplo, se lee y escribe en la misma lista), no se debe usar wildcard; se usa un parámetro de tipo ordinario.

Este principio permite escribir APIs flexibles pero seguras, evitando tanto el problema de los arrays covariantes como la rigidez de la invariante por defecto.