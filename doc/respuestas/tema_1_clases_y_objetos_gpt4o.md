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

### Respuesta

Las cuatro características fundamentales son abstracción, encapsulamiento, herencia y polimorfismo. La abstracción permite representar conceptos del mundo real quedándose sólo con lo esencial, dejando fuera detalles innecesarios. Es la base para modelar problemas complejos de forma sencilla mediante clases.
El encapsulamiento consiste en agrupar en una misma unidad los datos (atributos) y las acciones que los manipulan (métodos), controlando qué partes del interior de un objeto pueden ser accesibles. La herencia permite crear nuevas clases a partir de clases ya existentes, reutilizando código y añadiendo nuevas funcionalidades.
Por último, el polimorfismo permite que un mismo método se comporte de forma diferente según el tipo real del objeto que lo utilice. Esto otorga flexibilidad al diseño, facilitando la extensión del software sin modificar el código ya existente.



## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta
Cuatro lenguajes muy conocidos que soportan POO son Java, muy usado en aplicaciones empresariales, Android y servidores; C++, que combina programación estructurada con orientación a objetos y se utiliza en sistemas y videojuegos; C#, muy extendido en entornos .NET y en desarrollo con Unity; y Python, que soporta objetos de forma sencilla y se emplea ampliamente en ciencia de datos, IA y scripting.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta
La programación estructurada es un paradigma anterior a la POO basado en organizar el código mediante estructuras de control claras: secuencias, decisiones y bucles. Busca evitar el uso de saltos descontrolados como goto, favoreciendo programas más comprensibles y con un flujo de ejecución predecible. Normalmente apoya la división de la lógica en funciones.
La programación modular va un paso más allá: propone dividir un programa en módulos independientes, cada uno responsable de una parte concreta de la funcionalidad. Estos módulos pueden contener sus propias variables y funciones relacionadas. El objetivo es reducir la complejidad, facilitar la reutilización y permitir el trabajo colaborativo.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta
Todo objeto se caracteriza por identidad, estado y comportamiento. La identidad permite diferenciar un objeto de cualquier otro, aunque ambos tengan los mismos valores; en la práctica, corresponde a su referencia o dirección en memoria.
El estado está formado por los atributos que almacenan información sobre el objeto en un momento concreto. El comportamiento se define mediante los métodos, que expresan las acciones que el objeto puede realizar o las operaciones que admite.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta
Una clase es una plantilla o definición que describe qué atributos y métodos tendrán los objetos asociados a ella. No es un objeto en sí, sino una estructura conceptual. Un objeto es una instancia de la clase: un ejemplar concreto creado en memoria, con sus propios valores.
No todos los lenguajes orientados a objetos usan clases obligatoriamente. Por ejemplo, JavaScript implementa un modelo basado en prototipos, donde un objeto puede derivar de otro directamente sin necesidad de una definición de clase formal.
 

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta
 En Java, los objetos se almacenan en la memoria heap, que está gestionada por la Máquina Virtual de Java (JVM). Las variables primitivas locales, en cambio, se guardan en la pila (stack). No todos los lenguajes funcionan igual: por ejemplo, en C++ los objetos pueden estar en la pila, en el heap o incluso ser globales, dependiendo de cómo se definan.
La recolección de basura (garbage collection) es un proceso automático mediante el cual el lenguaje libera memoria ocupada por objetos que ya no se usan. En Java y Python está gestionado automáticamente; en C++ no existe de forma nativa y el programador debe liberar manualmente la memoria o usar smart pointers. -->


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta
Un método es una función definida dentro de una clase que describe comportamientos asociados a los objetos de esa clase. Opera normalmente sobre los atributos del propio objeto y define cómo interactúa o qué acciones puede realizar.
La sobrecarga de métodos consiste en definir varios métodos con el mismo nombre pero con diferentes parámetros. Java decide cuál utilizar en función del número y tipo de argumentos. Esto permite expresar variaciones de un mismo comportamiento sin cambiar el nombre de los métodos. 


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta
 class Punto {
    int x;
    int y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

class UsoPunto {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;
        System.out.println(p.calculaDistanciaAOrigen()); // 5.0
    }
}


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta
l punto de entrada de cualquier programa Java es el método:
public static void main(String[] args)

La palabra static indica que un método o atributo pertenece a la clase y no a un objeto concreto, por lo que puede utilizarse sin crear una instancia. Aunque se usa en main, también se emplea en variables o métodos que deben ser compartidos por todas las instancias.
La combinación static final sirve para crear constantes, es decir, valores globales que no pueden cambiar, equivalentes a const en C o C++.
 

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta
Para compilar un archivo Java se utiliza:
javac Archivo.java

Esto genera un archivo .class que contiene bytecode, un lenguaje intermedio. Para ejecutarlo se llama:
java Archivo

Java sí es compilado, pero no a código máquina nativo, sino a este bytecode. La Máquina Virtual de Java (JVM) interpreta o compila este bytecode a código nativo según el sistema donde se ejecute, lo que hace que Java sea un lenguaje multiplataforma.
 

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta
 new es la palabra reservada que crea un nuevo objeto en el heap y devuelve una referencia al mismo. El constructor es un método especial que se ejecuta al crear el objeto, encargándose de inicializar sus atributos.
Ejemplo:
class Empleado {
    String dni;
    String nombre;
    String apellidos;

    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}



## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta
 this es una referencia que apunta al objeto actual dentro de un método. Permite distinguir entre atributos y parámetros con nombres iguales y acceder explícitamente a los datos de la instancia en curso. En C++ también se llama this; en Python se usa self.
Ejemplo:
double calculaDistanciaAOrigen() {
    return Math.sqrt(this.x * this.x + this.y * this.y);
}


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta
 double distanciaA(Punto otro) {
    int dx = this.x - otro.x;
    int dy = this.y - otro.y;
    return Math.sqrt(dx * dx + dy * dy);
}


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta
 En Java, los objetos se pasan por copia de la referencia, no por copia del objeto. Esto significa que si dentro de un método se modifican los atributos del objeto recibido, los cambios se reflejan en el objeto externo. Sin embargo, si se reasigna la referencia dentro del método, eso no afecta fuera.
Los tipos primitivos como int se pasan por valor. Si un entero se modifica dentro de un método, ese cambio no afecta a la variable original, ya que se trabaja con una copia independiente del valor.




## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta
 toString() es un método que devuelve una representación en texto de un objeto. Todas las clases en Java lo heredan de Object, y se suele sobrescribir para mostrar información más útil. Lenguajes como C#, Python o JavaScript tienen métodos equivalentes.
Ejemplo:
@Override
public String toString() {
    return "(" + x + ", " + y + ")";
}


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta
 Un struct en C solo permite agrupar datos, pero no admite métodos ni mecanismos propios de la POO, como encapsulamiento, herencia o polimorfismo. Por lo tanto, es una estructura más limitada.
Para que un struct fuese equivalente a una clase, debería permitir incluir métodos dentro de sí, gestionar visibilidad, soportar inicialización automática mediante constructores y permitir instancias con comportamiento propio, algo que C no ofrece de forma nativa.
 


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
 Para imitar una clase en C se usa un struct para los datos y funciones externas que reciben un puntero al struct. Ese puntero funciona como el equivalente manual de this, indicando sobre qué objeto se opera.
Ejemplo:
typedef struct {
    int x;
    int y;
} Punto;

double distanciaAOrigen(Punto* p) {
    return sqrt(p->x * p->x + p->y * p->y);
}

Aquí no existe un this implícito; debe pasarse explícitamente como parámetro.

