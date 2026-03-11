<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

A continuación, se presenta un ejemplo en C que modela una línea compuesta por dos puntos, utilizando composición mediante estructuras anidadas, junto con funciones para calcular la distancia entre puntos y la longitud de la línea.

#include <stdio.h>
#include <math.h>

// Definición de la estructura Punto, compuesta por dos coordenadas
struct Punto {
    double x;
    double y;
};

// Definición de la estructura Linea, compuesta por dos puntos (inicio y fin)
struct Linea {
    struct Punto inicio;
    struct Punto fin;
};

// Función que calcula la distancia entre dos puntos
double calcularDistancia(struct Punto p1, struct Punto p2) {
    double dx = p2.x - p1.x;
    double dy = p2.y - p1.y;
    return sqrt(dx * dx + dy * dy);
}

// Función que calcula la longitud de una línea (distancia entre sus dos puntos)
double calcularLongitudLinea(struct Linea linea) {
    return calcularDistancia(linea.inicio, linea.fin);
}

int main() {
    // Crear dos puntos
    struct Punto puntoA = {0.0, 0.0};
    struct Punto puntoB = {3.0, 4.0};
    
    // Crear una línea compuesta por los dos puntos
    struct Linea miLinea = {puntoA, puntoB};
    
    // Calcular y mostrar la distancia entre los puntos
    double distancia = calcularDistancia(puntoA, puntoB);
    printf("Distancia entre puntos: %f\n", distancia);
    
    // Calcular y mostrar la longitud de la línea
    double longitud = calcularLongitudLinea(miLinea);
    printf("Longitud de la línea: %f\n", longitud);
    
    return 0;
}

En este ejemplo, la relación de composición "tiene-un" se manifiesta claramente: una Linea tiene dos objetos de tipo struct Punto como partes constituyentes. La estructura Linea no define coordenadas propias, sino que utiliza la abstracción Punto para representar sus extremos. Esto permite reutilizar la función calcularDistancia, que opera sobre puntos, para determinar la longitud de la línea, demostrando cómo la composición facilita la construcción de funcionalidad compleja a partir de componentes más simples y reutilizables. La función calcularLongitudLinea actúa como una capa de abstracción que delega el cálculo a la función especializada en puntos, manteniendo así una separación de responsabilidades.


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

A continuación, se presenta la transformación del ejemplo anterior a Java, aplicando los principios de orientación a objetos y composición, junto con la inmutabilidad solicitada:

// Clase Punto inmutable
public final class Punto {
    private final double x;
    private final double y;
    
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    public double getX() {
        return x;
    }
    
    public double getY() {
        return y;
    }
    
