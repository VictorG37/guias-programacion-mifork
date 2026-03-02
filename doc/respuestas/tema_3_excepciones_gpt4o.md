<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta

En C no hay un mecanismo de excepciones, por lo que el error se comunica mediante los valores que devuelve la función o mediante variables externas. Una estrategia común es devolver un código de estado e indicar el resultado por referencia; si la raíz no puede calcularse se devuelve un error y el valor de salida se ignora.

```c
#include <math.h>
#include <stdio.h>

int raiz(double x, double *resultado) {
    if (x < 0) {
        return -1; // código de error
    }
    *resultado = sqrt(x);
    return 0; // éxito
}

int main(void) {
    double r;
    if (raiz(-4.0, &r) != 0) {
        printf("El número es negativo\n");
    }
    return 0;
}
```

Otra opción es usar una variable global/`errno` o un `struct` que devuelva tanto el resultado como el estado, o incluso devolver un valor especial como `NAN` y que el llamador compruebe con `isnan`. El diseño depende de quién deba manejar el error y del estilo del proyecto.

```c
#include <stdlib.h>
#include <stdio.h>
#include <math.h>

/* se podría usar errno, pero aquí simplificamos */
struct R {
    double valor;
    int error; /* 0 si OK, 1 si negativo */
};

struct R raiz(double x) {
    struct R r;
    if (x < 0) {
        r.error = 1;
        r.valor = 0.0;
    } else {
        r.error = 0;
        r.valor = sqrt(x);
    }
    return r;
}

int main(void) {
    struct R r = raiz(-2.0);
    if (r.error) {
        printf("argumento negativo\n");
    }
    return 0;
}
```



## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta

Una excepción es un mecanismo del lenguaje para señalar que ha ocurrido una condición anómala durante la ejecución, distinta del flujo normal. No es un valor ordinario sino un objeto o entidad que interrumpe el camino habitual y debe ser atendida.

El programador lanza (throw) una excepción al detectar un error o situación inesperada, y la utiliza como una forma de separar la lógica normal del manejo de fallos. Quien llama a una función puede capturarla para recuperarse o dejar que se propague hasta un nivel que sepa tratarla adecuadamente.



## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta

En Java la clase puede lanzar una `IllegalArgumentException` si el argumento es negativo y el código llamador lo atrapa en un bloque `try-catch`.

```java
public class Calculadora {
    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("No se puede calcular la raíz de un negativo");
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        try {
            double r = raiz(-1);
            System.out.println(r);
        } catch (IllegalArgumentException ex) {
            System.out.println("Error: " + ex.getMessage());
        }
    }
}
```

El método `main` controla la excepción fuera de la función `raiz`.



## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta

"Lanzar" una excepción significa crearla y pasársela al motor de ejecución mediante la palabra clave `throw`; es el acto de avisar de que algo ha ido mal. "Controlar" o "capturar" implica rodear la llamada con un bloque `try` y asociar uno o varios `catch` que intercepten ese tipo concreto de excepción y ejecuten código de recuperación.

Si nadie la captura en el nivel donde se lanza, la excepción "se propaga" hacia el método que llamó, y así sucesivamente sube por la pila de llamadas. Cada función intermedia deja de ejecutarse y se desapila (se quitan sus marcos de pila) hasta que encuentra un bloque `catch`. Las funciones que no la controlan no se reanudan; su ejecución se aborta y sus recursos se liberan según el lenguaje (por ejemplo, se ejecuta el `finally` si existe).

Con el ejemplo de `Calculadora.raiz`, si `raiz` lanza la excepción, el control vuelve al `main`. Si `main` no la captura, el programa termina con un rastro de pila. El hilo de ejecución nunca vuelve a `raiz` ni a ninguna función que ya se ha desapilado.



## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta

La propagación natural permite dejar el manejo de un error en el nivel más adecuado sin tener que comprobar manualmente códigos de retorno en cada llamada. Reduce el ruido del código, ya que no se necesita escribir `if` tras cada invocación; si no se atiende en un lugar se traslada automáticamente arriba.

Además favorece la separación de responsabilidades: las funciones de bajo nivel pueden concentrarse en su lógica, y los operadores de más alto nivel toman decisiones sobre cómo recuperarse o informar al usuario. En C sería necesario propagar explícitamente los códigos de error o recurrir a variables globales, con riesgo de olvidos y errores.



## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta

En OO las excepciones son instancias de clases que normalmente heredan de una superclase común (`Throwable` en Java, `Exception` en otros). Como objetos, encapsulan datos (mensaje, pila, causa) y comportamientos (métodos para inspección) y se ajustan al principio de encapsulación: el llamador no necesita saber cómo se construye, solo cómo manejarla.

Gracias a ello es fácil definir nuestras propias clases de excepción que representen condiciones específicas del dominio, añadiendo campos extra si se requiere. La herencia permite atrapar grupos de excepciones mediante la superclase.



## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta

Un objeto excepción suele contener al menos un mensaje descriptivo y la traza de pila (stack trace) que indica dónde se generó y por qué camino pasó. Además puede llevar una causa (`cause`) que enlaza con otra excepción anterior. Estos datos facilitan el diagnóstico en el manejador, permitiendo registrar o mostrar el error sin perder contexto.

En C, un simple código de error no transmite esta riqueza de información y requiere que el programador la gestione explícitamente.



## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta

Sí, es posible encadenar varios bloques `catch` tras un mismo `try`, cada uno para un tipo de excepción distinto. Sólo se ejecuta el primer bloque cuyo tipo coincida con la excepción lanzada; los demás se ignoran. Esto permite tratar de forma diferenciada diferentes clases de errores.



## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta

El bloque `finally` se usa para ejecutar código que debe correr sí o sí, ocurra o no una excepción, y aunque ésta se propague. Su contenido se ejecuta después del `try` y de cualquier `catch` correspondiente.

```java
try {
    // operaciones que pueden fallar
    abrirArchivo();
} catch (IOException e) {
    System.err.println("error leyendo");
} finally {
    cerrarArchivo(); // siempre se ejecuta
}
```

También es válido emplear `finally` sin `catch`, simplemente para asegurar el cierre incluso si no se desea manejar la excepción en ese nivel.

```java
try {
    abrirArchivo();
    procesar();
} finally {
    cerrarArchivo();
}
```

En ambos casos `cerrarArchivo()` se invoca antes de que la excepción abandone el método.



## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta

Sí, el bloque `finally` puede aparecer después de un `try` aunque no exista ningún `catch` asociado. Su código se ejecuta siempre, independientemente de si se lanza o no una excepción. Incluso si el `try` contiene un `return`, el `finally` se ejecuta antes de que el método devuelva el valor.

```java
int f() {
    try {
        return 1;
    } finally {
        System.out.println("ejecutando finally");
    }
}
```

La salida incluirá el mensaje del `finally` antes de que `f()` termine.



## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta

Las excepciones controladas (checked) son aquellas que el compilador obliga a declarar con `throws` o a capturar con `try-catch`. Generalmente representan condiciones externas o recuperables, como `IOException` o `SQLException`. Las no controladas (unchecked) heredan de `RuntimeException` y no requieren declaración; se usan para errores de programación como `NullPointerException` o `IllegalArgumentException`.

Ejemplos propios: una `MiCheckedException extends Exception` para un servicio remoto que puede fallar, y `MiUncheckedException extends RuntimeException` para cuando se recibe un argumento inválido.

Preferencia por controladas:
- Acceso a archivos
- Conexión a bases de datos
- Operaciones de red
- Parsing de datos proporcionados por el usuario

Preferencia por no controladas:
- Error lógico interno (división por cero)
- Argumento nulo pasado a un método
- Índice fuera de rango en un array
- Estado inconsistente en el objeto



## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta

La cláusula `throws` en la firma de un método indica que éste puede lanzar ciertas excepciones controladas y que no las maneja internamente. Es alternativa a capturarlas porque traslada la responsabilidad de gestionarlas al llamador; el método decide no ocuparse de ellas, quizá porque no tiene suficiente contexto para recuperarse.

Usar `throws` permite diseñar APIs limpias donde los errores se documentan y se manejan en niveles superiores.



## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta

```java
import java.io.*;

public class Ejemplo {
    public void procesarArchivo(String nombre) throws IOException {
        FileInputStream fis = null;
        try {
            fis = new FileInputStream(nombre);
            // procesamiento...
        } finally {
            if (fis != null) {
                fis.close();
            }
        }
    }
}
```

El método declara `throws IOException` para que quien lo invoque se ocupe de la posibilidad de que el fichero no exista o no se pueda leer.



## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta

Legalmente sí se puede listar excepciones no controladas en `throws`, pero no es necesario ni suele hacerse, porque no obliga al llamador a capturarlas. El llamador no tiene por qué poner `try-catch`; sólo lo haría si quiere documentar o advertir a otros programadores de que esa situación puede producirse y merece atención.

Incluirlas puede tener sentido como tipo de documentación o para forzar cierto estilo de manejo en APIs críticas, pero en la práctica las excepciones `RuntimeException` se usan sin declaración y se considera que reflejan errores de programación que no deben recuperarse en tiempo de ejecución.



## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta

Se recomiendan las controladas para errores externos o recuperables que el código pueda razonablemente manejar, como fallos de E/S o redes. Las no controladas encarnan errores de lógica o condiciones que deberían evitarse mediante validaciones previas, como argumentos inválidos.

Java es uno de los pocos lenguajes con distinción obligatoria; muchos otros (C#, Python, C++) sólo tienen excepciones no controladas. En esos casos la práctica común es lanzar excepciones sin obligación de declararlas, y los programadores documentan en comentarios el comportamiento previsto. La opción más habitual en los lenguajes sin verificación es la no controlada.



## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta

Dentro de un bloque `catch` es habitual lanzar una nueva excepción, por ejemplo para envolver la original en una de mayor nivel o para cambiar el tipo a uno más apropiado. También se puede relanzar la misma excepción con `throw;` en C# o `throw e;` en Java; esto es útil cuando se quiere registrar información adicional y luego dejar que siga propagándose.

Ejemplo de nuevo lanzamiento:
```java
try {
    leer();
} catch (IOException e) {
    throw new MiErrorDeLectura("fallo al leer", e);
}
```

Ejemplo de relanzar la misma:
```java
try {
    procesar();
} catch (RuntimeException e) {
    log(e);
    throw e; // la misma vuelve a salir
}
```

Relanzar mantiene la traza original; se usa cuando el catch sólo hace tareas auxiliares como el log.



## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

Que una excepción sea la causa de otra significa que cuando se crea una nueva instancia se le pasa la excepción original como parámetro constructor; así se conserva el rastro del error inicial. La excepción resultante encapsula la original y permite a quien la maneja inspeccionar ambas. Cuando se imprime la excepción, la salida habitual (`printStackTrace()` en Java) muestra la traza de la excepción principal seguida de "Caused by" y la traza de la causa.

```java
class MiExcepcion extends Exception {
    public MiExcepcion(String msg, Throwable causa) {
        super(msg, causa);
    }
}

try {
    // código de bajo nivel
    throw new IOException("no se puede leer");
} catch (IOException e) {
    throw new MiExcepcion("fallo en la operación de más alto nivel", e);
}
```

Si esta `MiExcepcion` se imprime, se verá el mensaje y luego la línea "Caused by: java.io.IOException: no se puede leer" con su propia traza. Esto facilita entender la cadena de fallos.


