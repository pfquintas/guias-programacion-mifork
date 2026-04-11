<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

La herencia es un mecanismo de la programación orientada a objetos mediante el cual una clase (subclase) deriva de otra clase (superclase), estableciendo una relación "es-un". Esto implica que todo objeto de la subclase puede ser tratado como un objeto de la superclase, ya que la subclase es una especialización de la superclase. Por ejemplo, "Artillero es-un Soldado" y "Zapador es-un Soldado" son relaciones válidas, mientras que "Soldado es-un Artillero" no lo sería.

Las dos implicaciones principales son:

1. Compatibilidad de tipos: una variable declarada del tipo de la superclase puede referenciar objetos de cualquier subclase, permitiendo polimorfismo y código más genérico.

2. Herencia de estado y comportamiento: la subclase recibe automáticamente todos los atributos y métodos no privados de la superclase, pudiendo agregar nuevos miembros o sobrescribir los existentes. En el ejemplo propuesto, tanto Artillero como Zapador heredan el atributo nombre y el método saludar(), pero cada uno añade su propio estado específico (número de cohetes o número de minas) y sus respectivos getters.

// Superclase
public class Soldado {
    private String nombre;
    
    public Soldado(String nombre) {
        this.nombre = nombre;
    }
    
    public void saludar() {
        System.out.println("Soy el soldado " + nombre);
    }
    
    public String getNombre() {
        return nombre;
    }
}

// Subclase Artillero
public class Artillero extends Soldado {
    private int numeroCohetes;
    
    public Artillero(String nombre, int numeroCohetes) {
        super(nombre);  // Llama al constructor de Soldado
        this.numeroCohetes = numeroCohetes;
    }
    
    public int getNumeroCohetes() {
        return numeroCohetes;
    }
}

// Subclase Zapador
public class Zapador extends Soldado {
    private int numeroMinas;
    
    public Zapador(String nombre, int numeroMinas) {
        super(nombre);
        this.numeroMinas = numeroMinas;
    }
    
    public int getNumeroMinas() {
        return numeroMinas;
    }
}

// Demostración de compatibilidad de tipos
public class Main {
    public static void main(String[] args) {
        // Array de tipo Soldado: puede contener cualquier subtipo
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Artillero("Juan", 10);
        ejercito[1] = new Zapador("Pedro", 5);
        ejercito[2] = new Artillero("Luis", 8);
        
        // Recorrido polimórfico: todos saludan
        for (Soldado s : ejercito) {
            s.saludar();  // Cada uno usa su versión heredada del método
        }
    }
}

La compatibilidad de tipos se demuestra claramente en el array de Soldado, que puede almacenar objetos de Artillero y Zapador sin necesidad de conversiones explícitas. Al recorrer el array, cada objeto responde al método saludar() que hereda de Soldado, mostrando que la subclase conserva el comportamiento definido en la superclase. Nótese que los atributos específicos como numeroCohetes o numeroMinas solo son accesibles mediante los getters de cada subclase, pero no desde la referencia de tipo Soldado a menos que se realice un casting explícito (lo cual rompería la abstracción y no es necesario para este ejemplo).


## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

Al crear un objeto de una subclase, por ejemplo new Artillero("Juan", 10), se ejecutan dos constructores en un orden estricto: primero el constructor de la superclase (Soldado) y luego el constructor de la subclase (Artillero). Esto ocurre porque la subclase depende de la parte heredada de la superclase; el constructor de Soldado inicializa el atributo nombre (privado), y después el constructor de Artillero inicializa numeroCohetes. Si la subclase tuviera una jerarquía más profunda, se ejecutarían todos los constructores desde la clase base más alta hasta la más baja, siguiendo la cadena de herencia.

