STM32F4 Arduino Core
===

This is an Arduino porting for STM32F405/407.


## How To Use



Copy and paste the json URL to **Additional Boards Manager URLs**

	https://raw.githubusercontent.com/Seeed-Studio/Seeed_Platform/master/package_legacy_seeeduino_boards_index.json

Search and install **Seeed STM32F4 Boards** within **Arduino Board Manager**.

## Special demostrations
### MCU enter standby mode

```C
#include <pwr.h>

void setup() {
  pwr_enter_standby_mode();

}

void loop() {
  // put your main code here, to run repeatedly:

}
```

### External interrupt
```C
#include <Arduino.h>

const int LedPin = 20;
const int BtnPin = 38;
const int SocketPwrPin = 26;
static bool led_state = false;

void led_change(void)
{
  led_state = !led_state;
  led_state ? digitalWrite(20, HIGH) : digitalWrite(20, LOW);  
}

void setup() {
  /* Turn on Seeed Wio LTE Cat.1 grove socket power */
  pinMode(SocketPwrPin, OUTPUT);
  digitalWrite(SocketPwrPin, HIGH);

  /* Led pin */
  pinMode(LedPin, OUTPUT);
  digitalWrite(LedPin, LOW);
  
  pinMode(BtnPin, INPUT);
  attachInterrupt(BtnPin, led_change, RISING);
  attachInterrupt(BtnPin, led_change, FALLING);
}

void loop() {
  
}
```




