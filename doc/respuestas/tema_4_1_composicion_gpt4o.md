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

### Respuesta

En C, la composición se logra mediante estructuras anidadas, donde una estructura contiene instancias de otras estructuras. En este ejemplo, se define una estructura `Punto` con coordenadas `x` e `y`, y una estructura `Linea` que contiene dos instancias de `Punto`, representando los extremos de la línea. Esta relación se describe como "Linea tiene-dos Punto".

La función `distancia` calcula la distancia euclidiana entre dos puntos utilizando la fórmula sqrt((x2 - x1)^2 + (y2 - y1)^2), requiriendo la inclusión de la biblioteca `math.h` para la función `sqrt`. La función `longitud_linea` utiliza `distancia` para calcular la longitud de la línea entre sus dos puntos extremos.

Este enfoque permite reutilizar la estructura `Punto` en otras composiciones, como polígonos o trayectorias, manteniendo la modularidad del código. Sin embargo, en C no hay mecanismos automáticos para gestionar la memoria o garantizar la integridad de los datos, lo que requiere atención manual del programador.

```c
#include <stdio.h>
#include <math.h>

struct Punto {
    double x;
    double y;
};

struct Linea {
    struct Punto p1;
    struct Punto p2;
};

double distancia(struct Punto a, struct Punto b) {
    return sqrt((b.x - a.x) * (b.x - a.x) + (b.y - a.y) * (b.y - a.y));
}

double longitud_linea(struct Linea l) {
    return distancia(l.p1, l.p2);
}
```


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta

En Java, la composición se implementa mediante clases que contienen instancias de otras clases como campos. La clase `Punto` se define con campos `x` e `y` finales para asegurar inmutabilidad, y un constructor que los inicializa. Incluye un método `distancia` que calcula la distancia a otro punto.

La clase `Linea` contiene dos instancias finales de `Punto`, `p1` y `p2`, inicializadas en el constructor, garantizando que la línea sea inmutable una vez creada. El método `longitud` utiliza el método `distancia` de `Punto` para calcular la longitud de la línea.

Esta aproximación orientada a objetos mejora la encapsulación respecto a C, ya que los campos son privados y la inmutabilidad previene modificaciones accidentales, promoviendo la robustez del código. Además, facilita la reutilización y el mantenimiento mediante métodos que operan sobre los objetos.

```java
public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        return Math.sqrt(Math.pow(otro.x - this.x, 2) + Math.pow(otro.y - this.y, 2));
    }
}

public class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}
```


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta

La multiplicidad en la composición describe el número de instancias de una clase que pueden estar asociadas con una instancia de otra clase. Se expresa mediante notaciones como 1, 1..*, 0..1, etc., indicando la cardinalidad de la relación.

En el ejemplo de `Linea` y `Punto`, la multiplicidad de `Linea` a `Punto` es exactamente 2, ya que cada línea tiene dos puntos fijos. En la dirección opuesta, de `Punto` a `Linea`, la multiplicidad es 0..*, puesto que un punto puede formar parte de cero o más líneas.

Esta especificación ayuda a modelar correctamente las relaciones, asegurando que el diseño refleje las restricciones del dominio. Por ejemplo, intentar crear una línea con menos o más de dos puntos violaría la multiplicidad definida.

La multiplicidad no solo define la estructura estática, sino que también guía la implementación de métodos que manipulan estas relaciones, como constructores o validadores que enforce estas cardinalidades.


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta

La composición fuerte implica que los objetos componentes son creados y destruidos junto con el objeto contenedor, compartiendo su ciclo de vida. En cambio, la composición débil permite que los objetos componentes existan independientemente, sin estar ligados al contenedor.

En términos de ciclo de vida, en la composición fuerte, cuando el contenedor se destruye, los componentes también lo hacen automáticamente, gestionado por el recolector de basura en Java. En la composición débil, los componentes pueden sobrevivir a la destrucción del contenedor.

Generalmente, se refiere a la composición débil como "asociación" o "agregación", donde el contenedor no posee los componentes. La composición fuerte se denomina "composición" propiamente, indicando propiedad exclusiva.

Esta distinción es crucial para el diseño, ya que afecta la gestión de memoria y la integridad de los objetos. Por ejemplo, en una composición fuerte, no se permite compartir componentes entre contenedores, mientras que en la débil sí.


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta

