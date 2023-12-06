# PROYECTO
El juego de los barcos, baxttleships o batalla naval.

#include <stdio.h>

int Inicializa_tablero (char tablero [11][11]);
int Pinta_tablero_propio (char tablero [11][11]);
int Pinta_tablero_oponente (char tablero [11][11]);
int Captura_coordenada_x (int player, char name [10]);
int Captura_coordenada_y (int player, char name [10]);
int Convierte_x_a_num (char X);
int Define_posicion_Portaviones (char tablero [11][11]);
int Define_posicion_Acorazados (char tablero [11][11]);
int Define_posicion_Cruceros (char tablero [11][11]);
int Define_posicion_Submarinos (char tablero [11][11]);
int Define_posicion_Destructores (char tablero [11][11]);
int Verifica_espacio_disponible (char tablero [11][11], int x, int y, char z, int q);
int Determina_quien_empieza_la_partida (char name1 [10], char name2 [10]);
int Verifica_barcos_vivos (char tablero [11][11]);
int Verifica_espacio_disponible_en_tablero (char tablero [11][11], int x, int y, char m);
int Verifica_jugador_ganador (char tablero1 [11][11], char tablero2 [11][11]);
char Elejir_disparo_o_mina (int player, char name [10]);
int Captura_coordenada_x_mina (int player, char name [10]);
int Captura_coordenada_y_mina (int player, char name [10]);
int Mueve_mina_aleatoriamente (char tablero [11][11], int player);
char Desplegar_ayuda ();
char Inicializa_nombre_jugador (char name [10]);

