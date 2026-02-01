<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

Las cuatro características fundamentales de la programación orientada a objetos son la abstracción, la encapsulación, la herencia y el polimorfismo. La abstracción es el proceso de identificar los atributos y comportamientos esenciales de una entidad del mundo real para modelarla como una clase, ignorando los detalles irrelevantes. Esto permite manejar la complejidad al centrarse en lo que un objeto es y hace, en lugar de en su implementación concreta. Por ejemplo, al modelar un "Coche" para un sistema, se abstraen características como su marca y velocidad, pero se pueden omitir detalles como el número de serie del fabricante del radio.

La encapsulación consiste en agrupar los datos (atributos) y los métodos que operan sobre ellos en una unidad llamada clase, y restringir el acceso directo a los componentes internos. Normalmente, los atributos se declaran como privados y se accede a ellos a través de métodos públicos (getters y setters). Este principio protege la integridad del objeto, ya que evita que su estado sea modificado de manera arbitraria o inconsistente desde fuera, y solo expone una interfaz controlada para la interacción.

La herencia es un mecanismo que permite crear una nueva clase (subclase o clase hija) a partir de una clase existente (superclase o clase padre), adoptando automáticamente sus atributos y métodos. Esto facilita la reutilización de código y establece una relación de "es-un" entre clases. Por ejemplo, una clase Vehiculo podría tener subclases como Coche y Moto, que heredan características generales (por ejemplo, velocidadMaxima) pero pueden añadir o modificar comportamientos específicos.

El polimorfismo, que significa "muchas formas", permite que una misma operación o método se comporte de manera diferente según el objeto sobre el que se aplique. Esto se logra comúnmente mediante la sobrescritura de métodos (override), donde una subclase redefine un método heredado de su superclase. Gracias al polimorfismo, una referencia de tipo genérico (como Vehiculo) puede apuntar a objetos de tipos específicos (como Coche o Moto), y al invocar un método como mover(), se ejecutará la versión correspondiente al objeto real, permitiendo un diseño más flexible y extensible.


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Cuatro lenguajes de programación populares que soportan el paradigma de la programación orientada a objetos son Java, Python, C++ y C#. Java es un lenguaje diseñado desde su origen para ser orientado a objetos, donde casi todos los elementos son objetos, con excepción de los tipos de datos primitivos. Su filosofía de "escribir una vez, ejecutar en cualquier lugar" y su fuerte tipado lo han hecho muy popular en entornos empresariales, aplicaciones Android y sistemas web a gran escala. La orientación a objetos en Java es estricta y clara, lo que la convierte en un referente educativo para aprender estos conceptos.

Python es un lenguaje multiparadigma que admite la programación orientada a objetos de una manera flexible y legible. A diferencia de Java, no fuerza el uso de clases para todo, permitiendo también programación procedural o funcional. Su sintaxis sencilla y la posibilidad de definir clases y herencia de forma intuitiva lo han hecho muy popular en campos como la ciencia de datos, la inteligencia artificial y el desarrollo web. La orientación a objetos en Python se caracteriza por su dinamismo y por principios como el "duck typing".

C++ es una extensión del lenguaje C que incorporó características de orientación a objetos, manteniendo la eficiencia y el control de bajo nivel del lenguaje base. Soporta clases, herencia, polimorfismo y encapsulación, pero también permite la manipulación directa de memoria y la programación procedural. Esta versatilidad lo ha consolidado en áreas donde el rendimiento es crítico, como los videojuegos, los sistemas operativos y las aplicaciones científicas. Su modelo de objetos es más complejo que el de Java, al ofrecer opciones como la herencia múltiple y el control manual de la memoria.

