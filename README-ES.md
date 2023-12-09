# Guerra de Naves

[![en](https://img.shields.io/badge/lang-en-red.svg)](/README.md)

Guerra de Naves es un trabajo de la asignatura _Fundamentos de Sistemas Operativos_, de 2º curso de Ingeniería Informática en la Universidad de Valladolid. Esta tarea se desarrolló conjuntamente con otro estudiante, Tomás Carretero.

Aunque todo este sistema se basa en el juego famoso "Hundir la flota", la intención con la tarea era aprender a manejar hilos en un entorno UNIX, semáforos, memoria compartida, reserva dinámica de memoria...

## Resumen Principal

El programa crea diferentes barcos que interactúan con un fichero de entrada (hay alugnos ejemplos en la [carpeta inputfiles](/inputfiles/)). Cada carácter se corresponde con un _token_ que solo puede ser leído una vez y por un solo barco.

| Token | Significado |
|-------|-------------|
| *     | El barco ha recibido un disparo. |
| _espacio_ | El barco ha esquivado un disparo. |
| b* | El barco ha recibido un botiquín. El * puede ser cualquier número entre 1 y 3 e indica el número de botiquines. |

La sintaxis al ejecutar el programa debe ser la siguiente:

`./<program> <inputFile> <outputFile> <tamBuffer> <numNaves>`

## Hilo _Disparador_

Este hilo es el encargado de leer el fichero de entrada, convertir cada carácter en un token válido (descartando caracteres inválidos) e introducirlos en un buffer circular del tamaño especificado (argumento `tambuffer`).

Si el buffer se llena, debe esperar a que las naves vayan leyéndolo para escribir más tokens.
Una vez haya terminado de leer el fichero de entrada, debe imprimir en la salida especificada el número de tokens válidos, inválidos y el total.

## Hilos _Naves_

Hay tantos de estos hilos como se especifique con `numNaves`. Estos hilos leen el buffer circular y registran cada token, consumiéndolo del buffer, a excepción de los botiquines. En estos casos si hay más de 1 botiquín, se debe reducir en 1 el número pero no consumir ese token. De esta forma si hay 3 botiquines (`b3`) se podría registrar cada uno en 3 barcos diferentes.

Una vez que el buffer se haya leído por completo y el _Disparador_ haya llegado al final del fichero, cada nave debe guardar sus resultados en una lista enlazada. Los resultados son el número de disparos recibidos, los esquivados, los botiquines usados y la puntuacion final: el número de disparos recibidos menos los botiquines usados.

## Hilo _Juez_

Este hilo lee la lista enlazada según vayan terminando las naves e imprime sus resultados. Una vez que todas las naves hayan terminado el juego, imprime otra vez los resultados de la nave ganadora y subcampeona. Un barco solo puede ser ganador o subcampeón si ha registrado al menos un token. Cuanto menor la puntuación, mejor.

Al final del fichero se imprime un resumen del juego con el total de tokens de cada tipo. Un ejemplo de salida es [output.txt](/output.txt).
