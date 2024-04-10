//----------------------------------------Including the libraries.
#include "WiFi.h"
#include <HTTPClient.h>
#include <ESP32Servo.h>
//----------------------------------------
//defining trans and rev pins
#define RXp2 16
#define TXp2 17
#define ser_pin 13
Servo myServo;
// #define fsr_pin 15
// int FSR;

//----------------------------------------SSID and PASSWORD of your WiFi network.
const char* WIFI_ssid     = "VITC-PGM";
const char* WIFI_password = "Pgm%$0910";
//----------------------------------------

// Google script Web_App_URL.
String Web_App_URL = "https://script.google.com/macros/s/AKfycby9NB868qTL4MmH1Rw9B-urIkQMh1b7oAPOL6AIR2QTx8LvhpwqfyoKZRiReDLfv8WG/exec";

String Status_Read_Sensor = "";
String Status_Read_Sensor1 ="";
String Status_Read_Sensor2="";
// float Temp;
// int Humd;
// String Switch_1_State = "";
// String Switch_2_State = "";


//__getValue()
// String function to process the data (Split String).
// I got this from : https://www.electroniclinic.com/reyax-lora-based-multiple-sensors-monitoring-using-arduino/
String getValue(String data, char separator, int index) {
  int found = 0;
  int strIndex[] = { 0, -1 };
  int maxIndex = data.length() - 1;
  
  for (int i = 0; i <= maxIndex && found <= index; i++) {
    if (data.charAt(i) == separator || i == maxIndex) {
      found++;
      strIndex[0] = strIndex[1] + 1;
      strIndex[1] = (i == maxIndex) ? i+1 : i;
    }
  }
  return found > index ? data.substring(strIndex[0], strIndex[1]) : "";
}
//________________________________________________________________________________ 

//__VOID SETUP()
void setup() {
  // put your setup code here, to run once:
  myServo.attach(ser_pin);
  Serial.begin(115200);
  //setting up UART communication
  Serial2.begin(9600, SERIAL_8N1, RXp2, TXp2);
  // pinMode(fsr_pin,INPUT);
  myServo.write(0);
  //clear old config
  WiFi.disconnect(true);        //disable wifi station 
  WiFi.softAPdisconnect(true);  //disable wifi access point 

  pinMode(2, OUTPUT);           // Inbuilt LED light
  delay(1000);                  // Some delay
  ///////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
  Serial.println("PROJECT NAME      : ESP32 WIFI STATION");
  Serial.print  ("WIFI SSID         : ");
  Serial.println(WIFI_ssid);
  Serial.print  ("WIFI PASSWORD     : ");
  Serial.println(WIFI_password);
  //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
  digitalWrite(2,HIGH);
  delay(500);
  digitalWrite(2,LOW);

  connectWiFi();

  while ((!(WiFi.status() == WL_CONNECTED)))
  {
    connectWiFi();
  }
  
  Serial.println("Get data from");
  Serial.println("Google Sheets");
  delay(1000);
  Serial.println("Please wait...");
  delay(1000);
    if (WiFi.status() == WL_CONNECTED) {
    // digitalWrite(On_Board_LED_PIN, HIGH);

    // Create a URL for reading or getting data from Google Sheets.
    String Read_Data_URL = Web_App_URL + "?sts=read";

    Serial.println();
    Serial.println("-------------");
    Serial.println("Read data from Google Spreadsheet...");
    Serial.print("URL : ");
    Serial.println(Read_Data_URL);

    //::::::::::::::::::The process of reading or getting data from Google Sheets.
      // Initialize HTTPClient as "http".
      HTTPClient http;

      // HTTP GET Request.
      http.begin(Read_Data_URL.c_str());
      http.setFollowRedirects(HTTPC_STRICT_FOLLOW_REDIRECTS);

      // Gets the HTTP status code.
      int httpCode = http.GET(); 
      Serial.print("HTTP Status Code : ");
      Serial.println(httpCode);
  
      // Getting response from google sheet.
      String payload;
      if (httpCode > 0) {
        payload = http.getString();
        Serial.println("Payload : " + payload);  
      }
  
      http.end();
    //::::::::::::::::::
    
    // digitalWrite(On_Board_LED_PIN, LOW);
    Serial.println("-------------");

    //::::::::::::::::::Conditions that are executed if reading or getting data from Google Sheets is successful (HTTP Status Codes : 200).
    if (httpCode == 200) {
      // The process of separating data that is read or obtained from Google Sheets.
      Status_Read_Sensor = getValue(payload, ',', 0);
      Status_Read_Sensor1= getValue(payload,',',1);
      Status_Read_Sensor2= getValue(payload,',',2);
      Serial.println(Status_Read_Sensor);
      Serial.println(Status_Read_Sensor1);
      Serial.println( Status_Read_Sensor2);
     }
    //::::::::::::::::::
  }
}
//________________________________________________________________________________ 

//__VOID LOOP()
void loop() {
  // put your main code here, to run repeatedly:

  //----------------------------------------Conditions that are executed when WiFi is connected.
  // This condition is the condition for reading or getting data from Google Sheets.
    Serial.println("Message Received: ");
    String UID=Serial2.readString();
    //Serial.println(UID);
    String FSR=UID.substring(5,10);
    FSR.trim();
    int data=FSR.toInt();
    //Serial.println(data);
    int a =UID.indexOf(Status_Read_Sensor);
    int b=UID.indexOf(Status_Read_Sensor1);
    int c=UID.indexOf(Status_Read_Sensor2);
    //String myarr[3]={Status_Read_Sensor,Status_Read_Sensor2,Status_Read_Sensor1};
    int myarr[3]={a,b,c};
    int d=0;
    for(int i=0;i<3;i++)
    {
      if(myarr[i]==-1)
      {
        d+=1;
      }
      // else
      // {
      //   break;
      // }
    }

    if(d==2)
    {
      Serial.println("Access Granted");
      myServo.write(45);
      data=1;
      delay(5000);
    }
    else
    {
      Serial.println("Unauthorized Access");
    }
    if(data>=75 && data<1000)
    {
      myServo.write(45);
    }
    else 
    {
      delay(1000);
      myServo.write(0);
    }
  delay(1000);
  //----------------------------------------
}
void connectWiFi()
{ 
  Serial.println("Connecting to WIFi....");
  int i=0;

  WiFi.mode(WIFI_STA);
  // WiFi.config(WIFI_local_IP, WIFI_gateway, WIFI_subnet, WIFI_primaryDNS, WIFI_secondaryDNS);
  WiFi.begin(WIFI_ssid, WIFI_password);
  
  while ((!(WiFi.status() == WL_CONNECTED)))
  {
    digitalWrite(2,HIGH);
    delay(300);
    digitalWrite(2,LOW);
    delay(200);
    
    Serial.println(" - ");
    i++;
    if(i > 10)    
    { 
      return;
    }
  }

  digitalWrite(2,HIGH);
  delay(400);
  digitalWrite(2,LOW);

  Serial.println("WIFI connected");

  Serial.print("WIFI IP ADDRESS       : ");
  Serial.println(WiFi.localIP());

  Serial.print("WIFI Strength         : ");
  Serial.println(WiFi.RSSI());

  Serial.print("ESP32 MAC address     : ");
  Serial.println(WiFi.macAddress());
}
