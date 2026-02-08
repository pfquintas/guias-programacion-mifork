<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

La encapsulación busca agrupar y empaquetar los datos (atributos) y los comportamientos (métodos) que operan sobre esos datos en una única unidad lógica, la clase. Este empaquetamiento establece los límites claros de un objeto y mantiene juntos los elementos que están íntimamente relacionados. La ocultación de información (o de implementación) es un aspecto específico y crucial de la encapsulación que se enfoca en restringir el acceso a los detalles internos de esa unidad, exponiendo solo lo que es estrictamente necesario a través de una interfaz pública y bien definida. Mientras que la encapsulación es el concepto de "caja", la ocultación es el principio de "qué mostrar de la caja".

Algunas ventajas clave de la ocultación de información son:

- Prevención de modificaciones inapropiadas: al declarar los atributos como private, se impide que otras clases modifiquen su estado de manera directa y arbitraria, lo que protege la integridad de los datos y garantiza que el objeto siempre se encuentre en un estado válido y consistente.

- Reducción del acoplamiento: al ocultar la implementación, las clases externas no dependen de los detalles internos de una clase, sino solo de su interfaz pública. Esto permite modificar la implementación interna (por ejemplo, cambiar el tipo de un dato o el algoritmo de un método) sin afectar a ningún otro componente del sistema que utilice la clase.

- Facilita el mantenimiento y la depuración: dado que el acceso a los datos está canalizado a través de métodos (como setters), se puede centralizar la validación, el registro de actividad (logging) o la notificación de cambios en un solo lugar, haciendo el código más robusto y más fácil de inspeccionar y corregir.

Para un programador con experiencia en C, puede entenderse este concepto como una evolución de la separación entre el archivo de cabecera (.h) y el de implementación (.c). El encabezado declara las funciones públicas (la interfaz), mientras que el archivo .c contiene la implementación concreta y los detalles privados que el usuario no necesita conocer. En Java, esta separación y protección se aplica de manera más estricta y a nivel de objeto, utilizando palabras clave del lenguaje como private y public.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

La interfaz pública de un objeto o clase en POO se entiende como el conjunto de métodos y atributos declarados con acceso public que están disponibles para ser utilizados por cualquier otra clase o componente del sistema. Constituye el contrato o la faceta visible que el objeto ofrece al mundo exterior, definiendo qué operaciones se pueden realizar con él, pero sin revelar cómo se implementan dichas operaciones internamente. Es la única vía de comunicación y manipulación autorizada para los clientes de la clase, actuando como un punto de interacción bien definido y estable.

La relación entre la interfaz pública y la ocultación de información es intrínseca y complementaria. La ocultación de información se consigue precisamente al diseñar cuidadosamente una interfaz pública minimalista y funcional, mientras todos los detalles de implementación (atributos private, métodos auxiliares, estructuras de datos internas) permanecen inaccesibles desde el exterior. La interfaz pública es, por tanto, el mecanismo que permite un uso controlado y seguro del objeto, a la vez que se mantiene estrictamente oculta su complejidad interna.

Desde la perspectiva de un programador con experiencia en C, puede trazarse un paralelo con el uso de bibliotecas. El programador utiliza las funciones declaradas en un archivo .h (la interfaz pública) sin necesidad de conocer ni modificar el código fuente de la implementación en los archivos .c (los detalles ocultos). En Java, este principio se aplica de manera más rigurosa y granular a nivel de cada objeto, utilizando los modificadores de acceso del lenguaje. La interfaz pública bien diseñada es lo que permite lograr el bajo acoplamiento: las otras clases dependen solo de esta interfaz estable, no de los detalles volátiles que hay detrás.

En consecuencia, una interfaz pública robusta y coherente es la piedra angular que hace viable la ocultación de información. Permite que un objeto sea tratado como una "caja negra", donde el usuario confía en su comportamiento especificado por la interfaz, mientras el desarrollador tiene la libertad de refactorizar o optimizar la implementación interna sin romper el código que depende de ella. Esta separación entre el "qué" (interfaz) y el "cómo" (implementación oculta) es fundamental para la construcción de sistemas modulares y mantenibles.


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

Hay que ser conscientes y diseñar con cuidado la interfaz pública de una clase porque ésta establece un contrato formal con todo el código cliente que la utilice. Una vez que otras clases o módulos dependen de esa interfaz, cualquier cambio en ella puede obligar a modificar todas las partes del sistema que la invocan, lo que conlleva un alto costo en esfuerzo de mantenimiento y un riesgo elevado de introducir errores. Un diseño cuidadoso implica exponer solo lo absolutamente necesario, asegurando que los métodos públicos sean coherentes, estables y cumplan una responsabilidad única y clara, para así minimizar futuras modificaciones.

La respuesta a si es fácil cambiarla es un claro no. Cambiar la interfaz pública de una clase existente y ampliamente utilizada es una de las operaciones más costosas y propensas a errores en el desarrollo de software. Un cambio tan simple como modificar la firma de un método público (parámetros, tipo de retorno) o eliminar un método que otros usan, provoca que el código dependiente deje de compilar o falle en tiempo de ejecución. Esto contrasta con la modificación de los detalles privados de implementación, que, gracias a la ocultación de información, puede realizarse con relativa facilidad y sin impacto externo.

