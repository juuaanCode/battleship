# Battleship

[![es](https://img.shields.io/badge/lang-es-red.svg)](/README-ES.md)

Battleship is an assignment from _Fundamentos de Sistemas Operativos_ (OS Fundamentals), a 2nd year subject of Computer Science at University of Valladolid. It was developed in colaboration with another student, Tomás Carretero and in Spanish.

Although the system is based on the famous "Battleship" game, the main purpose of this task was to learn how to manage threads in a UNIX environment, semaphores, shared memory, dynamic memory allocation...

## Main Summary

The program creates several ships that react to an input file (some examples are provided on the [inputfiles folder](/inputfiles/)). Each character corresponds to a token which can only be read once and only by one ship.

| Token | Meaning |
|-------|---------|
| *     | The ship has been hit by a shot. |
| _blank space_ | The ship has missed a shot. |
| b* | The ship has received a _cure_. The * can be any number between 1 and 3, indicating the number of cures.|

The syntax when running the program must be as follows:

`./<program> <inputFile> <outputFile> <tamBuffer> <numNaves>`

## _Disparador_ Thread

This thread is in charge of reading the input file, parse each character into a valid token (and discard invalid ones) and place them into a circular buffer of the specified size (`tambuffer` argument).

If the buffer is full it must wait for the ships (naves) to read it before writing new tokens.
Once it has finished reading the input file, it must print on the specified output the number of valid and invalid tokens, and the total number of them.

## _Naves_ Threads

As many of this threads are created as the argument `numNaves` specifies (the number of ships). This threads read the circular buffer and register each token, consuming it from the buffer. The only exception is with the cures: if there is a number greater than 1, then it must be reduced by one unit but the token must not be consumed. This way, if there are 3 cures (`b3`) then each one can end up being registered by 3 different ships.

Once the buffer is completely read and the _Disparador_ has reached the end of the file, each ship must save their results on a linked list. This final result are the number of shots received, the number of missed shots, number of cures used, and the final score: the number of shots received minus the cures used.

## _Juez_ Thread

This thread (the judge) will read the linked list as the ships finish reading the buffer and will print their results. Once every ship has ended the game, it must print once again the results of the winner and the second ship. A ship can only be winner or second if it has registered at least one token. The lower the score, the better.

At the end of the file a summary of the game will be printed, displaying the total number of each type of token. A sample output is [output.txt](/output.txt).
