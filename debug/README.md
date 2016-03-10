# Debug

Aquí hay cuatro carpetas, cada una con sus ejercicios. Las respuestas a los ejercicios 
tienen que estar en esta carpeta, bajo el nombre `respuestas.md`

## Various bugs

En la carpeta `bugs/` hay varios bugs ya hechos y resueltos, con un `Makefile` que los compila todos

## Floating point exception

En la carpeta `fpe/` hay tres códigos de C, independientes, para compilar. 
Cada uno de estos códigos genera un ejecutable. Hay además una carpeta que
define la función `set_fpe_x87_sse_`. Una vez compilados los tres ejecutables
sin la opción `-DTRAPFPE`, responder las siguientes preguntas:

- ¿Qué función requiere agregar `-DTRAPFPE`? ¿Cómo pueden hacer que el
programa *linkee* adecuadamente?
- Para cada uno de los ejecutables, ¿qué hace agregar la opción `-DTRAPFPE` al compilar? ¿En qué se diferencian 
los mensajes de salida con y sin esa opción?

Cmpile cada archivo con -Lfpe_x87_sse <nombre_archivo>. Para el 3, inclui math en el archivo y le agregue-lm al final del comando para compilar. Cree los ejecutables y corri cada archivo. Al dividir por cero el resultado es infinito. Para que usar TRAPFPE, hay que agregar el path al archivo donde se declaran las funciones para el manejo de excepciones (fpe_x87_sse). Luego hay que compilar con el flag -DTRAPFPE mediante:
gcc -DTRAPFPE  fpe_x87_sse/fpe_x87_sse.o test_fpe3.c -lm -o test_fpe3.e
(el -lm se usa solo en el test_fpe3.c para que se linkee la funcion sqrt() de math).
Al ejecutar los archivos con -DTRAPFPE y dividir por cero, se genera una excepcion:
"Excepción de coma flotante (`core' generado)"

## Segmentation Fault

En la carpeta `sigsegv/` hay códigos de C y de FORTRAN. Elijan alguno
y compilen y corren el programa de acuerdo a los comentarios en el código,
para obtener los ejecutables `small.x` y `big.x`.
Identifiquen los errores que devuelven (¡si devuelven alguno!) los ejecutables.
Ahora ejecuten `ulimit -s unlimited` en la terminal y vuelvan a correrlo. Luego
responder las siguientes preguntas:

- ¿Devuelven el mismo error que antes?
- Averigüen qué hicieron al ejecutar la sentencia `ulimit -s unlimited`. Algunas pistas
son: abran otra terminal distinta y fíjense si vuelve al mismo error, fíjense la diferencia
entre `ulimit -a` antes y después de ejecutar `ulimit -s unlimited`, googleen, etcétera.
- La "solución" anterior, ¿es una solución en el sentido de debugging?

El problema es que el archivo sin la directiva SMALL asigna 2500 a la variable size. Al intentar alocar memoria pide 2500**2*sizeof(float), superando ampliamente lo que tiene asignado para el stack, el sistema no se lo asigna. Al ejecutar ulimit con el flag -s definimos el stack como unlimited y el archivo compila bien.
No es una solucion de debugging, porque en realidad ejecuta el archivo a costa de una cantidad excesiva de memoria y tarda mucho tiempo en hacerlo.

## Valgrind

En la carpeta `valgrind/` hay ejemplos en C y FORTRAN que se pueden ejecutar
con valgrind. Describan el error y por qué sucede

## Funny

En la carpeta `funny/` hay un código de C. Describan las diferencias de los ejecutables
al compilar con y sin el flag `-DDEBUG`. ¿De dónde vienen esas diferencias?