    // Método que calcula la distancia hasta otro punto
    public double distancia(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

// Clase Linea inmutable compuesta por dos puntos
public final class Linea {
    private final Punto inicio;
    private final Punto fin;
    
    public Linea(Punto inicio, Punto fin) {
        // Se guardan las referencias a los puntos proporcionados
        // Como Punto es inmutable, no hay riesgo de modificación externa
        this.inicio = inicio;
        this.fin = fin;
    }
    
    public Punto getInicio() {
        return inicio;
    }
    
    public Punto getFin() {
        return fin;
    }
    
    // Método que calcula la longitud de la línea
    public double longitud() {
        // Delega el cálculo al método distancia de la clase Punto
        return inicio.distancia(fin);
    }
}

// Clase de prueba para demostrar el funcionamiento
public class PruebaLinea {
    public static void main(String[] args) {
        // Crear dos puntos inmutables
        Punto puntoA = new Punto(0.0, 0.0);
        Punto puntoB = new Punto(3.0, 4.0);
        
        // Crear una línea inmutable compuesta por los dos puntos
        Linea miLinea = new Linea(puntoA, puntoB);
        
        // Calcular y mostrar la distancia entre los puntos
        double distancia = puntoA.distancia(puntoB);
        System.out.println("Distancia entre puntos: " + distancia);
        
        // Calcular y mostrar la longitud de la línea
        double longitud = miLinea.longitud();
        System.out.println("Longitud de la línea: " + longitud);
        
        // Demostración de inmutabilidad:
        // Las siguientes líneas no compilarían o no modificarían el estado:
        // puntoA.x = 5.0;  // Error: x es private y no hay setter
        // miLinea.inicio = new Punto(1.0, 1.0);  // Error: inicio es private y final
    }
}

En esta implementación orientada a objetos, se superan las limitaciones del ejemplo en C mediante la aplicación de la encapsulación y la inmutabilidad. La clase Punto declara sus atributos como private final, lo que significa que solo son accesibles dentro de la propia clase y que deben ser asignados en el constructor, no pudiendo modificarse posteriormente. Al no proporcionar métodos setters, se garantiza que cualquier objeto Punto, una vez creado, mantendrá sus coordenadas inalterables durante todo su ciclo de vida. Esto proporciona una seguridad que no existía en C, donde cualquier función con acceso a la estructura podía modificar sus campos directamente.

La clase Linea sigue el mismo principio de inmutabilidad, declarando sus referencias a Punto también como private final. La relación de composición se establece en el constructor, donde se asignan los puntos que forman la línea. Al ser Punto inmutable, no existe el riesgo de que el estado interno de la línea se vea alterado por modificaciones externas en los puntos que la componen. El método longitud() delega el cálculo en el método distancia() del punto inicial, demostrando cómo la composición permite una interacción limpia entre objetos. Esta arquitectura refleja fielmente la intención de diseño: una línea es definida por sus dos extremos en el momento de su creación y permanece inalterable, lo que facilita el razonamiento sobre el código y previene errores por modificaciones accidentales.


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

La multiplicidad en el contexto de la composición (y de las relaciones entre clases en general) indica cuántas instancias de una clase participan en la relación con otra clase. Especifica el número de objetos de una clase que pueden estar asociados con un objeto de la otra clase. En el diseño orientado a objetos, comprender la multiplicidad es fundamental para modelar correctamente las reglas de negocio y las restricciones del mundo real que se intenta representar. Define los cardinales de la relación, pudiendo ser valores como "1" (exactamente uno), "0..1" (cero o uno), "" (cero o muchos), o "1.." (uno o muchos), entre otros.

En el ejemplo anterior de Linea y Punto, la multiplicidad es diferente en cada dirección, lo que refleja la naturaleza de la relación "tiene-un". De Linea a Punto, la multiplicidad es 2 (o "dos"): una línea concreta está compuesta exactamente por dos puntos, su inicio y su fin. No puede tener menos de dos puntos (no sería una línea) ni más de dos puntos en esta definición simple (una línea entre dos puntos). Esta cardinalidad fija es una característica clave de la composición en este caso.

De Punto a Linea, la multiplicidad es 0..* (cero o muchos). Un punto individual no "sabe" que forma parte de una línea ni tiene por qué estarlo. Un mismo objeto Punto puede ser el inicio de una línea, el fin de otra, o puede no pertenecer a ninguna línea. Incluso podría ser utilizado en múltiples líneas diferentes simultáneamente. Por lo tanto, desde la perspectiva del punto, no hay un límite superior definido en la cantidad de líneas de las que puede formar parte, pudiendo ser cero o muchas. Esta asimetría en las multiplicidades es típica en las relaciones de composición, donde el objeto compuesto (el todo) tiene un conocimiento y una dependencia estructural de sus partes, pero las partes (los componentes) son independientes y pueden existir y ser reutilizadas en múltiples contextos sin tener conocimiento del todo que las contiene.


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

La composición fuerte, conocida simplemente como composición en terminología UML (Lenguaje Unificado de Modelado), implica una relación de ciclo de vida dependiente entre el contenedor y sus partes. En este tipo de relación, las partes no tienen existencia independiente fuera del todo; son creadas y destruidas junto con el objeto contenedor. Si se elimina el objeto compuesto, todas sus partes componentes también desaparecen o dejan de tener sentido por sí mismas. Esta relación suele representarse en diagramas con un rombo relleno (◆) en el extremo del contenedor. Un ejemplo clásico sería la relación entre una Casa y sus Habitaciones: las habitaciones se crean cuando se construye la casa y se destruyen cuando la casa es demolida; una habitación no puede existir independientemente y luego ser asignada a otra casa.

La composición débil, denominada técnicamente agregación, establece una relación donde las partes pueden existir independientemente del todo. El objeto contenedor tiene referencias a otros objetos, pero estos pueden ser creados antes, compartidos con otros contenedores, o seguir existiendo después de que el contenedor sea destruido. El ciclo de vida de las partes no está ligado al del todo. En UML, se representa con un rombo vacío (◇) en el extremo del agregado. Por ejemplo, en la relación entre un Equipo y sus Jugadores, los jugadores existen como personas antes de fichar por el equipo, pueden ser traspasados a otro equipo, y si el equipo desaparece, los jugadores continúan existiendo.

En cuanto al ciclo de vida de los objetos, la consecuencia práctica es radicalmente diferente. En la composición propiamente dicha (fuerte), la creación y destrucción están sincronizadas: el constructor del contenedor suele encargarse de instanciar las partes, y cuando el contenedor es recolectado por el garbage collector y no hay otras referencias a las partes, estas también serán recolectadas. En la agregación (débil), las partes se crean externamente y se pasan al contenedor a través del constructor o métodos, pudiendo ser compartidas. La destrucción del contenedor no implica la destrucción de las partes, que seguirán vivas si otras partes del sistema las referencian. Es importante destacar que en la práctica, con la notación y el lenguaje cotidiano, se suele usar el término genérico "composición" para ambas, pero técnicamente, cuando se habla de "asociación o agregación" se hace referencia a la débil, mientras que el término "composición" sin apellidos se reserva para la fuerte.


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

Cuando una clase utiliza a otra únicamente como tipo de parámetro en un método, como tipo de retorno, creando instancias locales dentro de un método o declarando variables locales de ese tipo, no se está ante un caso de composición ni agregación, sino ante una relación de dependencia o uso. En estos escenarios, la clase no guarda una referencia a la otra como parte de su estado (atributo), sino que la relación es meramente circunstancial y temporal, limitada al ámbito de ejecución de un método específico. La clase dependiente necesita conocer a la otra clase para poder invocar sus métodos o recibir objetos de ella, pero no establece un vínculo estructural que perdure más allá de la llamada al método.

La diferencia fundamental con la composición radica en la permanencia y la propiedad de la relación. En la composición, la clase contenedora tiene un atributo que referencia a otra clase, estableciendo un vínculo que persiste durante toda la vida del objeto contenedor y que forma parte de su identidad y estructura. En cambio, en la dependencia, la colaboración es puntual y no deja huella en el estado del objeto. Por ejemplo, si una clase Calculadora tiene un método que recibe un Punto y devuelve la distancia al origen, se dice que Calculadora depende de Punto, pero no que esté compuesta por puntos. Esta distinción es importante para el diseño, ya que las dependencias representan un acoplamiento más débil y fácil de gestionar que la composición, y suelen identificarse en los diagramas de clases con una línea discontinua y una flecha abierta.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

A continuación, se presentan las dos implementaciones solicitadas que muestran la diferencia entre composición fuerte y débil en la relación entre Linea y Punto:

- Composición fuerte (ciclo de vida ligado):

// Clase Punto (podría ser compartida, pero en este caso se crea internamente)
public class Punto {
    private double x;
    private double y;
    
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    // Getters y setters (por simplicidad se permiten modificaciones)
    public double getX() { return x; }
    public double getY() { return y; }
    public void setX(double x) { this.x = x; }
    public void setY(double y) { this.y = y; }
    
    public double distancia(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

// Clase Linea con composición fuerte
public class LineaFuerte {
    private Punto inicio;
    private Punto fin;
    
    // Constructor que crea internamente los puntos
    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.inicio = new Punto(x1, y1);  // Los puntos nacen con la línea
        this.fin = new Punto(x2, y2);
    }
    
    public double longitud() {
        return inicio.distancia(fin);
    }
    
    // Método que demuestra que al modificar la línea se modifican sus puntos internos
    public void mover(double dx, double dy) {
        inicio.setX(inicio.getX() + dx);
        inicio.setY(inicio.getY() + dy);
        fin.setX(fin.getX() + dx);
        fin.setY(fin.getY() + dy);
    }
    
    // No hay getters que expongan los puntos directamente para evitar que escapen
    public double getInicioX() { return inicio.getX(); }
    public double getInicioY() { return inicio.getY(); }
    public double getFinX() { return fin.getX(); }
    public double getFinY() { return fin.getY(); }
}

- Composición débil (agregación, ciclo de vida independiente):

// Clase Linea con composición débil (agregación)
public class LineaDebil {
    private Punto inicio;
    private Punto fin;
    
    // Constructor que recibe puntos ya existentes desde el exterior
    public LineaDebil(Punto inicio, Punto fin) {
        this.inicio = inicio;  // Los puntos viven independientemente
        this.fin = fin;
    }
    
    public double longitud() {
        return inicio.distancia(fin);
    }
    
    // Método que mueve la línea (delega en los puntos)
    public void mover(double dx, double dy) {
        inicio.setX(inicio.getX() + dx);
        inicio.setY(inicio.getY() + dy);
        fin.setX(fin.getX() + dx);
        fin.setY(fin.getY() + dy);
    }
    
    // Getters que exponen los puntos directamente
    public Punto getInicio() { return inicio; }
    public Punto getFin() { return fin; }
}

- Clase de prueba que demuestra las diferencias:

public class PruebaComposicion {
    public static void main(String[] args) {
        // Demostración de composición fuerte
        System.out.println("=== COMPOSICIÓN FUERTE ===");
        LineaFuerte lineaF = new LineaFuerte(0, 0, 3, 4);
        System.out.println("Longitud línea fuerte: " + lineaF.longitud());
        
        // Los puntos no son accesibles desde fuera, están encapsulados
        // No se puede hacer: Punto p = lineaF.getInicio(); // ¡Error!
        
        // Si la línea es recolectada, sus puntos también lo serán (en teoría)
        lineaF = null;  // El objeto LineaFuerte y sus dos Puntos son candidatos a GC
        
        // Demostración de composición débil
        System.out.println("\n=== COMPOSICIÓN DÉBIL ===");
        
        // Los puntos pueden existir independientemente
        Punto puntoA = new Punto(0, 0);
        Punto puntoB = new Punto(3, 4);
        
        // Se crea una línea a partir de puntos existentes
        LineaDebil lineaD = new LineaDebil(puntoA, puntoB);
        System.out.println("Longitud línea débil: " + lineaD.longitud());
        
        // Los puntos son accesibles desde fuera
        System.out.println("Punto A desde línea: (" + 
                           lineaD.getInicio().getX() + ", " + 
                           lineaD.getInicio().getY() + ")");
        
        // Se puede modificar un punto independientemente de la línea
        puntoA.setX(10);
        System.out.println("Punto A modificado externamente: (" + 
                           puntoA.getX() + ", " + puntoA.getY() + ")");
        System.out.println("¿Afecta a la línea? Nueva longitud: " + 
                           lineaD.longitud()); // La longitud cambia
        
        // La línea puede desaparecer pero los puntos siguen existiendo
        lineaD = null;  // Solo desaparece la línea, los puntos puntoA y puntoB siguen vivos
        System.out.println("Punto A sigue vivo: (" + 
                           puntoA.getX() + ", " + puntoA.getY() + ")");
    }
}

En la composición fuerte (LineaFuerte), los objetos Punto son creados internamente por el constructor de la línea a partir de coordenadas primitivas. La línea tiene total responsabilidad sobre el ciclo de vida de sus puntos: nacen con ella y, conceptualmente, mueren con ella. Además, se protege la encapsulación al no exponer las referencias directas a los puntos, ofreciendo únicamente métodos para consultar o modificar sus coordenadas de forma controlada. Si la línea deja de ser referenciada y el recolector de basura actúa, sus puntos internos también quedarán sin referencias y serán recolectados (asumiendo que no hay otras referencias externas hacia ellos, lo cual está garantizado al no haber getters que los expongan).

En la composición débil o agregación (LineaDebil), los objetos Punto existen independientemente y se pasan a la línea a través del constructor. La línea simplemente guarda referencias a esos puntos, pero no controla su creación ni su destrucción. Los puntos pueden ser compartidos con otras partes del sistema y seguir existiendo aunque la línea desaparezca. Esto se demuestra en el ejemplo cuando se modifica puntoA externamente y la longitud de la línea cambia, evidenciando que la línea solo "observa" el estado actual de los puntos sin poseerlos realmente. Cuando la línea es anulada (lineaD = null), los puntos continúan accesibles a través de las referencias originales. Esta distinción en el ciclo de vida y la propiedad es la diferencia conceptual fundamental entre ambos tipos de relación.


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

En Java, en la composición fuerte, el contenedor no destruye explícitamente los objetos componentes porque Java gestiona la memoria automáticamente mediante un recolector de basura (garbage collector). A diferencia de C o C++, donde el programador debe liberar manualmente la memoria con free o delete, en Java no existe una instrucción para destruir objetos de forma explícita. La destrucción de objetos en Java es responsabilidad del recolector de basura, que periódicamente identifica y libera la memoria de aquellos objetos que ya no son alcanzables desde ninguna referencia activa en el programa. Esto significa que un objeto permanece vivo mientras exista al menos una referencia a él desde algún punto del código en ejecución.

En el caso de la composición fuerte, los objetos componentes son creados por el contenedor y las únicas referencias a ellos son las que el propio contenedor guarda en sus atributos privados. Mientras el contenedor sea alcanzable (es decir, mientras exista una referencia a él desde alguna parte del programa), sus componentes también serán alcanzables a través de él, por lo que permanecerán vivos. Cuando el contenedor deja de ser alcanzable (por ejemplo, cuando se sale de su ámbito o se le asigna null a la última referencia que apuntaba a él), entonces tanto el contenedor como sus componentes se convierten en candidatos para ser recolectados. Al no existir otras referencias externas a los componentes (gracias a la encapsulación que evita que sus referencias escapen), todos ellos serán recolectados juntos cuando el recolector de basura lo determine.

Por lo tanto, aunque en el código no se observe una destrucción explícita, conceptualmente se cumple la semántica de la composición fuerte: el ciclo de vida de los componentes está ligado al del contenedor. El programador solo debe asegurarse de que las referencias a los componentes no escapen del contenedor (por ejemplo, no proporcionando getters que devuelvan las referencias directas) y de que el contenedor sea el único que posee referencias a ellos. Con estas condiciones, el recolector de basura se encarga automáticamente de que el contenedor y sus partes tengan el mismo ciclo de vida, liberando al programador de la compleja y propensa a errores gestión manual de memoria que sería necesaria en otros lenguajes. Esta característica es una de las ventajas significativas de Java para implementar composición fuerte de manera segura y fiable.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

A continuación, se presenta la implementación solicitada que modela un departamento con una colección de profesores y un director, utilizando composición débil (agregación) y manteniendo las invariantes mediante excepciones:

import java.util.Arrays;

public class Profesor {
    private String nombre;
    private String identificacion;
    
    public Profesor(String nombre, String identificacion) {
        this.nombre = nombre;
        this.identificacion = identificacion;
    }
    
    public String getNombre() {
        return nombre;
    }
    
    public String getIdentificacion() {
        return identificacion;
    }
    
    @Override
    public String toString() {
        return nombre + " (" + identificacion + ")";
    }
}

public class Departamento {
    private static final int MAX_PROFESORES = 50;
    private Profesor[] profesores;
    private int numProfesores;  // Contador de profesores realmente almacenados
    private Profesor director;
    
    // Constructor que recibe el director inicial y opcionalmente algunos profesores
    public Departamento(Profesor directorInicial, Profesor... profesoresIniciales) {
        // Validar que el director inicial no sea null
        if (directorInicial == null) {
            throw new IllegalArgumentException("El director no puede ser null");
        }
        
        this.profesores = new Profesor[MAX_PROFESORES];
        this.numProfesores = 0;
        
        // Añadir el director como primer profesor (si no está ya incluido)
        anyadirProfesorInterno(directorInicial);
        this.director = directorInicial;
        
        // Añadir el resto de profesores iniciales
        for (Profesor p : profesoresIniciales) {
            if (p != null && !p.equals(directorInicial)) { // Evitar duplicados del director
                anyadirProfesorInterno(p);
            }
        }
    }
    
    // Método privado para añadir profesor sin romper encapsulación
    private void anyadirProfesorInterno(Profesor profesor) {
        if (profesor == null) {
            throw new IllegalArgumentException("No se puede añadir un profesor null");
        }
        if (numProfesores >= MAX_PROFESORES) {
            throw new IllegalStateException("Se ha alcanzado el máximo de profesores (" + MAX_PROFESORES + ")");
        }
        // Verificar que no esté ya en la lista (por identificador)
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i].getIdentificacion().equals(profesor.getIdentificacion())) {
                throw new IllegalArgumentException("Ya existe un profesor con esa identificación");
            }
        }
        profesores[numProfesores] = profesor;
        numProfesores++;
    }
    
