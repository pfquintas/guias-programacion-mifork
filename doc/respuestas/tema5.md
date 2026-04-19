<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

El polimorfismo es la capacidad de que un mismo nombre de método o una misma interfaz se comporte de manera diferente según el objeto concreto que lo ejecute. En programación orientada a objetos, sirve para escribir código más genérico y reutilizable, permitiendo que una variable de un tipo padre (superclase) pueda hacer referencia a objetos de distintos tipos hijos (subclases), y al invocar un método se ejecute la versión correspondiente al objeto real. Esto facilita la extensión del software: se pueden agregar nuevas subclases sin modificar el código que usa las referencias polimórficas.

La sobreescritura (override) ocurre cuando una subclase redefine un método heredado de su superclase, manteniendo el mismo nombre, tipo de retorno (o un tipo covariante) y lista de parámetros. De esta forma, la subclase proporciona una implementación específica que reemplaza a la de la superclase. La sobreescritura es la base técnica que permite el polimorfismo dinámico en Java: cuando se llama a un método sobrescrito a través de una referencia de la superclase, la máquina virtual ejecuta la versión de la subclase (no la de la superclase).


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

La ligadura dinámica (o enlace tardío) es el mecanismo por el cual la decisión de qué método ejecutar no se toma en tiempo de compilación, sino en tiempo de ejecución, basándose en el tipo real del objeto (no en el tipo de la referencia que lo apunta). Esto es fundamental para el polimorfismo de subtipo: cuando se invoca un método sobrescrito a través de una referencia de superclase, el programa debe determinar en ese momento cuál versión del método (la de la superclase o la de alguna subclase) corresponde al objeto almacenado. Sin la ligadura dinámica, el polimorfismo dinámico no sería posible.

La ligadura dinámica es el mecanismo subyacente que hace funcionar al polimorfismo por sobrescritura. En cuanto a si debe indicarse explícitamente, depende del lenguaje. En C++, los métodos deben declararse explícitamente como virtual en la superclase para que se aplique ligadura dinámica; de lo contrario, se usa ligadura estática (en tiempo de compilación) según el tipo de la referencia. En Java, todos los métodos no estáticos, no privados y no final usan ligadura dinámica por defecto, sin necesidad de palabra clave especial (aunque existe @Override como anotación de verificación). En Python, todo es dinámico: la búsqueda de métodos ocurre siempre en tiempo de ejecución, sin necesidad de declaraciones especiales, lo que hace que la ligadura dinámica sea la regla absoluta.


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

class Soldado {
    public void saluda() {
        System.out.println("Soldado: ¡Buenos días, mi general!");
    }
}

class Zapador extends Soldado {
    @Override
    public void saluda() {
        System.out.println("Zapador: ¡A sus órdenes, con la pala lista!");
    }
}

class Artillero extends Soldado {
    @Override
    public void saluda() {
        System.out.println("Artillero: ¡Fuego a discreción, mi comandante!");
    }
}

// En el método main u otro método:
Soldado[] ejercito = new Soldado[3];
ejercito[0] = new Soldado();
ejercito[1] = new Zapador();
ejercito[2] = new Artillero();

for (int i = 0; i < ejercito.length; i++) {
    ejercito[i].saluda();  // Ligadura dinámica en acción
}

En este ejemplo, el array ejercito es declarado del tipo Soldado, pero almacena objetos de Soldado, Zapador y Artillero. Al recorrer el bucle y llamar al método saluda() mediante la referencia de tipo Soldado, Java aplica ligadura dinámica y ejecuta la versión correspondiente al objeto real almacenado en cada posición. La subclase Zapador sobrescribe completamente el comportamiento heredado, reemplazando el saludo genérico del soldado por uno propio. Esto demuestra el polimorfismo: el mismo mensaje (saluda()) produce resultados diferentes según el tipo concreto de objeto en tiempo de ejecución.


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