int main (int argc, char *argv[]) {
  char tablero1 [11][11], tablero2 [11][11], tiro, argumento [50], name1 [10], name2 [10], semilla [10];
  int player, first_player, hay_barcos_vivos, p, num, ganador, resto, tirada_ok, x, y, xy, mina1, mina2, primera_vez_mina1, primera_vez_mina2;
  int i, j;
  
  mina1 = 1; /* opcion de juego con minas desactivada */
  mina2 = 1; /* opcion de juego con minas desactivada */
  primera_vez_mina1 = 0;
  primera_vez_mina2 = 0;
  Inicializa_nombre_jugador (name1);
  Inicializa_nombre_jugador (name2);
  
  i = 1; /* primer argumento */
  j = 0;
  if (argc > 1) {
    while (argv [i][j] != '\0') {
      argumento [j] = argv [i][j];
      j++;
    }
    if (argumento[0] == '-' && argumento [1] == 'h') {
      Desplegar_ayuda ();
      return 0;
    }
    else {
      if (argumento[0] == '-' && argumento [1] == '2' && argumento [2] == 'p') {/* obtener nombre de los jugadores */
        if (argc > 2) {
          i = 2;
		  j = 0; 
          while (argv [i][j] != '\0') { /* nombre del jugador 1 */
            name1 [j] = argv [i][j];
            j++;
          }
          i = 3;
		  j = 0; 
          while (argv [i][j] != '\0') { /* nombre del jugador 2 */
            name2 [j] = argv [i][j];
            j++;
          }
        }
        else {
          printf ("Proporcione el nombre de los dos jugadores: pgm -2p name1 name2");
          return 0;
		}
      }
      if (argc > 3) {/* se especifica semila y/o mina */
        i = 4; /* cuarto argumento (semilla o mina) */
        j = 0;
        while (argv [i][j] != '\0') {
          argumento [j] = argv [i][j];
          j++;
        }
        if (argumento[0] == '-' && argumento [1] == 's') { /* se especifico semilla */
          if (argc > 4) {/* obtener el valor de la semilla */
            i = 5;
            j = 0;
            while (argv [i][j] != '\0') {
              semilla [j] = argv [i][j];
              j++;
			}
          }
          else {
          	printf ("Se requiere especificar el valor de la semilla...");
          	return 0;
		  }
		  if (argc > 5) { /* opcion de juego con minas */
		    i = 6;
            j = 0;
            while (argv [i][j] != '\0') {
              argumento [j] = argv [i][j];
              j++;
			}
		    if (argumento [0] == '-' && argumento [1] == 'm') {/* opcion de mina especificada */
              mina1 = 0; /* cero, si el jugador desea jugar con minas */
              mina2 = 0; /* cero, si el jugador desea jugar con minas */
            }
            else {
          	 printf ("Argumento 6 invalido...");
          	 return 0;
		    }	
		  }
        }
        else {
          if (argumento [0] == '-' && argumento [1] == 'm') {/* opcion de mina especificada */
            mina1 = 0; /* cero, si el jugador desea jugar con minas */
            mina2 = 0; /* cero, si el jugador desea jugar con minas */
          }
          else {
          	printf ("Argumento 4 invalido...");
          	return 0;
		  }
		}
      }
	}
  }
  if (argc > 7) {
  	printf ("Exceso de parametros invalido...");
  	return 0;
  }
    
  Inicializa_tablero (tablero1);
  Inicializa_tablero (tablero2);
  printf ("Tablero del jugador 1 %s:\n", name1);
  Pinta_tablero_propio (tablero1);
  printf ("Tablero del jugador 2 %s:\n", name2);
  Pinta_tablero_propio (tablero2);
  printf ("Posicionando embarcaciones del jugador 1 %s...\n", name1);
  /* Define_posicion_Portaviones (tablero1);
  Define_posicion_Acorazados (tablero1);
  Define_posicion_Cruceros (tablero1);
  Define_posicion_Submarinos (tablero1); */
  Define_posicion_Destructores (tablero1);
  printf ("Posicionando embarcaciones del jugador 2 %s...\n", name2);
  /* Define_posicion_Portaviones (tablero2);
  Define_posicion_Acorazados (tablero2);
  Define_posicion_Cruceros (tablero2);
  Define_posicion_Submarinos (tablero2); */
  Define_posicion_Destructores (tablero2);
  printf ("Tablero del jugador 1 %s:\n", name1);
  Pinta_tablero_propio (tablero1);
  printf ("Tablero del jugador 2 %s:\n", name2);
  Pinta_tablero_propio (tablero2);
  first_player = Determina_quien_empieza_la_partida (name1, name2);
  p = first_player;
  num = 9999; 
  ganador = 0;
  hay_barcos_vivos = 0;
  while (num != 0) {
  	resto = p % 2;
  	if (resto == 1) {
  	  player = 1;
    }
  	else {
  	  player = 2;
    }
    do {
      if (player == 1) {
        hay_barcos_vivos = Verifica_barcos_vivos (tablero2);
        if (hay_barcos_vivos == 0) {
          tiro = 'D';
		  if (mina1 == 0) { /* mina del jugador 1 vigente */
            tiro = Elejir_disparo_o_mina (player, name1);
          } 
        }
      }
      else {
        hay_barcos_vivos = Verifica_barcos_vivos (tablero1);
        if (hay_barcos_vivos == 0) {
          tiro = 'D';
		  if (mina2 == 0) { /*mina del jugador 2 vigente */
            tiro = Elejir_disparo_o_mina (player, name2);
          }
        }
      }
      if (hay_barcos_vivos == 0) {
    	if (tiro == 'M') {
    	  if (player == 1) {
    	    if (primera_vez_mina1 == 0) {
    	      x = Captura_coordenada_x_mina (player, name1);
      	      y = Captura_coordenada_y_mina (player, name1);
      	      primera_vez_mina1 = 1; /* no vuelve a solicitar coordenadas de la mina */
			}
			else { 
			  xy = Mueve_mina_aleatoriamente (tablero2, player);
			  if (xy == 888) {/* autodestruccion de la mina */
			    mina1 = 1; /* apagar la mina */
			    x = 0;
			    y = 0;
		      }
		      else {
		      	x = xy / 100; /* los dos digitos de la izquierda */
		      	y = xy % 100; /* los dos digitos de la derecha */
			  }
			}
          }
          else {
          	if (primera_vez_mina2 == 0) {
    	      x = Captura_coordenada_x_mina (player, name2);
      	      y = Captura_coordenada_y_mina (player, name2);
      	      primera_vez_mina2 = 1; /* no vuelve a solicitar coordenadas de la mina */
			}
			else { 
			  xy = Mueve_mina_aleatoriamente (tablero1, player);
			  if (xy == 888) {/* autodestruccion de la mina */
			    mina2 = 1; /* apagar la mina */
			    x = 0;
			    y = 0;
		      }
		      else {
		      	x = xy / 100; /* los dos digitos de la izquierda */
		      	y = xy % 100; /* los dos digitos de la derecha */
			  }
			}
		  }
        }
      	else {
      	  if (player == 1) {
      	    x = Captura_coordenada_x (player, name1);
      	    y = Captura_coordenada_y (player, name1);
          }
          else {
          	x = Captura_coordenada_x (player, name2);
      	    y = Captura_coordenada_y (player, name2);
		  }
        }
      	if (x > 0 && y > 0) {
      	  num = x;
      	  if (player == 1) {
      		tirada_ok = Verifica_espacio_disponible_en_tablero (tablero2, x, y, tiro);
          }
          else {
          	tirada_ok = Verifica_espacio_disponible_en_tablero (tablero1, x, y, tiro);
		  }
		  if (tirada_ok == 2) {
		    printf ("\nCoordenada no disponible. Intente nuevamente...");
	      } 
		  else {
		    if (tirada_ok == 1) {
		      printf ("\nHundiste un barco enemigo. Buen tino...");
		      if (tiro == 'M'){
		      	if (player == 1) {
		      	  mina1 = 1; /* apagar la mina del jugador 1*/
		        }
		        else {
		          mina2 = 1; /* apagar la mina del jugador 2*/
				}
			  }
		    }
		    else {
		      if (tirada_ok == 0) {
		        printf ("\nDisparo errado. Suerte para la proxima...");
		      }
		    }
	      }
		}
	  }
	  else {
	  	printf ("\nYa no hay mas barcos del adversario disponibles para hundir...");
	  	ganador = Verifica_jugador_ganador (tablero1, tablero2);
	  	if (ganador == 0) {
	  	  printf ("\nEmpate entre jugadores. Misma cantidad de barcos hundidos mutuamente...");
		}
		else {
		  num = 0; /* existe un ganador, fin de la partida */
	  	  if (ganador == 1) {
	  	    printf ("\nEl jugador 1 %s ha ganado la partida. Congratulations...", name1);
		  }
	  	  else {
	  	    printf ("\nEl jugador 2 %s ha ganado la partida. Congratulations...", name2);
		  }
	    }
	  }
	} while (tirada_ok == 2 && num > 0 && ganador == 0 && hay_barcos_vivos == 0);
	if (resto == 1) {
	  hay_barcos_vivos = Verifica_barcos_vivos (tablero2);
	  if (ganador == 0) {
	  	printf ("\nTablero del jugador 1 %s despues del ultimo tiro:\n", name1);
	    Pinta_tablero_propio (tablero1);
	    printf ("\nTablero del jugador oponente %s:\n", name2);
	    Pinta_tablero_oponente (tablero2);
      }
      p = 2; /* siguiente jugador: 2 */
    } 
	else {
	  hay_barcos_vivos = Verifica_barcos_vivos (tablero1);
	  if (ganador == 0) {
	  	printf ("\nTablero del jugador 2 %s despues del ultimo tiro:\n", name2);
	    Pinta_tablero_propio (tablero2);
	    printf ("\nTablero del jugador oponente %s:\n", name1);
	    Pinta_tablero_oponente (tablero1);
      }
      p = 1; /* siguiente jugador: 1 */
    }
	if (hay_barcos_vivos == 0) {
	}
	else {
      ganador = Verifica_jugador_ganador (tablero1, tablero2);
	  if (ganador == 1) {
	    printf ("\nEl jugador 1 %s ha ganado la partida. Congratulations...", name1);
	    num = 0; /* existe un ganador, fin de la partida */
      }
	  else {
	    if (ganador == 2) {
	      printf ("\nEl jugador 2 %s ha ganado la partida. Congratulations...", name2);
	      num = 0; /* existe un ganador, fin de la partida */
	    }
      }  
	}
  }
  printf ("\nTablero del jugador 1 %s al terminar la partida:\n", name1);
  Pinta_tablero_propio (tablero1);
  printf ("\nTablero del jugador oponente %s:\n", name2);
  Pinta_tablero_oponente (tablero2);
  printf ("\nTablero del jugador 2 %s al terminar la partida:\n", name2);
  Pinta_tablero_propio (tablero2);
  printf ("\nTablero del jugador oponente %s:\n", name1);
  Pinta_tablero_oponente (tablero1);
  return 0;
}