    // Método público para añadir profesor (expone la funcionalidad)
    public void anyadirProfesor(Profesor profesor) {
        anyadirProfesorInterno(profesor);
    }
    
    // Método para obtener el número de profesores
    public int getNumProfesores() {
        return numProfesores;
    }
    
    // Método para obtener un profesor por posición (sin exponer el array)
    public Profesor getProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida: " + posicion);
        }
        return profesores[posicion];
    }
    
    // Método para eliminar un profesor por posición
    public void eliminarProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida: " + posicion);
        }
        
        Profesor profesorEliminado = profesores[posicion];
        
        // Verificar que no sea el director
        if (profesorEliminado.equals(director)) {
            throw new IllegalStateException("No se puede eliminar al director del departamento");
        }
        
        // Desplazar los elementos siguientes una posición hacia la izquierda
        for (int i = posicion; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[numProfesores - 1] = null; // Liberar referencia
        numProfesores--;
    }
    
    // Método para cambiar el director
    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser null");
        }
        
        // Verificar que el nuevo director está en la lista de profesores
        boolean encontrado = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i].equals(nuevoDirector)) {
                encontrado = true;
                break;
            }
        }
        
        if (!encontrado) {
            throw new IllegalArgumentException("El nuevo director debe ser un profesor del departamento");
        }
        
        this.director = nuevoDirector;
    }
    
    // Getter para el director (seguro, no expone referencia interna modificable)
    public Profesor getDirector() {
        return director;
    }
    
    // Método para mostrar el estado del departamento
    public void mostrarEstado() {
        System.out.println("=== DEPARTAMENTO ===");
        System.out.println("Director: " + director);
        System.out.println("Profesores (" + numProfesores + "):");
        for (int i = 0; i < numProfesores; i++) {
            System.out.println("  " + i + ": " + profesores[i]);
        }
    }
}

