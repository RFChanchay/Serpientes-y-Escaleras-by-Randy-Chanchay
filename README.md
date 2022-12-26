# Serpientes-y-Escaleras-by-Randy-Chanchay

## 1.Contexto del ejercicio
Serpientes y escaleras es un juego de mesa en el que dos o más jugadores lanzan dados para mover sus fichas en un tablero. El tablero se compone de una colección de cuadrados numerados y está adornado con 'serpientes' y 'escaleras', que unen dos cuadrados en el tablero: las serpientes unen los cuadrados hacia abajo mientras que las escaleras los unen hacia arriba. Esto significa que aterrizar en la parte inferior de una escalera te lleva a la parte superior de esa escalera, mientras que aterrizar en la parte superior de una serpiente te lleva a la parte inferior de esa serpiente. El objetivo del juego es llevar tu ficha a la casilla final antes que tus oponentes.

Desde un punto de vista técnico, la implementación de este juego debe ser independiente de la plataforma. Querremos lanzar este juego en varios dispositivos y necesitaremos una biblioteca de juegos sólida y robusta que podamos usar como backend para cada frontend que agreguemos. Por esta razón, no estamos interesados en la interfaz que elijas usar para probar el juego, solo que la lógica principal del juego esté separada y probada.

## 2.Mapa del juego
![](img/snakesandladdersboard.jpg)

## 3. Historias de usuario
La primera característica que queremos implementar es la capacidad de mover su ficha por el tablero usando tiradas de dados. Los jugadores necesitarán la capacidad de lanzar un dado, mover su ficha la cantidad de casillas indicadas por la tirada de dados y deberían ganar si aterrizan en la casilla final. La característica se divide en tres historias de usuario.

### 3.1 Historia de Usuario 1: La ficha puede moverse por el tablero
Como jugador quiero poder mover mi token para que pueda acercarme a la meta
  #### UAT1
  Dado que el juego se inicia

  Cuando la ficha se coloca en el tablero

  Entonces la ficha está en el cuadrado 1.
  #### UAT2
  Dado que el token está en el cuadrado 1

  Cuando la ficha se mueve 3 espacios

  Entonces la ficha está en el cuadrado 4.
  #### UAT3
  Given the token is on square 1

  When the token is moved 3 spaces

  And then it is moved 4 spaces

  Then the token is on square 8

### 3.2 Historia de Usuario 2: Un jugador puede ganar el juego
Como jugador, quiero poder ganar el juego, para poder regodearme con todos los que me rodean.
  #### UAT1
  Dado que la ficha está en el cuadrado 97

  Cuando la ficha se mueve 3 espacios

  Entonces la ficha está en el cuadrado 100

  Y el jugador ha ganado el juego.
  #### UAT2
  Dado que la ficha está en el cuadrado 97

  Cuando la ficha se mueve 4 espacios

  Entonces la ficha está en el cuadrado 97

  Y el jugador no ha ganado la partida.
  
### 3.3 Historia de Usuario 3: Los movimientos son determinados por dados
Como jugador, quiero mover mi ficha en función de la tirada de un dado para que haya un elemento de azar en el juego.
  #### UAT1
Dado que el juego se inicia

Cuando el jugador tira un dado

Entonces el resultado debe estar entre 1-6 inclusive
  #### UAT2
Dado que el jugador saca un 4

Cuando mueven su ficha

Entonces la ficha debe moverse 4 espacios.
 
## 4. Requisitos Generales
- La aplicación debe usar la consola para entrada y salida.
- Utilizar un lenguaje orientado a objetos, preferiblemente C o Swift. (proporcionar instrucciones sobre cómo ejecutar la aplicación).
- Implemente los requisitos centrándose en escribir el mejor código que pueda producir.
## 5.Metodo de Evaluación
- Producir código que sea legible y mantenible, utilizando principios de diseño orientado a objetos.
- Los UAT deben estar respaldados por pruebas.

## 6.Implementación del código
Para la implementación del código se opto por usar el lenguaje C++. Debido a su facilidad en el manejo de objetos y ejecución en dispositivos Windows para la elaboración de pruebas en usuarios externos.