int Inicializa_tablero (char tablero [11][11]) {
  int i, j;
  for (i = 0; i < 11; i++) {
    for (j = 0; j < 11; j++) {
      tablero [i][j] = ' ';
    }
  }
  for (j = 0; j < 11; j++) {
    switch (j) {
    case 0: tablero [0][j] = ' ';
	        break;
	case 1: tablero [0][j] = '1';
	        break;
	case 2: tablero [0][j] = '2';
	  		break;
	case 3: tablero [0][j] = '3';
	        break;
	case 4: tablero [0][j] = '4';
	        break;
	case 5: tablero [0][j] = '5';
	        break;
	case 6: tablero [0][j] = '6';
	        break;
	case 7: tablero [0][j] = '7';
	        break;
	case 8: tablero [0][j] = '8';
	        break;
  	case 9: tablero [0][j] = '9';
	        break;
	case 10: tablero [0][j] = '0';
	        break;
    }
  }
  for (i = 0; i < 11; i++) {
  	switch (i) {
    case 0: tablero [i][0]= ' ';
	        break;
	case 1: tablero [i][0]= 'A';
	        break;
	case 2: tablero [i][0]= 'B';
	        break;
	case 3: tablero [i][0]= 'C';
	        break;
	case 4: tablero [i][0]= 'D';
	        break;
	case 5: tablero [i][0]= 'E';
	        break;
	case 6: tablero [i][0]= 'F';
	        break;
	case 7: tablero [i][0]= 'G';
	        break;
	case 8: tablero [i][0]= 'H';
	        break;
	case 9: tablero [i][0]= 'I';
	        break;
	case 10: tablero [i][0]= 'J';
	        break;
    }
  }
  return 0;
}

