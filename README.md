#include <Arduino.h>
#include <TM1637Display.h>
#include <Servo.h>

// Pinos do Display TM1637
#define CLK 3
#define DIO 2

// Pinos do Sensor Ultrassónico e Servo
const int TRIG_PIN = 7;
const int ECHO_PIN = 6;
const int SERVO_PIN = 5;
const int DIST_LIMITE = 15;

TM1637Display display(CLK, DIO);
Servo myServo;

long lerDistancia() {
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);
  
  return pulseIn(ECHO_PIN, HIGH) * 0.034 / 2;
}

void setup() {
  // Configuração do Display
  display.setBrightness(7); // Brilho máximo
  display.showNumberDec(67, false); // Acende o display a mostrar "67"

  // Configuração do Sensor e Servo
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  
  myServo.attach(SERVO_PIN);
  myServo.write(90); // Servo 360° parado

  Serial.begin(9600);
  Serial.println("--- Sistema Iniciado. Display em 67. Aguardando deteccao... ---");
}

void loop() {
  int distancia = lerDistancia();

  if (distancia > 0 && distancia <= DIST_LIMITE) {
    Serial.print("\nPRIMEIRA DETECCAO: ");
    Serial.print(distancia);
    Serial.println(" cm");
    Serial.println("Girando motor...");

    myServo.write(180); // Liga o motor
    delay(2000);        // Gira por 2 segundos
    myServo.write(90);  // Para o motor

    Serial.println("Giro concluido. Cooldown de 3s...");
    delay(3000);
    Serial.println("Pronto para nova deteccao!");
  }

  delay(100); // Pequena pausa entre leituras do sensor
}