C# es un lenguaje desarrollado por Microsoft, fuertemente influenciado por Java y C++, que se ejecuta principalmente en la plataforma .NET. Está diseñado como un lenguaje completamente orientado a objetos, moderno y con una sintaxis clara. Es ampliamente utilizado en el desarrollo de aplicaciones de escritorio con Windows, videojuegos con el motor Unity y, más recientemente, en desarrollo web y multiplataforma. Al igual que Java, promueve un modelo de objetos robusto con gestión automática de memoria (recolección de basura) y una rica biblioteca de clases estándar.


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

Programación estructurada es un paradigma de programación que enfatiza la claridad y la calidad del código mediante el uso de tres estructuras de control fundamentales: la secuencia (ejecución de instrucciones en orden), la selección (sentencias condicionales como if/else) y la iteración (bucles como while o for). Su principal objetivo es combatir la complejidad y los errores asociados al "código espagueti", que ocurría con el uso indiscriminado de sentencias goto en lenguajes como los primeros FORTRAN o BASIC. Este paradigma promueve que cualquier programa, independientemente de su complejidad, puede construirse utilizando solo estas tres estructuras, lo que da lugar a programas más legibles, mantenibles y fáciles de verificar.

En contraste, la programación modular va un paso más allá al descomponer un programa grande en unidades más pequeñas, autocontenidas y manejables llamadas módulos. Cada módulo agrupa un conjunto de funciones y datos relacionados que realizan una tarea específica, y se diseña para tener una interfaz clara con el resto del programa. Este enfoque permite desarrollar, compilar y probar cada módulo de forma independiente. La programación modular es una evolución natural de la programación estructurada, ya que aplica sus principios de organización no solo al flujo de control, sino también a la organización física y lógica del código.

La principal diferencia radica en el nivel de organización y reutilización. Mientras la programación estructurada se centra en el flujo lógico dentro de un programa, la programación modular se ocupa de la estructura del programa en sí, dividiéndolo en partes con interdependencias mínimas. En la práctica, lenguajes como C implementan la programación modular mediante el uso de archivos fuente (.c) y cabeceras (.h), donde cada archivo puede considerarse un módulo que agrupa funciones relacionadas. Este paradigma sentó las bases conceptuales para la encapsulación y la separación de preocupaciones que luego la POO llevaría al nivel de los objetos, siendo ambos predecesores clave en la evolución hacia un software más organizado y escalable.


## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

Los tres elementos que definen a un objeto en programación orientada a objetos son su identidad, su estado y su comportamiento. La identidad es la propiedad que distingue a un objeto como una entidad única, independientemente de los valores de sus atributos en un momento dado. En la práctica, esta identidad se manifiesta a través de una referencia o dirección de memoria que apunta de manera exclusiva a ese objeto, permitiendo diferenciarlo de cualquier otro, incluso si su estado es idéntico. Este concepto es fundamental para operaciones de comparación y gestión de objetos en el sistema.

El estado de un objeto está compuesto por los valores concretos de sus atributos o propiedades en un instante específico. Estos atributos, definidos en la clase del objeto, representan las características o datos que describen al objeto. El estado es dinámico y puede modificarse a lo largo del tiempo, generalmente a través de la interacción con los métodos del objeto. Por ejemplo, el estado de un objeto CuentaBancaria incluiría atributos como numeroCuenta y saldo, donde el valor de saldo puede cambiar tras una operación.

El comportamiento define las operaciones que el objeto puede realizar o a las que puede responder, implementadas a través de sus métodos. Estos métodos representan las capacidades del objeto y definen cómo interactúa con otros objetos o cómo modifica su propio estado interno. El comportamiento está íntimamente ligado al estado, ya que los métodos suelen leer o alterar los valores de los atributos. Por ejemplo, el objeto CuentaBancaria tendría métodos como depositar() o retirar(), los cuales alteran el estado del saldo. La interacción de estos tres elementos permite modelar entidades del mundo real como unidades software coherentes y autónomas.


## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Una clase es una plantilla o modelo que define la estructura y el comportamiento común para un conjunto de objetos. Específicamente, una clase declara los atributos (variables que almacenan el estado) y los métodos (funciones que definen el comportamiento) que tendrán todos los objetos creados a partir de ella. Se la puede entender como un tipo de dato abstracto definido por el programador, que actúa como un plano a partir del cual se construyen las entidades concretas en tiempo de ejecución.

No es lo mismo una clase que un objeto; son conceptos relacionados pero distintos. La clase es la definición abstracta, mientras que el objeto es una concreción o realización específica de esa clase, que ocupa memoria y posee un estado particular. Esta relación se asemeja a la que existe entre un plano arquitectónico (la clase) y una casa construida (el objeto). Un instancia es sinónimo de objeto; se refiere a la materialización concreta de una clase. Por lo tanto, se dice que un objeto es una instancia de una clase.

No todos los lenguajes orientados a objetos manejan el concepto de clase. Existen lenguajes basados en prototipos, como JavaScript, donde la orientación a objetos se logra mediante la clonación y extensión de objetos existentes (prototipos), en lugar de definirlos a partir de clases abstractas. En estos lenguajes, cualquier objeto puede servir como prototipo para crear nuevos objetos, delegando la herencia de propiedades y métodos. Sin embargo, lenguajes como Java, C++, Python y C# son lenguajes basados en clases, donde la clase es el concepto central y fundamental para la creación de objetos.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

En la mayoría de los lenguajes orientados a objetos, como Java, C# y Python, los objetos se almacenan en una región de memoria denominada heap o montón. Cuando se crea un objeto con el operador new, se reserva dinámicamente espacio en el heap para alojar sus atributos y metadatos internos. La variable que referencia al objeto (por ejemplo, MiClase obj) no contiene el objeto en sí, sino una dirección de memoria (un puntero) que apunta a su ubicación en el heap. Esta variable de referencia suele residir en una región de memoria más rápida, como el stack (pila), asociada al contexto de ejecución de un método.

No es igual en todos los lenguajes, ya que la gestión de memoria depende del diseño y del paradigma del lenguaje. En C++, por ejemplo, el programador tiene un control explícito y puede decidir si un objeto se crea en el heap (con new) o en el stack (como una variable local automática), asumiendo también la responsabilidad de liberar esa memoria manualmente. En lenguajes basados en prototipos como JavaScript, los objetos también se asignan dinámicamente en el heap, pero el modelo de memoria y los detalles de implementación pueden variar significativamente entre entornos de ejecución.

La recolección de basura es un mecanismo de gestión automática de memoria que libera el espacio ocupado por objetos que ya no son accesibles por el programa. Un recolector de basura (garbage collector) monitorea las referencias a los objetos en el heap; cuando un objeto deja de tener referencias activas (por ejemplo, porque todas sus variables de referencia han salido de ámbito o han sido asignadas a null), se marca como "basura" y su memoria se reclama automáticamente en algún momento posterior. Este mecanismo previene errores comunes como las fugas de memoria, pero introduce una sobrecarga de ejecución y reduce el control directo del programador sobre el momento exacto de la liberación de memoria.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

Un método es una función o procedimiento definido dentro de una clase que describe un comportamiento o una operación que los objetos de dicha clase pueden realizar. A diferencia de las funciones independientes de la programación procedural, un método está asociado a un objeto específico (o a la clase misma, si es estático) y generalmente opera sobre los datos internos (atributos) de ese objeto. Los métodos definen la interfaz pública a través de la cual se interactúa con el objeto, permitiendo modificar su estado, realizar cálculos o comunicarse con otros objetos. Por ejemplo, un método calcularArea() en una clase Rectangulo usaría los atributos base y altura del objeto para devolver un resultado.