int Pinta_tablero_propio (char tablero [11][11]) {
  int i, j;
  for (i = 0; i < 11; i++) {
    for (j = 0; j < 11; j++) {
      printf ("%c", tablero [i][j]);
    }
    printf ("\n");
  }
  return 0;
}

int Pinta_tablero_oponente (char tablero [11][11]) {
  int i, j;
  for (i = 0; i < 11; i++) {
    for (j = 0; j < 11; j++) {
      if (i == 0) {
        printf ("%c", tablero [i][j]);
      }
      else {
        if (j == 0) {
          printf ("%c", tablero [i][j]);
        }
        else {
          if (tablero [i][j] == 'X' || tablero [i][j] == '0' || tablero [i][j] == 'M') {
            printf ("%c", tablero [i][j]); 
          }
          else {
            printf (" ");
          }
        }
      }
    }
    printf ("\n");
  }
  return 0;
}

int Captura_coordenada_x (int player, char name [10]) {
  char X;
  int x;
  printf ("\nTurno para el jugador %d %s. Digite coordenada x de su objetivo (A-J) o 0 para abandonar): ", player, name);
  do 
    X = getchar ();
  while (X != 'a' && X != 'b' && X != 'c' && X != 'd' && X != 'e' && X != 'f' && X != 'g' && X != 'h' && X != 'i' && X != 'j' &&
         X != 'A' && X != 'B' && X != 'C' && X != 'D' && X != 'E' && X != 'F' && X != 'G' && X != 'H' && X != 'I' && X != 'J' && X != '0');
  x = Convierte_x_a_num (X);
  return x;
}

int Captura_coordenada_y (int player, char name [10]) {
  int y;
  printf ("\nTurno para el jugador %d %s. Digite coordenada y de su objetivo (1-10) o 0 para abandonar): ", player, name);
  do 
    scanf ("%d", &y);
  while (y < 0 || y > 10);
  return y;
}

int Define_posicion_Portaviones (char tablero [11][11]) {
  int x, y, disponible, i;
  char X, z;
  disponible = 0;
  printf ("Colocando posiciones para Portaaviones...\n");
  /* Pinta_tablero (tablero); */
  do {
    printf ("\nDigite coordenada x (A-J) para 5 portaviones: ");
    do 
      X = getchar ();
    while (X != 'a' && X != 'b' && X != 'c' && X != 'd' && X != 'e' && X != 'f' && X != 'g' && X != 'h' && X != 'i' && X != 'j' &&
           X != 'A' && X != 'B' && X != 'C' && X != 'D' && X != 'E' && X != 'F' && X != 'G' && X != 'H' && X != 'I' && X != 'J' && X != '0');
    x = Convierte_x_a_num (X);
    printf ("\nDigite coordenada y (1-10) para 5 portaviones: ");
    do
      scanf ("%d", &y);
    while (y < 0 || y > 10);
    printf ("\nDigite direccion de estos barcos (a=arriba, b=abajo, i=izquierda, d=derecha):");
    do 
	  z = getchar ();
	while (z != 'a' && z != 'b' && z != 'i' && z != 'd' && z != 'A' && z != 'B' && z != 'I' && z != 'D');
    disponible = Verifica_espacio_disponible (tablero, x, y, z, 5);
    if (disponible > 0)
      printf ("\nEspacio no disponible para 5 portaviones...Intente nuevamente.");
  }  while (disponible > 0);
  if (z == 'a'|| z == 'A' || z == 'b'|| z == 'B') {
    for (i = 5; i > 0; i-- ) {
      tablero [x][y] = 'P';
      if (z == 'a' || z == 'A')
        x--;
      else
        x++;
    }
  }
  if (z == 'i'|| z == 'I' || z == 'd'|| z == 'D') {
    for (i = 5; i > 0; i-- ) {
      tablero [x][y]  = 'P';
      if (z == 'i' || z == 'I')
        y--;
      else
        y++;
    }
  }
  return 0;
}

