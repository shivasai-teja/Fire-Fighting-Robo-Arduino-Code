# Fire-Fighting-Robo-Arduino-Code
This code is used in our project fire-fighting robot using arduino c
#include <Servo.h>
#include <SoftwareSerial.h>

SoftwareSerial mySerial(12,11); //SIM800L Tx & Rx is connected to Arduino #12 & #11
Servo myservo;
String voice;

int fire1 = 6;
int fire2 = 7;
int fire3 = 8;

int motor = 9;
int k=1;
void setup()
{
  mySerial.begin(9600);
  Serial.begin(9600);
  myservo.attach(10);
  pinMode(fire1,INPUT);
  pinMode(fire2,INPUT);
  pinMode(fire3,INPUT);
  pinMode(motor,OUTPUT);
  pinMode(2, OUTPUT);
  pinMode(3, OUTPUT);
  pinMode(4, OUTPUT);
  pinMode(5, OUTPUT);
}

void loop() {
    fire();

  while (Serial.available())
  { 
  delay(10); 
  char c = Serial.read(); 
  if (c == '#') 
  {
    break;
  } 
  voice += c;
  }  
 
  if (voice.length() > 0)
  { Serial.println(voice); 
    if(voice == "front") 
    {
     digitalWrite(2, LOW);
     digitalWrite(3, HIGH);
     digitalWrite(4, LOW);
     digitalWrite(5, HIGH);
     delay(300);  
     digitalWrite(2, LOW);
     digitalWrite(3, LOW);
     digitalWrite(4, LOW);
     digitalWrite(5, LOW);
     delay(100);
      digitalWrite(2, LOW);
     digitalWrite(3, HIGH);
     digitalWrite(4, LOW);
     digitalWrite(5, HIGH);
     delay(300);
    } 
    if(voice == "back") 
    {
     digitalWrite(2, HIGH);
     digitalWrite(3, LOW);
     digitalWrite(4, HIGH);
     digitalWrite(5, LOW);
     delay(300);  
     digitalWrite(2, LOW);
     digitalWrite(3, LOW);
     digitalWrite(4, LOW);
     digitalWrite(5, LOW);
     delay(100);
      digitalWrite(2, HIGH);
     digitalWrite(3, LOW);
     digitalWrite(4, HIGH);
     digitalWrite(5, LOW);
     delay(300); 
    }  
    if(voice == "left") 
    {
     digitalWrite(2, LOW);
     digitalWrite(3, HIGH);
     digitalWrite(4, LOW);
     digitalWrite(5, LOW);
     delay(1000);  
     digitalWrite(2, LOW);
     digitalWrite(3, LOW);
     digitalWrite(4, LOW);
     digitalWrite(5, LOW);
     delay(100);
    }   
    if(voice == "right") 
    {
     digitalWrite(2, LOW);
     digitalWrite(3, LOW);
     digitalWrite(4, LOW);
     digitalWrite(5, HIGH);
     delay(1000);  
     digitalWrite(2, LOW);
     digitalWrite(3, LOW);
     digitalWrite(4, LOW);
     digitalWrite(5, LOW);
     delay(100);
    }
    if(voice == "stop") 
    {
     digitalWrite(2, LOW);
     digitalWrite(3, LOW);
     digitalWrite(4, LOW);
     digitalWrite(5, LOW);
     delay(100);
    }    
    voice="";
  }
}

void fire()
{
  myservo.write(90);
  delay(500);
  int x = digitalRead(fire1);
  int y = digitalRead(fire2);
  int z = digitalRead(fire3);
  Serial.print(x);
  Serial.print(y);
  Serial.println(z);
  if (x==1 && y==1 && z==1)
  {   
    digitalWrite(motor,HIGH); 
    delay(100);
  }
  else
  { 
    if (k==1)
    {
     message();
     k==0;
    }
     digitalWrite(2, LOW);
     digitalWrite(3, LOW);
     digitalWrite(4, LOW);
     digitalWrite(5, LOW);
     delay(100);
     
    digitalWrite(motor,LOW);
     delay(100);
    myservo.write(90);
    delay(2000);
    myservo.write(5);
    delay(2000);
    myservo.write(170);
    delay(2000);
    myservo.write(90);
    delay(2000);
    
  }
}


void message()
{
  
  mySerial.println("AT"); //Once the handshake test is successful, it will back to OK
  updateSerial();
  mySerial.println("AT+CMGF=1"); // Configuring TEXT mode
  updateSerial();
  mySerial.println("AT+CMGS=\"+917794035394\"");//change ZZ with country code and xxxxxxxxxxx with phone number to sms
  updateSerial();
  mySerial.print("Fire alert at your home"); //text content
  updateSerial();
  mySerial.write(26); 
  
}



void updateSerial()
{
  delay(500);
  while (Serial.available()) 
  {
    mySerial.write(Serial.read());//Forward what Serial received to Software Serial Port
  }
  while(mySerial.available()) 
  {
    Serial.write(mySerial.read());//Forward what Software Serial received to Serial Port
  }
}