La palabra clave super dentro de un constructor sirve para invocar explícitamente un constructor de la superclase. Debe ser la primera instrucción del constructor de la subclase y solo puede aparecer una vez. Si no se escribe super(...), el compilador inserta automáticamente una llamada a super() (constructor sin parámetros) al inicio del constructor de la subclase. Esto significa que, si la superclase no tiene un constructor sin parámetros visible (porque se definió un constructor con parámetros y no se incluyó explícitamente el constructor por defecto), entonces es obligatorio llamar a super(...) con los argumentos adecuados; de lo contrario, el código no compilará.

En el ejemplo anterior, la clase Soldado define únicamente public Soldado(String nombre), por lo que no existe el constructor sin parámetros. Por esta razón, tanto Artillero como Zapador deben invocar super(nombre) como primera línea de sus constructores. Si se omitiera esa llamada, el compilador intentaría insertar super() y fallaría al no encontrar ese constructor en Soldado. Esta regla garantiza que la parte heredada del objeto quede correctamente inicializada antes de que la subclase agregue su propia inicialización, respetando la encapsulación (pues el atributo nombre es privado y solo el constructor de Soldado puede asignarlo).


## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

Sí, los atributos privados de la superclase forman parte de la instancia de la subclase en memoria. Cuando se crea un objeto Artillero, la memoria reservada incluye espacio para todos los atributos declarados en Soldado (el nombre privado) más los atributos propios de Artillero (numeroCohetes). La herencia implica que la subclase contiene físicamente los atributos heredados, aunque no pueda acceder a ellos directamente si son privados. Desde una perspectiva de memoria, un objeto de una subclase es un objeto de la superclase con extensiones adicionales.

Sin embargo, que el atributo nombre forme parte del objeto Artillero no implica que se pueda usar directamente desde el código de la subclase. El modificador private impide el acceso incluso a las subclases. En el ejemplo, dentro de Artillero, no se puede escribir this.nombre ni nombre = "Carlos", porque nombre es privado en Soldado. La subclase solo puede acceder a ese atributo a través de los métodos públicos o protegidos de la superclase, como el getter getNombre() (si existiera) o mediante el constructor super(nombre). Esto refuerza el principio de encapsulación: la superclase controla cómo se accede y modifica su propio estado interno.

En el código propuesto, Artillero no puede leer ni modificar directamente nombre. Para obtener el nombre de un Artillero desde fuera, habría que usar el getter público getNombre() heredado de Soldado. Si se intentara algo como System.out.println(artillero.nombre) desde cualquier clase, sería un error de compilación. La subclase hereda la presencia del atributo en memoria, pero no el derecho de acceso directo; esto obliga a diseñar la superclase con métodos accesibles (por ejemplo, protected o públicos) si se desea que las subclases manipulen esos valores, manteniendo así el control de la implementación interna.


## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

La compatibilidad de tipos implica que el código escrito para trabajar con la superclase puede manejar cualquier subclase existente o futura sin necesidad de modificaciones. Esto otorga una gran extensibilidad, ya que nuevas funcionalidades se incorporan simplemente agregando nuevas subclases, sin alterar el código cliente que depende únicamente de la superclase. El código cliente queda "abierto para extensión, pero cerrado para modificación", cumpliendo el principio de abierto/cerrado (OCP) de la programación orientada a objetos.

Para ilustrarlo, se añade un nuevo tipo Francotirador que hereda de Soldado, sin necesidad de modificar el bucle que saluda a todos los soldados. El array de Soldado y el recorrido que invoca saludar() siguen funcionando exactamente igual, demostrando que el código original es extensible de forma natural:

// Nueva subclase agregada sin modificar el código existente
public class Francotirador extends Soldado {
    private int alcance;
    
    public Francotirador(String nombre, int alcance) {
        super(nombre);
        this.alcance = alcance;
    }
    
    public int getAlcance() {
        return alcance;
    }
}