int Define_posicion_Acorazados (char tablero [11][11]) {
  int x, y, disponible, i;
  char X, z;
  disponible = 0;
  printf ("Colocando posiciones para Acorazados...\n");
  /* Pinta_tablero (tablero); */
  do {
    printf ("\nDigite coordenada x (A-J) para 4 acorazados: ");
    do 
      X = getchar ();
    while (X != 'a' && X != 'b' && X != 'c' && X != 'd' && X != 'e' && X != 'f' && X != 'g' && X != 'h' && X != 'i' && X != 'j' &&
           X != 'A' && X != 'B' && X != 'C' && X != 'D' && X != 'E' && X != 'F' && X != 'G' && X != 'H' && X != 'I' && X != 'J' && X != '0');
    x = Convierte_x_a_num (X);
    printf ("\nDigite coordenada y (1-10) para 4 acorazados: ");
    do
      scanf ("%d", &y);
    while (y < 0 || y > 10);
    printf ("\nDigite direccion de estos barcos (a=arriba, b=abajo, i=izquierda, d=derecha):");
    do 
	  z = getchar ();
	while (z != 'a' && z != 'b' && z != 'i' && z != 'd' && z != 'A' && z != 'B' && z != 'I' && z != 'D');
    disponible = Verifica_espacio_disponible (tablero, x, y, z, 4);
    if (disponible > 0)
      printf ("\nEspacio no disponible para 4 acorazados...Intente nuevamente.");
  }  while (disponible > 0);
  if (z == 'a'|| z == 'A' || z == 'b'|| z == 'B') {
    for (i = 4; i > 0; i-- ) {
      tablero [x][y] = 'A';
      if (z == 'a' || z == 'A')
        x--;
      else
        x++;
    }
  }
  if (z == 'i'|| z == 'I' || z == 'd'|| z == 'D') {
    for (i = 4; i > 0; i-- ) {
      tablero [x][y]  = 'A';
      if (z == 'i' || z == 'I')
        y--;
      else
        y++;
    }
  }
  return 0;
}

int Define_posicion_Cruceros (char tablero [11][11]) {
  int x, y, disponible, i;
  char X, z;
  disponible = 0;
  printf ("Colocando posiciones para Cruceros...\n");
  /* Pinta_tablero (tablero); */
  do {
    printf ("\nDigite coordenada x para 3 cruceros: ");
    do 
      X = getchar ();
    while (X != 'a' && X != 'b' && X != 'c' && X != 'd' && X != 'e' && X != 'f' && X != 'g' && X != 'h' && X != 'i' && X != 'j' &&
           X != 'A' && X != 'B' && X != 'C' && X != 'D' && X != 'E' && X != 'F' && X != 'G' && X != 'H' && X != 'I' && X != 'J' && X != '0');
    x = Convierte_x_a_num (X);
    printf ("\nDigite coordenada y (1-10) para 3 cruceros: ");
    do
      scanf ("%d", &y);
    while (y < 0 || y > 10);
    printf ("\nDigite direccion de estos barcos (a=arriba, b=abajo, i=izquierda, d=derecha):");
    do 
	  z = getchar ();
	while (z != 'a' && z != 'b' && z != 'i' && z != 'd' && z != 'A' && z != 'B' && z != 'I' && z != 'D');
    disponible = Verifica_espacio_disponible (tablero, x, y, z, 3);
    if (disponible > 0)
      printf ("\nEspacio no disponible para 3 cruceros...Intente nuevamente.");
  }  while (disponible > 0);
  if (z == 'a'|| z == 'A' || z == 'b'|| z == 'B') {
    for (i = 3; i > 0; i-- ) {
      tablero [x][y] = 'C';
      if (z == 'a' || z == 'A')
        x--;
      else
        x++;
    }
  }
  if (z == 'i'|| z == 'I' || z == 'd'|| z == 'D') {
    for (i = 3; i > 0; i-- ) {
      tablero [x][y]  = 'C';
      if (z == 'i' || z == 'I')
        y--;
      else
        y++;
    }
  }
  return 0;
}

int Define_posicion_Submarinos (char tablero [11][11]) {
  int x, y, disponible, i;
  char X, z;
  disponible = 0;
  printf ("Colocando posiciones para Submarinos...\n");
  /* Pinta_tablero (tablero); */
  do {
    printf ("\nDigite coordenada x (A-J) para 3 submarinos: ");
    do 
      X = getchar ();
    while (X != 'a' && X != 'b' && X != 'c' && X != 'd' && X != 'e' && X != 'f' && X != 'g' && X != 'h' && X != 'i' && X != 'j' &&
           X != 'A' && X != 'B' && X != 'C' && X != 'D' && X != 'E' && X != 'F' && X != 'G' && X != 'H' && X != 'I' && X != 'J' && X != '0');
    x = Convierte_x_a_num (X);
    printf ("\nDigite coordenada y (1-10) para 3 submarinos: ");
    do
      scanf ("%d", &y);
    while (y < 0 || y > 11);
    printf ("\nDigite direccion de estos barcos (a=arriba, b=abajo, i=izquierda, d=derecha):");
    do 
	  z = getchar ();
	while (z != 'a' && z != 'b' && z != 'i' && z != 'd' && z != 'A' && z != 'B' && z != 'I' && z != 'D');
    disponible = Verifica_espacio_disponible (tablero, x, y, z, 3);
    if (disponible > 0)
      printf ("\nEspacio no disponible para 3 submarinos...Intente nuevamente.");
  }  while (disponible > 0);
  if (z == 'a'|| z == 'A' || z == 'b'|| z == 'B') {
    for (i = 3; i > 0; i-- ) {
      tablero [x][y] = 'S';
      if (z == 'a' || z == 'A')
        x--;
      else
        x++;
    }
  }
  if (z == 'i'|| z == 'I' || z == 'd'|| z == 'D') {
    for (i = 3; i > 0; i-- ) {
      tablero [x][y]  = 'S';
      if (z == 'i' || z == 'I')
        y--;
      else
        y++;
    }
  }
  return 0;
}

