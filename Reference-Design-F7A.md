# Rotorflight Reference Design F7A

Please read the page [Rotorflight FC Design Requirements](FC-Design-Requirements.md) first.
It is explaining the generic requirements for all Rotorflight FC designs.

Lots of effort has been put into these designs for making sure they are as flexible as possible, while not imposing any hardware limitations on the functionality. The manufacturer can choose not to implement any features that are marked _optional_. All other features must be implemented.

The reference designs are considering only the aspects that have effect on the software support, mostly the STM32 resource allocation and the minimum set of supported features. The rest - like size, form factor, connector locations - are left for the manufacturer to decide.

The F7A design is for the STM32F722RET (64 pins LQFP) chip.


## Variants

The design F7A has four variants, depending on the chosen port combinations.

| Variant   | Servos | Motors | RPM   | SBUS | DSM | Port A | Port B | Port C | Port D | Port E | Port G |
| --------- | ------ | ------ | ----- | ---- | --- | ------ | ------ | ------ | ------ | ------ | ------ |
| F7A1      |  ✔     |  ✔     |   ✔   |  ✔   |  ✓  |  ✔ ᴿˣ  |  ✓     |  ✓     |  ✓     |  ✓     |        |
| F7A2      |  ✔     |  ✔     |   ✔   |  ✔   |  ✓  |  ✔ ᴿˣ  |        |        |  ✓     |  ✓     |  ✔     |
| F7A3      |  ✔     |  ✔     |   ✔   |  ✔   |  ✓  |  ✔ ᴿˣ  |  ✓     |  ✓     |  ✓     | IntRx  |        |
| F7A4      |  ✔     |  ✔     |   ✔   |  ✔   |  ✓  |  ✔ ᴿˣ  |        |        |  ✓     | IntRx  |  ✔     |

Legend:
  ✔ = Mandatory Port,
  ✓ = Optional Port,
  ᴿˣ = Primary port for an external serial receiver


## Ports

### Servo and Motor Port

The servos and motors are connected to standard 0.1" pin headers.
The pin headers are organised as two blocks, of size 4x3 and 3x3 pins.

The cyclic servos and the tail servo/ESC connect to the 4x3 block.

The main ESC connects to the 3x3 block.

A receiver can be connected to the 3x3 block in some configurations.

All pin headers in these blocks have a common GND and a common VX (BEC power).

| Label    | Pin1 | Pin2 | Pin3 |
| -------- | ---- | ---- | ---- |
| S1       | GND  | VX   | PB4  |
| S2       | GND  | VX   | PB5  |
| S3       | GND  | VX   | PB0  |
| TAIL     | GND  | VX   | PB3  |
|          |      |      |      |
| ESC      | GND  | VX   | PB6  |
| RPM      | GND  | VX   | PA2  |
| SBUS     | GND  | VX   | PA3  |

The ESC pin headers are designed to accommodate all variations of traditional and drone ESCs.
The following combinations are possible:

| Type                  | ESC Header     | RPM Header    | SBUS Header   | Example ESC                     |
| --------------------- | -------------- | ------------- | ------------- | ------------------------------- |
| Large heli ESC        | PWM + BEC      | RPM + BEC     | Telem + BEC   | Hobbywing Platinum V4 120A      |
| Small heli ESC        | PWM + BEC      | RPM + BEC     | S.BUS⁶        | Hobbywing Platinum V3 50A       |
| AM32 ESC              | DShot          | BEC.          | Telemetry     | IFlight BLITZ E80               |
| BlueJay ESC           | DShot          | BEC           | S.BUS⁶        | Holybro 20A                     |

A one-wire receiver can be connected to the SBUS header, if the ESC Telemetry feature is not in use.
The receiver must be high voltage capable, e.g. the BEC voltage.


### Expansion Port A

Port A is primarily a UART port. It shall be labelled with Ⓐ.

The connector type is 4-pin JST-GH, with the following pinout:

| Pin1 | Pin2 | Pin3 | Pin4 |
| ---- | ---- | ---- | ---- |
| PA0  | PA1  | 5V   | GND  |

The signal pins are connected by default to TX (PA0) and RX (PA1) on UART4.

Port A can be also used as an RPM input port for two RPM signals,
or two voltage inputs for the ADC.


### Expansion Port B

Port B is primarily a UART port. It shall be labelled with Ⓑ.

The connector type is 4-pin JST-GH, with the following pinout:

| Pin1 | Pin2 | Pin3 | Pin4 |
| ---- | ---- | ---- | ---- |
| PC6  | PC7  | 5V   | GND  |

The signal pins are connected by default to TX (PC6) and RX (PC7) on UART6.

Port B can be also used for Camera Control, or for LED Strip.


### Expansion Port C

Port C is a UART port or an I2C port. It shall be labelled with Ⓒ.

The connector type is 4-pin JST-GH, with the following pinout:

| Pin1  | Pin2  | Pin3 | Pin4 |
| ----- | ----- | ---- | ---- |
| PB10  | PB11  | 5V   | GND  |

The signal pins are connected by default to TX (PB10) and RX (PB11) on UART3.


### Expansion Port D

Port D is a UART port. It shall be labelled with Ⓓ.

The connector type is 4-pin JST-GH, with the following pinout:

| Pin1 | Pin2 | Pin3 | Pin4 |
| ---- | ---- | ---- | ---- |
| PA9  | PA10 | 5V   | GND  |

The signal pins are connected by default to TX (PA9) and RX (PA10) on UART1.

Port D can be also used for Camera Control or LED Strip.


### Expansion Port E

Port E is a UART port. It shall be labelled with Ⓔ.

If the FC is equipped with an integrated receiver, it shall be
connected to this port.

The external connector type is 4-pin JST-GH, with the following pinout:

| Pin1 | Pin2 | Pin3 | Pin4 |
| ---- | ---- | ---- | ---- |
| PC12 | PD2  | 5V   | GND  |

The signal pins are connected by default to TX (PC12) and RX (PD2) on UART5.


### Expansion Port G

Port G is primarily for a UART/GPS and a I2C/Compass. It shall be labelled with Ⓖ.

The connector type is 6-pin JST-GH, with the following pinout:

| Pin1 | Pin2 | Pin3 | Pin4 | Pin5 | Pin6 |
| ---- | ---- | ---- | ---- | ---- | ---- |
| 5V   | PC6  | PC7  | PB10 | PB11 | GND  |

This socket is Pixhawk GPS compatible.

The signal pins are connected by default to TX (PC6), RX (PC7) on UART6;
and SCL (PB10), SDA (PB11) on I2C2.

Port G is an alternative to Ports B and C. Either Port G can be implemented,
or Ports B and C - but not both.


### DSM Port

The DSM Port is a dedicated port for connecting a Spektrum DSM2 satellite.

The connector type is 3-pin JST-ZH, with the following pinout:

| Pin1 | Pin2 | Pin3 |
| ---- | ---- | ---- |
| 3.3V | GND  | PD2  |

The signal pin is connected to RX (PD2) on UART5.

The DSM Port is an alternative to Port D. It is possible to implement
either the DSM Port, or Port D - but not both.


### Integrated Receiver

The F7A3 and F7A4 designs support an integrated receiver.

The receiver is connected PC12 (TX) and PD2 (RX) on Port E.

An external Port E is not available on these designs.


## MCU Pin Allocation

| Pin     | Function            | Usage            | Notes                           |
| ------- | ------------------- | ---------------- | ------------------------------- |
| PA0     | TX4 / T5CH1         | Port A           |                                 |
| PA1     | RX4 / T5CH2         | Port A           |                                 |
| PA2     | TX2 / T5CH3         | RPM              |                                 |
| PA3     | RX2 / T5CH4         | TLM              |                                 |
| PA4     | SPI1 NSS            | Gyro CS          |                                 |
| PA5     | SPI1 SCK            | Gyro SCK         |                                 |
| PA6     | SPI1 MISO           | Gyro SDO         |                                 |
| PA7     | SPI1 MOSI           | Gyro SDI         |                                 |
| PA8     | T1CH1               | LED Strip        | Optional                        |
| PA9     | TX1                 | Port D           |                                 |
| PA10    | RX1                 | Port D           | DSM                             |
| PA11    | USB DM              |                  |                                 |
| PA12    | USB DP              |                  |                                 |
| PA13    | SWD                 |                  | Note¹                           |
| PA14    | SWD                 |                  |                                 |
| PA15    | EXTI                | Gyro INT         |                                 |
|||||
| PB0     | T3CH3               | Servo 3          |                                 |
| PB1     | T3CH4               | Servo 4          | Optional²                       |
| PB2     |                     |                  | Free                            |
| PB3     | T2CH2               | Tail             |                                 |
| PB4     | T3CH1               | Servo 1          |                                 |
| PB5     | T3CH2               | Servo 2          |                                 |
| PB6     | T4CH1               | ESC              |                                 |
| PB7     |                     |                  | Free                            |
| PB8     | SCL1                | Baro             |                                 |
| PB9     | SDA1                | Baro             |                                 |
| PB10    | TX3 / SCL2 / T2CH3  | Port C           |                                 |
| PB11    | RX3 / SDA2 / T2CH4  | Port C           |                                 |
| PB12    | SPI2 NSS            | Flash            |                                 |
| PB13    | SPI2 SCK            | Flash            |                                 |
| PB14    | SPI2 MISO           | Flash            |                                 |
| PB15    | SPI2 MOSI           | Flash            |                                 |
|||||
| PC0     | ADC10               | Vbat             |                                 |
| PC1     | ADC11               | Vbec (VX)        |                                 |
| PC2     | ADC12               | Vbus (5V)        |                                 |
| PC3     | ADC13               |                  | Free                            |
| PC4     | ADC14 / EXTI        |                  | Free³                           |
| PC6     | TX6 / T8CH1         | Port B           |                                 |
| PC7     | RX6 / T8CH2         | Port B           |                                 |
| PC8     |                     |                  | Free                            |
| PC9     |                     |                  | Free                            |
| PC10    |                     |                  | Free                            |
| PC11    |                     |                  | Free                            |
| PC12    | TX5                 | Port E           |                                 |
| PC13    | GPIO                | Buzzer           | Optional                        |
| PC14    | GPIO                | LED Green        |                                 |
| PC15    | GPIO                | LED Red          |                                 |
|||||
| PD2     | RX5                 | Port E           |                                 |


¹ For easy debugging, GND,3V3,SWDIO,SWCLK solder pads (test points) should be available on the PCB.

² The optional servo may have a solder pad on the PCB.

³ Suitable as EXTI because the pin is close to the other gyro pins


## ADC Inputs

There are total six ADC inputs available. The first three are reserved for the allocated
functions, while the rest are optional or free for other use.

The ADC inputs are limited to 3.3V, and often require a voltage divider.

The resistors should have at least 1% precision.

Example divider values can be found below.

| Usage    |  Vmax    | Vreso    | R1       | R2         |   C         |
| -------- | -------- | -------- |--------- | ---------- | ----------- |
| Bat 16S  | 80V      | 20mV     | 243k     | 10k2       | 560nF       |
| Bat 8S   | 40V      | 10mV     | 162k     | 14k2       | 390nF       |
| BEC      | 20V      | 5mV      | 102k     | 19k6       | 270nF       |
| 5V       | 10V      | 2.5mV    | 57k6     | 27k4       | 270nF       |