Para un programador procedente de C, puede entenderse este problema como análogo a cambiar la firma de una función declarada en un archivo de cabecera (.h) que es incluido por decenas de otros archivos fuente. En Java, la situación es incluso más estructurada debido a la fuerte tipificación y al sistema de clases. La dificultad para cambiar la interfaz pública refuerza la necesidad de un diseño inicial reflexivo y de técnicas como el uso de interfaces abstractas o el patrón de diseño Façade, que pueden proporcionar una capa de abstracción adicional y más estable. En resumen, la interfaz pública debe considerarse un compromiso a largo plazo, mientras que la implementación privada conserva la flexibilidad para evolucionar.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las invariantes de clase son condiciones o reglas lógicas que definen un estado consistente y válido para los objetos de una clase, y que deben preservarse en todo momento (excepto, transitoriamente, durante la ejecución interna de un método). Se trata de propiedades que son verdaderas para el objeto desde el momento de su construcción (después del constructor) hasta el final de su vida útil. Un ejemplo típico es que, en una clase CuentaBancaria, el atributo saldo nunca debe ser negativo; o que en una clase Fecha, los valores de día, mes y año deben corresponder a una fecha calendario real.

La ocultación de información ayuda de manera fundamental a garantizar y mantener estas invariantes de clase. Al declarar los atributos como private y forzar que toda interacción con el estado interno del objeto se realice únicamente a través de métodos públicos (los setters, constructores y otros métodos de negocio), se centraliza y controla el punto donde se puede modificar dicho estado. Esto permite que, dentro de esos métodos, se incluyan las validaciones, comprobaciones y lógica necesarias para asegurar que, tras cualquier operación, las invariantes de la clase se restauran y se cumplen.

Para un programador con experiencia en C, puede verse como la diferencia entre permitir que cualquier parte del programa modifique directamente los campos de una struct (lo que haría imposible garantizar reglas globales) versus tener funciones específicas que, cada vez que manipulan la estructura, aplican las comprobaciones necesarias. En Java, la ocultación de información (atributos private) hace que este control sea obligatorio y aplicado por el propio lenguaje. Sin esta protección, cualquier clase externa podría, por ejemplo, asignar un valor negativo directamente al saldo, rompiendo la invariante fundamental y llevando al objeto a un estado corrupto e inconsistente, mucho más difícil de detectar y depurar.

En resumen, la ocultación de información es el mecanismo que permite a una clase ser la única responsable de su propia integridad. Al ocultar los datos, la clase se asegura de que todas las modificaciones pasen por su filtro de validación, haciendo cumplir las invariantes de forma sistemática. Esto convierte a cada objeto en una entidad autónoma y fiable, cuyo comportamiento es predecible y seguro para el resto del sistema.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

public class Punto {
    // Atributos privados: ocultación de información
    private double x;
    private double y;

    // Constructor público: parte de la interfaz
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Método público para calcular distancia: parte de la interfaz
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    // Getters públicos: parte de la interfaz controlada
    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }

    // Setters públicos con validación básica (opcional): parte de la interfaz
    public void setX(double x) {
        this.x = x;
    }

    public void setY(double y) {
        this.y = y;
    }
}

La interfaz pública de la clase Punto está compuesta por todos sus elementos declarados con la palabra clave public: el constructor Punto(double x, double y), los métodos calcularDistanciaAOrigen(), getX(), getY(), setX(double x) y setY(double y). Estos elementos son los únicos accesibles desde cualquier otra clase y definen cómo se puede crear un objeto Punto, cómo se puede consultar o modificar su estado de manera controlada y qué operaciones se pueden realizar con él. El resto de los detalles, específicamente los valores almacenados en los atributos x e y, permanecen ocultos.

Los modificadores public y private son fundamentales para implementar la ocultación de información. El modificador public significa que el elemento (atributo, método o constructor) es accesible desde cualquier otra clase en cualquier paquete. En la clase Punto, los métodos públicos forman la interfaz que se ofrece al exterior. En contraste, el modificador private restringe el acceso al elemento exclusivamente a la propia clase donde se declara. Los atributos x e y son privados, por lo que ninguna clase externa puede leerlos o modificarlos directamente mediante, por ejemplo, miPunto.x = 5;. Este acceso debe realizarse forzosamente a través de los métodos públicos (getters/setters), que actúan como guardianes.

Para un programador con base en C, esta distinción puede asociarse a la idea de exponer solo prototipos de funciones en un archivo .h (público) mientras se mantiene la implementación y las variables globales estáticas dentro del archivo .c (privadas). La ocultación garantiza que la representación interna de las coordenadas (dos double) pueda cambiarse en el futuro (por ejemplo, a un array de dos elementos) sin afectar a las clases que usan Punto, siempre que se mantenga la interfaz pública. Además, los setters permitirían añadir validaciones si fuera necesario, aunque en este ejemplo son básicos.


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

En Java, los modificadores public y private se pueden aplicar a miembros de una clase, que incluyen sus atributos (campos), métodos, constructores y clases o interfaces anidadas. Su propósito es controlar la visibilidad y el acceso desde otras partes del código. Es importante destacar que estos modificadores no se aplican a variables locales declaradas dentro de un método o bloque, ya que su ámbito de visibilidad está siempre limitado al propio bloque donde se definen, independientemente de cualquier modificador.

La aplicación más común es en los atributos y métodos de una clase. Un miembro declarado como public es accesible desde cualquier otra clase, paquete o módulo, siempre que la clase contenedora sea también accesible. Por el contrario, un miembro private solo es visible y utilizable dentro de la misma clase en la que se define, lo que lo hace ideal para encapsular los detalles de implementación. También es posible aplicar estos modificadores a constructores: un constructor private impide que se pueda instanciar la clase desde fuera, un patrón usado en el Singleton, mientras que un constructor public permite la creación libre de objetos.