Sí, es posible invocar el método de la clase base dentro de un método sobrescrito, para trabajar a partir de su comportamiento original y luego extenderlo o modificarlo parcialmente. Esto se logra mediante la palabra clave super, que permite acceder explícitamente a miembros (métodos o atributos) de la superclase directa. A continuación se muestra cómo quedaría la clase Zapador modificada:

class Zapador extends Soldado {
    @Override
    public void saluda() {
        super.saluda();               // Invoca al método saluda() de Soldado
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}

En este código, al llamar a saluda() sobre un objeto Zapador, primero se ejecuta super.saluda(), que imprime el saludo genérico del soldado base ("Soldado: ¡Buenos días, mi general!"), y luego se añade la línea adicional específica del zapador. La palabra clave super es la que permite invocar al método de la clase base desde la subclase, incluso cuando ese método ha sido sobrescrito. Sin super, la llamada saluda() dentro de la clase Zapador sería recursiva (se llamaría a sí misma infinitamente). De esta forma se combina la reutilización del comportamiento heredado con la especialización propia del polimorfismo.


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

Al sobrescribir un método en Java, la lista de parámetros debe ser idéntica en número, tipo y orden a la del método de la superclase; no se pueden cambiar los parámetros, porque eso convertiría la sobrescritura en una sobrecarga. En cuanto al tipo de retorno, puede ser el mismo o un tipo covariante (una subclase del tipo de retorno original). Por ejemplo, si la superclase declara Animal obtenerMascota(), una subclase puede sobrescribir con Perro obtenerMascota(), siendo Perro subclase de Animal. Además, el método sobrescrito no puede tener un nivel de acceso más restrictivo (ej. de public a private), y no puede lanzar excepciones nuevas o más amplias que las declaradas en el método original.

La sobreescritura (overriding) ocurre entre clases relacionadas por herencia, redefine un método existente con la misma firma y se resuelve en tiempo de ejecución (ligadura dinámica). La sobrecarga (overloading) ocurre dentro de la misma clase (o entre clases con herencia, pero con distinta firma), permite múltiples métodos con el mismo nombre pero diferentes parámetros, y se resuelve en tiempo de compilación (ligadura estática). Mientras la sobreescritura es polimorfismo dinámico, la sobrecarga es polimorfismo estático o ad-hoc.

La anotación @Override le indica al compilador que el método anotado pretende sobrescribir un método de la superclase. Sirve para detectar errores comunes: si se escribe mal el nombre del método, se equivoca la lista de parámetros o se usa un tipo de retorno no covariante, el compilador mostrará un error en lugar de asumir silenciosamente que se está creando un método nuevo (y no sobrescribiendo el esperado). Es recomendable usarla siempre porque mejora la legibilidad del código, documenta la intención del programador y previene bugs sutiles difíciles de rastrear.


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

Sí, se emplea polimorfismo desde el principio en Java, aunque muchas veces no se mencione explícitamente. Cuando se sobrescribe toString() o equals(), se está utilizando polimorfismo de subtipo. La clase Object es la superclase de todas las clases en Java, y sus métodos (toString, equals, hashCode) están diseñados para ser sobrescritos. Al crear una clase propia y sobrescribir toString(), luego se puede pasar un objeto de esa clase a System.out.println(), que internamente llama al método toString() del objeto. Como System.out.println() recibe un Object (polimorfismo), la máquina virtual ejecuta la versión sobrescrita, no la de Object. Esto es exactamente el mismo mecanismo polimórfico que con Soldado y Zapador.

Por lo tanto, cualquier uso de una referencia de tipo superclase (incluyendo Object) para invocar un método sobrescrito en una subclase es polimorfismo. Cada vez que se escribe @Override sobre toString() o equals(), se está participando del polimorfismo que Java ofrece desde su núcleo. Esto demuestra que el polimorfismo no es un tema avanzado y opcional, sino un principio fundamental que acompaña al programador desde los primeros ejercicios, incluso antes de definir jerarquías propias de herencia.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

Una clase abstracta es una clase que no puede ser instanciada (no se pueden crear objetos de ella) y que normalmente sirve como plantilla para otras clases hijas. Un método abstracto es un método declarado sin implementación (sin cuerpo, solo la firma), que obliga a las subclases concretas a proporcionar dicha implementación. Cualquier clase que contenga al menos un método abstracto debe declararse a su vez como abstracta. Las clases abstractas permiten definir comportamientos comunes (métodos concretos) junto con operaciones que deben ser implementadas obligatoriamente por las subclases, combinando herencia y polimorfismo.

No se pueden crear instancias de Soldado porque es abstracta. La palabra clave abstract debe colocarse tanto en la declaración de la clase como en la del método sin cuerpo (terminado en punto y coma). Las subclases concretas (Zapador, Artillero) están obligadas a sobrescribir atacar(), o de lo contrario también deberían ser abstractas.

abstract class Soldado {
    public void saluda() {
        System.out.println("Soldado: ¡Buenos días, mi general!");
    }
    