La sobrecarga de métodos es una característica que permite definir múltiples métodos dentro de una misma clase con el mismo nombre, pero con diferentes listas de parámetros (diferente número, tipo u orden de los mismos). El compilador (o intérprete) determina cuál de los métodos sobrecargados debe ejecutarse en función de los argumentos proporcionados en la llamada. Esto no debe confundirse con la sobrescritura (override), que implica redefinir un método heredado en una subclase. La sobrecarga proporciona flexibilidad y legibilidad, permitiendo que una operación conceptualmente única pueda manejarse de distintas maneras según el contexto.

El propósito principal de la sobrecarga es ofrecer una interfaz coherente y intuitiva para operaciones similares. Por ejemplo, en una clase Calculadora, se podría tener varios métodos llamados sumar: uno que acepte dos enteros, otro que acepte dos números decimales y otro que acepte tres enteros. Cada implementación se adapta a los tipos de datos recibidos, pero el nombre unificado facilita su uso. Cabe destacar que la sobrecarga se resuelve en tiempo de compilación (enlaces estáticos) en lenguajes como Java y C++, basándose en la firma del método, y es una forma de polimorfismo en tiempo de compilación o polimorfismo ad-hoc.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

A continuación se presenta el ejemplo mínimo solicitado, cumpliendo con las especificaciones.

public class Punto {
    // Atributos con visibilidad por defecto (sin modificador)
    double x;
    double y;