public class PruebaDepartamento {
    public static void main(String[] args) {
        try {
            // Crear algunos profesores
            Profesor prof1 = new Profesor("Ana García", "A001");
            Profesor prof2 = new Profesor("Carlos López", "C002");
            Profesor prof3 = new Profesor("María Martínez", "M003");
            Profesor prof4 = new Profesor("Juan Rodríguez", "J004");
            
            // Crear departamento con director inicial y algunos profesores
            Departamento dpto = new Departamento(prof1, prof2, prof3);
            System.out.println("Departamento creado:");
            dpto.mostrarEstado();
            
            // Añadir más profesores
            System.out.println("\nAñadiendo profesor Juan...");
            dpto.anyadirProfesor(prof4);
            dpto.mostrarEstado();
            
            // Cambiar director
            System.out.println("\nCambiando director a Carlos...");
            dpto.cambiarDirector(prof2);
            dpto.mostrarEstado();
            
            // Intentar eliminar al director (debe lanzar excepción)
            System.out.println("\nIntentando eliminar al director (Carlos)...");
            dpto.eliminarProfesor(1); // Suponiendo que Carlos está en posición 1
            // Esta línea no se ejecutará si se lanza la excepción
            
        } catch (Exception e) {
            System.out.println("Excepción capturada: " + e.getMessage());
        }
        
        // Demostración de violación de invariante al crear departamento
        System.out.println("\n--- Demostración de validaciones ---");
        try {
            // Intentar crear departamento con director null
            Departamento dptoInvalido = new Departamento(null);
        } catch (IllegalArgumentException e) {
            System.out.println("Error al crear: " + e.getMessage());
        }
        
        try {
            // Crear departamento y luego intentar cambiar director a alguien que no está
            Profesor profExterno = new Profesor("Externo", "X999");
            Departamento dpto2 = new Departamento(new Profesor("Director", "D001"));
            dpto2.cambiarDirector(profExterno);
        } catch (IllegalArgumentException e) {
            System.out.println("Error al cambiar director: " + e.getMessage());
        }
    }
}

