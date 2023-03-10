#include <Arduino.h>
#include <LiquidCrystal.h>
#include "DHT.h"
#define DHTPIN 22
#define DHTTYPE DHT22   // DHT 22  (AM2302), AM2321
DHT dht(DHTPIN, DHTTYPE);
unsigned long previousMillis_dht22 = 0;
const long interval_dht22 = 500;
//เวลา
int seconds = 0;
int minutes = 0;
int hours =   0;
int days = 1;
int V = 0; //กำหนดค่าตัวแปร ของลูกศร ถ้าเป็น 0 จะชี้ที่ตัวแปรตัวแรก , ถ้าเป็น 1 จะชี้ที่เครื่องหมาย + - * % , ถ้าเป็น 2 จะชี้ที่ตัวแปรตัวที่ 2
int A = 0; //กำหนดค่าตัวแปร ตัวแรก
int B = 0; //กำหนดค่าตัวแปร ตัวที่สอง
int C; //ประกาศตัวแปรของผลลัพธ์

bool timerRun=HIGH;
unsigned long previousMillis = 0;
const long interval = 1000;


const int pin_RS = 8;
const int pin_EN = 9;
const int pin_d4 = 4;
const int pin_d5 = 5;
const int pin_d6 = 6;
const int pin_d7 = 7;
const int pin_BL = 10;
LiquidCrystal lcd( pin_RS,  pin_EN,  pin_d4,  pin_d5,  pin_d6,  pin_d7);
int lcd_key     = 0;
int adc_key_in  = 0;
#define btnRIGHT  0
#define btnUP     1
#define btnDOWN   2
#define btnLEFT   3
#define btnSELECT 4
#define btnNONE   5

int read_LCD_buttons()
{
 adc_key_in = analogRead(0);      
  
 if (adc_key_in > 1500) return btnNONE;  
 if (adc_key_in < 50)   return btnRIGHT;  
 if (adc_key_in < 195)  return btnUP;    
 if (adc_key_in < 380)  return btnDOWN;  
 if (adc_key_in < 500)  return btnLEFT;  
 if (adc_key_in < 800)  return btnSELECT;
 return btnNONE; 
}

//mode
int mode = 0;

int modeButton = 26;
int modeButtonstate = 0;
int LastmodeBottonState =0;

//mode ตั้งเวลา
bool timeOn_1 = LOW;
int Displaymode = 0;

//mode_1
bool modeOn_1 = LOW;
int Displaymode_1 = 0;

int seconds_1setup = 0;
int minutes_1setup = 0;
int hours_1setup = 6;

int seconds_2setup = 0;
int minutes_2setup = 0;
int hours_2setup = 12;

int seconds_3setup = 0;
int minutes_3setup = 0;
int hours_3setup = 18;

int seconds_time_setup = 0;
int minutes_time_setup = 10;
int hours_time_setup =   0;

//mode_2
bool modeOn_2 = LOW;


void setup(){
 lcd.begin(16, 2);

 lcd.setCursor(1,1);        
 lcd.print("^");  
}

void loop(){
  dht22();
  set_time();
  set_mode();

 }