// En el Main original, se agrega un francotirador al array
public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[4];
        ejercito[0] = new Artillero("Juan", 10);
        ejercito[1] = new Zapador("Pedro", 5);
        ejercito[2] = new Artillero("Luis", 8);
        ejercito[3] = new Francotirador("Ana", 500);  // Nuevo tipo añadido
        
        // Este bucle NO se modifica, pero ahora también saluda el francotirador
        for (Soldado s : ejercito) {
            s.saludar();  // Funciona con cualquier subtipo de Soldado
        }
    }
}

Como se observa, la adición de Francotirador no requiere cambiar ni una sola línea del bucle ni del array tipado como Soldado. La compatibilidad de tipos garantiza que Francotirador es un Soldado, por lo que puede almacenarse en el array y responder al método saludar(). Si en el futuro se agregaran Medico, Comandante o cualquier otra especialización, el código que procesa genéricamente a los soldados seguiría funcionando sin alteraciones. Esta característica reduce drásticamente el acoplamiento y facilita el mantenimiento, ya que las nuevas funcionalidades se añaden por extensión (nuevas clases) y no por modificación de código ya probado.


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

Sí, es completamente válido tener una referencia del supertipo que apunte a objetos reales de un subtipo. En el ejemplo anterior, la referencia Soldado s apunta a objetos Artillero, Zapador o Francotirador. Sin embargo, no se puede invocar con una referencia del supertipo los métodos públicos que son específicos del subtipo, porque el compilador solo conoce el tipo declarado de la referencia (Soldado). Es decir, s.getNumeroCohetes() daría error de compilación, aunque el objeto real sea un Artillero, porque Soldado no tiene ese método declarado. Solo se pueden invocar métodos definidos en la superclase o métodos sobrescritos.

El upcasting es la conversión implícita de un subtipo a un supertipo (ej. Soldado s = new Artillero(...)), siempre es seguro y no requiere sintaxis especial. El downcasting es la conversión explícita de un supertipo a un subtipo (ej. Artillero a = (Artillero) s), y puede fallar si el objeto real no es del subtipo esperado, lanzando ClassCastException. El operador instanceof permite comprobar si un objeto es de un tipo específico antes de hacer downcasting, evitando así excepciones en tiempo de ejecución.

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[4];
        ejercito[0] = new Artillero("Juan", 10);
        ejercito[1] = new Zapador("Pedro", 5);
        ejercito[2] = new Artillero("Luis", 8);
        ejercito[3] = new Francotirador("Ana", 500);
        
        for (Soldado s : ejercito) {
            s.saludar();  // Método de supertipo: siempre funciona
            
            // Comprobar si el objeto real es un Artillero usando instanceof
            if (s instanceof Artillero) {
                // Downcasting seguro: la referencia supertipo se convierte a subtipo
                Artillero a = (Artillero) s;
                System.out.println("  → Tiene " + a.getNumeroCohetes() + " cohetes");
            }
        }
    }
}

En este recorrido, instanceof verifica dinámicamente si el objeto real referenciado por s es de tipo Artillero (o una subclase de Artillero). Solo cuando la comprobación es verdadera se realiza el downcasting explícito con (Artillero) s, obteniendo una referencia de tipo Artillero que sí puede invocar getNumeroCohetes(). Para los objetos Zapador o Francotirador, la condición es falsa y se saltan sin lanzar excepción. Este patrón es muy común cuando se procesan colecciones heterogéneas de objetos relacionados por herencia, permitiendo tratar genéricamente a todos (saludo) y acceder a la funcionalidad específica solo cuando es relevante.


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

El acceso "protegido" (protected) en herencia significa que un atributo o método es visible para todas las clases del mismo paquete y, además, para todas las subclases, incluso si están en paquetes diferentes. Este nivel de visibilidad es intermedio entre private (solo visible en la misma clase) y public (visible para cualquier clase). En el contexto de la herencia, protected permite que las subclases accedan directamente a miembros de la superclase que se consideran parte de su implementación interna pero que deben ser personalizables o reutilizables por las subclases, sin exponerlos completamente al resto del sistema.