int Define_posicion_Destructores (char tablero [11][11]) {
  int x, y, disponible, i;
  char X, z;
  disponible = 0;
  printf ("Colocando posiciones para Destructores...\n");
  /* Pinta_tablero (tablero); */
  do {
    printf ("\nDigite coordenada x (A-J) para 2 destructores: ");
    do 
      X = getchar ();
    while (X != 'a' && X != 'b' && X != 'c' && X != 'd' && X != 'e' && X != 'f' && X != 'g' && X != 'h' && X != 'i' && X != 'j' &&
           X != 'A' && X != 'B' && X != 'C' && X != 'D' && X != 'E' && X != 'F' && X != 'G' && X != 'H' && X != 'I' && X != 'J' && X != '0');
    x = Convierte_x_a_num (X);
    printf ("\nDigite coordenada y (1-10) para 2 destructores: ");
    do
      scanf ("%d", &y);
    while (y < 0 || y > 10);
    printf ("\nDigite direccion de estos barcos (a=arriba, b=abajo, i=izquierda, d=derecha): ");
    do 
	  z = getchar ();
	while (z != 'a' && z != 'b' && z != 'i' && z != 'd' && z != 'A' && z != 'B' && z != 'I' && z != 'D');
    disponible = Verifica_espacio_disponible (tablero, x, y, z, 2);
    if (disponible > 0)
      printf ("\nEspacio no disponible para 2 destructores...Intente nuevamente.");
  }  while (disponible > 0);
  if (z == 'a'|| z == 'A' || z == 'b'|| z == 'B') {
    for (i = 2; i > 0; i-- ) {
      tablero [x][y] = 'D';
      if (z == 'a' || z == 'A')
        x--;
      else
        x++;
    }
  }
  if (z == 'i'|| z == 'I' || z == 'd'|| z == 'D') {
    for (i = 2; i > 0; i-- ) {
      tablero [x][y]  = 'D';
      if (z == 'i' || z == 'I')
        y--;
      else
        y++;
    }
  }
  return 0;
}

int Convierte_x_a_num (char X) {
  /* int x;
  if ((X >= 'a' && X <= 'j') || (X >= 'A' && X <= 'J')) {
    x = (X & 0xDF) - 'A' + 1;
 †}
  return x;
} */
  int x;
  switch (X) {
  case 'a': x = 1;
    break;
  case 'b': x = 2;
    break;
  case 'c': x = 3;
    break;
  case 'd': x = 4;
    break;
  case 'e': x = 5;
    break;
  case 'f': x = 6;
    break;
  case 'g': x = 7;
    break;
  case 'h': x = 8;
    break;
  case 'i': x = 9;
    break;
  case 'j': x = 10;
    break;
  case 'A': x = 1;
    break;
  case 'B': x = 2;
    break;
  case 'C': x = 3;
    break;
  case 'D': x = 4;
    break;
  case 'E': x = 5;
    break;
  case 'F': x = 6;
    break;
  case 'G': x = 7;
    break;
  case 'H': x = 8;
    break;
  case 'I': x = 9;
    break;
  case 'J': x = 10;
    break;
  }
  return x;
} 

int Verifica_espacio_disponible (char tablero [11][11], int x, int y, char z, int q) {
  int i, disponible;
  disponible = 0;
  /* printf ("\nVerificando espacio disponible %d %d %c %d", x, y, z, q); */
  if (z == 'a'|| z == 'A' || z == 'b'|| z == 'B') {
    for (i = q; i > 0; i-- ) {
      if (x > -1 && y > -1 && x < 11 && y < 11)  {
        if (tablero [x][y] == ' ') {
	    }  
	    else {
	      disponible = 1;
        }
        if (z == 'a' || z == 'A')
          x--;
        else
          x++;
      }
      else {
        disponible = 1;
      }
    }
  }
  if (z == 'i'|| z == 'I' || z == 'd'|| z == 'D') {
    for (i = q; i > 0; i-- ) {
      if (x > -1 && y > -1 && x < 11 && y < 11) {
        if (tablero [x][y] == ' ') {
	    }  
	    else {
	      disponible = 1;
        }
        if (z == 'i' || z == 'I')
          y--;
        else
          y++;
      }
      else {
        disponible = 1;
      }
    }
  }
  return disponible;
}

