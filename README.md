# projeto-pedra-papel-tesoura-arduino

#include <Servo.h>

const int TRIG_PIN   = 7;
const int ECHO_PIN   = 6;
const int SERVO_PIN  = 5;

Servo myServo;

const int DISTANCE_THRESHOLD = 15; // Detecta até 15 cm
long duration;
int distance;

void setup() {
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  
  myServo.attach(SERVO_PIN);
  
  // No servo 360°, o valor 90 é o comando para PARAR
  myServo.write(90); 
  
  Serial.begin(9600);
  Serial.println("--- Sistema Iniciado. Aguardando deteccao... ---");
}

void loop() {
  // Leitura do sensor ultrassônico
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);
  
  duration = pulseIn(ECHO_PIN, HIGH);
  distance = duration * 0.034 / 2;

  // Se detectar algo na primeira leitura dentro de 15 cm
  if (distance > 0 && distance <= DISTANCE_THRESHOLD) {
    // Salva e exibe a PRIMEIRA distância registrada
    int primeiraDistancia = distance;

    Serial.println("\n------------------------------------");
    Serial.print(" PRIMEIRA DETECCAO REGISTRADA: ");
    Serial.print(primeiraDistancia);
    Serial.println(" cm");
    Serial.println("------------------------------------");
    Serial.println("Iniciando giro do motor...");

    // Aciona o motor
    myServo.write(180); 
    
    // Gira durante 2 segundos
    delay(2000); 

    // Para o motor
    myServo.write(90); 

    Serial.print("Giro concluido para a deteccao de ");
    Serial.print(primeiraDistancia);
    Serial.println(" cm.");
    
    Serial.println("Cooldown de 3 segundos iniciado...");
    
    // Pausa de 3 segundos ignorando novas leituras
    delay(3000); 
    
    Serial.println("\nPronto para uma nova deteccao!");
  }
  
  delay(100);
}