En Java, protected se implementa anteponiendo esta palabra clave al declarar un atributo o método. Para el ejemplo, si el atributo nombre de Soldado se declara como protected en lugar de private, entonces Zapador podrá acceder directamente a nombre dentro de sus métodos. Esto permite, por ejemplo, que el método ponerMina() muestre un mensaje que incluya el nombre del soldado sin necesidad de llamar a un getter público. Sin embargo, esta decisión reduce la encapsulación porque el estado interno de Soldado queda expuesto a todas las subclases, lo que debe hacerse con cuidado para no crear dependencias frágiles.

// Superclase con atributo protected
public class Soldado {
    protected String nombre;  // Ahora es accesible para subclases
    
    public Soldado(String nombre) {
        this.nombre = nombre;
    }
    
    public void saludar() {
        System.out.println("Soy el soldado " + nombre);
    }
}

// Subclase Zapador que usa directamente el nombre heredado
public class Zapador extends Soldado {
    private int numeroMinas;
    
    public Zapador(String nombre, int numeroMinas) {
        super(nombre);
        this.numeroMinas = numeroMinas;
    }
    
    public void ponerMina() {
        // Acceso directo al atributo protected de la superclase
        System.out.println("El zapador " + nombre + " ha puesto una mina");
        numeroMinas--;
    }
    
    public int getNumeroMinas() {
        return numeroMinas;
    }
}

// En el Main:
Zapador zapador = new Zapador("Pedro", 5);
zapador.ponerMina();  // Muestra: "El zapador Pedro ha puesto una mina"

En este ejemplo, Zapador puede acceder directamente a nombre porque está declarado como protected en Soldado. Dentro del método ponerMina(), se usa nombre como si fuera propio, sin necesidad de super.nombre ni de getters. Esto es práctico para jerarquías bien controladas, pero se debe tener precaución: si en el futuro se cambiara la forma de almacenar el nombre en Soldado (ej. dividido en nombrePila y apellido), todas las subclases que accedan directamente al atributo se romperían. Por eso, el acceso protected es útil pero debe usarse principalmente para métodos o atributos que formen parte de un contrato interno estable entre la superclase y sus subclases, no para exponer indiscriminadamente el estado interno.


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

En muchos lenguajes orientados a objetos existe una clase base raíz de la que derivan todas las demás clases, aunque no ocurre en todos los lenguajes. Por ejemplo, en Smalltalk y Java existe esta clase raíz, mientras que en C++ (sin orientación a objetos en los conocimientos previos, pero relevante como contraste) no hay una clase base universal: cualquier clase puede existir sin heredar de una clase común, y la herencia múltiple complica la existencia de una raíz única. Otros lenguajes como Python tienen object como clase base implícita (similar a Java), mientras que en lenguajes como Go (que no tiene herencia clásica) no existe este concepto.

En Java, efectivamente existe una clase base para todos los objetos: la clase Object. Toda clase en Java, ya sea explícitamente declarada o no, hereda directa o indirectamente de Object. Si una clase no usa extends, el compilador la hace extender implícitamente de Object. Esto significa que cualquier objeto en Java tiene ciertos métodos heredados de Object, como toString(), equals(), hashCode(), getClass() y clone(). Esta característica unifica el sistema de tipos y permite, por ejemplo, que un array de Object pueda contener cualquier tipo de objeto, aunque se pierda la información específica del tipo.

En el contexto del ejemplo de Soldado, aunque no se haya declarado explícitamente, la clase Soldado extiende de Object. Por tanto, todo Soldado, Artillero o Zapador también es un Object, y se pueden aprovechar métodos heredados como toString() para representar el estado del objeto. Si se desea, se puede sobrescribir toString() en Soldado para que devuelva el nombre, y luego cualquier subtipo heredará esa implementación o podrá personalizarla. La existencia de Object facilita la creación de contenedores genéricos (previos a los genéricos de Java) y proporciona un conjunto mínimo de operaciones comunes para todos los objetos, aunque también impone ciertas decisiones de diseño que afectan a toda la jerarquía de clases.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