    // Método que calcula la distancia al origen (0,0)
    public double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

Para utilizar la clase, se crea una instancia (objeto) de tipo Punto, se asignan valores a sus atributos y se invoca al método. La visibilidad por defecto de los atributos permite acceder a ellos directamente desde cualquier clase dentro del mismo paquete.

public class Main {
    public static void main(String[] args) {
        // Creación de una instancia de Punto
        Punto miPunto = new Punto();
        
        // Asignación de valores a los atributos (acceso directo)
        miPunto.x = 3.0;
        miPunto.y = 4.0;
        
        // Uso del método e impresión del resultado
        double distancia = miPunto.calculaDistanciaAOrigen();
        System.out.println("La distancia al origen es: " + distancia); // Deberá mostrar 5.0
    }
}

En este ejemplo, la clase Punto define la estructura básica de un punto en un plano bidimensional. Los atributos x e y almacenan las coordenadas, y el método calculaDistanciaAOrigen aplica el teorema de Pitágoras para calcular la distancia desde el punto definido hasta el origen de coordenadas. La clase de uso (Main) demuestra el ciclo de vida típico: instanciación con new, inicialización de estado y ejecución de un comportamiento a través de un método. El resultado impreso confirma el cálculo correcto, ya que la raíz cuadrada de (3² + 4²) es 5.


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El punto de entrada en un programa Java es el método public static void main(String[] args). Este método es invocado automáticamente por la Máquina Virtual de Java (JVM) al iniciar la ejecución del programa. Debe ser public para ser accesible desde fuera de la clase, static para que pueda ser llamado sin necesidad de crear una instancia de la clase que lo contiene, y void porque no devuelve ningún valor. El parámetro String[] args permite recibir argumentos de línea de comandos.

La palabra clave static denota que un miembro (método o atributo) pertenece a la clase en sí misma, y no a instancias individuales de la clase. Un método estático puede ser invocado directamente usando el nombre de la clase (por ejemplo, Clase.metodo()), sin necesidad de un objeto. Un atributo estático es una variable compartida por todas las instancias de la clase, actuando esencialmente como una variable global dentro del contexto de la clase. El método main debe ser estático porque la JVM necesita poder ejecutarlo antes de que exista ningún objeto.

No, static no se emplea únicamente para el método main. Se utiliza ampliamente para métodos de utilidad (como los métodos de la clase Math) o para atributos que deben mantener un estado común a todos los objetos, como contadores de instancias o constantes. Por ejemplo, un atributo static int contador que se incremente en cada constructor permite rastrear cuántos objetos de una clase se han creado.

La combinación static final se emplea para definir constantes a nivel de clase. Mientras static indica que el miembro es único para la clase, final especifica que su valor no puede modificarse después de su inicialización. Por ejemplo, public static final double PI = 3.141592; define una constante matemática que puede ser accedida globalmente a través del nombre de la clase. Esta combinación es la forma estándar en Java para definir valores constantes y compartidos, asegurando que solo exista una copia en memoria y que su valor sea inmutable.


## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Para compilar y ejecutar un programa Java desde la línea de comandos, se utilizan las herramientas javac y java proporcionadas por el JDK. Primero, se escribe el código fuente en un archivo con extensión .java, por ejemplo, HolaMundo.java. Luego, desde la terminal, se navega al directorio del archivo y se compila con el comando javac HolaMundo.java. Si no hay errores, esto genera un archivo compilado llamado HolaMundo.class. Finalmente, para ejecutar el programa, se usa el comando java HolaMundo (sin la extensión .class). Es importante que el nombre del archivo coincida con el nombre de la clase pública que contiene.

Java es un lenguaje compilado e interpretado. El compilador javac no traduce el código fuente directamente a lenguaje máquina, sino a un código intermedio llamado byte-code, que se almacena en archivos .class. Este byte-code es independiente de la arquitectura del hardware y del sistema operativo. Posteriormente, la Máquina Virtual de Java (JVM) interpreta y ejecuta este byte-code instrucción por instrucción, o lo compila justo a tiempo (JIT compilation) a código nativo para mejorar el rendimiento.

La máquina virtual (JVM) es un programa que simula una computadora abstracta y proporciona un entorno de ejecución para el byte-code de Java. Actúa como una capa intermedia entre el byte-code y el sistema operativo real, manejando tareas como la gestión de memoria, la recolección de basura y la seguridad. Su existencia es lo que permite el lema "escribir una vez, ejecutar en cualquier lugar", ya que un mismo archivo .class puede ejecutarse en cualquier sistema que tenga instalada una JVM adecuada.

El byte-code es un conjunto de instrucciones optimizadas y de bajo nivel, similar al código máquina, pero diseñado para ser ejecutado por la JVM. Los archivos .class son los contenedores de este byte-code, resultantes de la compilación. Cada archivo .class contiene el byte-code correspondiente a una clase, junto con metadatos sobre su estructura. La JVM carga estos archivos cuando son referenciados durante la ejecución, los verifica por seguridad y luego los ejecuta, combinando así las ventajas de la compilación (detección temprana de errores, optimización) con la portabilidad de la interpretación.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

En el código de la clase Punto, new es un operador de Java que solicita memoria dinámica en el heap para crear una nueva instancia (objeto) de la clase especificada. Además de reservar la memoria necesaria para almacenar los atributos del objeto, el operador new invoca automáticamente al constructor de la clase para inicializar el estado interno del objeto recién creado. La expresión new Punto() devuelve una referencia a la ubicación de memoria de ese objeto, la cual se asigna a la variable de tipo Punto.

Un constructor es un método especial dentro de una clase que se ejecuta únicamente en el momento de la creación de un objeto. Su propósito principal es inicializar los atributos del objeto con valores válidos. Un constructor se distingue por tener exactamente el mismo nombre que la clase y por no especificar ningún tipo de retorno (ni siquiera void). Si no se define explícitamente ningún constructor, Java proporciona automáticamente un constructor por defecto sin parámetros.

A continuación, se presenta un ejemplo de una clase Empleado con un constructor que recibe DNI, nombre y apellidos:

public class Empleado {
    // Atributos
    String dni;
    String nombre;
    String apellidos;

