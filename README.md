#include <EEPROM.h>
#define EE_NILAI 0
#include <FS.h>
int button=D4;
int val;
int count=0;
int press;
int net=0;
int bnet=0;
int tot=0;
int x=0;
#define BLYNK_PRINT Serial
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#include <WiFiManager.h>

char blynk_token[34] = "1p6J2LIT0aO7-CCAQfH1oUlbliSf3evN";
//flag for saving data
bool shouldSaveConfig = false;



void setup() {

initialize();
Blynk.begin(blynk_token, WiFi.SSID().c_str(), WiFi.psk().c_str());
EEPROM.begin(512);
net = EEPROM_READ_LONG(EE_NILAI);
pinMode(button,INPUT);
Serial.begin(9600); 
 
}

// the loop function runs over and over again forever
void loop() {
Blynk.run(); 
  while(count<5){
  val=digitalRead(button);
  if(val==LOW){
    count++;
    delay(1000);
  }
  tot=bnet+count;
  Blynk.virtualWrite(V2, tot);
  Serial.println(count);
  delay(250);
  }

 Serial.println("Selesai");
 net++;
 Serial.println(net);
 EEPROM_WRITE_LONG(EE_NILAI,net);
 EEPROM.commit();
 bnet=net*5;
 Blynk.virtualWrite(V1, bnet);
 Serial.println(bnet);

 count=0;
}