La herencia múltiple es un mecanismo mediante el cual una clase puede heredar estado y comportamiento de más de una superclase directamente. Por ejemplo, una clase Hijo podría heredar simultáneamente de Padre y de Madre, adquiriendo atributos y métodos de ambos. Esto permite modelar situaciones donde un concepto participa en múltiples jerarquías, pero también introduce problemas complejos, como el problema del diamante: si dos superclases tienen un método con el mismo nombre y firma, no queda claro cuál debe heredar la subclase, ni cómo resolver conflictos. Lenguajes como C++ sí permiten herencia múltiple y proporcionan mecanismos (como herencia virtual) para manejar estos problemas, aunque a costa de una mayor complejidad.

Java no permite herencia múltiple de clases. Una clase en Java solo puede extender directamente de una única superclase, usando la palabra clave extends. Esto fue una decisión de diseño deliberada para evitar la complejidad y ambigüedades del diamante, simplificando el modelo de objetos y haciéndolo más robusto. Sin embargo, Java sí permite que una clase implemente múltiples interfaces (con implements), lo que proporciona una forma de herencia múltiple de tipos (contratos) sin herencia de implementación. Desde Java 8, las interfaces pueden tener métodos default, lo que permite cierta herencia múltiple de comportamiento, pero con reglas claras de resolución de conflictos (la clase que implementa debe sobrescribir el método conflictivo).

En el ejemplo de los soldados, no se necesita herencia múltiple porque Artillero y Zapador son especializaciones directas de Soldado. Si se quisiera modelar, por ejemplo, un ArtilleroZapador que pudiera tanto disparar cohetes como poner minas, Java obligaría a elegir una única superclase (o a usar composición, o a definir interfaces como Disparable y Minable que contengan los métodos respectivos). La ausencia de herencia múltiple de clases fomenta diseños más predecibles y evita ambigüedades, aunque a veces requiere más código mediante composición o interfaces para lograr comportamientos combinados.


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente.

En Java, las excepciones son objetos que heredan de la clase Throwable. Para crear una excepción personalizada no controlada (unchecked), la clase debe extender de RuntimeException. Las excepciones no controladas no obligan a declararlas en la cláusula throws ni a capturarlas con try-catch, aunque se puede hacer. Para incluir composición con un objeto Usuario, se añade un atributo de tipo Usuario y se proporciona un getter. La causa subyacente se maneja mediante los constructores de RuntimeException que aceptan un Throwable como causa.

El objeto Usuario se define como una clase simple con atributos como identificador y nombre. La excepción UsuarioNoEncontradoException almacena una referencia al Usuario que no pudo encontrarse (o que causó el problema), permitiendo que el manejador de la excepción acceda a los detalles completos del usuario implicado. La sobrecarga de constructores ofrece dos versiones: una que solo recibe el mensaje y el usuario, y otra que además recibe la causa (otra excepción que provocó esta, por ejemplo, un error de base de datos).

// Clase Usuario que forma parte de la composición
public class Usuario {
    private int id;
    private String nombre;
    
    public Usuario(int id, String nombre) {
        this.id = id;
        this.nombre = nombre;
    }
    
    public int getId() { return id; }
    public String getNombre() { return nombre; }
    
    @Override
    public String toString() {
        return "Usuario{id=" + id + ", nombre='" + nombre + "'}";
    }
}

// Excepción personalizada no controlada (hereda de RuntimeException)
public class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;  // Composición: la excepción contiene al usuario problemático
    
    // Constructor sin causa
    public UsuarioNoEncontradoException(String mensaje, Usuario usuario) {
        super(mensaje);
        this.usuario = usuario;
    }
    
    // Constructor con causa subyacente (sobrecarga)
    public UsuarioNoEncontradoException(String mensaje, Usuario usuario, Throwable causa) {
        super(mensaje, causa);
        this.usuario = usuario;
    }
    
    public Usuario getUsuario() {
        return usuario;
    }
}