    // Constructor con parámetros
    public Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}

En este constructor, los parámetros dni, nombre y apellidos se utilizan para asignar valores a los atributos del objeto recién creado. La palabra clave this es una referencia al objeto actual y se emplea para desambiguar los atributos de la clase de los parámetros del método que tienen el mismo nombre. Para crear un objeto Empleado con este constructor, se usaría: Empleado emp = new Empleado("12345678A", "Ana", "García López");, lo que garantiza que el objeto empiece con un estado inicial definido y consistente.


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

La referencia this es una palabra clave en Java que, dentro de un método de instancia o de un constructor, hace referencia al objeto actual sobre el que se está actuando. Proporciona una manera de acceder a los atributos y métodos de la propia instancia, especialmente útil para desambiguar cuando los parámetros de un método o constructor tienen el mismo nombre que los atributos de la clase. En esencia, this actúa como un puntero implícito al objeto mismo, permitiendo distinguir entre las variables miembro y las variables locales con nombres idénticos.

No se llama igual en todos los lenguajes, aunque el concepto es común en la programación orientada a objetos. Por ejemplo, en C++ y C# también se utiliza this (aunque en C++ es un puntero, por lo que se accede con this->atributo). En Python, la referencia equivalente es self, que se debe declarar explícitamente como primer parámetro de cada método de instancia. En JavaScript, la palabra clave es this, pero su comportamiento y valor pueden variar significativamente según el contexto de ejecución, lo que la hace más compleja que en los lenguajes basados en clases.

Un ejemplo del uso de this en la clase Punto podría ser en un constructor que reciba parámetros con el mismo nombre que los atributos, para inicializarlos correctamente:

public class Punto {
    double x;
    double y;

    // Constructor que utiliza 'this' para desambiguar
    public Punto(double x, double y) {
        this.x = x;  // this.x se refiere al atributo, x al parámetro
        this.y = y;
    }

    // Método que también podría usar this, aunque aquí no es estrictamente necesario
    public void establecerOrigen() {
        this.x = 0;  // Referencia explícita al atributo x del objeto
        this.y = 0;
    }
}

En este ejemplo, this.x y this.y dentro del constructor se refieren inequívocamente a los atributos del objeto que se está construyendo, mientras que x e y solos se refieren a los parámetros pasados. Su uso mejora la claridad del código y previene errores de sombreado de variables, asegurando que los valores de los parámetros se asignen correctamente al estado interno del objeto.


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

A continuación se añade el método distanciaA a la clase Punto. Este método calcula la distancia euclidiana entre el objeto actual (this) y otro objeto de tipo Punto que se recibe como parámetro.

public class Punto {
    double x;
    double y;

