# HC-SR04 Ultrasonic Sensor

#include <HCSR04.h>

#include <HCSR04.h>

#include <HCSR04.h>

#define trigger 5
#define echo 4

HCSR04 distance (trigger, echo );

void setup(){
  Serial.begin(9600);
}

void loop(){
  float jarak = distance.dist();
  Serial.println(jarak);
  delay(1000);
}

# ServoWrite

#include <Servo.h>

Servo myservo;
int pos = 0;

void setup() {
  myservo.attach(16);
}

void loop(){
  for(pos = 0; pos < 180; pos += 1){
    myservo.write(pos);
    delay(10);
   }
   for (pos = 180; pos>=1; pos-=1){
    myservo.write(pos);
    delay(10);
   }
}

# Smart Parking

#define BLYNK_PRINT Serial


#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>

char auth[] = "YourAuthToken";

char ssid[] = "YourNetworkName";
char pass[] = "YourPassword";

WidgetLED led1(V1);

int sensor = D4;

void setup()
{
  Serial.begin(9600);
  Blynk.begin(auth, ssid, pass);
  pinMode(sensor,INPUT);
  while(Blynk.connect() == false);
  Serial.println("tidak terdeteksi");
}
void loop()
{
  int sensorval = digitalRead(sensor);
  delay(1000);
    if (sensorval == 1){
    led1.on();
  }
    if (sensorval == 0){
    led1.off();
  }
  Blynk.run();
}

# Blink

void setup() {
 
  pinMode(LED_BUILTIN, OUTPUT);
}

void loop() {
  digitalWrite(LED_BUILTIN, HIGH);   
  delay(1000);                      
  digitalWrite(LED_BUILTIN, LOW);    
  delay(1000);                      
}

# RFID

#include <SPI.h>
#include <MFRC522.h>

#define RST_PIN D1
#define SDA_PIN D2

MFRC522 mfrc522(SS_PIN, RST_PIN);

void setup() {
  Serial.begin(9600);
  SPI.begin();
  mfrc522.PCD_Init();
  Serial.println("Put your card to the reader...");

}

void loop() {
  if(!mfrc522.PICC_IsNewCardPresent()){
    return;
  }

  if(!mfrc522.PICC_ReadCardSerial()){
    return;
  }

  Serial.print("UID tag :");
  String content = "";
  byte letter;

  for(byte i = 0; i < mfrc522.uid.size; i++){
    Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " ");
    Serial.print(mfrc522.uid.uidByte[i], HEX);
    content.concat(String(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " "));
    content.concat(String(mfrc522.uid.uidByte[i], HEX));
  }

  Serial.println();
  Serial.print("Pesan : ");
  content.toUpperCase();

  if(content.substring(1) == "D9 DF 60 B9"){
    Serial.println("Kartu cocok");
    Serial.println();
    delay(1000);
  }

  else if(content.substring(1) == "79 D0 24 A3"){
    Serial.println("Kartu cocok");
    Serial.println();
    delay(1000);
  }

  else{
    Serial.println("Kartu Tidak cocok");
    delay(1000);
  }
