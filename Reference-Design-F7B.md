# Rotorflight Reference Design F7B

Please read the page [Rotorflight FC Design Requirements](FC-Design-Requirements.md) first.
It is explaining the generic requirements for all Rotorflight FC designs.

Lots of effort has been put into these designs for making sure they are as flexible as possible, while not imposing any hardware limitations on the functionality. The manufacturer can choose not to implement any features that are marked _optional_. All other features must be implemented.

The reference designs are considering only the aspects that have effect on the software support, mostly the STM32 resource allocation and the minimum set of supported features. The rest - like size, form factor, connector locations - are left for the manufacturer to decide.

The F7B design is for the STM32F722RET (64 pins LQFP) chip.


## Variants

The design F7B has six variants, depending on the chosen port combinations.

| Variant   | Servos | Motors | RPM   | TLM   | SBUS | DSM | Port A | Port B | Port C | Port E | Port G |
| --------- | ------ | ------ | ----- | ----- | ---- | --- | ------ | ------ | ------ | ------ | ------ |
| F7B1      |  ✔     |  ✔     |   ✔   |   ✔   |  ✔   |  ✓  |  ✔ ᴿˣ  |  ✓     |  ✓     |        |        |
| F7B2      |  ✔     |  ✔     |   ✔   |   ✔   |  ✔   |  ✓  |  ✔ ᴿˣ  |        |        |        |  ✔     |
| F7B3      |  ✔     |  ✔     |   ✔   |   ✔   |  ✔   |     |  ✔ ᴿˣ  |  ✓     |  ✓     |  ✓     |        |
| F7B4      |  ✔     |  ✔     |   ✔   |   ✔   |  ✔   |     |  ✔ ᴿˣ  |        |        |  ✓     |  ✔     |
| F7B5      |  ✔     |  ✔     |   ✔   |   ✔   |  ✔   |     |  ✔ ᴿˣ  |  ✓     |  ✓     | IntRx  |        |
| F7B6      |  ✔     |  ✔     |   ✔   |   ✔   |  ✔   |     |  ✔ ᴿˣ  |        |        | IntRx  |  ✔     |

Legend:
  ✔ = Mandatory Port,
  ✓ = Optional Port,
  ᴿˣ = Primary port for an external serial receiver (CRSF)


## Ports

### Servo and Motor Port

The servos and motors are connected to standard 0.1" pin headers.
The pin headers are organised as two blocks, of size 4x3 and 5x3 pins.

The cyclic servos and the tail servo/ESC connect to the 4x3 block.

The main ESC connects to the 5x3 block.

A receiver can be connected to the 5x3 block, or to one of the JST-GH sockets.

All pin headers in these blocks have a common GND and a common VX (BEC power).

| Label    | Pin1 | Pin2 | Pin3 | MCU Pin |
| -------- | ---- | ---- | ---- | ------- |
| S1       | GND  | VX   | PWM  | PB4     |
| S2       | GND  | VX   | PWM  | PB5     |
| S3       | GND  | VX   | PWM  | PB0     |
| TAIL     | GND  | VX   | PWM  | PA15    |
|          |      |      |      |         |
| ESC      | GND  | VX   | SIG  | PA9     |
| RPM      | GND  | VX   | SIG  | PA2     |
| TLM      | GND  | VX   | SIG  | PA3     |
| AUX      | GND  | VX   | SIG  | PB6     |
| SBUS     | GND  | VX   | SIG  | PB7     |

The ESC pin headers are designed to accommodate all variations of traditional and drone ESCs.
The following combinations are possible with an ESC (electric).

| Type                  | ESC Header     | RPM Header    | TLM Header    | AUX Header     | Example ESC                     |
| --------------------- | -------------- | ------------- | ------------- | -------------- | ------------------------------- |
| Large heli ESC        | PWM + BEC      | RPM + BEC     | Telem + BEC   | Buffer Pack    | Hobbywing Platinum V5 260A      |
| Small heli ESC        | PWM + BEC      | RPM + BEC     | Telem         |                | Hobbywing Platinum V5 80A       |
| AM32 ESC              | DShot          | BEC           | Telemetry     | BEC            | IFlight BLITZ E80               |