En esta implementación, se establece una relación de composición débil (agregación) entre Departamento y Profesor. Los objetos Profesor existen independientemente del departamento y pueden ser creados antes, compartidos con otros departamentos (aunque no se muestra en el ejemplo) y seguir existiendo después de que el departamento desaparezca. El departamento simplemente mantiene referencias a estos objetos en su array interno. La gestión del ciclo de vida es independiente: cuando un objeto Departamento es recolectado, los objetos Profesor referenciados seguirán existiendo si hay otras referencias a ellos en el sistema.

La clase Departamento mantiene dos relaciones de agregación simultáneas: una colección de profesores (representada mediante un array primitivo de Profesor con un máximo de 50) y una referencia individual al director, que debe ser siempre uno de los profesores de la colección. Para preservar la encapsulación, el array profesores y el contador numProfesores son privados, y se proporcionan métodos controlados para interactuar con la colección: anyadirProfesor, getNumProfesores y getProfesor (este último devuelve la referencia al objeto, lo cual es aceptable en agregación ya que los objetos pueden ser compartidos). El método eliminarProfesor lanza una excepción si se intenta eliminar al director, preservando así la invariante de que el director debe pertenecer a la lista. El cambio de director mediante cambiarDirector valida que el nuevo director sea un profesor existente en la colección, manteniendo también la coherencia. Esta implementación demuestra cómo la composición débil permite relaciones flexibles donde los componentes tienen identidad y ciclo de vida propios, pero el objeto contenedor impone reglas de negocio sobre cómo se relacionan entre sí.


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

A continuación, se presenta la versión del código anterior utilizando List en lugar de arrays primitivos, junto con las explicaciones solicitadas:

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Profesor {
    private String nombre;
    private String identificacion;
    
    public Profesor(String nombre, String identificacion) {
        this.nombre = nombre;
        this.identificacion = identificacion;
    }
    
    public String getNombre() {
        return nombre;
    }
    
    public String getIdentificacion() {
        return identificacion;
    }
    
    @Override
    public String toString() {
        return nombre + " (" + identificacion + ")";
    }
}

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Departamento {
    private static final int MAX_PROFESORES = 50;
    private List<Profesor> profesores;
    private Profesor director;
    
    // Constructor que recibe el director inicial y opcionalmente algunos profesores
    public Departamento(Profesor directorInicial, Profesor... profesoresIniciales) {
        // Validar que el director inicial no sea null
        if (directorInicial == null) {
            throw new IllegalArgumentException("El director no puede ser null");
        }
        
        this.profesores = new ArrayList<>();
        
        // Añadir el director como primer profesor (si no está ya incluido)
        anyadirProfesorInterno(directorInicial);
        this.director = directorInicial;
        
        // Añadir el resto de profesores iniciales
        for (Profesor p : profesoresIniciales) {
            if (p != null && !p.equals(directorInicial)) {
                anyadirProfesorInterno(p);
            }
        }
    }
    
    // Método privado para añadir profesor
    private void anyadirProfesorInterno(Profesor profesor) {
        if (profesor == null) {
            throw new IllegalArgumentException("No se puede añadir un profesor null");
        }
        if (profesores.size() >= MAX_PROFESORES) {
            throw new IllegalStateException("Se ha alcanzado el máximo de profesores (" + MAX_PROFESORES + ")");
        }
        // Verificar que no esté ya en la lista por identificador
        for (Profesor p : profesores) {
            if (p.getIdentificacion().equals(profesor.getIdentificacion())) {
                throw new IllegalArgumentException("Ya existe un profesor con esa identificación");
            }
        }
        profesores.add(profesor);
    }
    
    // Método público para añadir profesor
    public void anyadirProfesor(Profesor profesor) {
        anyadirProfesorInterno(profesor);
    }
    
    // Método para obtener el número de profesores
    public int getNumProfesores() {
        return profesores.size();
    }
    
    // Método para obtener un profesor por posición
    public Profesor getProfesor(int posicion) {
        if (posicion < 0 || posicion >= profesores.size()) {
            throw new IndexOutOfBoundsException("Posición inválida: " + posicion);
        }
        return profesores.get(posicion);
    }
    
    // Método para eliminar un profesor por posición
    public void eliminarProfesor(int posicion) {
        if (posicion < 0 || posicion >= profesores.size()) {
            throw new IndexOutOfBoundsException("Posición inválida: " + posicion);
        }
        
        Profesor profesorEliminado = profesores.get(posicion);
        
        // Verificar que no sea el director
        if (profesorEliminado.equals(director)) {
            throw new IllegalStateException("No se puede eliminar al director del departamento");
        }
        
        profesores.remove(posicion);
    }
    
    // Método para cambiar el director
    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser null");
        }
        
        // Verificar que el nuevo director está en la lista de profesores
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException("El nuevo director debe ser un profesor del departamento");
        }
        
        this.director = nuevoDirector;
    }
    
    // Getter para el director
    public Profesor getDirector() {
        return director;
    }
    
    // Método para obtener todos los profesores de forma segura
    public List<Profesor> getTodosLosProfesores() {
        // Devuelve una vista no modificable para proteger la encapsulación
        return Collections.unmodifiableList(profesores);
    }
    
    // Método para mostrar el estado del departamento
    public void mostrarEstado() {
        System.out.println("=== DEPARTAMENTO ===");
        System.out.println("Director: " + director);
        System.out.println("Profesores (" + profesores.size() + "):");
        for (int i = 0; i < profesores.size(); i++) {
            System.out.println("  " + i + ": " + profesores.get(i));
        }
    }
}

