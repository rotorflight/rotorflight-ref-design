# Rotorflight Reference Design H7A

Please read the page [Rotorflight FC Design Requirements](FC-Design-Requirements.md) first.
It is explaining the generic requirements for all Rotorflight FC designs.

Lots of effort has been put into these designs for making sure they are as flexible as possible, while not imposing any hardware limitations on the functionality. The manufacturer can choose not to implement any features that are marked _optional_. All other features must be implemented.

The reference designs are considering only the aspects that have effect on the software support, mostly the STM32 resource allocation and the minimum set of supported features. The rest - like size, form factor, connector locations - are left for the manufacturer to decide.

The H7A design is for the STM32H743V (100 pins) chip.


## Variants

The design H7A has four variants, depending on the chosen port combinations.

| Variant   | Servos | Motors | RPM   | TLM   | AUX  | SBUS | Port A | Port B | Port C | Port E | Port F | Port G |
| --------- | ------ | ------ | ----- | ----- | ---- | ---- | ------ | ------ | ------ | ------ | ------ | ------ |
| H7A1      |  ✔     |  ✔     |   ✔   |   ✔   |  ✔   |  ✔   |  ✔ ᴿˣ  |  ✓     |  ✓     |  ✓     |   ✓    |        |
| H7A2      |  ✔     |  ✔     |   ✔   |   ✔   |  ✔   |  ✔   |  ✔ ᴿˣ  |        |        |  ✓     |   ✓    |  ✔     |
| H7A3      |  ✔     |  ✔     |   ✔   |   ✔   |  ✔   |  ✔   |  ✔ ᴿˣ  |  ✓     |  ✓     | IntRx  |   ✓    |        |
| H7A4      |  ✔     |  ✔     |   ✔   |   ✔   |  ✔   |  ✔   |  ✔ ᴿˣ  |        |        | IntRx  |   ✓    |  ✔     |

Legend:
  ✔ = Mandatory Port,
  ✓ = Optional Port,
  ᴿˣ = Primary port for an external serial receiver (CRSF)


## Ports

### Servo and Motor Port

The servos and motors are connected to standard 0.1" pin headers.
The pin headers are organised as two blocks, of size 5x3 pins.

A receiver can be connected to the 5x3 block, or to one of the JST-GH sockets.

All pin headers in these blocks have a common GND and a common VX (BEC power).

| Label    | Pin1 | Pin2 | Pin3 | MCU Pin |
| -------- | ---- | ---- | ---- | ------- |
| S1       | GND  | VX   | PWM  | PB4     |
| S2       | GND  | VX   | PWM  | PB5     |
| S3       | GND  | VX   | PWM  | PB0     |
| S4       | GND  | VX   | PWM  | PB1     |
| TAIL     | GND  | VX   | PWM  | PA15    |
|          |      |      |      |         |
| ESC      | GND  | VX   | SIG  | PA8     |
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


### Expansion Port F

Port F is a UART port. It shall be labelled with Ⓕ.

The external connector type is 4-pin JST-GH, with the following pinout:

| Pin1 | Pin2 | Pin3 | Pin4 |
| ---- | ---- | ---- | ---- |
| TX   | RX   | 5V   | GND  |

The signal pins are connected to PE1 (TX) and PE0 (RX) on UART8.


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



## MCU Resource Allocation

