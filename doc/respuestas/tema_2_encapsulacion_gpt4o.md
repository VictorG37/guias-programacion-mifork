```markdown
# TEMA 2. Encapsulación — Respuestas

Las respuestas están redactadas en impersonal y adaptadas a conocimientos previos en C/C++ (sin OOP) y bases de Java.

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta

Se busca agrupar el estado (datos) y el comportamiento (métodos) en una unidad coherente —la clase— y controlar el acceso a ese estado mediante una interfaz bien definida. La ocultación de información consiste en mantener detalles de la representación interna fuera del alcance de los clientes.

Entre las ventajas destaca la reducción del acoplamiento entre módulos, la facilidad para cambiar la implementación sin afectar a los clientes, la preservación de invariantes y una mayor claridad semántica en el diseño.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta

La interfaz pública es el conjunto de métodos y atributos accesibles desde fuera que definen cómo se puede interactuar con la clase. Incluye firmas de métodos, tipos de retorno y las garantías o contratos que ofrece la clase.

La ocultación obliga a exponer únicamente lo necesario en esa interfaz, de forma que el resto de la implementación pueda cambiar libremente sin romper a los usuarios de la clase.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta

La interfaz pública es el contrato con los clientes; un diseño deficiente puede producir APIs confusas, inconsistentes o difíciles de evolucionar. Una interfaz mal pensada obliga a mantener compatibilidades que complican el mantenimiento futuro.

Cambiar una interfaz pública es costoso cuando existe código dependiente; por ello conviene mantener la interfaz mínima, estable y documentada correctamente.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta

Las invariantes de clase son condiciones que deben cumplirse siempre para que un objeto esté en un estado válido (por ejemplo, tamaños no negativos, referencias no nulas, o relaciones entre campos). Estas propiedades deben preservarse tras cualquier operación pública.

La ocultación centraliza el acceso al estado interno, impidiendo que código externo modifique directamente los campos y posibilitando que los métodos internos verifiquen y mantengan las invariantes.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta

Ejemplo de implementación con ocultación de las coordenadas:

```java
public class Punto {
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() { return x; }
    public double getY() { return y; }

