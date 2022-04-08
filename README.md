# Alfiya
#include <Wire.h> 
#include <Servo.h> 
#include <LiquidCrystal_I2C.h>  
 
#include <LiquidCrystal_I2C.h> // Подключение библиотеки 
 
//#include <LiquidCrystal_PCF8574.h> // Подключение альтернативной библиотеки 
 
LiquidCrystal_I2C lcd(0x27,16,2); // Указываем I2C адрес (наиболее распространенное значение), а также параметры экрана (в случае LCD 1602 - 2 строки по 16 символов в каждой  
//LiquidCrystal_PCF8574 lcd(0x27); // Вариант для библиотеки PCF8574  
Servo servo; 
int trigPin = 11;    //Триггер – зеленый проводник 
 
int echoPin = 12;    //Эхо – желтый проводник 
long duration, cm, inches; 
 
void setup() 
{ 
  Serial.begin (9600); 
  pinMode(trigPin, OUTPUT); 
  pinMode(echoPin, INPUT); 
 
  servo.attach(7); 
  lcd.init();                      // Инициализация дисплея   
  lcd.backlight();                 // Подключение подсветки 
  lcd.setCursor(0,0);              // Установка курсора в начало первой строки 
  lcd.print("Wash your hands");       // Набор текста на первой строке 
  lcd.setCursor(0,1);              // Установка курсора в начало второй строки 
  
} 
void loop(){ 
  digitalWrite(trigPin, LOW); 
 
  delayMicroseconds(5); 
 
  digitalWrite(trigPin, HIGH); 
   
  delayMicroseconds(10); 
   
  digitalWrite(trigPin, LOW); 
 
// Считываем данные с ультразвукового датчика: значение HIGH, которое 
 
// зависит от длительности (в микросекундах) между отправкой 
 
// акустической волны и ее обратном приеме на эхолокаторе. 
   
  pinMode(echoPin, INPUT); 
 
  duration = pulseIn(echoPin, HIGH); 
 
// преобразование времени в расстояние 
 
  cm = (duration/2) / 29.1; 
  if(cm > 15){ 
    servo.write(0); 
  } else{ 
    servo.write(180); 
  } 
   
}