Para un programador con experiencia en C, puede resultar útil la analogía con las funciones y variables static declaradas en un archivo .c. Una función static en C es "privada" a ese archivo, similar a un método private en Java que es accesible solo dentro de su clase. Una función no estática (pública) en C, declarada en el .h, es análoga a un método public en Java. Sin embargo, Java formaliza y extiende este control al nivel de cada miembro individual dentro de una clase, ofreciendo una granularidad más fina que en C, donde la ocultación se maneja principalmente a nivel de archivo.

Además, estos modificadores pueden aplicarse a clases e interfaces en sí mismas, pero con una regla importante: una clase de nivel superior (no anidada) solo puede tener el modificador public o el modificador por defecto (sin especificar). No puede declararse como private. La palabra clave private sí es válida para clases e interfaces anidadas (miembro de otra clase), restringiendo su uso únicamente a la clase que la contiene. Esta jerarquía de visibilidad asegura que el principio de ocultación de información pueda aplicarse de manera coherente en todos los niveles de la estructura del programa.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

En Programación Orientada a Objetos, la visibilidad no se limita únicamente a pública y privada. Existen niveles intermedios que ofrecen un control de acceso más granular, adaptándose a relaciones específicas entre clases, como la herencia o la agrupación por paquetes. Estos niveles varían entre lenguajes, reflejando diferentes filosofías de diseño sobre cómo se debe estructurar y compartir la información en un sistema.

En Java, además de public (acceso desde cualquier lugar) y private (acceso solo dentro de la misma clase), existen dos modificadores más: protected y el modificador por defecto (también llamado package-private). El modificador protected permite el acceso desde la misma clase, las subclases (herederas) y cualquier clase del mismo paquete, siendo fundamental para habilitar el acceso controlado en jerarquías de herencia. El modificador por defecto, que se aplica al no escribir ningún modificador explícito, restringe el acceso únicamente a las clases que pertenecen al mismo paquete, promoviendo una visibilidad a nivel de módulo o agrupación lógica. Esta granularidad permite un diseño más matizado que la simple dicotomía público/privado.

En otros lenguajes la implementación varía. Por ejemplo, en C++ existen los mismos tres modificadores (public, private, protected), pero no existe un análogo directo al modificador por defecto de paquete de Java. En su lugar, C++ utiliza el concepto de amistad (friend) para otorgar acceso privilegiado a funciones o clases específicas, rompiendo deliberadamente la encapsulación de manera controlada. En C#, además de public, private y protected, existen variantes como internal (acceso dentro del mismo ensamblado, similar al paquete en Java) y protected internal (combinación de ambos). Python adopta un enfoque diferente, basado en convenciones: no hay palabras clave estrictas, sino que se usa un guion bajo simple (_atributo) para indicar "protegido" por convención y doble guion bajo (__atributo) para producir un name mangling que simula ser privado.

Para un programador con experiencia en C, estas abstracciones pueden entenderse como extensiones sofisticadas de la idea básica de ocultar información. Mientras que en C la visibilidad se gestiona principalmente a nivel de archivo (con static), los lenguajes orientados a objetos ofrecen mecanismos integrados para controlar el acceso en función de relaciones más complejas, como la herencia (protected) o la modularidad lógica (package-private). Estos niveles adicionales buscan equilibrar la necesidad de encapsulación con la flexibilidad requerida para construir jerarquías y componentes reutilizables sin romper la integridad del diseño.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

La respuesta correcta es que los miembros de instancia privados de un objeto están ocultos para (a) otras clases, pero no están ocultos para (b) otras instancias de la misma clase. Una instancia puede acceder directamente a los atributos privados de otra instancia del mismo tipo, porque la visibilidad private se define a nivel de clase, no a nivel de objeto. Esto permite que los métodos de una clase puedan operar sobre los datos internos de cualquier objeto de esa clase, lo que es fundamental para implementar funcionalidades que requieren comparar o combinar estados entre objetos.

Un ejemplo que ilustra esto es añadir el método calcularDistanciaAPunto(Punto otro) a la clase Punto. Dentro de este método, el objeto actual (this) puede acceder directamente a los atributos privados x e y del objeto recibido como parámetro (otro), porque ambos pertenecen a la misma clase Punto. La implementación sería:

public double calcularDistanciaAPunto(Punto otro) {
    double diffX = this.x - otro.x; // Acceso directo a otro.x (privado)
    double diffY = this.y - otro.y; // Acceso directo a otro.y (privado)
    return Math.sqrt(diffX * diffX + diffY * diffY);
}

En este código, otro.x y otro.y son accesibles aunque sean privados, porque la referencia otro es de tipo Punto y el acceso se realiza desde dentro de un método de la misma clase Punto. Este diseño es intencional y útil: permite que la lógica de manipulación de los datos encapsulados se mantenga concentrada dentro de la clase, sin necesidad de exponer getters para operaciones internas. Para un programador con experiencia en C, puede pensarse como una función que opera sobre dos estructuras del mismo tipo y que, al estar definida en el mismo módulo, tiene acceso completo a los campos de ambas.

Esta regla refuerza el principio de que la encapsulación y la ocultación en Java protegen la información de accesos externos no autorizados (de otras clases), pero confían en que la propia clase es responsable de manejar su información privada de manera coherente, independientemente de en qué instancia específica resida esa información. Esto es clave para mantener la integridad y simplificar la implementación de operaciones entre objetos del mismo tipo.


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