    // Método que calcula la distancia al origen
    public double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    // Método que calcula la distancia a otro punto
    public double distanciaA(Punto otroPunto) {
        double diferenciaX = this.x - otroPunto.x;
        double diferenciaY = this.y - otroPunto.y;
        return Math.sqrt(diferenciaX * diferenciaX + diferenciaY * diferenciaY);
    }
}

El método distanciaA utiliza la fórmula matemática de la distancia entre dos puntos en un plano cartesiano. Para ello, calcula las diferencias entre las coordenadas x e y del punto actual (accedidas mediante this.x e this.y) y las del punto pasado como argumento (otroPunto.x y otroPunto.y). Luego, aplica el teorema de Pitágoras a estas diferencias para obtener la distancia final. El uso de this es explícito para clarificar el acceso a los atributos del objeto receptor, aunque en este contexto no sería estrictamente necesario, ya que no hay ambigüedad con los nombres de los parámetros.

Para utilizar este nuevo método, se podrían crear dos instancias de Punto y calcular la distancia entre ellas:

public class Main {
    public static void main(String[] args) {
        Punto p1 = new Punto();
        p1.x = 3.0;
        p1.y = 4.0;

        Punto p2 = new Punto();
        p2.x = 0.0;
        p2.y = 0.0;

        double distancia = p1.distanciaA(p2);
        System.out.println("Distancia entre p1 y p2: " + distancia); // Mostrará 5.0
    }
}

En este ejemplo, el método p1.distanciaA(p2) se invoca sobre el objeto p1, por lo que dentro del método distanciaA, la referencia this apuntará a p1, mientras que el parámetro otroPunto será una referencia a p2. El método demuestra cómo los objetos pueden interactuar entre sí, pasándose referencias como argumentos para realizar cálculos que involucran el estado de múltiples instancias.


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

En Java, el paso de parámetros es siempre por valor. Sin embargo, es crucial distinguir lo que se está copiando. Cuando se pasa un objeto de tipo Punto (o de cualquier clase) como parámetro, lo que se copia y pasa al método es el valor de la referencia (la dirección de memoria), no una copia del objeto en sí. Dentro del método, la variable parámetro es una copia de esa referencia, pero sigue apuntando al mismo objeto en el heap. Por lo tanto, si el método modifica los atributos del objeto a través de esa referencia (por ejemplo, otroPunto.x = 10;), los cambios sí afectan al objeto original fuera del método, porque se está operando sobre la misma ubicación de memoria.

Si, en lugar de un objeto, se recibe un tipo primitivo como int, el comportamiento es diferente. En este caso, el valor del entero se copia directamente en el parámetro del método. Cualquier modificación que se realice sobre esta variable local dentro del método (por ejemplo, parametro = 20;) no afectará para nada a la variable original que se usó en la llamada. La variable original mantendrá su valor, porque los tipos primitivos en Java se pasan por valor de manera directa, copiando el dato en sí, no una referencia.

Para ilustrar la diferencia: con un objeto, el método recibe una copia de la "dirección de la casa" y puede modificar los muebles dentro de ella, afectando a todos los que conozcan esa dirección. Con un primitivo, el método recibe una copia del "mueble" mismo, y cualquier cambio realizado sobre esa copia no altera el mueble original. Esta distinción es fundamental para entender la semántica de Java y evitar errores comunes, ya que la inmutabilidad o mutabilidad de los datos dependerá de si se trabaja con referencias a objetos o con valores primitivos directos.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

El método toString() en Java es un método definido en la clase base Object (de la cual heredan implícitamente todas las clases en Java) cuyo propósito es devolver una representación textual del objeto. Su implementación por defecto en Object suele devolver una cadena que combina el nombre de la clase y el código hash del objeto (por ejemplo, Punto@15db9742), lo cual no es informativo. Por convención, se recomienda sobrescribir (override) este método en las clases definidas por el usuario para proporcionar una descripción legible y significativa del estado del objeto, lo cual es especialmente útil para depuración, registro de logs o interfaz de usuario.

Este concepto existe en otros lenguajes orientados a objetos, aunque con nombres o implementaciones diferentes. En C++, por ejemplo, se suele sobrecargar el operador << para los flujos de salida o implementar métodos personalizados. En C#, existe el método ToString() con exactamente el mismo propósito y comportamiento, ya que también hereda de una clase base común (System.Object). En Python, el equivalente es el método __str__(), que es invocado por las funciones str() y print(). Todos estos mecanismos persiguen el mismo objetivo: ofrecer una representación de cadena legible para un objeto.

A continuación se presenta un ejemplo de cómo sobrescribir toString() en la clase Punto:

public class Punto {
    double x;
    double y;