    public abstract void atacar();  // Método abstracto, sin implementación
}

class Zapador extends Soldado {
    @Override
    public void saluda() {
        super.saluda();
        System.out.println("ZAPADOR A SUS ORDENES");
    }
    
    @Override
    public void atacar() {
        System.out.println("Zapador: ¡Plantando minas y dinamitando bunkers!");
    }
}

class Artillero extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Artillero: ¡Lanzando obuses a 10 km!");
    }
}

// En el código:
Soldado s1 = new Zapador();   // Válido: referencia abstracta, objeto concreto
Soldado s2 = new Artillero();
// Soldado s3 = new Soldado(); // ERROR: no se puede instanciar clase abstracta
s1.atacar();  // Polimorfismo: ejecuta atacar de Zapador
s2.atacar();  // Polimorfismo: ejecuta atacar de Artillero

Las clases abstractas potencian el polimorfismo al garantizar que todas las subclases tengan ciertos métodos (como atacar), pero cada una con su comportamiento específico.


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

La palabra clave final en Java, cuando se aplica a un método, impide que las subclases puedan sobrescribirlo. Un método final no puede ser redefinido mediante overriding, lo que significa que todas las subclases heredarán exactamente la misma implementación sin posibilidad de cambiarla. Cuando se aplica a una clase, impide que se pueda heredar de ella; es decir, ninguna otra clase puede extender una clase declarada como final. En ambos casos, final restringe la capacidad de extensión y modificación del comportamiento a través de la herencia.

final limita el polimorfismo dinámico: si un método es final, no se puede sobrescribir, por lo que cualquier llamada a ese método mediante una referencia de la superclase siempre ejecutará la versión de la superclase, sin posibilidad de que una subclase aporte un comportamiento diferente. De manera similar, una clase final no puede tener subclases, por lo que no puede participar en jerarquías polimórficas como superclase (aunque sí puede ser usada como subtipo de otras clases no finales). Java permite esta restricción por razones de seguridad, rendimiento (los métodos final pueden ser optimizados por el compilador) y diseño claro, cuando se desea que un comportamiento o tipo no sea alterado.

Un ejemplo conocido de clase final en la API de Java es la clase String. String es declarada como final para garantizar su inmutabilidad y seguridad (por ejemplo, que nadie pueda crear una subclase de String que modifique su comportamiento en contextos sensibles como la autenticación o el classloading). Otros ejemplos incluyen Integer, Double y todas las clases envoltorio de tipos primitivos (wrapper classes), así como System y Math. Estas clases no pueden ser extendidas, lo que previene alteraciones en comportamientos fundamentales del lenguaje.


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

