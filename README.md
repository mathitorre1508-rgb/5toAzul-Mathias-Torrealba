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

<img width="521" height="317" alt="image" src="https://github.com/user-attachments/assets/9737c1b7-218b-41d2-b63b-bfa6e618947c" />
<img width="1936" height="2528" alt="image" src="https://github.com/user-attachments/assets/b972cc76-881a-4884-b7d4-ac564f16b9d0" />

# Actividad de Robotica del problema a resolver
Mi problema es los robos en las tiendas y lo resolvi con un sistema de un sensor de movimiento que en las nochesve si hay moviemiento, y si lo hay prende la alarma

// 1. Declaramos los pines según nuestras conexiones
const int pinBoton = 2;   // El botón que simula "ya pasó la hora"
const int pinPIR = 3;     // El sensor de movimiento
const int pinLED = 4;     // La alarma visual
const int pinPiezo = 5;   // La alarma sonora

// 2. Variable para saber si el sistema está armado o no
bool sistemaArmado = false; 

void setup() {
  Serial.begin(9600); // Iniciamos el monitor serial para ver mensajes
  
  // Configuramos los pines
  pinMode(pinBoton, INPUT_PULLUP); // ¡Aquí usamos la magia del Pullup!
  pinMode(pinPIR, INPUT);
  pinMode(pinLED, OUTPUT);
  pinMode(pinPiezo, OUTPUT);
  
  Serial.println("Sistema listo. Esperando que pase 1 hora (presiona el boton)...");
}

void loop() {
  // --- LEER ENTRADA ---
  // Leemos el botón. Como usamos PULLUP, presionado es LOW (0)
  int estadoBoton = digitalRead(pinBoton);
  
  // Si el botón se presiona, "pasa la hora" y armamos el sistema
  if (estadoBoton == LOW) {
    sistemaArmado = true;
    Serial.println("¡Ha pasado 1 hora! Sistema ARMADO.");
    delay(500); // Pequeña pausa para que no se repita mil veces el mensaje
  }

  // --- PROCESAR CONDICIÓN Y GENERAR RESPUESTA ---
  // Si el sistema está armado, revisamos el sensor PIR
  if (sistemaArmado == true) {
    
    int movimiento = digitalRead(pinPIR); // Leemos el PIR
    
    if (movimiento == HIGH) {
      // ¡Hay gente en la tienda! ¡Alarma!
      digitalWrite(pinLED, HIGH);       // Encendemos el LED
      tone(pinPiezo, 1000);             // Encendemos el Piezo a 1000Hz (sonido agudo)
      Serial.println("🚨 ¡ALERTA! Movimiento detectado en el negocio. 🚨");
    } 
    else {
      // No hay movimiento
      digitalWrite(pinLED, LOW);        // Apagamos el LED
      noTone(pinPiezo);                 // Apagamos el sonido
      Serial.println("Sistema armado. Todo tranquilo. Sin movimiento.");
    }
  } 
  else {
    // Si el sistema no está armado, nos aseguramos de que todo esté apagado
    digitalWrite(pinLED, LOW);
    noTone(pinPiezo);
  }
  
  delay(200); // Una pequeña espera para que la simulación corra suave
}

https://www.tinkercad.com/things/1VVVUxsVzAx-actividad-robotica-4/editel?returnTo=https%3A%2F%2Fwww.tinkercad.com%2Fdashboard&sharecode=19MP_GYtnMz3lER7f3TItQ5zfiaf8NK4v-La3POHMO4

<img width="533" height="353" alt="Captura de pantalla 2026-10-15 221818" src="https://github.com/user-attachments/assets/cb1e2356-a193-4d4f-8755-11f555116788" />

