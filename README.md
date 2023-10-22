# parte-01-parcial-spd
// C++ code
// Mauricio Solis del Castillo

#define A 13
#define B 12
#define C 11
#define D 10
#define E 9
#define F 8
#define G 7
#define SUBE 3
#define BAJA 2
#define RESET 4
#define UNIDAD A4
#define DECENA A5
#define APAGADOS 0
#define TIMEDISPLAYON 5

int contadorDigito = 0;
int sube = 1;
int subePrevia = 1;
int baja = 1;
int bajaPrevia = 1;
int reset = 1;
int resetPrevia = 1;

void setup()
{
  pinMode(BAJA, INPUT_PULLUP);
  pinMode(SUBE, INPUT_PULLUP);
  pinMode(RESET, INPUT_PULLUP);
  pinMode(C, OUTPUT);
  pinMode(D, OUTPUT);
  pinMode(E, OUTPUT);
  pinMode(G, OUTPUT);
  pinMode(F, OUTPUT);
  pinMode(A, OUTPUT);
  pinMode(B, OUTPUT);
  pinMode(UNIDAD, OUTPUT);
  pinMode(DECENA, OUTPUT);
  digitalWrite(UNIDAD, 0);
  digitalWrite(DECENA, 0);
  imprimirDigito(0);
}

void loop()
{
  int boton = botonPresionado();
  if (boton == SUBE)
  {
    contadorDigito++;
    if (contadorDigito > 99)
    {
      contadorDigito = 0;
    } 
  } 
  else if (boton == BAJA)
  {
    contadorDigito--;
    if (contadorDigito < 0)
    {
      contadorDigito = 99;
    } 
  }
  else if (boton == RESET)
  {
    contadorDigito = 0;
  } 
  imprimirCuenta(contadorDigito);
}

int botonPresionado(void) // CHEQUEA SI SE PRESIONO UNA TECLA Y SI LA TECLA NO ESTABA YA APRETADA
{
	sube = digitalRead(SUBE);
  baja = digitalRead(BAJA);
  reset = digitalRead(RESET);
  
  if (sube)  // Pregunto si la tecla no esta presionada
  {
    subePrevia = 1;
  }
  if (baja)
  {
    bajaPrevia = 1;
  }
  if (reset)
  {
    resetPrevia = 1;
  }
  if (sube == 0 && sube != subePrevia)
  {
    subePrevia = sube;
    return SUBE;
  }
  
  if (baja == 0 && baja != bajaPrevia)
  {
    bajaPrevia = baja;
    return BAJA;
  }

  if (reset == 0 && reset != resetPrevia)
  {
    resetPrevia = reset;
    return RESET;
  }
  return 0;
}

void prendeDigito(int digito)
{
	if (digito == UNIDAD)
  {
    digitalWrite(UNIDAD, LOW);
    digitalWrite(DECENA, HIGH);
    delay(TIMEDISPLAYON);
  }
  else if (digito == DECENA)
  {
    digitalWrite(UNIDAD, HIGH);
    digitalWrite(DECENA, LOW);
    delay(TIMEDISPLAYON);
  }
  else
  {
    digitalWrite(UNIDAD, HIGH);
    digitalWrite(DECENA, HIGH);
  }
}

void imprimirCuenta(int cuenta)
{
	prendeDigito(APAGADOS);
  imprimirDigito(cuenta / 10);
  prendeDigito(DECENA);
  prendeDigito(APAGADOS);
  imprimirDigito(cuenta - 10 * ((int)cuenta / 10));
 	prendeDigito(UNIDAD); 
}

void imprimirDigito(int digito)
{
	digitalWrite(A, LOW);
  digitalWrite(B, LOW);
  digitalWrite(C, LOW);
  digitalWrite(D, LOW);
  digitalWrite(E, LOW);
  digitalWrite(F, LOW);
  digitalWrite(G, LOW);
  
  switch (digito)
  {
    case 1:
    {
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      break;
    }
    case 2:
    {
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 3:
    {
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 4:
    {
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 5:
    {
      digitalWrite(A, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 6:
    {
      digitalWrite(A, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 7:
    {
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      break;
    }
    case 8:
    {
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 9:
    {
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(F, HIGH);
      digitalWrite(G, HIGH);
      break;
    }
    case 0:
    {
      digitalWrite(A, HIGH);
      digitalWrite(B, HIGH);
      digitalWrite(C, HIGH);
      digitalWrite(D, HIGH);
      digitalWrite(E, HIGH);
      digitalWrite(F, HIGH);
      break;
    }  	
  }   	
}

primero se definen todos los pin a usar y las variables enteras, en el setup()
se inician BAJA, SUBE, RESET que son los pines para los botones como imput_pullup
los pines para los display se inician como output y se escriben la unidad y la decena en 0 para empezar el bucle, en el loop() se verifica el estado de los botones para poder incrementar, disminuir o resetear el contador que se imprime en los displays con al funcion imprimirCuenta().

La funcion imprimirDigito() los que hace es, segun el numero recibido por el switch prender y apagar los diferentes segmentos del display para mostrar ese mismo numero.

La funcion botonPresionado() se lee el estado de los botones, si el boton no esta presionado se asigna 1 a subir, bajar o resetear (previa) para tener el estado previo del boton, entonces, si el boton esta presionado y el estado previo no es presionado, se asigna el estado del boton al estodo previo y se retorna el boton (el que se presiono), en caso no haber presionado ningun boton, se retorna 0 para que no se haga nada. Esta funcion sirve para tener control sobre los botones y no tomar el estado del boton si se mantiene apretado.

La funcion prendeDigito() se encarga de hacer la multiplexacion para poder tener prendido los dos display "a la vez", verificando si esta prendida la unidad, apagarla, prender la decena por un tiempo muy corto y viceversa, asi parece que estan prendidos los dos displays.

La funcion imprimirCuenta() lo que hace es realizar unas cuentas para poder imprimir el numero generado en el contador del loop(), recibe el numero del contador, lo divide en 10, y el resultado lo muestra en el display de la decena, seguido de eso, ese mismo numero le resta 10 y lo multiplica por el numero dividido en 10 para obtener la unidad y asi imprimirlo.  