Una interfaz en Java es un tipo de referencia que define un contrato: declara un conjunto de métodos (normalmente abstractos, aunque desde Java 8 pueden incluir métodos default y static) sin proporcionar su implementación. Una clase que implementa una interfaz se compromete a proporcionar el cuerpo concreto de todos esos métodos. Las interfaces permiten definir comportamientos comunes que pueden ser adoptados por clases completamente no relacionadas por herencia (por ejemplo, Serializable, Comparable, Runnable). A diferencia de las clases abstractas, las interfaces no pueden tener estado (atributos de instancia, salvo constantes static final) y no pueden tener constructores.

Las interfaces son similares a las clases abstractas en que ambas pueden declarar métodos abstractos y no pueden ser instanciadas. Sin embargo, se diferencian en varios aspectos: una clase abstracta puede tener métodos concretos (con implementación) y atributos de instancia, mientras que una interfaz tradicionalmente solo declaraba métodos abstractos y constantes. Además, una clase puede heredar de una sola clase (abstracta o concreta) debido a la herencia simple de Java, pero puede implementar múltiples interfaces. Esto último resuelve el problema del "diamante" de la herencia múltiple, ya que las interfaces no tienen estado y, desde Java 8, los conflictos entre métodos default deben resolverse explícitamente.

Sí, una clase en Java puede implementar tantas interfaces como sea necesario, separándolas por comas en la declaración. Por ejemplo: class SoldadoProfesional extends Soldado implements Combatiente, Entrenable, Serializable. Esto permite que una clase adopte múltiples roles o comportamientos de manera flexible, combinando herencia de una clase base con implementación de varios contratos. El polimorfismo también funciona con interfaces: una variable de tipo interfaz puede referenciar cualquier objeto de una clase que implemente dicha interfaz, y al invocar un método se ejecuta la implementación concreta de la clase (ligadura dinámica).


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

// Clase abstracta Punto
abstract class Punto {
    protected double x, y;
    
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    // Método abstracto: cada subtipo implementará su propia fórmula de distancia
    public abstract double calcularDistanciaA(Punto otro);
    
    // Método concreto para obtener coordenadas (útil para downcasting)
    public double getX() { return x; }
    public double getY() { return y; }
}

// Punto2D: implementa distancia euclidiana en 2D
class Punto2D extends Punto {
    public Punto2D(double x, double y) {
        super(x, y);
    }
    
    @Override
    public double calcularDistanciaA(Punto otro) {
        // Verifica que el otro punto sea realmente un Punto2D
        if (otro instanceof Punto2D) {
            Punto2D p2 = (Punto2D) otro;  // Downcasting seguro
            double dx = this.x - p2.x;
            double dy = this.y - p2.y;
            return Math.sqrt(dx*dx + dy*dy);
        } else {
            // Si no es compatible, se lanza una excepción
            throw new IllegalArgumentException("No se puede calcular distancia entre Punto2D y otro tipo");
        }
    }
}

// Punto3D: añade coordenada z y distancia en 3D
class Punto3D extends Punto {
    private double z;
    
    public Punto3D(double x, double y, double z) {
        super(x, y);
        this.z = z;
    }
    
    public double getZ() { return z; }
    
    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto3D) {
            Punto3D p3 = (Punto3D) otro;  // Downcasting seguro
            double dx = this.x - p3.x;
            double dy = this.y - p3.y;
            double dz = this.z - p3.z;
            return Math.sqrt(dx*dx + dy*dy + dz*dz);
        } else {
            throw new IllegalArgumentException("No se puede calcular distancia entre Punto3D y otro tipo");
        }
    }
}

// Clase Linea: polimórfica, trabaja con cualquier subtipo de Punto
class Linea {
    private Punto puntoA;
    private Punto puntoB;
    
    public Linea(Punto a, Punto b) {
        this.puntoA = a;
        this.puntoB = b;
    }
    
    public double longitud() {
        // El polimorfismo decide qué versión de calcularDistanciaA se ejecuta
        // según el tipo real de puntoA y puntoB
        return puntoA.calcularDistanciaA(puntoB);
    }
}