En estos casos, se habla de "dependencia" en lugar de composición. La composición implica que una clase contiene instancias de otra como campos, formando parte de su estructura interna.

La dependencia ocurre cuando una clase utiliza otra temporalmente, como en parámetros de métodos, creaciones locales con `new`, o variables locales, sin mantener referencias permanentes.

Esta distinción es importante para el diseño orientado a objetos, ya que la composición afecta la estructura y el ciclo de vida, mientras que la dependencia es más flexible y no implica propiedad.

Por ejemplo, un método que recibe un objeto como parámetro depende de él durante la ejecución, pero no lo compone. Esto permite mayor desacoplamiento y reutilización de código.


## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta

Para la composición fuerte, la clase `LineaFuerte` crea instancias de `Punto` en su constructor, ligando sus ciclos de vida. Cuando `LineaFuerte` se destruye, los puntos también lo hacen.

Para la composición débil, la clase `LineaDebil` recibe instancias de `Punto` como parámetros, sin crearlas, permitiendo que existan independientemente.

En la implementación, `LineaFuerte` usa `new Punto(...)` en el constructor, mientras que `LineaDebil` toma `Punto` existentes. Esto demuestra cómo la composición fuerte implica propiedad, y la débil no.

Ambas clases mantienen la inmutabilidad y proporcionan el método `longitud`, pero difieren en la gestión del ciclo de vida de los puntos.

```java
// Composición fuerte
public class LineaFuerte {
    private final Punto p1;
    private final Punto p2;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}

// Composición débil
public class LineaDebil {
    private final Punto p1;
    private final Punto p2;

    public LineaDebil(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}
```


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta

En Java, la destrucción de objetos se gestiona automáticamente mediante el recolector de basura, que libera memoria cuando no hay referencias activas a un objeto. En la composición fuerte, cuando el contenedor `Linea` pierde todas sus referencias y es elegible para recolección, los objetos `Punto` que contiene también lo son, ya que no hay otras referencias a ellos.

No se observa destrucción explícita porque Java no requiere que el programador gestione la memoria manualmente, a diferencia de lenguajes como C++. El recolector de basura se encarga de esto de forma transparente.

Esta gestión automática simplifica el código, pero requiere entender que la composición fuerte implica que los componentes no deben compartirse entre contenedores para evitar referencias inesperadas que impidan la recolección.

En consecuencia, el ciclo de vida se liga implícitamente a través de las referencias, sin necesidad de código destructor explícito.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta

La composición débil se implementa en la clase `Departamento`, que mantiene referencias a instancias de `Profesor` sin crearlas. Utiliza un array primitivo `Profesor[]` de tamaño máximo 50, encapsulado para no exponer el array interno.

El constructor requiere un director inicial, que debe estar en la lista de profesores. Métodos como `agregarProfesor` añaden al final si hay espacio, y `eliminarProfesor` remueve por posición, verificando que no sea el director.

El método `cambiarDirector` cambia el director a otro profesor existente en la lista. Se lanzan excepciones `IllegalArgumentException` o `IllegalStateException` si se violan invariantes, como intentar eliminar el director o asignar uno no presente.

Esta implementación asegura la integridad mediante validaciones en cada operación que afecta la invariante.

```java
public class Profesor {
    private String nombre;

    public Profesor(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

public class Departamento {
    private static final int MAX_PROFESORES = 50;
    private Profesor[] profesores;
    private int numProfesores;
    private Profesor director;

    public Departamento(Profesor director) {
        this.profesores = new Profesor[MAX_PROFESORES];
        this.numProfesores = 0;
        this.director = director;
        agregarProfesor(director); // El director debe estar en la lista
    }

    public void agregarProfesor(Profesor p) {
        if (numProfesores >= MAX_PROFESORES) {
            throw new IllegalStateException("No se pueden agregar más profesores");
        }
        profesores[numProfesores++] = p;
    }

    public void eliminarProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IllegalArgumentException("Posición inválida");
        }
        if (profesores[posicion] == director) {
            throw new IllegalStateException("No se puede eliminar al director");
        }
        for (int i = posicion; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        numProfesores--;
    }

    public int getNumProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IllegalArgumentException("Posición inválida");
        }
        return profesores[posicion];
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        boolean encontrado = false;
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == nuevoDirector) {
                encontrado = true;
                break;
            }
        }
        if (!encontrado) {
            throw new IllegalArgumentException("El nuevo director debe ser un profesor del departamento");
        }
        this.director = nuevoDirector;
    }

    public Profesor getDirector() {
        return director;
    }
}
```


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta

Al usar `List<Profesor>` en lugar de arrays primitivos, se simplifica la gestión dinámica de la colección. La clase `Departamento` utiliza `ArrayList<Profesor>` para almacenar profesores, eliminando la necesidad de manejar índices manualmente y el límite fijo de 50.

Se ahorra el código para redimensionar el array y desplazar elementos al eliminar, ya que `List` proporciona métodos como `add`, `remove`, y acceso por índice. La encapsulación se mantiene al no exponer la lista interna.

Si se devolviera directamente la lista interna en un método como `getProfesores()`, se expondría el estado interno, permitiendo modificaciones externas que violen invariantes. Para resolverlo, se devolvería una copia inmutable, como `Collections.unmodifiableList(new ArrayList<>(profesores))`, o una nueva lista.

Esta aproximación mejora la flexibilidad y reduce errores, aprovechando las colecciones de Java.

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Collections;

public class Departamento {
    private List<Profesor> profesores;
    private Profesor director;

    public Departamento(Profesor director) {
        this.profesores = new ArrayList<>();
        this.director = director;
        profesores.add(director);
    }

    public void agregarProfesor(Profesor p) {
        profesores.add(p);
    }

    public void eliminarProfesor(int posicion) {
        if (posicion < 0 || posicion >= profesores.size()) {
            throw new IllegalArgumentException("Posición inválida");
        }
        if (profesores.get(posicion) == director) {
            throw new IllegalStateException("No se puede eliminar al director");
        }
        profesores.remove(posicion);
    }

    public int getNumProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int posicion) {
        if (posicion < 0 || posicion >= profesores.size()) {
            throw new IllegalArgumentException("Posición inválida");
        }
        return profesores.get(posicion);
    }

    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(new ArrayList<>(profesores));
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException("El nuevo director debe ser un profesor del departamento");
        }
        this.director = nuevoDirector;
    }

    public Profesor getDirector() {
        return director;
    }
}
```


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta

Las composiciones recursivas ocurren cuando una clase se refiere a sí misma, creando estructuras jerárquicas. En el ejemplo, la clase `Persona` inmutable tiene un campo `madre` de tipo `Persona`, permitiendo representar árboles genealógicos.

La inmutabilidad se asegura con campos finales y un constructor que los inicializa. El método `main` crea instancias para una familia: abuela, madre, y nieto, mostrando cómo se construye la jerarquía recursivamente.

Otros ejemplos clásicos incluyen estructuras de datos como árboles binarios, donde un nodo contiene referencias a nodos hijos del mismo tipo, o listas enlazadas, donde cada elemento apunta al siguiente.

Esta composición facilita modelar relaciones recursivas de forma natural y eficiente.

```java
public class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }

    public static void main(String[] args) {
        Persona abuela = new Persona("Abuela", null);
        Persona madre = new Persona("Madre", abuela);
        Persona nieto = new Persona("Nieto", madre);

        System.out.println(nieto.getNombre() + " -> " + nieto.getMadre().getNombre() + " -> " + nieto.getMadre().getMadre().getNombre());
    }
}
```

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta

Las relaciones de composición bidireccionales implican que ambas clases mantienen referencias entre sí, permitiendo navegación en ambas direcciones. Por ejemplo, un `Profesor` conoce su `Departamento`, y el `Departamento` conoce sus `Profesores`.

Para implementar en el ejemplo de `Profesor` y `Departamento`, se añadiría un campo `departamento` en `Profesor`, y se actualizaría al agregar o cambiar. Al eliminar un profesor, se limpiaría su referencia al departamento.

Esto requiere sincronización para mantener consistencia, como métodos que actualicen ambas direcciones. Sin embargo, aumenta la complejidad y el riesgo de referencias circulares, por lo que se debe usar con cuidado.

En Java, se puede lograr mediante setters que actualicen mutuamente, asegurando que las invariantes se mantengan en ambas clases.