    public double calcularDistanciaAOrigen() {
        return Math.hypot(x, y);
    }
}
```

La interfaz pública está formada por el constructor público y los métodos `getX()`, `getY()` y `calcularDistanciaAOrigen()`. `public` permite el acceso desde cualquier código, mientras que `private` restringe el acceso al interior de la misma clase, ocultando la implementación.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta

En Java los modificadores de acceso se aplican a campos, métodos, constructores y tipos anidados (clases o interfaces internas). Las clases de nivel superior sólo pueden ser `public` o tener acceso por defecto (package-private); `private` no es válido a nivel top-level.

Por convención, `private` se usa para campos y métodos auxiliares que no deben ser visibles, y `public` para la interfaz que se desea exponer públicamente.

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta

Sí. En Java existen cuatro niveles: `public`, `protected`, paquete (default, sin modificador) y `private`. `protected` permite acceso a subclases y clases del mismo paquete; el acceso por defecto limita al paquete.

Otros lenguajes ofrecen matices distintos: C++ tiene `public/protected/private`; C# añade `internal` y combinaciones como `protected internal`; Python usa convenciones de nombre (`_privado`, `__nombre`) en lugar de restricciones estrictas. Cada lenguaje aplica sus reglas según su modelo de encapsulación.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta

En Java el modificador `private` aplica por clase, no por instancia: el código dentro de la misma clase puede acceder a los campos privados de cualquier instancia de esa clase. Por tanto, los miembros privados están ocultos a otras clases, pero no a otras instancias cuando el acceso se realiza desde métodos de la misma clase.

Ejemplo:

```java
public double calcularDistanciaAPunto(Punto otro) {
    return Math.hypot(this.x - otro.x, this.y - otro.y); // válido dentro de la clase
}
```

El acceso a `otro.x` es válido porque se trata de código de la propia clase `Punto`; una clase externa no podría acceder a `otro.x` si fuera `private`.

## 9. ¿Qué son los métodos "getter" y "setter"?

### Respuesta

Los getters y setters son métodos que permiten el acceso controlado a atributos privados: un getter devuelve el valor de un campo y un setter lo modifica, a menudo con validaciones. Constituyen la forma habitual de preservar encapsulación y comprobaciones.

Su uso facilita mantener invariantes, registrar cambios y permitir cambios internos en la representación sin romper clientes.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta

La seguridad a la que suele referirse la ocultación es la integridad y corrección del programa: impedir usos indebidos que violen invariantes o produzcan estados inconsistentes. No implica protección contra ataques maliciosos externos por sí sola.

Para protección frente a amenazas se requieren controles adicionales (autenticación, autorización, criptografía, sandboxing). La encapsulación contribuye a la robustez, no sustituye medidas de seguridad externas.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase se pueden ocultar?

### Respuesta

Un miembro de instancia (no estático) pertenece a cada objeto; cada instancia mantiene su propio estado. Un miembro de clase (`static`) pertenece a la clase y es compartido por todas las instancias.

Los miembros de clase también pueden ocultarse usando `private static`; las mismas reglas de visibilidad (`public`, `private`, `protected`, paquete) aplican tanto a miembros de instancia como de clase.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta

Sí. Los constructores privados se utilizan para controlar la creación de instancias en patrones como singleton, factorías estáticas o clases utilitarias que no deben ser instanciadas. Permiten forzar el uso de métodos de fábrica o validaciones previas.

Además, un constructor privado permite ocultar detalles de construcción y garantizar invariantes antes de exponer las instancias.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta

En Java se usan miembros `static` para representar datos o métodos de clase. Un ejemplo que mantiene los máximos observados:

```java
public class Punto {
    private double x;
    private double y;
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x; this.y = y;
        synchronized(Punto.class) {
            if (x > maxX) maxX = x;
            if (y > maxY) maxY = y;
        }
    }

    public static double getMaxX() { return maxX; }
    public static double getMaxY() { return maxY; }
}
```

La palabra clave `static` identifica miembros de clase; se ha añadido sincronización básica para seguridad en entornos concurrentes.

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`?

### Respuesta

Un método factoría se declara típicamente `static` porque crea instancias sin depender de un objeto existente.

```java
public static Punto crearRedondeado(double x, double y) {
    return new Punto((double)Math.round(x), (double)Math.round(y));
}
```

Sí, se ha usado `static` para definir el método factoría.

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta

Se puede reemplazar `x` e `y` por un arreglo `private final double[] coords = new double[2];` y adaptar constructores y getters para mantener la interfaz pública sin exponer el array.

Ejemplo esquemático:

```java
public class Punto {
    private final double[] coords = new double[2];

    public Punto(double x, double y) {
        coords[0] = x;
        coords[1] = y;
    }

    public double getX() { return coords[0]; }
    public double getY() { return coords[1]; }

    public double calcularDistanciaAOrigen() {
        return Math.hypot(coords[0], coords[1]);
    }
}
```

No se debe devolver ni exponer la referencia a `coords` directamente, para no romper la encapsulación ni las invariantes.

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta

La convención habitual es declarar los atributos `private` y exponer acceso mediante getters/setters. Esto permite validar valores, controlar efectos secundarios y evolucionar la representación sin romper clientes.

La práctica está directamente relacionada con las invariantes: los setters permiten comprobar y preservar condiciones necesarias; un campo público permitiría modificaciones directas y riesgo de violar invariantes.

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta

Una clase inmutable es aquella cuyo estado no cambia tras la creación: todos los campos relevantes son finales y no existen métodos que alteren el estado observable. Un método modificador es cualquier método que altera el estado del objeto; no todos los métodos modificadores se denominan "setter", aunque los setters son un ejemplo común.