Los métodos "getter" y "setter" son métodos públicos definidos en una clase que proporcionan un acceso controlado a los atributos privados de un objeto. Un getter (o método de acceso) es un método que devuelve el valor actual de un atributo privado, típicamente con un nombre como getAtributo(). Un setter (o método de modificación) es un método que permite cambiar el valor de un atributo privado, usualmente con un nombre como setAtributo(valor), y que puede incluir validaciones para asegurar la integridad del dato. Estos métodos forman la interfaz pública controlada que permite leer y modificar el estado interno sin violar el principio de encapsulación.

Su función principal es implementar la ocultación de información de manera práctica. Al hacer que los atributos sean privados y exponer solo estos métodos, se evita el acceso directo a los datos, lo que permite a la clase mantener el control absoluto sobre cómo se lee y, sobre todo, cómo se escribe su estado interno. Por ejemplo, un setter puede rechazar valores negativos para un saldo bancario, registrar cambios o lanzar excepciones, garantizando así que se mantengan las invariantes de clase. Este nivel de control sería imposible si los atributos fueran públicos y cualquier parte del código pudiera modificarlos arbitrariamente.

Para un programador con experiencia en C, puede entenderse como una evolución del manejo de estructuras. En lugar de acceder directamente a los campos de una struct (ej: punto.x), se definen funciones específicas (ej: obtenerX(&punto) y asignarX(&punto, valor)). En Java, este patrón se estandariza y el lenguaje lo respalda, convirtiéndose en una convención fundamental. Los getters y setters, aunque pueden parecer redundantes al principio, son la puerta de entrada para futuras mejoras: permiten cambiar la representación interna de los datos (por ejemplo, almacenar coordenadas polares en lugar de cartesianas) sin afectar el código cliente, ya que este solo interactúa con los métodos.

En resumen, los getters y setters son la materialización del principio de que una clase debe exponer qué hace (a través de su interfaz pública) y ocultar cómo lo hace (sus datos privados). No son un fin en sí mismos, sino un mecanismo para lograr un diseño robusto, mantenible y flexible. Su uso correcto separa claramente la interfaz de la implementación, protegiendo la coherencia del objeto y facilitando la evolución del software.


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

No, cuando se habla de que la ocultación de información mejora la "seguridad" en el contexto de la programación orientada a objetos, no se refiere principalmente a la seguridad contra ataques externos o "hackeos" (seguridad informática a nivel de sistema). En su lugar, el término se refiere a la seguridad, integridad y robustez del diseño del software en sí mismo. Es una medida de protección contra errores de programación, mal uso accidental y la corrupción del estado interno de los objetos por parte de otros componentes del mismo programa desarrollado por el propio equipo.

La "seguridad" que proporciona la ocultación se manifiesta al evitar que el código cliente (otras partes de la aplicación) ponga a los objetos en estados inconsistentes o inválidos. Al restringir el acceso directo a los datos y forzar que todas las modificaciones pasen por métodos públicos (como setters con validación), se garantiza que las invariantes de clase se mantengan. Por ejemplo, una clase Temperatura podría tener un setter que rechace valores por debajo del cero absoluto, "protegiendo" así la integridad física del dato. Sin esta protección, cualquier módulo podría asignar un valor absurdo directamente, llevando a cálculos erróneos y fallos difíciles de rastrear.

Para un programador con experiencia en C, esta idea se relaciona con el beneficio de usar funciones en lugar de manipular variables globales directamente: las funciones pueden imponer precondiciones. La ocultación en Java lleva esto al nivel de cada objeto, formalizando el control. Mientras que la seguridad contra intrusiones ("hacking") depende de mecanismos como la validación de entradas, el cifrado y los controles de acceso del sistema, la "seguridad" por ocultación es una defensa contra los bugs y la fragilidad del diseño, haciendo el sistema más predecible, mantenible y menos propenso a errores humanos durante el desarrollo. Es, en esencia, seguridad frente a uno mismo y frente a la evolución descontrolada del código.


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

La diferencia fundamental radica en a qué entidad pertenecen y cómo se accede a ellos. Un miembro de instancia (atributo o método no estático) pertenece a cada objeto individual creado a partir de la clase. Cada instancia tiene su propia copia independiente de los atributos de instancia, y los métodos de instancia operan sobre el estado específico del objeto (this). En cambio, un miembro de clase (declarado con static) pertenece a la clase en sí misma, no a ningún objeto particular. Existe una única copia del atributo estático, compartida por todas las instancias de la clase, y los métodos estáticos se invocan sobre la clase y no pueden acceder directamente a miembros de instancia (no tienen this).

Sí, los miembros de clase (estáticos) también se pueden y deben ocultar mediante los mismos modificadores de acceso (private, protected, etc.), aplicando así el principio de encapsulación a nivel de clase. Un atributo estático private solo es accesible desde dentro de la clase, típicamente a través de métodos estáticos públicos que actúan como su interfaz controlada. Esto es crucial para proteger datos globales compartidos por toda la aplicación que gestiona la clase. Por ejemplo, un contador estático privado de instancias creadas solo debería modificarse dentro del constructor de la clase, y su lectura podría ofrecerse mediante un getter estático público.