// Ejemplo de uso
public class ServicioUsuarios {
    public void buscarUsuario(int id) {
        Usuario u = new Usuario(id, "Juan");  // Simulación
        if (id == 0) {
            // Lanza la excepción personalizada con composición
            throw new UsuarioNoEncontradoException(
                "No se encontró el usuario con ID " + id, u
            );
        }
        // Si hubiera una excepción de base de datos, se usaría el segundo constructor:
        // throw new UsuarioNoEncontradoException("Error de BD", u, e);
    }
    
    public static void main(String[] args) {
        ServicioUsuarios servicio = new ServicioUsuarios();
        try {
            servicio.buscarUsuario(0);
        } catch (UsuarioNoEncontradoException e) {
            System.out.println("Excepción: " + e.getMessage());
            System.out.println("Usuario implicado: " + e.getUsuario());
            if (e.getCause() != null) {
                System.out.println("Causa original: " + e.getCause());
            }
        }
    }
}

En este diseño, la excepción no controlada permite que el código cliente decida si capturarla o no, pero cuando se captura, se tiene acceso directo al objeto Usuario que causó el problema, gracias a la composición. Esto es mucho más informativo que simplemente pasar un ID como cadena en el mensaje. Además, la sobrecarga del constructor con Throwable causa permite encadenar excepciones, preservando la traza original del error (por ejemplo, una SQLException que se envuelve dentro de UsuarioNoEncontradoException). Así se respetan los principios de encapsulación (los atributos de Usuario son privados con getters) y composición (la excepción contiene un objeto Usuario), integrando conceptos previos con herencia de excepciones.


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

No se debe emplear herencia simplemente por reutilizar código porque la herencia crea un acoplamiento muy fuerte entre la superclase y la subclase. Cuando una clase hereda de otra, la subclase queda atada a los detalles de implementación de la superclase, no solo a su interfaz pública. Esto significa que cambios aparentemente inocuos en la superclase (como modificar un método privado que usa un atributo interno) pueden romper el comportamiento de todas las subclases, incluso si estas no han sido modificadas. Además, la herencia viola la encapsulación de la superclase, porque las subclases a menudo necesitan conocer detalles internos para sobrescribir métodos correctamente. La composición, en cambio, mantiene un acoplamiento más débil mediante delegación explícita.

La herencia debe modelar una relación genuina "es-un" que respete el principio de sustitución de Liskov: una subclase debe poder reemplazar a su superclase sin alterar el comportamiento esperado del programa. Si solo se busca reutilizar código sin que exista dicha relación jerárquica natural, la composición es más adecuada. Por ejemplo, una clase Coche no debería heredar de Motor para reutilizar el código de arranque, porque un coche no es un motor (es "tiene-un" motor). Con composición, Coche contiene un objeto Motor y delega en él; si se cambia la implementación de Motor, solo se necesita modificar la composición, no toda la jerarquía. Además, la composición ofrece mayor flexibilidad en tiempo de ejecución (se puede cambiar el objeto compuesto dinámicamente), mientras que la herencia es estática en tiempo de compilación.

La fragilidad de la jerarquía de herencia es otro problema: cambios en la superclase pueden tener efectos en cascada en todas las subclases, lo que dificulta el mantenimiento. La composición, combinada con interfaces, permite diseñar sistemas más modulares y probables de forma aislada. En resumen, se debe preferir composición sobre herencia a menos que exista una relación "es-un" clara y estable, y que la subclase realmente necesite comportarse como un subtipo de la superclase en todo contexto. La reutilización de código por sí sola no justifica la herencia, porque la composición logra reutilización con menor acoplamiento y mayor flexibilidad.


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