La inmutabilidad facilita el razonamiento, evita errores por aliasing y condiciones de carrera, y permite compartir instancias entre hilos sin sincronización adicional.

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta

No es recomendable añadir setters indiscriminadamente. Los setters deben existir sólo cuando la mutabilidad sea necesaria por diseño; para datos que no deben cambiar es preferible omitir setters o diseñar la clase como inmutable.

Limitar la mutación reduce la superficie de errores y ayuda a mantener invariantes y coherencia en el diseño.

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta

`String` en Java es inmutable: las operaciones que parecen modificarla producen nuevas instancias. Concatenar cadenas repetidamente genera muchas instancias intermedias y costes de copia.

Para concatenaciones intensivas conviene usar `StringBuilder` (o `StringBuffer` si se necesita sincronización), construir la cadena incrementalmente y llamar a `toString()` al final para obtener la `String` resultante.

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java?

### Respuesta

Los objetos pueden compararse por identidad (misma referencia) o por contenido (estado igual). En Java, `equals` define la comparación por contenido cuando se sobrescribe; por defecto `Object.equals()` compara identidad (equivalente a `==`).

Para comparar cadenas por su contenido en Java se debe usar `s1.equals(s2)`, ya que `==` compara referencias y no garantiza igualdad semántica.

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers?

### Respuesta

En Java las clases wrapper (`Integer`, `Double`, `Boolean`, etc.) encapsulan valores primitivos en objetos, permitiendo su uso en contextos que requieren objetos (por ejemplo, colecciones genéricas). Java proporciona autoboxing/unboxing automático entre primitivos y wrappers.

Las ventajas incluyen métodos utilitarios, la posibilidad de representar ausencia de valor con `null` y compatibilidad con APIs que esperan objetos. No todos los lenguajes distinguen primitivos y objetos (por ejemplo, Python trata números como objetos), por lo que la necesidad de wrappers es específica de cada lenguaje.

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta

Un tipo enumerado es un conjunto finito de valores nombrados que representan alternativas válidas. En Java, `enum` es una clase especial y cada constante es una instancia de esa clase; los enums pueden tener campos, métodos y constructores privados.

Los enums encapsulan el conjunto válido de valores y la lógica asociada, mejoran la seguridad de tipo y permiten centralizar comportamiento relacionado con cada constante, contribuyendo a un diseño más claro y menos propenso a errores.

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### Respuesta

Implementación ejemplo:

```java
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

    private final int numero;
    private final int dias;

    private Mes(int numero, int dias) {
        this.numero = numero;
        this.dias = dias;
    }

    public int getDias() { return dias; }
    public int getNumero() { return numero; } // 1-12
}
```

Se usa `getNumero()` en lugar de `ordinal()` para devolver 1-12 explícitamente.

## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`).

### Respuesta

Una implementación simple basada en el número del mes:

```java
public boolean esDePrimavera(boolean enHemisferioNorte) {
    int n = this.numero;
    return enHemisferioNorte ? (n >= 3 && n <= 5) : (n >= 9 && n <= 11);
}

public boolean esDeVerano(boolean enHemisferioNorte) {
    int n = this.numero;
    return enHemisferioNorte ? (n >= 6 && n <= 8) : (n == 12 || n == 1 || n == 2);
}

public boolean esDeOtoño(boolean enHemisferioNorte) {
    int n = this.numero;
    return enHemisferioNorte ? (n >= 9 && n <= 11) : (n >= 3 && n <= 5);
}

public boolean esDeInvierno(boolean enHemisferioNorte) {
    int n = this.numero;
    return enHemisferioNorte ? (n == 12 || n == 1 || n == 2) : (n >= 6 && n <= 8);
}
```

Se ha usado una definición simplificada por meses completos; las fronteras exactas pueden adaptarse según criterio astronómico o regional.

---