Para un programador con base en C, puede entenderse esta distinción comparando una variable global static dentro de un archivo (miembro de clase privado) con una variable automática dentro de una función (miembro de instancia). La encapsulación de miembros estáticos evita los problemas clásicos de las variables globales indiscriminadas. Ocultar un método estático también es común para encapsular lógica de utilidad interna de la clase que no debe ser expuesta. En resumen, la encapsulación y la ocultación de información son principios que se aplican por igual a miembros de instancia y de clase, protegiendo la coherencia tanto del estado individual de cada objeto como del estado global gestionado por la clase.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Sí, tiene sentido y es una práctica útil en escenarios específicos donde se desea controlar estrictamente cómo y cuándo se crean las instancias de una clase. Un constructor privado impide que otras clases utilicen el operador new para instanciar objetos directamente desde fuera, lo que fuerza a usar mecanismos alternativos proporcionados por la propia clase. Esto refuerza la encapsulación al convertir a la clase en la única responsable de su propio ciclo de vida, gestionando la creación de sus objetos de acuerdo a reglas internas.

Los casos de uso más comunes son la implementación del patrón Singleton (donde solo puede existir una única instancia de la clase, accesible a través de un método estático como getInstance()), los patrones de Factory Method o Factory estática (donde la creación de objetos es compleja o debe seguir lógica específica, y se exponen métodos públicos estáticos que internamente llaman al constructor privado), y las clases de utilidad que solo contienen métodos estáticos (como Math en Java), donde un constructor privado evita la instanciación completamente, ya que no tiene sentido crear objetos de esa clase.

Para un programador con experiencia en C, puede verse como análogo a hacer que una función de inicialización de una estructura sea la única manera válida de obtener una instancia de esa estructura, mientras se oculta el mecanismo de reserva de memoria o la lógica de preparación. En Java, el constructor privado es una herramienta del lenguaje que formaliza este control. Por lo tanto, lejos de carecer de sentido, es una técnica de diseño poderosa que permite implementar ciertos patrones y garantizar invariantes de creación, demostrando que la encapsulación puede aplicarse no solo a los datos, sino también al propio proceso de construcción de los objetos.


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

Los miembros de clase en Java se indican mediante la palabra clave static. Esta palabra clave se coloca antes del tipo de dato en la declaración de un atributo o antes del tipo de retorno en la declaración de un método. Los atributos estáticos existen como una única copia compartida por todas las instancias de la clase, y los métodos estáticos se invocan sobre la clase misma, sin necesidad de una instancia.

Para extender la clase Punto con miembros de clase que rastreen los valores máximos de x e y establecidos en cualquier instancia, se deben declarar dos atributos estáticos privados. Luego, en los setters y el constructor (los únicos lugares donde se pueden modificar x e y), se debe actualizar estos máximos cada vez que se asigne un nuevo valor. Finalmente, se proporcionan getters estáticos públicos para consultar estos máximos. Un ejemplo sería:

public class Punto {
    private double x;
    private double y;
    // Miembros de clase (estáticos) privados
    private static double maxX = Double.MIN_VALUE;
    private static double maxY = Double.MIN_VALUE;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        // Actualizar máximos al crear un nuevo punto
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    public void setX(double x) {
        this.x = x;
        if (x > maxX) maxX = x; // Actualizar el máximo estático
    }
    // Método setY similar para maxY...

    // Getters de instancia (getX, getY)...
    // Métodos de clase (estáticos) públicos para acceder a los máximos
    public static double getMaxX() {
        return maxX;
    }
    public static double getMaxY() {
        return maxY;
    }
}

En este diseño, maxX y maxY están encapsulados a nivel de clase (son private static). Ninguna clase externa puede modificarlos directamente, lo que garantiza que solo se actualicen de manera controlada a través de la lógica interna de Punto (en el constructor y los setters). Los métodos getMaxX() y getMaxY() son public static, formando parte de la interfaz pública de la clase y permitiendo consultar estos valores sin necesidad de tener una instancia concreta (ej: Punto.getMaxX()). Esto ilustra que la ocultación de información se aplica con igual rigor a los miembros de clase, protegiendo datos compartidos globales detrás de una interfaz controlada.


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

public static Punto crearPuntoRedondeado(double x, double y) {
    int xRedondeado = (int) Math.round(x);
    int yRedondeado = (int) Math.round(y);
    return new Punto(xRedondeado, yRedondeado);
}

Sí, se ha utilizado la palabra clave static. El método es estático porque es un método de fábrica (o factoría) que pertenece a la clase Punto en sí misma, no a una instancia particular. Su propósito es encapsular la lógica de creación alternativa (el redondeo) y proporcionar una forma más expresiva y controlada de construir objetos Punto que el constructor estándar. Al ser estático, se puede invocar directamente sobre la clase sin necesidad de tener un objeto preexistente, por ejemplo: Punto p = Punto.crearPuntoRedondeado(3.7, 2.2);.

Este método aprovecha el constructor de la clase (que se asume que toma dos parámetros double) para finalmente crear la instancia. La encapsulación se mantiene, ya que el constructor puede seguir siendo público o incluso privado si se desea forzar el uso de este método factoría. El método factoría actúa como una interfaz pública adicional que oculta el detalle de transformación (redondeo) y devuelve un objeto listo para usar, cumpliendo con el principio de ocultación de información al mantener esta lógica de pre-procesamiento dentro de la clase y no en el código cliente.


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

public class Punto {
    // Implementación interna cambiada: array privado
    private double[] coordenadas = new double[2]; // índice 0: x, índice 1: y

    // La interfaz pública del constructor se mantiene igual
    public Punto(double x, double y) {
        this.coordenadas[0] = x;
        this.coordenadas[1] = y;
    }

    // Los getters mantienen la misma firma pública
    public double getX() {
        return coordenadas[0];
    }