void set_time() {

unsigned long currentMillis = millis();
 if (currentMillis - previousMillis >= interval) {
     previousMillis = currentMillis;
  if (timerRun) {
    seconds++;
    if (seconds == 60) {
      minutes++;
      seconds = 0;
    }
    if (minutes == 60) {
      hours++;
      minutes = 0;
    }
    if (hours == 24){
      hours =0;
    }
   
  }
 }


lcd.setCursor(0, 0);
    if (hours < 10) {
      lcd.print("0");
    }
    lcd.print(hours);

   lcd.print(":");

    if (minutes < 10) {
      lcd.print("0");
    }
    
    lcd.print(minutes);
    lcd.setCursor(5, 0);
    lcd.print("|");
   lcd.setCursor(12, 0);
    
    lcd.print("Mode");
    
   

    

 lcd_key = read_LCD_buttons();      
 switch (lcd_key)  
 {
   case btnRIGHT:
     {
            V = (V + 1) % 3;            
            if (V == 0){
            lcd.setCursor(1,1);      
            lcd.print("^");          


            lcd.setCursor(4,1);      
            lcd.print(" ");          


            lcd.setCursor(13,1);      
            lcd.print(" ");          
            }


            else if(V==1){
                   
            lcd.setCursor(1,1);      
            lcd.print(" ");          

            lcd.setCursor(4,1);      
            lcd.print("^");          

            lcd.setCursor(13,1);      
            lcd.print(" ");          
            }

            else {
                      
            lcd.setCursor(1,1);      
            lcd.print(" ");          


           lcd.setCursor(4,1);      
            lcd.print(" ");          


            lcd.setCursor(13,1);      
            lcd.print("^");          
            }


            delay(500);
           break;
     }
   case btnLEFT:  
     {
          V--;                              
                                             
                                             
            if (V < 0)   { V = 2 ; }          


            if (V == 0){
            lcd.setCursor(1,1);              
            lcd.print("^");


            lcd.setCursor(4,1);
            lcd.print(" ");


            lcd.setCursor(13,1);
            lcd.print(" ");
            }


            else if(V==1){
            lcd.setCursor(1,1);
            lcd.print(" ");


            lcd.setCursor(4,1);
            lcd.print("^");


            lcd.setCursor(13,1);
            lcd.print(" ");
            }


            else {
            lcd.setCursor(1,1);
           lcd.print(" ");


            lcd.setCursor(4,1);
            lcd.print(" ");


            lcd.setCursor(13,1);
            lcd.print("^");
            }


            delay(500);
        break;
           
     }
   case btnUP:      
    {
        if (V == 0)
           
          
                {
                hours++;                          
                if (hours >= 24)   { hours = 24; }      
                           
                }
            else if(V==1)
                    
                {
                    minutes++;                          
                if (minutes >= 60)   { minutes = 60; }
                }

            else {
             
                    seconds++;                          
                if (seconds >= 60)   { seconds = 60; }
                }
            delay(500);

            
        break;
     }
   case btnDOWN:            
   {
      if (V == 0)
           
           
                {
                hours--;                          
                if (hours <= 0)   { hours = 0; }      
                    
                }
            else if(V==1)
           
           
                {
                    minutes--;                          
                if (minutes <= 0)   { minutes = 0; }
                }


            else {
             
                    seconds--;                          
                if (seconds <= 0)   { seconds = 0; }
                }
            delay(500);
    break;      
   }
   case btnSELECT:    {
      
        if (timerRun == LOW) {
        timerRun = HIGH;
      } else {
        timerRun = LOW;
      }

      
      delay(500);      
     break;                  
    
}
 }

}

void dht22(){
    unsigned long currentMillis = millis();
    if (currentMillis - previousMillis_dht22 >= interval_dht22) {
            previousMillis_dht22 = currentMillis;
            //delay(200);
            // Reading temperature or humidity takes about 250 milliseconds!
            // Sensor readings may also be up to 2 seconds 'old' (its a very slow sensor)
            float h = dht.readHumidity();
            // Read temperature as Celsius (the default)
            float t = dht.readTemperature();
            // Read temperature as Fahrenheit (isFahrenheit = true)
            float f = dht.readTemperature(true);


            // Check if any reads failed and exit early (to try again).
            if (isnan(h)  ,isnan(t) , isnan(f)) {
                        Serial.println(F("Failed to read from DHT sensor!"));
                        return;
                    }


            // Compute heat index in Fahrenheit (the default)
            float hif = dht.computeHeatIndex(f, h);
            // Compute heat index in Celsius (isFahreheit = false)
            float hic = dht.computeHeatIndex(t, h, false);


            Serial.print(F("Humidity: "));
            Serial.print(h);
            Serial.print(F("%  Temperature: "));
            Serial.print(t);
            Serial.print(F("°C "));
            Serial.print(f);
            Serial.print(F("°F  Heat index: "));
            Serial.print(hic);
            Serial.print(F("°C "));
            Serial.print(hif);
            Serial.println(F("°F"));


            
           

    }
    

}

void set_mode(){

      if (modeOn_1 == HIHG){




      }


}