- Código ahorrado respecto a la versión con arrays:

Al utilizar List en lugar de arrays primitivos, se ha eliminado la necesidad de gestionar manualmente múltiples aspectos de la colección:

1. Contador manual: Desaparece la variable numProfesores y toda la lógica para incrementarla, decrementarla y mantenerla sincronizada con el array.

2. Gestión de capacidad: No es necesario verificar constantemente que no se supere el tamaño del array ni llevar un control del espacio disponible más allá del límite máximo.

3. Desplazamiento de elementos: En la eliminación, ya no se necesita el bucle manual para desplazar elementos y liberar la última posición; List.remove() gestiona internamente el reajuste de la colección.

4. Inicialización del array: Se elimina la creación del array con tamaño fijo y la asignación de valores nulos iniciales.

5. Búsqueda simplificada: En la verificación de duplicados y en la comprobación de pertenencia del director, se puede usar el método contains() en lugar de implementar bucles manuales, aunque en el ejemplo se ha mantenido el bucle para la verificación por identificador por coherencia con la lógica de negocio.

- Problema al devolver la lista interna directamente:

Si en lugar del método getProfesor(int pos) existiera un método que devolviera todos los profesores a la vez, como por ejemplo public List<Profesor> getTodosLosProfesores(), devolver directamente la lista interna profesores rompería la encapsulación. Esto permitiría que el código externo modificara la colección sin pasar por los métodos controlados del departamento, pudiendo añadir o eliminar profesores arbitrariamente, violando las invariantes de clase (como que el director debe estar siempre en la lista o que no puede haber más de 50 profesores). Por ejemplo, un cliente podría hacer departamento.getTodosLosProfesores().clear() y eliminar todos los profesores, dejando el departamento en un estado inconsistente.

- Solución al problema:

La solución consiste en devolver una vista no modificable de la lista interna, como se muestra en el método getTodosLosProfesores() del código. Collections.unmodifiableList(profesores) envuelve la lista original y lanza una excepción UnsupportedOperationException ante cualquier intento de modificación (add, remove, set, etc.). Esto permite que los clientes del departamento puedan iterar sobre los profesores y consultarlos, pero no alterar la colección. Si se necesita que el cliente pueda modificar la colección, debería hacerlo siempre a través de los métodos públicos del departamento (anyadirProfesor, eliminarProfesor), que mantienen las invariantes. Esta técnica protege la integridad del objeto sin necesidad de realizar copias defensivas, que serían otra alternativa válida pero más costosa en memoria y rendimiento.


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

A continuación, se presenta un ejemplo de composición recursiva con una clase Persona inmutable que tiene una referencia a su madre, también de tipo Persona, junto con un ejemplo de uso que construye una familia de tres generaciones:

// Clase Persona inmutable con composición recursiva
public final class Persona {
    private final String nombre;
    private final int edad;
    private final Persona madre;  // Referencia a otra Persona (composición recursiva)
    
    public Persona(String nombre, int edad, Persona madre) {
        this.nombre = nombre;
        this.edad = edad;
        this.madre = madre;  // Puede ser null si no se conoce o no tiene madre
    }
    
    // Constructor para crear una persona sin información de la madre
    public Persona(String nombre, int edad) {
        this(nombre, edad, null);
    }
    
    public String getNombre() {
        return nombre;
    }
    
    public int getEdad() {
        return edad;
    }
    
    public Persona getMadre() {
        return madre;  // Se devuelve la referencia directamente por ser inmutable
    }
    
    // Método para obtener el nombre de la madre de forma segura
    public String getNombreMadre() {
        return (madre != null) ? madre.getNombre() : "Desconocida";
    }
    
    // Método recursivo para obtener la edad de la abuela materna
    public int getEdadAbuelaMaterna() {
        if (madre == null) {
            return 0;  // No hay información de la madre
        }
        Persona abuela = madre.getMadre();
        return (abuela != null) ? abuela.getEdad() : 0;
    }
    
    // Método recursivo para obtener el nombre completo del árbol materno
    public String getArbolMaterno() {
        if (madre == null) {
            return nombre + " (sin información materna)";
        }
        return nombre + " (hija de " + madre.getArbolMaterno() + ")";
    }
    
    @Override
    public String toString() {
        return nombre + " (" + edad + " años)";
    }
}