The following combinations are possible with ICE (nitro).

| Type                  | ESC Header     | RPM Header    | TLM Header    | AUX Header         |
| --------------------- | -------------- | ------------- | ------------- | ------------------ |
| Nitro                 | THR servo      | RPM sensor    | BEC           | BEC + Glow switch  |


### Expansion Port A

Port A is primarily a UART port. It shall be labelled with Ⓐ.

The connector type is 4-pin JST-GH, with the following pinout:

| Pin1 | Pin2 | Pin3 | Pin4 |
| ---- | ---- | ---- | ---- |
| TX   | RX   | 5V   | GND  |

The signal pins are connected to PA0 (TX) and PA1 (RX) on UART4.

Port A can be also used as an RPM input port for two RPM signals,
or two voltage inputs for the ADC.


### Expansion Port B

Port B is primarily a UART port. It shall be labelled with Ⓑ.

The connector type is 4-pin JST-GH, with the following pinout:

| Pin1 | Pin2 | Pin3 | Pin4 |
| ---- | ---- | ---- | ---- |
| TX   | RX   | 5V   | GND  |

The signal pins are connected to PC6 (TX) and PC7 (RX) on UART6.

Port B can be also used for Camera Control, or for LED Strip.


### Expansion Port C

Port C is a UART port or an I2C port. It shall be labelled with Ⓒ.

The connector type is 4-pin JST-GH, with the following pinout:

| Pin1   | Pin2   | Pin3 | Pin4 |
| ------ | ------ | ---- | ---- |
| TX/SCL | RX/SDA | 5V   | GND  |

The signal pins are connected to PB10 (TX) and PB11 (RX) on UART3.


### Expansion Port E

Port E is a UART port. It shall be labelled with Ⓔ.

If the FC is equipped with an integrated receiver, it shall be
connected to this port.

The external connector type is 4-pin JST-GH, with the following pinout:

| Pin1 | Pin2 | Pin3 | Pin4 |
| ---- | ---- | ---- | ---- |
| TX   | RX   | 5V   | GND  |

The signal pins are connected to PC12 (TX) and PD2 (RX) on UART5.


### Expansion Port G

Port G is primarily for a UART/GPS and a I2C/Compass. It shall be labelled with Ⓖ.

The connector type is 6-pin JST-GH, with the following pinout:

| Pin1 | Pin2 | Pin3 | Pin4 | Pin5 | Pin6 |
| ---- | ---- | ---- | ---- | ---- | ---- |
| 5V   | TX   | RX   | SCL  | SDA  | GND  |

This socket is Pixhawk GPS compatible.

The signal pins are connected to PB10 (SCL), PB11 (SDA) on I2C2; and PC6 (TX), PC7 (RX) on UART6.

Port G is an alternative to Ports B and C. Either Port G can be implemented,
or Ports B and C - but not both.


### DSM Port

The DSM Port is a dedicated port for connecting a Spektrum DSM2 satellite.

The connector type is 3-pin JST-ZH, with the following pinout:

| Pin1 | Pin2 | Pin3 |
| ---- | ---- | ---- |
| 3.3V | GND  | SIG  |

The signal pin is connected to PD2 (RX) on UART5.

The DSM Port and Port E are exclusive. If the design has the Port E, it can't have
the DSM port, and vice versa


## MCU Resource Allocation

