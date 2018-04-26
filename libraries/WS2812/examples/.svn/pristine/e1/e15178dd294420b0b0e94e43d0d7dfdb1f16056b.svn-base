/*
	Copyright (c) 2013 Rikard Lindstrom <ornotermes@gmail.com>

	This program is free software: you can redistribute it and/or modify
	it under the terms of the GNU General Public License as published by
	the Free Software Foundation, either version 3 of the License, or
	(at your option) any later version.

	This program is distributed in the hope that it will be useful,
	but WITHOUT ANY WARRANTY; without even the implied warranty of
	MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
	GNU General Public License for more details.

	You should have received a copy of the GNU General Public License
	along with this program.  If not, see <http://www.gnu.org/licenses/>.
*/

#include "Seeed_ws2812.h"
#include <arduino.h>
#include <Wire.h>

#define nop() __asm__ __volatile__ ("nop")
#define _DELAY_NOP(x) for(int i=0; i<x; i++){nop();}


WS2812::WS2812(uint32_t ledn, uint8_t pin) {
	ledNum = ledn;
	sigPin = pin;
	WS2812Buffer = (uint8_t *)malloc(ledNum * 3);
	
}
void WS2812::begin(void) {
	Wire.begin();
	pinMode(sigPin, OUTPUT);
	WS2812Clear();
	WS2812Send();
	
}
uint32_t WS2812::getLedNum(void) {
	return ledNum;
}
void WS2812::setLedNum(uint32_t lednum) {
	ledNum = lednum;
}
void WS2812::WS2812Clear(void) {
	int i;
	for(i = 0; i < ledNum*3; i++) WS2812Buffer[i] = 0;
	WS2812Send();
}
void WS2812::pureColor(uint8_t mode) {
	switch ( mode ) {
    case 0:
		for ( int i=0; i<ledNum; i++ ) {
			WS2812SetRGB(i, 0, 0, 255);
		}
		break;
    case 1:
		for ( int i=0; i<ledNum; i++ ) {
			WS2812SetRGB(i, 0, 255, 0);
		}
		break;
    case 2:
		for ( int i=0; i<ledNum; i++ ) {
			WS2812SetRGB(i, 255, 0, 0);
		}
		break;   
    default: break;  
  } 
	
}
void WS2812::WS2812SetHSV(uint32_t led, uint32_t hue, 
							uint32_t saturation, uint32_t value) {	
	
	if(hue < 1536 && saturation < 256 && value < 256)
	{
		uint8_t red, green, blue;
		uint8_t min, max, inc, dec, hquot, hrem;
		
		if(saturation == 0)
		{
			WS2812SetRGB(led, value, value, value);
			return;
		}
		
		hquot = hue / 256;
		hrem = hue % 256;
		
		max = value;
		min = (value * (255 - saturation)) / 255;
		inc = (value * ((saturation * hrem) / 255)) / 255;
		dec = (value * ((saturation * (255-hrem)) / 255)) / 255;
		
		
		switch (hquot)
		{
		case 0:
			red = max;
			green = inc;
			blue = min;
			break;
		case 1:
			red = dec;
			green = max;
			blue = min;
			break;
		case 2:
			red = min;
			green = max;
			blue = inc;
			break;
		case 3:
			red = min;
			green = dec;
			blue = max;
			break;
		case 4:
			red = inc;
			green = min;
			blue = max;
			break;
		case 5:
			red = max;
			green = min;
			blue = dec;
			break;
		}
		WS2812SetRGB(led, red, green, blue);
	}
}

void WS2812::WS2812SetRGB(uint32_t led, uint8_t red, 
							uint8_t green, uint8_t blue) {
	WS2812Buffer[led*3] = green;
	WS2812Buffer[1+led*3] = red;
	WS2812Buffer[2+led*3] = blue;
}

void WS2812::WS2812Send(void) {	
	uint32_t c;
	static uint32_t endTime = 0;  
	while(micros() - endTime < 50L);
	
	noInterrupts();
	
	for(c = 0; c < (ledNum * 3); c++)
	{	
		uint8_t b;			
		for( b = 8; b; b--)		
		{					
			if(WS2812Buffer[c] & (1<<b))
			{				
				digitalWrite(sigPin, HIGH);
				_DELAY_NOP(130)			
				digitalWrite(sigPin, LOW);
				_DELAY_NOP(36)
			}
			else
			{
				digitalWrite(sigPin, HIGH);
				_DELAY_NOP(2)			
				digitalWrite(sigPin, LOW);
				_DELAY_NOP(150)
			}		
		}		
	}
	interrupts();
	endTime	= micros();
}	

void WS2812::rainbowCycle(uint8_t wait) {
  uint16_t i, j;
  for(j=0; j<256; j++) { 
    for(i=0; i< getLedNum(); i++) {
      colorWheel((i+j) & 0xff, i);
    }
    WS2812Send();
    delay(wait);
  }
}

uint32_t WS2812::colorWheel(byte WheelPos, uint8_t n) {
  if(WheelPos < 85) {
     WS2812SetRGB(n, 0, WheelPos * 3, 255 - WheelPos * 3);     
  } else if(WheelPos < 170) {
     WheelPos -= 85;
	 WS2812SetRGB(n, WheelPos * 3, 255 - WheelPos * 3, 0);     
  } else {
     WheelPos -= 170;
     WS2812SetRGB(n, 255 - WheelPos * 3, 0, WheelPos * 3);
  }  
}	

void WS2812::RGBCycle(uint16_t wait_ms) {
	for ( int i=0; i<ledNum; i++ ) {
			WS2812SetRGB(i, 0, 0, 255);
	}
	WS2812Send();
	delay(wait_ms);
	for ( int i=0; i<ledNum; i++ ) {
			WS2812SetRGB(i, 0, 255, 0);
	}
	WS2812Send();
	delay(wait_ms);
	for ( int i=0; i<ledNum; i++ ) {
			WS2812SetRGB(i, 255, 0, 0);
	}
	WS2812Send();
	delay(wait_ms);
}