    public double getY() {
        return coordenadas[1];
    }

    // Los setters mantienen la misma firma pública
    public void setX(double x) {
        this.coordenadas[0] = x;
    }

    public void setY(double y) {
        this.coordenadas[1] = y;
    }

    // Método calcularDistanciaAOrigen mantiene su firma y lógica,
    // pero ahora accede al array interno.
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coordenadas[0] * coordenadas[0] + 
                         coordenadas[1] * coordenadas[1]);
    }
}

La clave del cambio es que la interfaz pública de la clase permanece intacta. Los métodos getX(), getY(), setX(double), setY(double), el constructor Punto(double, double) y calcularDistanciaAOrigen() mantienen exactamente la misma firma (nombre, parámetros y tipo de retorno) que antes. Cualquier código externo que utilice la clase Punto seguirá compilando y funcionando sin necesidad de modificación alguna, ya que solo interactúa con estas firmas públicas. La encapsulación y la ocultación de información han logrado su objetivo: se ha cambiado completamente la representación interna (de dos double independientes a un array) sin impactar a los clientes de la clase.

Internamente, ahora se utiliza un array private double[] coordenadas para almacenar los valores. Todos los métodos públicos actúan como adaptadores: traducen las operaciones basadas en x e y a operaciones sobre los índices 0 y 1 del array. Esto demuestra el poder de la encapsulación. Para un programador con experiencia en C, es análogo a cambiar los campos de una struct pero mantener las mismas funciones de interfaz que manejan la nueva representación de manera transparente para el usuario.


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

No, no es mejor declararlo público, incluso si tiene getter y setter. La convención más habitual y la práctica sólida en Java es declarar todos los atributos como private por defecto, sin excepción. Esto se considera una regla fundamental de diseño orientado a objetos y encapsulación. La exposición de atributos como public está fuertemente desaconsejada, ya que rompe el control que la clase tiene sobre su propio estado y anula los beneficios de la ocultación de información.

La razón principal es que los getters y setters públicos, a pesar de proporcionar acceso, mantienen a la clase en control del proceso. Un setter puede incluir validación, lógica de notificación o sincronización antes de modificar el valor real. Un getter puede calcular un valor sobre la marcha, devolver una copia defensiva de un objeto mutable o incluso retrasar la inicialización. Si el atributo es público, estos mecanismos de control son imposibles de implementar posteriormente sin cambiar la interfaz pública y romper el código existente. La convención de atributos privados con acceso a través de métodos es universal en Java para cualquier atributo que no sea una constante estática inmutable (public static final).

Esto tiene una relación directa y crucial con las invariantes de clase. Las invariantes son condiciones que deben ser verdaderas para el estado del objeto. Si un atributo es público, cualquier parte del código puede modificarlo arbitrariamente, posiblemente violando esas invariantes y dejando al objeto en un estado inconsistente y corrupto. Los setters privados (o la lógica dentro de los métodos) son el único lugar donde se puede y debe garantizar que, tras cualquier modificación, las invariantes se restauran. Por tanto, declarar los atributos como privados no es una mera convención estilística, sino una necesidad técnica para garantizar la integridad y corrección de los objetos, que es el núcleo de la programación orientada a objetos robusta.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

Una clase inmutable es aquella cuyas instancias, una vez creadas, no pueden modificar su estado interno. Todos los datos que representan su estado (atributos) se declaran como final y private, y no se proporcionan métodos que permitan cambiar su valor después de la construcción. La única manera de obtener un objeto con un estado diferente es crear una nueva instancia. Ejemplos clásicos en Java son String y las clases envoltorio como Integer.

Un método modificador es cualquier método que altera el estado interno del objeto sobre el que se invoca. Un "setter" tradicional (setX(valor)) es el tipo más común de método modificador, ya que cambia directamente el valor de un atributo. Sin embargo, no todos los métodos modificadores son setters. Un método como depositar(double monto) en una clase CuentaBancaria modifica el estado (el saldo), pero no es un simple setter, ya que implica lógica de negocio (sumar, validar) en lugar de una asignación directa.

Las ventajas de una clase inmutable son significativas:

- Seguridad y simplicidad en entornos concurrentes: al no poder cambiar su estado, los objetos inmutables pueden ser compartidos libremente entre múltiples hilos sin necesidad de sincronización, eliminando riesgos de condiciones de carrera.

- Fiabilidad y predictibilidad: una vez creado, el objeto es siempre válido y su estado es conocido. No hay riesgo de que otra parte del código lo corrompa, lo que facilita el razonamiento sobre el programa y la depuración.

- Reutilización segura: pueden almacenarse en caché y compartirse sin preocupaciones por efectos laterales, como se hace internamente con el pool de cadenas de String.

Para un programador con experiencia en C, puede verse como el equivalente a usar datos constantes (const) a nivel de objeto completo, pero con el beneficio añadido de que el lenguaje garantiza la inmutabilidad. La inmutabilidad es una forma extrema y muy beneficiosa de encapsulación, donde no solo se oculta la implementación, sino que se elimina por completo la posibilidad de modificación después de la construcción.


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

No, no es recomendable incluir métodos "setter" siempre y como convención automática. La decisión de proporcionar un setter debe ser un diseño deliberado basado en la semántica y las responsabilidades de la clase. La inclusión indiscriminada de setters para todos los atributos debilita la encapsulación y puede violar el principio de que una clase debe controlar rigurosamente su estado interno. En muchos casos, la capacidad de modificar un atributo después de la construcción no tiene sentido conceptual o es perjudicial para la integridad del objeto.

