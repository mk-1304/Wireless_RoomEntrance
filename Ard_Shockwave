#include <SPI.h>
#include <PN532_SPI.h>
#include <PN532.h>
#include <NfcAdapter.h>

PN532_SPI interface(SPI,10);
NfcAdapter nfc = NfcAdapter(interface);
String tagId1 = "A2 22 19 20";
String tagId2= "24 BC E2 03";
String tagId = "None";
byte nuidPICC[4];
//String ldr="A5";
int FSR;
void setup() {
  Serial.begin(9600);        
  Serial.println('\n');
  pinMode(A5,INPUT);
  Serial.println("System initialized");

  nfc.begin();
//  digitalWrite(5, LOW);  
}


void loop() {

readNFC();
//if(tagId==tagId1){
//    digitalWrite(5,HIGH);
//}
//if(tagId==tagId2){
//    digitalWrite(5,LOW);
//  }


}
void readNFC() {

    FSR=analogRead(A5);
    
    Serial.println("FSR= ");
    Serial.println(FSR);
    
  if (nfc.tagPresent())
  {
    NfcTag tag = nfc.read();
    //tag.print();
    tagId = tag.getUidString();
    Serial.println("Tag id");
    Serial.println(tagId);
   
  }
  delay(1000);
}