int Determina_quien_empieza_la_partida (char name1 [10], char name2 [10]) {
  int my_random, residuo, num_jugador;
  srand (time(NULL));
  my_random = rand() % 10 + 1;
  printf ("Mi numero aleatorio: %d\n", my_random);
  residuo = my_random % 10;
  if (residuo < 6) {
  	printf ("Empieza el jugador 1 %s\n", name1);
  	num_jugador = 1; /* empieza el jugador 1 */
  }
  else {
  	printf ("Empieza el jugador 2 %s\n", name2);
  	num_jugador = 2; /* empieza el jugador 2 */
  }
  return num_jugador;
}

int Verifica_barcos_vivos (char tablero [11][11]) {
  int hay_barcos_vivos, i, j; 
  hay_barcos_vivos = 1;
  for (i = 1; i < 11; i++) {
  	for (j = 1; j < 11; j++) {
  	  if (tablero [i][j] == 'P' || tablero [i][j] == 'A' || tablero [i][j] == 'C' || tablero [i][j] == 'S' || tablero [i][j] == 'D')
  	    hay_barcos_vivos = 0;
    }
  }
  return hay_barcos_vivos;
}

int Verifica_espacio_disponible_en_tablero (char tablero [11][11], int x, int y, char m)  {
  int tirada_ok;
  tirada_ok = 2;
  if (tablero [x][y] == ' ') {
  	if (m == 'M') {
  	  tablero [x][y] = 'M'; /* tiro fallido con mina */
      tirada_ok = 0;
	}
  	else {
      tablero [x][y] = '0'; /* tiro fallido con disparo*/
      tirada_ok = 0;
    }
  }
  else {
    if (tablero [x][y] == 'P' || tablero [x][y] == 'A' || tablero [x][y] == 'C' || tablero [x][y] == 'S' || tablero [x][y] == 'D') {
      tablero [x][y] = 'X'; /* tiro acertado */
      tirada_ok = 1;
    }
    else {
      if (tablero [x][y] == '0' || 'X' || 'M')  {
        tirada_ok = 2;
      }
    }
  }
  return tirada_ok;
}

int Verifica_jugador_ganador (char tablero1 [11][11], char tablero2 [11][11]) {
  int ganador, mis_barcos1, mis_barcos2, i, j;
  ganador = 0;
  mis_barcos1 = 0;
  mis_barcos2 = 0;
  for (i = 1; i < 11; i++) {
    for (j = 1; j < 11; j++) {
      if (tablero1 [i][j] == 'P' || tablero1 [i][j] == 'A' || tablero1 [i][j] == 'C' || tablero1 [i][j] == 'S' || tablero1 [i][j] == 'D') {
        mis_barcos1++;
      }
    }
  }
  for (i = 1; i < 11; i++) {
    for (j = 1; j < 11; j++) {
      if (tablero2 [i][j] == 'P' || tablero2 [i][j] == 'A' || tablero2 [i][j] == 'C' || tablero2 [i][j] == 'S' || tablero2 [i][j] == 'D') {
        mis_barcos2++;
      }
    }
  }
  if (mis_barcos1 == mis_barcos2) {
    ganador = 0;
  }
  else {
    if (mis_barcos1 > mis_barcos2) {
      ganador = 1;
    }
    else {
      ganador = 2;
    }
  }
  return ganador;
}

char Elejir_disparo_o_mina (int player, char name [10]) {
  char disparo;
  printf ("\nJugador %d %s, digite seleccionar disparo (D) o empujar mina (M): ", player, name);
  do 
    disparo = getchar ();
  while (disparo != 'd' && disparo != 'm' && disparo != 'D' && disparo != 'M');
  if (disparo == 'd') {
    disparo = 'D';
  }
  else {
    if (disparo == 'm') {
      disparo = 'M';
    }
  }
  return disparo;
}

int Captura_coordenada_x_mina (int player, char name [10]) {
  char X;
  int x;
  printf ("\nColocando mina del jugador %d %s. Digite coordenada x de su objetivo (A-J) o 0 para abandonar): ", player, name);
  do 
    X = getchar ();
  while (X != 'a' && X != 'b' && X != 'c' && X != 'd' && X != 'e' && X != 'f' && X != 'g' && X != 'h' && X != 'i' && X != 'j' &&
         X != 'A' && X != 'B' && X != 'C' && X != 'D' && X != 'E' && X != 'F' && X != 'G' && X != 'H' && X != 'I' && X != 'J' && X != '0');
  x = Convierte_x_a_num (X);
  return x;
}

