#include <Servo.h>

// Definindo os pinos e o limite de distância (em cm)
const int TRIG_PIN = 7;
const int ECHO_PIN = 6;
const int SERVO_PIN = 5;
const int DIST_LIMITE = 15; 

Servo myServo;

void setup() { 
  pinMode(TRIG_PIN, OUTPUT); 
  pinMode(ECHO_PIN, INPUT); 
  
  myServo.attach(SERVO_PIN); 
  myServo.write(90); // 90 = parado (para servos 360°) 
  
  Serial.begin(9600); 
  Serial.println("--- Sistema Iniciado. Aguardando deteccao... ---"); 
}

long lerDistancia() { 
  digitalWrite(TRIG_PIN, LOW); 
  delayMicroseconds(2); 
  digitalWrite(TRIG_PIN, HIGH); 
  delayMicroseconds(10); 
  digitalWrite(TRIG_PIN, LOW); 
  
  return pulseIn(ECHO_PIN, HIGH) * 0.034 / 2; 
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