public class EjemploComposicionRecursiva {
    public static void main(String[] args) {
        // Crear una familia de tres generaciones
        Persona abuela = new Persona("María", 75);
        Persona madre = new Persona("Ana", 45, abuela);
        Persona hijo = new Persona("Carlos", 20, madre);
        Persona nieta = new Persona("Laura", 5, madre);  // Misma madre, hermana de Carlos
        
        System.out.println("=== INFORMACIÓN FAMILIAR ===");
        
        // Mostrar información de cada persona
        System.out.println("Abuela: " + abuela);
        System.out.println("  Madre de abuela: " + abuela.getNombreMadre());
        
        System.out.println("\nMadre: " + madre);
        System.out.println("  Madre de madre: " + madre.getNombreMadre());
        
        System.out.println("\nHijo: " + hijo);
        System.out.println("  Madre de hijo: " + hijo.getNombreMadre());
        System.out.println("  Edad de abuela materna: " + hijo.getEdadAbuelaMaterna());
        
        System.out.println("\nNieta: " + nieta);
        System.out.println("  Madre de nieta: " + nieta.getNombreMadre());
        
        // Demostrar que la composición es recursiva navegando por la cadena
        System.out.println("\n=== ÁRBOL MATERNO ===");
        System.out.println("Árbol de Carlos: " + hijo.getArbolMaterno());
        System.out.println("Árbol de Ana: " + madre.getArbolMaterno());
        System.out.println("Árbol de María: " + abuela.getArbolMaterno());
        
        // Demostrar inmutabilidad: no se puede modificar la madre después de creada
        // Las siguientes líneas no compilarían o no modificarían el estado:
        // hijo.madre = abuela;  // Error: madre es private y no hay setter
        // hijo.getMadre().setNombre("otro");  // Error: Persona no tiene setters
        
        // Las personas pueden compartir la misma madre (comprobación de igualdad)
        System.out.println("\n=== COMPARTIENDO MADRE ===");
        System.out.println("¿Carlos y Laura tienen la misma madre? " + 
                           (hijo.getMadre() == nieta.getMadre())); // true
        System.out.println("¿Carlos y Ana tienen la misma madre? " + 
                           (hijo.getMadre() == madre.getMadre())); // false
    }
}

La composición recursiva se produce cuando una clase contiene una referencia a otra instancia de la misma clase, creando una estructura que puede ser recorrida de forma recursiva. En el ejemplo, la clase Persona tiene un atributo madre que es también de tipo Persona, lo que permite construir cadenas o árboles de objetos relacionados. Esta estructura es inmutable porque todos los atributos son final y no se proporcionan métodos modificadores; una vez creada una persona, su madre no puede cambiar, lo que garantiza que las relaciones familiares permanezcan estables y predecibles a lo largo del tiempo. Los métodos como getArbolMaterno() demuestran cómo se puede navegar recursivamente por la cadena de madres, llamando al mismo método sobre el objeto madre hasta alcanzar el caso base (cuando no hay madre).

Otros ejemplos clásicos de composiciones recursivas en programación incluyen:

1. Estructuras de datos: Las listas enlazadas, donde un nodo contiene una referencia al siguiente nodo del mismo tipo; los árboles binarios, donde un nodo tiene referencias a sus hijos izquierdo y derecho, también del tipo nodo.

2. Sistemas de archivos: Una carpeta puede contener otras carpetas, creando una jerarquía recursiva donde cada carpeta es del mismo tipo y puede contener más instancias de sí misma.

3. Organigramas empresariales: Un empleado puede tener subordinados que son también empleados, formando una estructura jerárquica donde cada nodo es del mismo tipo.

4. Expresiones matemáticas: En un procesador de expresiones, una operación puede contener otras operaciones como operandos (por ejemplo, una suma puede tener como operandos otras sumas o multiplicaciones).

5. Documentos con secciones: Un documento puede contener secciones, y cada sección puede contener subsecciones, todas del mismo tipo, permitiendo estructuras anidadas de contenido.

Estas estructuras recursivas son fundamentales en informática porque permiten representar de forma natural y eficiente relaciones jerárquicas o anidadas que aparecen en multitud de dominios.


## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

Las relaciones de composición bidireccionales (o agregación bidireccional) ocurren cuando dos clases están relacionadas de forma que cada una tiene una referencia a la otra, permitiendo la navegación en ambos sentidos de la relación. En una relación unidireccional, solo una de las clases conoce a la otra (por ejemplo, Departamento conoce a sus Profesores, pero Profesor no sabe a qué departamento pertenece). En una relación bidireccional, ambas clases mantienen referencias mutuas: un Profesor sabría a qué Departamento pertenece, y un Departamento conocería a sus Profesores. Esto crea un vínculo de doble dirección que debe mantenerse coherente en todo momento, lo que implica una responsabilidad adicional en la implementación para garantizar que ambas referencias apunten correctamente entre sí y que las modificaciones en un extremo se reflejen adecuadamente en el otro.

Para implementar una relación bidireccional en el ejemplo de Profesor y Departamento, sería necesario modificar ambas clases para que cada una contenga una referencia a la otra, y establecer mecanismos que mantengan la consistencia de la relación. A continuación, se muestra cómo podría implementarse:

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Profesor {
    private String nombre;
    private String identificacion;
    private Departamento departamento;  // Referencia al departamento (puede ser null)
    
    public Profesor(String nombre, String identificacion) {
        this.nombre = nombre;
        this.identificacion = identificacion;
        this.departamento = null;  // Inicialmente sin departamento
    }
    
    public String getNombre() {
        return nombre;
    }
    
    public String getIdentificacion() {
        return identificacion;
    }
    
    public Departamento getDepartamento() {
        return departamento;
    }
    
    // Método de paquete o protegido para establecer el departamento
    // Se evita hacerlo público para que solo el Departamento pueda gestionar la relación
    void setDepartamento(Departamento departamento) {
        this.departamento = departamento;
    }
    
    @Override
    public String toString() {
        return nombre + " (" + identificacion + ")";
    }
}

public class Departamento {
    private static final int MAX_PROFESORES = 50;
    private List<Profesor> profesores;
    private Profesor director;
    