int Captura_coordenada_y_mina (int player, char name [10] ) {
  int y;
  printf ("\nColocando mina del jugador %d %s. Digite coordenada y de su objetivo (1-10) o 0 para abandonar): ", player, name);
  do 
    scanf ("%d", &y);
  while (y < 0 || y > 10);
  return y;
}

int Mueve_mina_aleatoriamente (char tablero [11][11], int player) {
  printf ("\nMueve_mina_aleatoriamente");
  int i, j, xy;
  xy = 0;
  for (i = 1; i < 11; i++) {
  	for (j = 1; j < 11; j++) {
  	  if (tablero [i][j] == 'M') {/* mina localizada */
  	    printf ("\nMina localizada %d %d", i, j);
  	    tablero [i][j] = '0'; /* marcar el espacio de la mina como ya utilizado */
  	    if (i == 1) {/* sin espacio libre hacia arriba */
  	      printf ("\nSin espacio arriba");
  	    }
  	    else {
  	      if (tablero [i - 1] [j] == ' ' || tablero [i - 1] [j] == 'P' || tablero [i - 1] [j] == 'A' || tablero [i - 1] [j] == 'C' 
  	        || tablero [i - 1] [j] == 'S' || tablero [i - 1] [j] == 'D')	{/* verificando espacio libre hacia arriba */
  	        xy = (i - 1) * 100 + j;
  	        printf ("\nCon espacio arriba %d", xy);
  	      }
  	    }
  	    if (xy == 0) { /* verificando espacio libre hacia abajo */
	      if (i == 10) {/* sin espacio libre hacia abajo */
	        printf ("\nSin espacio abajo");
  	      }
  	      else {
		    if (tablero [i + 1][j] == ' ' || tablero [i + 1][j] == 'P' || tablero [i + 1][j] == 'A' || tablero [i + 1][j] == 'C'
			  || tablero [i + 1][j] == 'S' || tablero [i + 1][j] == 'D') {/* verificando espacio libre hacia abajo */
			  xy = (i + 1) * 100 + j;
			  printf ("\nCon espacio abajo %d", xy);
		    }
		  }
  	    } 
		if (xy == 0) { /* verificando espacio libre a la izquierda */
	      if (j == 1) {/* sin espacio libre a la izquierda */
	        printf ("\nSin espacio a la izquierda");
  	      }
  	      else {
		    if (tablero [i][j - 1] == ' ' || tablero [i][j - 1] == 'P' || tablero [i][j - 1] == 'A' || tablero [i][j - 1] == 'C'
			  || tablero [i][j - 1] == 'S' || tablero [i][j - 1] == 'D')  {/* verificando espacio libre a la izquierda */
			  xy = i * 100 + j - 1;
			  printf ("\nCon espacio a la izquierda %d", xy);
		    }
		  }
  	    } 
		if (xy == 0) { /* verificando espacio libre a la derecha */
	      if (j == 10) {/* sin espacio libre a la derecha */
	        printf ("\nSin espacio a la derecha");
  	      }
  	      else {
		    if (tablero [i][j + 1] == ' ' || tablero [i][j + 1] == 'P' || tablero [i][j + 1] == 'A' || tablero [i][j + 1] == 'C'
			  || tablero [i][j + 1] == 'S' || tablero [i][j + 1] == 'D') {/* verificando espacio libre a la derecha */
			  xy = i * 100 + j + 1;
			  printf ("\nCon espacio a la derecha %d", xy);
		    }
		  }
  	    }  
  	    i = 11;
  	    j = 11;
  	  }
    }
  }
  if (xy == 0) {/* sin espacio disponible arriba, abajo, a la izquierda y a la derecha */
    xy = 888; /* destruir la  mina */
  }
  return xy;
}

char Desplegar_ayuda () {
	
  int c, n;
  n = 0;
  FILE * ptr;
  char archivo [25] = "Battle_ships_help.txt";
  if ((ptr = fopen (archivo, "rt")) == NULL) { /*modalidad lectura texto */
    printf ("Falla al abrir el archivo");
    return 1;
  }
  while ((c = fgetc (ptr)) != EOF) {
    if (c == '\n') {
      printf ("\n");
	}
	else {
	  putchar (c);
	}
  }
  fclose (ptr);
  return 0;
}

char Inicializa_nombre_jugador (char name [10]) {
  int i;
  for (i = 0; i < 10; i++) {
  	name [i] = ' ';
  }
}


