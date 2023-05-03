// Подключение библиотек
#include <Servo.h>


//  Объявление адресации портов
byte flash_pin = A3; // Фотодатчик
byte serv_pin = 3;
byte photo_pin = 4;
byte power_pin = 2;

#define light 5; // Светодиод
#define LED_ON  150   // порог включенного светодиода
#define LED_OFF 800   // порог выключенного светодиода

// Объявление переменных
int wait = 500;
int step = 1;
int flash = 0;
int count = 0;

int open = 8; // Серво открыт 1 и 2 сброс
int open_half = 30; // Серво открыт 1 сброс
int close = 57; // Серво закрыт

Servo serv; // Объявление названия серво

void setup() 
{
  Serial.begin(9600); //инициализация связи с монитором порта

  //конфигурация входа контроллера и подключение внутреннего подтягивающего резистора
  pinMode(photo_pin, INPUT_PULLUP);
  pinMode(power_pin, OUTPUT); // Подключение цифрового пина к питанию

  digitalWrite(power_pin, HIGH);

  digitalWrite(flash_pin, HIGH); // включить подтягивающий резистор к входу A3

}

void loop() 
{
  
  flash = analogRead(flash_pin);
  Serial.println(flash);
  Serial.println("Step" + String(step));
  
  count = count + 1;
  
  if (count == 3)
  {
    digitalWrite(power_pin, HIGH);
    delay(30);
    digitalWrite(power_pin, LOW);
    delay(80);
    digitalWrite(power_pin, HIGH);
    count = 0;
  }
  
  switch (step) 
  {
    case 1: // Серво закрыт
      serv.attach(serv_pin); //Объявление подключения серво
      serv.write(close);
      delay(wait);
      if (flash < LED_ON)
      {
        step = 2;
      }
      break;
    case 2: // Серво открыто 1 сброс
      serv.write(open_half);
      delay(wait);
      serv.write(close);
      delay(wait);
      step = 3;
      break;
    case 3:
      serv.write(close);
      delay(wait);
      if (flash > LED_OFF)
      {
        step = 4;
      }
      break;
    case 4:
      serv.write(close);
      delay(wait);
      if (flash < LED_ON)
      {
        step = 5;
      }
      break;
    case 5: // Серво открыто 1 и 2 сброс
      serv.write(open);
      delay(wait);
      step = 6;
      break;
    case 6:
      serv.write(close);
      delay(wait); 
      serv.detach(); // Отключить серво  
      step = 7;
      break;         
    default:
      delay(wait);    
  }

}