// Demostración:
public class Main {
    public static void main(String[] args) {
        Punto p1 = new Punto2D(0, 0);
        Punto p2 = new Punto2D(3, 4);
        Linea linea2D = new Linea(p1, p2);
        System.out.println("Longitud 2D: " + linea2D.longitud());  // 5.0
        
        Punto p3 = new Punto3D(0, 0, 0);
        Punto p4 = new Punto3D(1, 2, 2);
        Linea linea3D = new Linea(p3, p4);
        System.out.println("Longitud 3D: " + linea3D.longitud());  // sqrt(1+4+4)=3.0
        
        // Esto lanzaría excepción porque mezcla tipos incompatibles:
        // Linea lineaMixta = new Linea(new Punto2D(0,0), new Punto3D(1,1,1));
    }
}

En este diseño, la clase Linea solo conoce la superclase Punto y no necesita saber si sus puntos son 2D o 3D. Gracias al polimorfismo y la ligadura dinámica, al invocar calcularDistanciaA desde el método longitud(), Java ejecuta la implementación correspondiente al tipo real de cada Punto. El uso de instanceof dentro de cada clase hija permite verificar la compatibilidad antes de realizar el downcasting, asegurando que solo se calculen distancias entre puntos del mismo subtipo (2D con 2D, 3D con 3D). De esta forma se mantiene la seguridad de tipos y se evitan excepciones inesperadas.


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

La herencia de interfaces es el mecanismo mediante el cual una interfaz puede extender una o más interfaces padre utilizando la palabra clave extends. A diferencia de la herencia de clases (que es simple), una interfaz puede heredar de múltiples interfaces simultáneamente, combinando todas las declaraciones de métodos de sus predecesoras. La interfaz resultante contendrá todos los métodos abstractos (y eventualmente default o static) de las interfaces extendidas. Esto permite construir jerarquías de contratos cada vez más especializados, sin los problemas de ambigüedad que tendría la herencia múltiple de clases, ya que las interfaces no tienen estado ni implementación (excepto por los métodos default, que en caso de conflicto deben ser resueltos por la interfaz que los hereda).

Java permite que una interfaz herede de múltiples interfaces. Por ejemplo: interface C extends A, B { }. Esto es posible porque no existe el problema del "diamante" relacionado con atributos o constructores, y los conflictos entre métodos default pueden ser solucionados explícitamente sobrescribiendo el método conflictivo en la interfaz hija. Una clase que implemente la interfaz hija deberá proporcionar implementación para todos los métodos heredados (tanto directos como indirectos).

Ejemplo:

interface Fichero {
    String leerContenido();  // Devuelve el contenido como String
}

interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);  // Escribe o sobrescribe el fichero
    void eliminar();  // Elimina el fichero
}

// Una clase concreta que implementa la interfaz hija
class DocumentoTexto implements FicheroEscribible {
    private String contenido = "";
    
    @Override
    public String leerContenido() {
        return contenido;
    }
    
    @Override
    public void escribirContenido(String contenido) {
        this.contenido = contenido;
        System.out.println("Contenido escrito: " + contenido);
    }
    
    @Override
    public void eliminar() {
        this.contenido = "";
        System.out.println("Fichero eliminado");
    }
}

// Uso polimórfico:
Fichero f = new DocumentoTexto();        // Solo se puede leer
FicheroEscribible fe = new DocumentoTexto();  // Se puede leer, escribir y eliminar

En este ejemplo, FicheroEscribible extiende a Fichero, heredando el método leerContenido() y añadiendo dos métodos nuevos. Una clase como DocumentoTexto debe implementar los tres métodos. El polimorfismo permite tratar un objeto DocumentoTexto como Fichero (solo lectura) o como FicheroEscribible (acceso completo), según la interfaz que se utilice como tipo de referencia.
