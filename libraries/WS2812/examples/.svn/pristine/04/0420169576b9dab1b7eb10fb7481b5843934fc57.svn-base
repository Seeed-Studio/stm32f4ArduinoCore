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

#ifndef SEEED_WS2812_h
#define SEEED_WS2812_h

#include <arduino.h>
#include <stdlib.h>

class WS2812 {
	
	public:
		WS2812(uint32_t ledn, uint8_t sigPin);
		void begin(void);
		uint32_t getLedNum(void);
		void setLedNum(uint32_t);
		void WS2812Clear(void);
		void pureColor(uint8_t);
		void WS2812SetRGB(uint32_t led, uint8_t red,
							uint8_t green, uint8_t blue);
		void WS2812Send(void);
		void WS2812SetHSV(uint32_t led, uint32_t hue, 
							uint32_t saturation, uint32_t value);
        void rainbowCycle(uint8_t wait);
		uint32_t colorWheel(byte WheelPos, uint8_t n);
		void RGBCycle(uint16_t);
		
		uint8_t sigPin;
		uint32_t ledNum;
		uint8_t *WS2812Buffer;

};

#endif
