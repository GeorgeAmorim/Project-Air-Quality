# Project-Air-Quality
#include <Adafruit_NeoPixel.h>
#ifdef _AVR_
  #include <avr/power.h>
#endif
#define PIN        4
#define NUMPIXELS 1

Adafruit_NeoPixel pixels(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);
#define DELAYVAL 500

int buzzPin= 8;
int nivel_sensor = 340;

void setup() {
  pinMode(buzzPin, OUTPUT);
  Serial.begin(9600);
  #if defined(_AVR_ATtiny85_) && (F_CPU == 16000000)
    clock_prescale_set(clock_div_1);
  #endif

  pixels.begin();
}

void loop() {
  pixels.clear(); // desliga led
  digitalWrite(buzzPin, HIGH); // desliga buzzer
 
  int sensor = analogRead(0);
  if (sensor > nivel_sensor){
    allert();
    pixels.setPixelColor(0, pixels.Color(150, 0, 0));
    pixels.show();
  } else{
    pixels.setPixelColor(0, pixels.Color(0, 150, 0));
    pixels.show();
  }    

  Serial.println(sensor);
  delay(500);  
}

void allert(){
  digitalWrite(buzzPin, LOW); // Tone ON
  delay(500); // Tone length
  digitalWrite(buzzPin, HIGH); // Tone OFF
  delay(200); // Symbol pause
  return;
}