Existen escenarios claros donde los setters son inapropiados. Para las clases inmutables, por definición, no debe haber ningún método modificador, incluidos setters. Para atributos que forman parte de la identidad esencial de un objeto (como un id único o la fecha de creación), modificarlos podría corromper la lógica del sistema. Además, si un atributo debe mantener una invariante compleja con otros atributos, exponer un setter individual haría imposible garantizar esa coherencia; en su lugar, se debe proporcionar un método de negocio que modifique el estado de manera controlada y atómica (por ejemplo, transferirSaldo(...) en lugar de setters individuales para dos saldos).

La práctica recomendada es partir de la inmutabilidad y solo añadir setters cuando exista un requerimiento claro y se pueda mantener la integridad del objeto. Incluso entonces, los setters deben incluir validación robusta. Para un programador con experiencia en C, esto equivale a preguntarse si una variable de una estructura debería ser modificable por cualquier función, o si el cambio debe canalizarse a través de una función específica que garantice la coherencia. En resumen, los setters no son una convención obligatoria, sino una herramienta que debe usarse con juicio, priorizando siempre el diseño de objetos robustos y autocontenidos sobre la mera exposición de datos.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

La clase String en Java es inmutable. Una vez creado un objeto String, su secuencia de caracteres no puede ser alterada. Cualquier operación que parezca modificar una cadena, como toUpperCase() o concat(), en realidad devuelve un nuevo objeto String con el resultado, dejando la cadena original intacta. Esta inmutabilidad es fundamental para la seguridad, el pool de cadenas y el comportamiento en concurrencia.

Al concatenar dos cadenas con el operador + o el método concat(), se crea un nuevo objeto String que contiene la unión de ambas cadenas. Los objetos originales permanecen inmutables y sin cambios. Esto implica que, en un bucle donde se concatena repetidamente usando +, en cada iteración se genera un nuevo objeto String, copiando todos los caracteres anteriores. Para unos pocos concatenados esto es aceptable, pero para construir una cadena muy larga paso a paso resulta muy ineficiente en términos de uso de memoria y tiempo de CPU debido a las múltiples copias intermedias.

Para operaciones que implican concatenar muchas veces, la solución es utilizar la clase StringBuilder (o StringBuffer para código concurrente). Estas clases son mutables y están diseñadas específicamente para construir cadenas de manera eficiente. En lugar de crear un nuevo objeto en cada modificación, StringBuilder mantiene un buffer interno de caracteres que se amplía según sea necesario, minimizando las copias. Al final, se llama a su método toString() para obtener el objeto String inmutable final. Por ejemplo:

StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    sb.append("pedazo").append(i);
}
String resultado = sb.toString(); // Una única cadena inmutable al final

Esta práctica aprovecha la mutabilidad controlada de StringBuilder para la construcción eficiente, sin renunciar a la inmutabilidad y seguridad de String como resultado final. Para un programador de C, es análogo a construir una cadena en un buffer de caracteres (char[]) antes de considerarla final.


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

En POO, la comparación de objetos puede realizarse de dos formas fundamentales: por identidad y por contenido (equivalencia). La identidad se refiere a si dos referencias apuntan exactamente al mismo objeto en memoria (misma instancia). La comparación por contenido evalúa si dos objetos distintos tienen un estado interno considerado equivalente según la lógica de negocio de la clase. Ambas comparaciones son necesarias en distintos contextos.

En Java, el operador == compara solamente la identidad de los objetos (es decir, las direcciones de memoria). Para comparar por contenido, se utiliza el método equals(Object obj), definido en la clase Object. Por defecto, la implementación en Object también compara la identidad (return (this == obj);). Por lo tanto, para que una clase personalizada compare por contenido, debe sobrescribir el método equals con la lógica adecuada que compare los atributos relevantes. Por ejemplo, dos objetos Punto con las mismas coordenadas deberían ser equals aunque sean instancias distintas.

Para comparar dos cadenas (String) en Java, nunca se debe usar == (a menos que se busque específicamente verificar si son el mismo objeto). Debido a la inmutabilidad y al pool de cadenas, == a veces funciona para literales, pero es un comportamiento no fiable. La forma correcta es utilizar el método equals() (ej: cadena1.equals(cadena2)), que está sobrescrito en la clase String para comparar el contenido carácter por carácter. Para comparación sin distinguir mayúsculas/minúsculas, se usa equalsIgnoreCase(). Esto asegura la comparación semántica correcta, independientemente de cómo se hayan creado las cadenas en memoria.


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

Las clases "wrapper" (envoltorio) en un lenguaje orientado a objetos son clases que encapsulan o "envuelven" un tipo de dato primitivo (como int, double, char) dentro de un objeto. Su propósito es permitir que los valores primitivos sean tratados como objetos, lo que es necesario para ciertas funcionalidades del lenguaje que requieren referencias a objetos. En Java, los wrappers correspondientes son Integer, Double, Character, Boolean, etc., y se encuentran en el paquete java.lang.

La conversión entre un primitivo y su wrapper se puede hacer de forma explícita mediante constructores o métodos estáticos, pero Java también proporciona autoboxing y unboxing automáticos. El autoboxing es la conversión automática que hace el compilador de un tipo primitivo a su correspondiente clase wrapper (ej: de int a Integer). El unboxing es el proceso inverso (ej: de Integer a int). Esto hace que el proceso sea casi transparente, aunque no deja de ser una conveniencia sintáctica; internamente, se crean y manejan objetos.

