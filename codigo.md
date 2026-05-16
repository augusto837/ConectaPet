CÓDIGOS UTILIZADOS NO PROJETO

1. Teste inicial de funcionamento ESP32
void setup() {
  Serial.begin(115200);
}

void loop() {
  Serial.println("ESP32 funcionando!");
  delay(1000);
}

2. Teste básico do servo para ver se aciona o movimento contínuo
#include <ESP32Servo.h>

Servo meuServo;

int pinoServo = 13;

void setup() {
  Serial.begin(115200);

  meuServo.setPeriodHertz(50);
  meuServo.attach(pinoServo, 500, 2400);
}

void loop() {
  meuServo.write(0);
  delay(2000);

  meuServo.write(90);
  delay(2000);

  meuServo.write(180);
  delay(2000);
}

3. Teste de abertura e fechamento para movimento controlado – com definição do grau de abertura
#include <ESP32Servo.h>

Servo meuServo;

int pinoServo = 13;

int posFechado = 20;      GRAU
int posAberto = 70;        GRAU

void setup() {
  Serial.begin(115200);

  meuServo.setPeriodHertz(50);
  meuServo.attach(pinoServo, 500, 2400);

  meuServo.write(posFechado);
  delay(2000);

  meuServo.write(posAberto);
  delay(2000);

  meuServo.write(posFechado);
}

void loop() {}

4. Código com controle de acionamento via Blynk
#define BLYNK_TEMPLATE_ID "TMPL2rjCcqNJL"
#define BLYNK_TEMPLATE_NAME "Alimentador PET"
#define BLYNK_AUTH_TOKEN "SEU_TOKEN"

#include <WiFi.h>
#include <BlynkSimpleEsp32.h>
#include <ESP32Servo.h>

char ssid[] = "WIFI";
char pass[] = "SENHA";

Servo meuServo;

int pinoServo = 13;

int posFechado = 20;
int posAberto = 70;
int tempoAberto = 1500;

void liberarRacao() {
  meuServo.write(posAberto);
  delay(tempoAberto);
  meuServo.write(posFechado);
}

BLYNK_WRITE(V0) {
  if (param.asInt() == 1) {
    liberarRacao();
  }
}

void setup() {
  Serial.begin(115200);

  meuServo.setPeriodHertz(50);
  meuServo.attach(pinoServo, 500, 2400);
  meuServo.write(posFechado);

  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);
}

void loop() {
  Blynk.run();
}
