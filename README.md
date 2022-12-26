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
  Dado que el token está en el cuadrado 1

  Cuando la ficha se mueve 3 espacios

  Y luego se mueve 4 espacios

  Entonces la ficha está en el cuadrado 8.

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
### 6.1 Codigo:
```// SerpientesEscaleras.cpp : Este archivo contiene la función "main". La ejecución del programa comienza y termina ahí.
//
#include<stdio.h>
#include<stdlib.h>
#include<time.h>
//Representacion de una casilla
class Casilla
{
private:
	//propiedades
	int posicion;//posicion real en el tablero
	int tipo;//determina si la casilla es normal(0), es casilla con serpiente(1)  o escalera(2)
	int destino;//destino en caso de ser serpiente o escalera
public:

	Casilla(int posicion, int tipo, int destino) {
		this->posicion = posicion;
		this->tipo = tipo;
		this->destino = destino;
	}
	int getPosicion() { return this->posicion; }
	int getTipo() { return this->tipo; }
	int getDestino() { return this->destino; }
	void setTipo(int tipo) { this->tipo = tipo; }
	void setDestino(int destino) { this->destino = destino; }

	~Casilla();

};
class Tablero
{
private:
	// Propiedades del tablero
	int numCasillas;  // Número de casillas del tablero
	Casilla** casillas;  // Array de punteros a casillas
public:
	Tablero(int numCasillas) {
		this->numCasillas = numCasillas;
		this->casillas = (Casilla**)malloc(numCasillas * sizeof(Casilla*));
		for (int i = 0; i < numCasillas; i++) {
			this->casillas[i] = new Casilla(i, 0, i);  // Todas las casillas son normales al principio, es decir sin serpientes ni escaleras
		}
	}
	//agregamos casilla
	void agregarCasillaEspecial(int posicion, int tipo, int destino) {
		this->casillas[posicion]->setTipo(tipo);
		this->casillas[posicion]->setDestino(destino);
	}
	int getNumCasillas() { return this->numCasillas; }
	// Metodo para obtener la casilla en una determinada posición del tablero
	Casilla* obtenerCasilla(int posicion) { return this->casillas[posicion]; }
	
	~Tablero();



};

// Clase que representa a un jugador en el juego
class Jugador {
private:
	// Propiedades del jugador
	char* nombre;
	int posicion;  // Posición actual del jugador en el tablero

public:
	Jugador(char* nombre) {
		this->nombre = nombre;
		this->posicion = 0;
	}
	// Getters y setters para las propiedades del jugador
	char* getNombre() { return this->nombre; }
	int getPosicion() { return this->posicion; }
	void setPosicion(int posicion) { this->posicion = posicion; }

	// Metodo para avanzar al jugador en el tablero
	void avanzar(Tablero* tablero) {
		// Lanzar el dado para obtener un valor aleatorio entre 1 y 6
		int dado = rand() % 6 + 1;
		printf("\n%s lanza el dado y obtiene %d\n", this->nombre, dado);
		system("pause");


		// Avanzar la posición del jugador en el tablero
		this->posicion += dado;

		// Si el jugador se pasó del final del tablero, volver a la última casilla
		if (this->posicion >= tablero->getNumCasillas()) {
			this->posicion = tablero->getNumCasillas() - 1;
		}

		// Obtener la casilla en la que se encuentra el jugador
		Casilla* casilla = tablero->obtenerCasilla(this->posicion);

		// Si la casilla es una serpiente o una escalera, mover al jugador a su destino
		if (casilla->getTipo() == 1) {  // Serpiente
			printf("\n                  __");
			printf("\n      _______    /*_>-<");
			printf("\n  ___/ _____ \__/ /");
			printf("\n< ____/     \____/");
			printf("\n%s encuentra una serpiente y baja a la casilla %d\n", this->nombre, casilla->getDestino()+1);
			this->posicion = casilla->getDestino();
			system("pause");

		}
		else if (casilla->getTipo() == 2) {  // Escalera
			
			printf("\n |--|");
			printf("\n |--|");
			printf("\n |--|");
			printf("\n |--|");

			printf("\n%s encuentra una escalera y sube a la casilla %d\n", this->nombre, casilla->getDestino()+1);
			this->posicion = casilla->getDestino();
			system("pause");
		}
		printf("%s encuentra en la casilla %d \n", this->nombre, this->posicion+1);
	}

	// Metodo para determinar si el jugador ha ganado el juego
	bool haGanado() { return this->posicion == 99; }

	
};

int main()
{
	srand(time(NULL));
	// Crear el tablero del juego con 100 casillas
	Tablero* tablero = new Tablero(100);


	// Agregar algunas serpientes y escaleras al tablero segun la imagen presentada
	//serpientes


	tablero->agregarCasillaEspecial(15, 1, 5);
	tablero->agregarCasillaEspecial(45, 1, 24);
	tablero->agregarCasillaEspecial(48, 1, 10);
	tablero->agregarCasillaEspecial(61, 1, 18);
	tablero->agregarCasillaEspecial(63, 1, 59);
	tablero->agregarCasillaEspecial(73, 1, 52);
	tablero->agregarCasillaEspecial(88, 1, 67);
	tablero->agregarCasillaEspecial(92, 1, 88);
	tablero->agregarCasillaEspecial(94, 1, 74);
	tablero->agregarCasillaEspecial(98, 1, 79);


	//escaleras
	tablero->agregarCasillaEspecial(1, 2, 37);
	tablero->agregarCasillaEspecial(6, 2, 13);
	tablero->agregarCasillaEspecial(7, 2, 30);
	tablero->agregarCasillaEspecial(14, 2, 25);
	tablero->agregarCasillaEspecial(20, 2, 41);
	tablero->agregarCasillaEspecial(27, 2, 83);
	tablero->agregarCasillaEspecial(35, 2, 43);
	tablero->agregarCasillaEspecial(50, 2, 66);
	tablero->agregarCasillaEspecial(70, 2, 90);
	tablero->agregarCasillaEspecial(77, 2, 97);
	tablero->agregarCasillaEspecial(86, 2, 93);
	//creamos 2 jugadores
	char nameJug1[] = { "Jugador 1" };
	char nameJug2[] = { "Jugador 2" };

	Jugador* jugador1 = new Jugador(nameJug1);
	Jugador* jugador2 = new Jugador(nameJug2);

	//interfaz inicial
	printf("\n********************************************************");
	printf("\n*  ___  ___  ___   ___  ___  ___       ___  ___  ___   *");
	printf("\n* |___ |___ |___| |___|  |  |___  |* |  |  |___ |___   *");
	printf("\n*  ___||___ | *.  |     _|_ |___  | *|  |  |___  ___|  *");
	printf("\n*  ___  ___  ___   ___        ___   ___   ___    ___   *");
	printf("\n* |___ |___ |     |___| |    |___  |___| |___|  |___   *");
	printf("\n* |___  ___||___  |   | |___ |___  | *.  |   |   ___|  *");
	printf("\n********************************************************");
	printf("\nBY: Randy Chanchay\n");
	system("pause");
	system("cls");
	printf("Hola vamos a jugar serpientes y escaleras donde tendremos 2 jugadores\n");
	printf("\nSerpientes y Escaleras es un juego de mesa en el que dos o más jugadores lanzan dados para mover sus fichas en un tablero.");
	printf("\nEl tablero se compone de una colección de cuadrados numerados y está adornado con 'serpientes' y 'escaleras', ");
	printf("\nque unen dos cuadrados en el tablero.");
	printf("\nLas serpientes unen los cuadrados hacia abajo mientras que las escaleras los unen hacia arriba.  ");
	printf("\nEsto significa que aterrizar en la parte inferior deuna escalera te lleva a la parte superior de esa escalera,mientras que aterrizar \nen la parte superior de una serpiente te lleva a la parte inferior de ");
	printf("\nesa serpiente. El objetivo del juego es llevar tu ficha a la casilla final antes que tus oponentes.\n");
	system("pause");
	system("cls");

	while (!jugador1->haGanado() && !jugador2->haGanado()) {
		// Hacer avanzar al primer jugador
		printf("\n*******TURNO JUGADOR 1********\n");
		system("pause");
		jugador1->avanzar(tablero);
		system("pause");
		system("cls");
		if (jugador1->haGanado()) {
			printf("%s ha ganado el juego!\n", jugador1->getNombre());
			break;
		}

		// Hacer avanzar al segundo jugador
		printf("\n*******TURNO JUGADOR 2********\n");


		system("pause");
		jugador2->avanzar(tablero);
		system("pause");
		system("cls");

		if (jugador2->haGanado()) {
			printf("%s ha ganado el juego!\n", jugador2->getNombre());
			break;
		}
	}
	printf("\n*************************************************");
	printf("\n*               GRACIAS                         *");
	printf("\n*             by:Rafch17                        *");
	printf("\n*************************************************\n");
	system("pause");

}
```
## 7. Explicación del codigo
Para elaborar el código se utilizó 3 clases:
-	Casilla: Representa cada uno de los recuadros donde una ficha puede ser ubicada, este puede contener una serpiente o una escalera con lo cual tendrá una posición y un destino si se da el caso.
-	Tablero: Representa el espacio general donde se va a jugar, para este caso tendremos un arreglo de casillas y también la cantidad de casillas como atributo. Podemos inicializar con cualquier número de casillas, pero para nuestro ejemplo descrito según la figura de nuestro mapa usaremos 100 casillas.
-	Jugador: Esta clase representa a nuestro jugador en cuestión, el cual contara con nombre y posición actual en el tablero. Para nuestro caso usaremos 2 jugadores.

En el caso de nuestros métodos contamos con getters, setters, constructores y destructores propios en la programación orientada a objetos, manteniendo siempre los principios de encapsulamiento. Algunos métodos nos permiten conocer aspectos solicitados en las historias de usuario como lo son: asignar posiciones en base a los dados, saber si alguien ha ganado o si ha caído en una serpiente o a ascendido por una escalera.

Para ganar se deberá caer en la casilla 100 ósea la posición 99 debido a que se comienza en 0, una vez ocurrido esto el programa se detendrá mostrando un mensaje final al ganador.
## 8. Ejecución del Código
![](img/Imagen1.jpg)
![](img/Imagen2.jpg)
![](img/Imagen3.jpg)
![](img/Imagen4.jpg)
![](img/Imagen5.jpg)
## 8. Verificación de requisitos de usuario


## 8. Manual de Instalacion para U


