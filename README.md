The Aurdino Code for this project is given below:

 * https://srituhobby.com
 */ 
#include <Servo.h>

#include <LiquidCrystal_I2C.h>

#include <SPI.h>

#include <MFRC522.h>

#define SS_PIN 10

#define RST_PIN 9

#define LED_G 5 //define green LED pin

#define LED_R 4 //define red LED

#define LED_B 6

#define BUZZER 2 //buzzer pin

String UID1 = "73 B1 79 AC";

String UID2 = "C4 8C 44 64";

byte lock = 0;

Servo servo;

LiquidCrystal_I2C lcd(0x27, 16, 2);

MFRC522 rfid(SS_PIN, RST_PIN);

void setup() {
  Serial.begin(9600);
                                     

		servo.write(0);
  
	pinMode(LED_G, OUTPUT);

	pinMode(LED_R, OUTPUT);
  
  pinMode(LED_B, OUTPUT);
  
  pinMode(BUZZER, OUTPUT);
  
  noTone(BUZZER);
  
  lcd.init();
  
  lcd.backlight();
  
  servo.attach(3);
  
  SPI.begin();
  
  rfid.PCD_Init();

}

void loop() {

  lcd.setCursor(0, 0);
  
  lcd.print("Welcome! To HYD>");
  
  lcd.setCursor(1, 1);
  
  lcd.print("Put your card");
  
  if ( ! rfid.PICC_IsNewCardPresent())
  
    return;

  if ( ! rfid.PICC_ReadCardSerial())
  
    return;

  lcd.clear();
  
  lcd.setCursor(0, 0);
  
  lcd.print("Scanning");

                                          
  String ID = "";

for (byte i = 0; i < rfid.uid.size; i++) {

  lcd.print(".");
  
  ID.concat(String(rfid.uid.uidByte[i] < 0x10 ? " 0" : " "));
  
  ID.concat(String(rfid.uid.uidByte[i], HEX));
  
  delay(300);

}

ID.toUpperCase();

if (ID.substring(1) == UID1 && lock == 0 ) {

  digitalWrite(LED_G, HIGH);
  
  tone(BUZZER, 500);
  
  delay(300);
  
  noTone(BUZZER);
  
  digitalWrite(LED_G, LOW);
  
  lcd.clear();
  
  lcd.setCursor(0, 0);
  
  lcd.print("TS XX XX XXXX");
  
  lcd.setCursor(0, 1);
  
  lcd.print(" Access Granted!");
  
  Serial.print("vechile no: 'TS XX XX XXXX' PASSED!!\n");
  
  Serial.print("-----------------------------------------\n");
  
  delay(1500);
  
  servo.write(70);
  
  delay(3000);
  
  servo.write(0);
  
  lcd.clear();
  
  lock = 0;

}

                                                                

  else if(ID.substring(1) == UID2 && lock == 0 ) {
  
  digitalWrite(LED_G, HIGH);
  
  tone(BUZZER, 500);
  
  delay(300);
  
  noTone(BUZZER);
  
  digitalWrite(LED_G, LOW);
  
  lcd.clear();
  
  lcd.setCursor(0, 0);
  
  lcd.print("TS YY YY YYYY");
  
  lcd.setCursor(0, 1);
  
  lcd.print(" Access Granted!");
  
  Serial.print("vechile no: 'TS YY YY YYYY' PASSED!!\n");
  
  Serial.print("-----------------------------------------\n");
  
  delay(1500);
  
  servo.write(70);
  
  delay(3000);
  
  servo.write(0);
  
  lcd.clear();
  
  lock = 0;

} 

else {

  digitalWrite(LED_R, HIGH);
  
  tone(BUZZER, 300);
  
  delay(2000);
  
  digitalWrite(LED_R, LOW);
  
  noTone(BUZZER);
  
  lcd.clear();
  
  lcd.setCursor(0, 0);
  
  lcd.print("Insufficient");

                                                       

lcd.setCursor(0, 1);

    lcd.print("Balance!");
    
    Serial.print("vechile no: 'TS ZZ ZZ ZZZZ' NOT PASSED!!- NO BALANCE

!!\n");

    Serial.print("-----------------------------------------\n");
    
    delay(1500);
    
    lcd.clear();

} }