    // Constructor
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Sobrescritura del método toString()
    @Override
    public String toString() {
        return "Punto [x=" + x + ", y=" + y + "]";
    }
}

Con esta implementación, al imprimir un objeto Punto con System.out.println(miPunto);, el compilador invocará automáticamente miPunto.toString() y mostrará una cadena como Punto [x=3.0, y=4.0]. Esto hace que la información del objeto sea inmediatamente comprensible, a diferencia de la salida por defecto. La anotación @Override es opcional pero recomendada, ya que indica al compilador que se está sobrescribiendo un método heredado, lo que ayuda a detectar errores de firma.


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

Una clase en Java o C++ comparte similitudes superficiales con un struct en C, ya que ambos son tipos de datos definidos por el usuario que permiten agrupar variables heterogéneas (atributos o miembros). Sin embargo, una clase es una entidad significativamente más rica y poderosa. La diferencia fundamental radica en que un struct en C es únicamente una agregación pasiva de datos; es una plantilla que define un formato de memoria, pero carece de comportamiento asociado intrínsecamente. Las variables de tipo struct son meros contenedores de datos a los que se accede directamente, sin encapsulación ni mecanismos para operar sobre ellos de forma inherente.

Para que un struct se aproximara a una clase y sus variables fueran verdaderas instancias, necesitaría al menos tres elementos clave. Primero, le falta comportamiento asociado: métodos o funciones que operen sobre los datos del struct y que estén vinculados lógicamente a él. En C, las funciones que manipulan un struct son externas y separadas, mientras que en una clase los métodos están definidos dentro de su ámbito. Segundo, le falta encapsulación: en un struct en C, todos los miembros son accesibles públicamente sin restricción. Una clase permite ocultar sus datos internos (con private o protected) y exponer solo una interfaz controlada mediante métodos públicos, protegiendo así la integridad del estado.

Tercero, le faltan mecanismos de inicialización y limpieza automáticos, como constructores y destructores. En C, una variable de tipo struct debe inicializarse manualmente campo por campo (a menos que se use inicialización por lista), y no hay un método automático que se ejecute al crearla o destruirla. Una clase tiene constructores que garantizan un estado inicial válido. Además, conceptos como herencia y polimorfismo, que permiten crear jerarquías y comportamientos dinámicos, están completamente ausentes en los struct de C. En resumen, un struct es un paso hacia la abstracción de datos, pero una clase añade la abstracción de comportamiento, el ocultamiento de información y las relaciones entre tipos, formando un paradigma completo de modelado.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

Para emular la clase Punto de Java utilizando un struct en C, se puede definir una estructura que contenga los mismos atributos y una función externa que reciba un puntero a esa estructura como primer parámetro. Esta función actuaría como un método, operando sobre los datos de la instancia específica. En C, no existe el concepto de método integrado en la estructura, por lo que la función debe ser declarada por separado y recibir explícitamente la referencia al objeto sobre el que actúa.

#include <math.h>

// Definición del "struct" que equivale a la clase
struct Punto {
    double x;
    double y;
};

// "Método" que calcula la distancia al origen
double Punto_calculaDistanciaAOrigen(struct Punto* self) {
    return sqrt(self->x * self->x + self->y * self->y);
}

En este código, la función Punto_calculaDistanciaAOrigen emula el método de instancia. El parámetro struct Punto* self cumple el papel de la referencia this en Java: es una referencia explícita al objeto sobre el cual se actúa. A diferencia de Java, donde this es una palabra clave implícita y automática, en C se debe pasar manualmente un puntero a la estructura como argumento, convencionalmente llamado self o this. Todos los accesos a los atributos dentro de la función se realizan a través de este puntero (por ejemplo, self->x).

Para usar esta emulación, se crea una variable de tipo struct Punto, se inicializan sus campos y se llama a la función pasando su dirección:

int main() {
    struct Punto p1;
    p1.x = 3.0;
    p1.y = 4.0;

    double distancia = Punto_calculaDistanciaAOrigen(&p1);
    // distancia será 5.0
    return 0;
}

Lo que ha pasado con this es que se ha hecho explícito y manual. En la orientación a objetos pura, el lenguaje oculta la referencia al objeto actual y la proporciona automáticamente en cada método. En esta emulación en C, el programador debe gestionar esa referencia de forma explícita, pasando el puntero a cada "método". Esto revela la magia subyacente: los métodos de objeto son esencialmente funciones que reciben un primer parámetro oculto (el puntero this), y la encapsulación y la sintaxis de puntos (objeto.metodo()) son azúcar sintáctico que el compilador traduce a este mecanismo más primitivo.
