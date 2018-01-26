#include <Wire.h>
#include "Seeed_ws2812.h"

#define SIG_PIN 12
#define LEN_NUM 5

WS2812 strip = WS2812(LEN_NUM, SIG_PIN);

void setup() {
  strip.begin();
  Serial.begin(115200);   
}

void loop() {  
  strip.RGBCycle(1000);   
  strip.rainbowCycle(20);
}
