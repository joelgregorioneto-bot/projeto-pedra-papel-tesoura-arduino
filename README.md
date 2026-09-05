# projeto-pedra-papel-tesoura-arduino

#include <Servo.h>

const int TRIG_PIN = 7, ECHO_PIN = 6, SERVO_PIN = 5, DIST_LIMITE = 15;
Servo myServo;

void setup() {
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  myServo.attach(SERVO_PIN);
  myServo.write(90); // 90 = parado (servo 360°)
  Serial.begin(9600);
  Serial.println("--- Sistema Iniciado. Aguardando deteccao... ---");
}

long lerDistancia() {
  digitalWrite(TRIG_PIN, LOW); delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH); delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);
  return pulseIn(ECHO_PIN, HIGH) * 0.034 / 2;
}

void loop() {
  int distancia = lerDistancia();

  if (distancia > 0 && distancia <= DIST_LIMITE) {
    Serial.print("\nPRIMEIRA DETECCAO: "); Serial.print(distancia); Serial.println(" cm");
    Serial.println("Girando motor...");

    myServo.write(180);
    delay(2000);
    myServo.write(90);

    Serial.println("Giro concluido. Cooldown de 3s...");
    delay(3000);
    Serial.println("Pronto para nova deteccao!");
  }
  delay(100);
}
    Serial.println("\nPronto para uma nova deteccao!");
  }
  
  delay(100);
}