Las ventajas principales son: 1) Permiten que los primitivos se utilicen en contextos que requieren objetos, como las colecciones genéricas (ArrayList<Integer>), ya que los genéricos en Java solo trabajan con tipos de referencia. 2) Proporcionan métodos de utilidad estáticos (como Integer.parseInt() o Double.isNaN()). 3) Pueden representar la ausencia de valor (null), algo imposible con los primitivos.

No todos los lenguajes orientados a objetos tienen esta dicotomía. Lenguajes como Python o C# (en su mayoría) unifican el modelo: todos los datos son objetos. En C#, los tipos primitivos como int son alias de estructuras (System.Int32) que son tipos por valor, pero se comportan como objetos mediante boxing automático cuando es necesario. La necesidad de wrappers es, por tanto, una característica de lenguajes como Java que mantienen una distinción explícita entre tipos primitivos (por valor, eficientes) y tipos de referencia (objetos), por razones históricas de rendimiento.


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

Un tipo de dato enumerado en POO es un tipo de dato que define un conjunto fijo y acotado de constantes con nombre, representando todos los valores posibles que una variable de ese tipo puede tomar. Su propósito es mejorar la claridad y seguridad del código al reemplazar el uso de literales numéricos o cadenas "mágicas" con identificadores significativos y tipados. Por ejemplo, un enumerado DiaSemana podría contener las constantes LUNES, MARTES, etc.

En Java, un tipo enumerado es, efectivamente, una clase especial. Se define con la palabra clave enum, y cada constante enumerada es una instancia única y predefinida de esa clase. Esto permite que los enumerados en Java sean mucho más potentes que simples listas de constantes, ya que pueden tener atributos, constructores, métodos e implementar interfaces. Por ejemplo, un enumerado Planeta podría tener atributos como masa y radio, y un método para calcular la gravedad superficial.

En términos de encapsulación, los enumerados en Java ofrecen ventajas significativas. En primer lugar, encapsulan el conjunto de valores válidos dentro de la propia definición del tipo, garantizando la seguridad en tiempo de compilación y evitando valores ilegales. En segundo lugar, pueden encapsular comportamiento y datos relacionados con cada constante, centralizando la lógica en un solo lugar bien definido. Finalmente, los enumerados son por defecto inmutables y sus instancias están controladas (no se pueden crear nuevas instancias externamente), lo que asegura la integridad del conjunto de valores. Esto contrasta con enfoques menos seguros, como usar constantes enteras públicas (public static final) en C/C++ o Java antiguo, donde no hay restricción sobre qué valores se pueden asignar.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

public enum Mes {
    ENERO(1, 31),
    FEBRERO(2, 28),
    MARZO(3, 31),
    ABRIL(4, 30),
    MAYO(5, 31),
    JUNIO(6, 30),
    JULIO(7, 31),
    AGOSTO(8, 31),
    SEPTIEMBRE(9, 30),
    OCTUBRE(10, 31),
    NOVIEMBRE(11, 30),
    DICIEMBRE(12, 31);

    // Atributos encapsulados
    private final int ordinal;
    private final int dias;

    // Constructor privado (implícitamente privado en un enum)
    Mes(int ordinal, int dias) {
        this.ordinal = ordinal;
        this.dias = dias;
    }

    // Métodos públicos para acceder a los atributos encapsulados
    public int getDias() {
        // Nota: Febrero podría manejarse con más lógica para años bisiestos
        return dias;
    }

    public int getOrdinal() {
        return ordinal;
    }

    // Métodos para determinar la estación
    public boolean esDePrimavera(boolean enHemisferioNorte) {
        int m = this.ordinal;
        if (enHemisferioNorte) {
            return m == 3 || m == 4 || m == 5;
        } else {
            return m == 9 || m == 10 || m == 11;
        }
    }

    public boolean esDeVerano(boolean enHemisferioNorte) {
        int m = this.ordinal;
        if (enHemisferioNorte) {
            return m == 6 || m == 7 || m == 8;
        } else {
            return m == 12 || m == 1 || m == 2;
        }
    }

    public boolean esDeOtonio(boolean enHemisferioNorte) {
        int m = this.ordinal;
        if (enHemisferioNorte) {
            return m == 9 || m == 10 || m == 11;
        } else {
            return m == 3 || m == 4 || m == 5;
        }
    }

    public boolean esDeInvierno(boolean enHemisferioNorte) {
        int m = this.ordinal;
        if (enHemisferioNorte) {
            return m == 12 || m == 1 || m == 2;
        } else {
            return m == 6 || m == 7 || m == 8;
        }
    }
}

Este enumerado Mes demuestra una encapsulación avanzada. Cada constante (ENERO, FEBRERO, etc.) es una instancia inmutable de la clase Mes, creada con sus propios valores de ordinal y dias a través del constructor privado. Los atributos son private final, garantizando que el estado de cada mes sea inmodificable y solo accesible a través de los getters públicos getOrdinal() y getDias(). Esto encapsula perfectamente la representación interna.

Los métodos de estación (esDePrimavera, etc.) encapsulan la lógica compleja de determinar la estación según el mes y el hemisferio. Centralizan este conocimiento en el propio enumerado, evitando que se disperse por el código cliente. El parámetro booleano enHemisferioNorte permite la adaptación. Esta implementación muestra cómo un enum en Java es una clase de primera clase con plena capacidad de encapsulación, ofreciendo un conjunto cerrado de valores con comportamiento y datos asociados, mucho más seguro y expresivo que usar constantes numéricas o cadenas.