Se debe favorecer la composición frente a la herencia porque la composición ofrece un acoplamiento más débil y una mayor flexibilidad en el diseño del software. Cuando se usa composición, una clase contiene instancias de otras clases como parte de su estado interno, y delega en ellas ciertas responsabilidades. Esto permite cambiar el comportamiento en tiempo de ejecución (por ejemplo, sustituyendo el objeto compuesto por otro de una interfaz diferente), algo imposible con la herencia, que es estática y se define en tiempo de compilación. Además, la composición respeta mejor la encapsulación, ya que la clase contenedora no necesita conocer los detalles internos de los objetos que contiene, solo su interfaz pública.

Otro motivo clave es que la herencia expone la implementación interna de la superclase a las subclases, creando una dependencia frágil. Cualquier modificación en la superclase puede romper el funcionamiento de las subclases, incluso si estas no se modificaron explícitamente. Este problema se conoce como "fragilidad de la jerarquía de herencia". La composición, en cambio, solo depende de las interfaces o de los métodos públicos de los objetos compuestos. Si un objeto componente cambia su implementación interna, mientras respete su contrato público, la clase contenedora sigue funcionando sin modificaciones. Esto hace que el sistema sea más mantenible y menos propenso a errores inducidos por cambios en otras partes del código.

Finalmente, la composición permite una reutilización más fina y selectiva. Con herencia, una subclase hereda todo el estado y comportamiento de la superclase, incluso aquello que no necesita. Con composición, una clase puede incluir solo los objetos que realmente requiere, y puede combinar comportamientos de múltiples fuentes sin sufrir las limitaciones de la herencia simple de Java (que solo permite una superclase). Aunque la herencia es útil para modelar relaciones jerárquicas naturales (como "Artillero es-un Soldado"), la experiencia ha demostrado que la composición conduce a diseños más flexibles, desacoplados y adaptables al cambio. Por ello, el principio "favorecer composición sobre herencia" es una de las recomendaciones fundamentales del diseño orientado a objetos, especialmente cuando no existe una relación "es-un" clara y estable.


## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

La afirmación de que "la herencia rompe la encapsulación" se refiere a que una subclase, para funcionar correctamente o para extender el comportamiento de la superclase, a menudo necesita conocer y depender de los detalles internos de implementación de la superclase, no solo de su interfaz pública. En una relación de composición, una clase solo interactúa con los métodos públicos de los objetos que contiene, manteniendo intacta la barrera de encapsulación. En cambio, en la herencia, la subclase puede acceder a atributos y métodos protected (e incluso a miembros públicos) de la superclase, y típicamente sobrescribe métodos, lo que requiere entender cómo la superclase usa internamente esos métodos y atributos. Si la superclase cambia su implementación interna (por ejemplo, modifica un método privado que es invocado desde un método público que la subclase sobrescribe), la subclase puede dejar de funcionar aunque no haya modificado su propio código.

Un ejemplo clásico es cuando una superclase Lista tiene métodos add() y addAll() (que internamente llama varias veces a add()). Si una subclase ListaContadora sobrescribe add() para incrementar un contador, pero no sobrescribe addAll(), entonces al invocar addAll() desde una referencia de la subclase, la versión heredada de addAll() llamará a la versión sobrescrita de add() (gracias al polimorfismo), incrementando el contador correctamente. Sin embargo, si en una nueva versión de la superclase addAll() se reimplementa sin usar add() (por ejemplo, con una copia directa de arreglos), el contador dejará de incrementarse al usar addAll(), rompiendo el comportamiento esperado de la subclase. La superclase no tenía forma de saber que la subclase dependía de que addAll() invocara a add(), y la subclase no puede evitar esta dependencia.

En el contexto del ejemplo de Soldado, si Soldado tuviera un método entrenar() que internamente llama a descansar(), y Zapador sobrescribe descansar() para reducir minas mientras descansa, cualquier cambio futuro en Soldado que modifique la implementación de entrenar() (por ejemplo, que ya no llame a descansar() o que lo llame de otra forma) afectaría el comportamiento de Zapador. Esto demuestra que la herencia expone a la subclase a los cambios en la implementación interna de la superclase, violando el principio de encapsulación que establece que los detalles internos de una clase no deberían afectar a otras clases. La composición evita este problema porque las clases solo se comunican a través de interfaces públicas bien definidas, sin depender de cómo se implementa internamente cada método.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