| Function      | PIN    | ALT1   | ALT2   | ALT3   | ALT4   | Notes     |
| ------------- | ------ | ------ | ------ | ------ | ------ | ----------|
| Servo1        | PB4    | T3Ch1  |
| Servo2        | PB5    | T3Ch2  |
| Servo3        | PB0    | T3Ch3  |
| Servo4        | PB1    | T3Ch4  |||| Optional: Fourth cyclic servo¹
| Rudder        | PA15   | T2Ch1  |||| Tail servo or motor |
| Motor1        | PA9    | T1Ch2  |||| Main motor |
|               |        |
| UART1 Tx      | PB6    | TX1    | T4Ch2 ||| Optional: UART, LED strip, CC |
| UART1 Rx      | PB7    | RX1    | T4Ch3 ||| Optional: UART, LED strip, CC |
| UART2 Tx      | PA2    | TX2    | T5Ch3 | T9Ch1 | A2 | ESC Telem, RPM, CPPM, ADC, LED strip |
| UART2 Rx      | PA3    | RX2    | T5Ch4 | T9Ch2 | A3 | ESC Telem, RPM, CPPM, ADC, LED strip |
| UART3 Tx      | PB10   | TX3    | SCL2  | T2Ch3 |
| UART3 Rx      | PB11   | RX3    | SDA2  | T2Ch4 |
| UART4 Tx      | PA0    | TX4    | T5Ch1 | A0 |
| UART4 Rx      | PA1    | RX4    | T5Ch2 | A1 |
| UART5 Tx      | PC12   | TX5    |
| UART5 Rx      | PD2    | RX5    |
| UART6 Tx      | PC6    | TX6    | T8Ch1 ||| Optional: UART, LED strip, CC, GPS |
| UART6 Rx      | PC7    | RX6    | T8Ch2 ||| Optional: UART, LED strip, CC, GPS |
|               |        |
| SCL1          | PB8    | SCL1   |||| Internal baro |
| SDA1          | PB9    | SDA1   |||| Internal baro |
|               |        |
| SCL2          | PB10   | SCL2   | TX3 | T2Ch3 || Optional: External compass, UART3, PWM |
| SDA2          | PB11   | SDA2   | RX3 | T2Ch4 || Optional: External compass, UART3, PWM |
|               |        |
| NSS           | PA4    | NSS1   |||| Gyro SPI NSS |
| SCK           | PA5    | SCK1   |||| Gyro SPI SCK |
| MISO          | PA6    | MISO1  |||| Gyro SPI MISO |
| MOSI          | PA7    | MOSI1  |||| Gyro SPI MOSI |
| GEXT          | PA8    | EXTI   |||| Gyro INT |
|               |        |
| NSS           | PB12   | NSS2   |||| Flash SPI NSS |
| SCK           | PB13   | SCK2   |||| Flash SPI SCK |
| MISO          | PB14   | MISO2  |||| Flash SPI MISO |
| MOSI          | PB15   | MOSI2  |||| Flash SPI MOSI |
|               |        |
| ADC1          | PC0    | A10 |||| Vbat voltage sensor⁷ |
| ADC2          | PC1    | A11 |||| Vx voltage sensor⁷ |
| ADC3          | PC2    | A12 |||| 5V voltage sensor⁷ |
| ADC4          | PC3    | A13 |||| Optional: extra ADC input |
| ADC5          | PC4    | A14 |||| Optional: extra ADC input |
|               |        |
| BEEPER        | PC13   | GPIO |||| Optional: buzzer |
| LED1          | PC14   | GPIO |||| Green status LED |
| LED2          | PC15   | GPIO |||| Red status LED |
|               |        |
| USB DM        | PA11   | DM |||| USB data- |
| USB DP        | PA12   | DP |||| USB data+ |
|               |        |
| SWDIO         | PA13   | SWDIO |||| Debugger test pad⁴ |
| SWCLK         | PA14   | SWCLK |||| Debugger test pad⁴ |
|               |        |
| Free⁹         | PB2    |
| Free⁹         | PB3    |
| Free⁹         | PB7    |
| Free⁹         | PC8    |
| Free⁹         | PC9    |

¹ The optional servo may have a solder pad on the PCB.

² ~~Not used~~

³ ~~Not used~~

⁴ For easy debugging, GND,3V3,SWDIO,SWCLK solder pads (test points) should be available on the PCB.

⁵ ~~Not used~~

⁶ ~~Not used~~

⁷ A voltage divider and a filter cap is needed on each ADC input. The cutoff frequency for the input filter should be ~ 25Hz.

⁸ ~~Not used~~

⁹ If space permits, all unused/free pins may have solder pads on the PCB.
