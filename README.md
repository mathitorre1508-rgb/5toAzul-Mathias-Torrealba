# 5toAzul-Mathias-Torrealba
Pensamiento Computacional
Clase del 24/09 creación del GitHub 

# Actividad arepa
1. La arepa que compres es 2 dolares mas barata que ka que comprastes antes
2. p= n*(11-n)
3. p=10*(11-10)=10$
4. p=(n*10)/n*(11-n)*100
5. sera distinto porque la formula le restas una variable que va dambiando por ende no puede ser disminuir el desvuento siemore un numero exacto

# user Tinkercad
Email: mathi.torre1508@gmail.com
user: Mathias Torrealba

# Codigo de Actividad# #3 y link a la actividad
// ===== PINES =====
const int pinPulsador    = 2;
const int pinInterruptor = 3;
const int pinLeds6       = 8;
const int pinLedVerde    = 9;
const int pinLedAzul     = 10;

// ===== VARIABLES 6 LEDs =====
bool sistemaActivo = false;
unsigned long inicioFase = 0;
bool faseParpadeo = true;      // true = parpadeando, false = apagado
unsigned long tiempoParpadeo = 0;
bool estadoLed6 = false;

const unsigned long DURACION_FASE = 8000;   // 8 segundos cada fase
const unsigned long VELOCIDAD_PARPADEO = 300; // Cambia cada 300 ms

// ===== VARIABLES 2 LEDs =====
unsigned long tiempoIntercalado = 0;
const unsigned long VELOCIDAD = 500;
bool toggle = false;

void setup() {
  pinMode(pinPulsador, INPUT_PULLUP);
  pinMode(pinInterruptor, INPUT);
  pinMode(pinLeds6, OUTPUT);
  pinMode(pinLedVerde, OUTPUT);
  pinMode(pinLedAzul, OUTPUT);

  digitalWrite(pinLeds6, LOW);
  digitalWrite(pinLedVerde, LOW);
  digitalWrite(pinLedAzul, LOW);
}

void loop() {
  // ===== PARTE 1: Pulsador arranca el ciclo de parpadeo =====
  int estadoPulsador = digitalRead(pinPulsador);

  if (estadoPulsador == LOW && !sistemaActivo) {
    sistemaActivo = true;
    inicioFase = millis();
    faseParpadeo = true;
    tiempoParpadeo = millis();
    estadoLed6 = true;
    digitalWrite(pinLeds6, HIGH);
  }

  if (sistemaActivo) {
    unsigned long tiempoActual = millis();

    // Cambio de fase cada 8 segundos
    if (tiempoActual - inicioFase >= DURACION_FASE) {
      inicioFase = tiempoActual;
      faseParpadeo = !faseParpadeo;

      if (!faseParpadeo) {
        // Empieza fase apagado
        digitalWrite(pinLeds6, LOW);
        estadoLed6 = false;
      } else {
        // Empieza fase parpadeo
        estadoLed6 = true;
        digitalWrite(pinLeds6, HIGH);
      }
    }

    // Durante la fase de parpadeo, alternar rápido
    if (faseParpadeo) {
      if (tiempoActual - tiempoParpadeo >= VELOCIDAD_PARPADEO) {
        tiempoParpadeo = tiempoActual;
        estadoLed6 = !estadoLed6;
        digitalWrite(pinLeds6, estadoLed6 ? HIGH : LOW);
      }
    }
  }

  // ===== PARTE 2: Interruptor deslizante alterna los 2 LEDs =====
  int estadoInterruptor = digitalRead(pinInterruptor);

  if (estadoInterruptor == HIGH) {
    if (millis() - tiempoIntercalado >= VELOCIDAD) {
      tiempoIntercalado = millis();
      toggle = !toggle;
      digitalWrite(pinLedVerde, toggle ? HIGH : LOW);
      digitalWrite(pinLedAzul, toggle ? LOW : HIGH);
    }
  } else {
    digitalWrite(pinLedVerde, LOW);
    digitalWrite(pinLedAzul, LOW);
  }
}

https://www.tinkercad.com/things/bEzW0rxxLcM-cool-bombul/editel?returnTo=https%3A%2F%2Fwww.tinkercad.com%2Fdashboard&sharecode=RA36k2pzXn7skN4pp9Bcrdy8sy3EGcTC2NEyWfLlsSM