| Pin     | Function            | Usage            | Notes                           |
| ------- | ------------------- | ---------------- | ------------------------------- |
| PA0     | TX4 / T5CH1         | Port A           |                                 |
| PA1     | RX4 / T5CH2         | Port A           |                                 |
| PA2     | TX2 / T5CH3         | RPM              |                                 |
| PA3     | RX2 / T5CH4         | TLM              |                                 |
| PA4     | SPI1 NSS            | Gyro             |                                 |
| PA5     | SPI1 SCK            | Gyro             |                                 |
| PA6     | SPI1 MISO           | Gyro             |                                 |
| PA7     | SPI1 MOSI           | Gyro             |                                 |
| PA8     | T1CH1 / RX7         | ESC              |                                 |
| PA9     |                     |                  |                                 |
| PA10    |                     |                  |                                 |
| PA11    | USB DM              |                  |                                 |
| PA12    | USB DP              |                  |                                 |
| PA13    | SWD                 |                  |                                 |
| PA14    | SWD                 |                  |                                 |
| PA15    | T2CH1 / TX7         | TAIL             |                                 |
|||||
| PB0     | T3CH3               | Servo 3          |                                 |
| PB1     | T3CH4               | Servo 4          |                                 |
| PB2     |                     |                  |                                 |
| PB3     |                     |                  |                                 |
| PB4     | T3CH1               | Servo 1          |                                 |
| PB5     | T3CH2               | Servo 2          |                                 |
| PB6     | T4CH1 / TX1 / SCL1  | AUX              |                                 |
| PB7     | T4CH2 / RX1 / SDA1  | SBUS             |                                 |
| PB8     | T16CH1              | Gyro SYNC        | For ICM4x                       |
| PB9     | T16CH2              | Gyro INT         |                                 |
| PB10    | TX3 / SCL2 / T2CH3  | Port C           |                                 |
| PB11    | RX3 / SDA2 / T2CH4  | Port C           |                                 |
| PB12    | SPI2 NSS            | Flash            |                                 |
| PB13    | SPI2 SCK            | Flash            |                                 |
| PB14    | SPI2 MISO           | Flash            |                                 |
| PB15    | SPI2 MOSI           | Flash            |                                 |
|||||
| PC0     | ADC10               | Vbat             |                                 |
| PC1     | ADC11               | Vx / Vbec        |                                 |
| PC2     | ADC12               | 5V / Vbus        |                                 |
| PC3     | ADC13               |                  |                                 |
| PC4     | ADC14               |                  |                                 |
| PC5     | ADC15               |                  |                                 |
| PC6     | TX6 / T8CH1         | Port B           |                                 |
| PC7     | RX6 / T8CH2         | Port B           |                                 |
| PC8     |                     |                  |                                 |
| PC9     |                     |                  |                                 |
| PC10    |                     |                  |                                 |
| PC11    |                     |                  |                                 |
| PC12    | TX5                 | Port E           |                                 |
| PC13    | GPIO                | BEEPER           | Optional                        |
| PC14    | GPIO                | LED Green        |                                 |
| PC15    | GPIO                | LED Red          |                                 |
|||||
| PD0     |                     |                  |                                 |
| PD1     |                     |                  |                                 |
| PD2     | RX5                 | Port E           |                                 |
| PD3     |                     |                  |                                 |
| PD4     |                     |                  |                                 |
| PD5     |                     |                  |                                 |
| PD6     |                     |                  |                                 |
| PD7     |                     |                  |                                 |
| PD8     |                     |                  |                                 |
| PD9     |                     |                  |                                 |
| PD10    |                     |                  |                                 |
| PD11    |                     |                  |                                 |
| PD12    | SCL4                | Baro             |                                 |
| PD13    | SDA4                | Baro             |                                 |
| PD14    |                     |                  |                                 |
| PD15    |                     |                  |                                 |
|||||
| PE0     | RX8                 | Port F           |                                 |
| PE1     | TX8                 | Port F           |                                 |
| PE2     |                     |                  |                                 |
| PE3     |                     |                  |                                 |
| PE4     |                     |                  |                                 |
| PE5     |                     |                  |                                 |
| PE6     |                     |                  |                                 |
| PE7     |                     |                  |                                 |
| PE8     |                     |                  |                                 |
| PE9     |                     |                  |                                 |
| PE10    |                     |                  |                                 |
| PE11    |                     |                  |                                 |
| PE12    |                     |                  |                                 |
| PE13    |                     |                  |                                 |
| PE14    |                     |                  |                                 |
| PE15    |                     |                  |                                 |
|||||



## ADC Inputs

There are total six ADC inputs available. The first three are reserved for the allocated
functions, while the rest are free.

The ADC inputs are limited to 3.3V, and thus require voltage dividers and protection diodes.


| Usage    |  Vmax    | Vreso    | R1       | R2         |   C         |
| -------- | -------- | -------- |--------- | ---------- | ----------- |
| Bat 16S  | 80V      | 20mV     | 243k     | 10k2       | 560nF       |
| Bat 8S   | 40V      | 10mV     | 162k     | 14k2       | 390nF       |
| BEC      | 20V      | 5mV      | 102k     | 19k6       | 270nF       |
| 5V       | 10V      | 2.5mV    | 57k6     | 27k4       | 270nF       |