    public Departamento(Profesor directorInicial, Profesor... profesoresIniciales) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("El director no puede ser null");
        }
        
        this.profesores = new ArrayList<>();
        
        // IMPORTANTE: Establecer la relación bidireccional al añadir profesores
        anyadirProfesorConBidireccion(directorInicial);
        this.director = directorInicial;
        
        for (Profesor p : profesoresIniciales) {
            if (p != null && !p.equals(directorInicial)) {
                anyadirProfesorConBidireccion(p);
            }
        }
    }
    
    // Método privado que añade profesor y establece la relación en ambos sentidos
    private void anyadirProfesorConBidireccion(Profesor profesor) {
        if (profesor == null) {
            throw new IllegalArgumentException("No se puede añadir un profesor null");
        }
        if (profesores.size() >= MAX_PROFESORES) {
            throw new IllegalStateException("Se ha alcanzado el máximo de profesores");
        }
        
        // Verificar que no esté ya en la lista
        for (Profesor p : profesores) {
            if (p.getIdentificacion().equals(profesor.getIdentificacion())) {
                throw new IllegalArgumentException("Profesor duplicado");
            }
        }
        
        // Si el profesor ya pertenece a otro departamento, habría que decidir la política
        if (profesor.getDepartamento() != null && profesor.getDepartamento() != this) {
            throw new IllegalStateException("El profesor ya pertenece a otro departamento");
        }
        
        // Establecer la relación en ambos sentidos
        profesores.add(profesor);
        profesor.setDepartamento(this);  // El profesor ahora sabe que pertenece a este departamento
    }
    
    // Método público para añadir profesor (delega en el privado)
    public void anyadirProfesor(Profesor profesor) {
        anyadirProfesorConBidireccion(profesor);
    }
    
    // Método para eliminar un profesor
    public void eliminarProfesor(int posicion) {
        if (posicion < 0 || posicion >= profesores.size()) {
            throw new IndexOutOfBoundsException("Posición inválida");
        }
        
        Profesor profesorEliminado = profesores.get(posicion);
        
        if (profesorEliminado.equals(director)) {
            throw new IllegalStateException("No se puede eliminar al director");
        }
        
        // Eliminar la relación en ambos sentidos
        profesores.remove(posicion);
        profesorEliminado.setDepartamento(null);  // El profesor ya no pertenece a ningún departamento
    }
    
    // Método para cambiar el director
    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser null");
        }
        
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException("El nuevo director debe ser un profesor del departamento");
        }
        
        this.director = nuevoDirector;
    }
    
    public Profesor getDirector() {
        return director;
    }
    
    public int getNumProfesores() {
        return profesores.size();
    }
    
    public Profesor getProfesor(int posicion) {
        if (posicion < 0 || posicion >= profesores.size()) {
            throw new IndexOutOfBoundsException("Posición inválida");
        }
        return profesores.get(posicion);
    }
    
    public List<Profesor> getTodosLosProfesores() {
        return Collections.unmodifiableList(profesores);
    }
    
    public void mostrarEstado() {
        System.out.println("=== DEPARTAMENTO ===");
        System.out.println("Director: " + director);
        System.out.println("Profesores (" + profesores.size() + "):");
        for (int i = 0; i < profesores.size(); i++) {
            Profesor p = profesores.get(i);
            System.out.println("  " + i + ": " + p + " -> " + 
                              (p.getDepartamento() == this ? "✔️ en este depto" : "❌ inconsistente"));
        }
    }
}

public class PruebaBidireccional {
    public static void main(String[] args) {
        // Crear profesores (inicialmente sin departamento)
        Profesor prof1 = new Profesor("Ana García", "A001");
        Profesor prof2 = new Profesor("Carlos López", "C002");
        Profesor prof3 = new Profesor("María Martínez", "M003");
        
        System.out.println("Antes de crear departamento:");
        System.out.println("prof1.departamento: " + prof1.getDepartamento()); // null
        System.out.println("prof2.departamento: " + prof2.getDepartamento()); // null
        
        // Crear departamento (establece relaciones bidireccionales automáticamente)
        Departamento dpto = new Departamento(prof1, prof2, prof3);
        
        System.out.println("\nDespués de crear departamento:");
        System.out.println("prof1.departamento: " + 
                          (prof1.getDepartamento() == dpto ? "Departamento correcto" : "Incorrecto"));
        System.out.println("prof2.departamento: " + 
                          (prof2.getDepartamento() == dpto ? "Departamento correcto" : "Incorrecto"));
        
        dpto.mostrarEstado();
        
        // Demostrar que la relación se mantiene al eliminar
        System.out.println("\nEliminando a María (posición 2)...");
        dpto.eliminarProfesor(2);
        
        System.out.println("Después de eliminar:");
        System.out.println("prof3.departamento: " + prof3.getDepartamento()); // null
        dpto.mostrarEstado();
    }
}

La implementación de una relación bidireccional requiere una gestión cuidadosa de la consistencia en ambos sentidos. Cada vez que se añade un profesor al departamento, no solo se agrega a la lista interna, sino que también se invoca profesor.setDepartamento(this) para que el profesor conozca su departamento. Análogamente, al eliminar un profesor, se debe establecer su referencia a null. Esta sincronización es crucial para evitar estados inconsistentes donde un profesor aparezca en la lista de un departamento pero su referencia a departamento apunte a otro lugar o siga apuntando a un departamento del que ya fue eliminado.

Las decisiones de diseño importantes incluyen: el método setDepartamento en Profesor debe tener visibilidad de paquete o protegida para que solo las clases del mismo paquete (como Departamento) puedan modificarlo, evitando que código externo altere la relación arbitrariamente. También debe considerarse la política cuando un profesor ya pertenece a un departamento: en este ejemplo se lanza una excepción, pero podría decidirse transferirlo automáticamente o permitir la pertenencia múltiple si el modelo de negocio lo admite. La bidireccionalidad añade complejidad pero permite navegar la relación en ambos sentidos, lo que puede ser muy útil en ciertos dominios donde ambas direcciones de la relación tienen significado en el modelo de negocio.