- Alternativa 1: Herencia

En este modelo, se define una superclase Persona que contiene los atributos y métodos comunes (DNI y nombre). Luego, Estudiante y Trabajador heredan de Persona, añadiendo sus propios atributos específicos (por ejemplo, carrera para Estudiante y puesto para Trabajador). Esta solución establece una relación "es-un": un Estudiante es una Persona, y un Trabajador es una Persona. La reutilización de código se logra por herencia, pero las subclases quedan acopladas a la implementación de Persona. Además, si en el futuro una misma persona fuera simultáneamente estudiante y trabajador, este modelo no permite herencia múltiple (una clase no puede extender de dos superclases), lo que obligaría a soluciones forzadas como duplicar código o crear una clase intermedia EstudianteTrabajador que herede de una sola.

// Herencia
public class Persona {
    private String dni;
    private String nombre;
    
    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
    // getters y setters
}

public class Estudiante extends Persona {
    private String carrera;
    
    public Estudiante(String dni, String nombre, String carrera) {
        super(dni, nombre);
        this.carrera = carrera;
    }
}

public class Trabajador extends Persona {
    private String puesto;
    
    public Trabajador(String dni, String nombre, String puesto) {
        super(dni, nombre);
        this.puesto = puesto;
    }
}

- Alternativa 2: Composición

Aquí no se usa herencia. En lugar de ello, se define una clase separada DatosPersonales que encapsula el DNI y el nombre. Las clases Estudiante y Trabajador reciben en su constructor una instancia de DatosPersonales y la almacenan como un atributo interno. Esto establece una relación "tiene-un": un Estudiante tiene unos datos personales. La reutilización se logra por composición, y el acoplamiento es más débil porque Estudiante y Trabajador dependen de la interfaz pública de DatosPersonales, no de su implementación interna. Además, este modelo permite que una misma persona sea estudiante y trabajador sin conflicto: se crea una única instancia de DatosPersonales y se inyecta en ambos objetos.

// Composición
public class DatosPersonales {
    private String dni;
    private String nombre;
    
    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
    // getters (sin setters para mantener inmutabilidad)
}

public class Estudiante {
    private DatosPersonales datos;  // Composición
    private String carrera;
    
    public Estudiante(DatosPersonales datos, String carrera) {
        this.datos = datos;
        this.carrera = carrera;
    }
    
    public String getNombre() {
        return datos.getNombre();  // Delegación
    }
}

public class Trabajador {
    private DatosPersonales datos;  // Composición
    private String puesto;
    
    public Trabajador(DatosPersonales datos, String puesto) {
        this.datos = datos;
        this.puesto = puesto;
    }
}

// Uso: una misma persona como estudiante y trabajador
DatosPersonales dp = new DatosPersonales("12345678A", "Ana Pérez");
Estudiante e = new Estudiante(dp, "Informática");
Trabajador t = new Trabajador(dp, "Ingeniera de software");

La composición es más flexible porque permite compartir la misma instancia de DatosPersonales entre varios objetos, reflejando mejor la realidad de que una persona puede desempeñar múltiples roles simultáneamente. Además, si se modifica DatosPersonales (por ejemplo, añadiendo fecha de nacimiento), no es necesario modificar Estudiante ni Trabajador a menos que se quiera exponer el nuevo dato. En cambio, con herencia, añadir un atributo a Persona afecta inmediatamente a todas las subclases, y si una persona cambia de nombre, habría que actualizarlo en un solo lugar (la instancia compartida de DatosPersonales) en lugar de en objetos separados de Estudiante y Trabajador. Por estas razones, la composición suele preferirse cuando la relación "tiene-un" es más natural o cuando se necesita mayor flexibilidad.