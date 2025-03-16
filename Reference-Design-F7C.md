# Rotorflight Reference Design F7C

Please read the page [Rotorflight FC Design Requirements](FC-Design-Requirements.md) first.
It is explaining the generic requirements for all Rotorflight FC designs.

Lots of effort has been put into these designs for making sure they are as flexible as possible, while not imposing any hardware limitations on the functionality. The manufacturer can choose not to implement any features that are marked _optional_. All other features must be implemented.

The reference designs are considering only the aspects that have effect on the software support, mostly the STM32 resource allocation and the minimum set of supported features. The rest - like size, form factor, connector locations - are left for the manufacturer to decide.

The F7C design is for the STM32F722RET (64 pins LQFP) chip.

## History

The F7C design is an evolution of the F7B, with minor changes based on feedback.
Further, it adds the required signals for separate gyro and acc chips, like with the BMI088.


## Variants

The F7C design has six variants, depending on the chosen port combinations.

| Variant   | Servos | Motors | RPM   | TLM   | AUX  | SBUS | DSM | Port A | Port B | Port C | Port E | Port G |
| --------- | ------ | ------ | ----- | ----- | ---- | ---- | --- | ------ | ------ | ------ | ------ | ------ |
| F7C1      |  ✔     |  ✔     |   ✔   |   ✔   |  ✔   |  ✔   |  ✓  |  ✔ ᴿˣ  |  ✓     |  ✓     |        |        |
| F7C2      |  ✔     |  ✔     |   ✔   |   ✔   |  ✔   |  ✔   |  ✓  |  ✔ ᴿˣ  |        |        |        |  ✔     |
| F7C3      |  ✔     |  ✔     |   ✔   |   ✔   |  ✔   |  ✔   |     |  ✔ ᴿˣ  |  ✓     |  ✓     |  ✓     |        |
| F7C4      |  ✔     |  ✔     |   ✔   |   ✔   |  ✔   |  ✔   |     |  ✔ ᴿˣ  |        |        |  ✓     |  ✔     |
| F7C5      |  ✔     |  ✔     |   ✔   |   ✔   |  ✔   |  ✔   |     |  ✔ ᴿˣ  |  ✓     |  ✓     | IntRx  |        |
| F7C6      |  ✔     |  ✔     |   ✔   |   ✔   |  ✔   |  ✔   |     |  ✔ ᴿˣ  |        |        | IntRx  |  ✔     |

Legend:
  ✔ = Mandatory Port,
  ✓ = Optional Port,
  ᴿˣ = Primary port for an external serial receiver (CRSF/SXRL2)


## Ports

### Servo and Motor Port

The servos and motors are connected to standard 0.1" pin headers.
The pin headers can be organised as two or more blocks.

All pin headers in these blocks have a common GND and a common VX (BEC power).

| Label    | Pin1 | Pin2 | Pin3 |
| -------- | ---- | ---- | ---- |
| S1       | GND  | VX   | PB4  |
| S2       | GND  | VX   | PB5  |
| S3       | GND  | VX   | PB0  |
| TAIL     | GND  | VX   | PA15 |
| ESC      | GND  | VX   | PA9  |
| RPM      | GND  | VX   | PA2  |
| TLM      | GND  | VX   | PA3  |
| AUX      | GND  | VX   | PB6  |
| SBUS     | GND  | VX   | PB7  |

The ESC pin headers are designed to accommodate all variations of traditional and drone ESCs.
The following combinations are possible with an ESC (electric).

| Type                  | ESC Header     | RPM Header    | TLM Header    | AUX Header     | Example ESC                     |
| --------------------- | -------------- | ------------- | ------------- | -------------- | ------------------------------- |
| Large heli ESC        | PWM + BEC      | RPM + BEC     | Telem + BEC   | Buffer Pack    | Hobbywing Platinum V5 260A      |
| Small heli ESC        | PWM + BEC      | RPM + BEC     | Telem         |                | Hobbywing Platinum V5 80A       |
| Drone ESC             | DShot          | BEC           | Telemetry     | BEC            | IFlight BLITZ E80               |

The following combinations are possible with ICE (nitro).

| Type                  | ESC Header     | RPM Header    | TLM Header    | AUX Header         |
| --------------------- | -------------- | ------------- | ------------- | ------------------ |
| Nitro                 | THR servo      | RPM sensor    | BEC           | BEC + Glow switch  |


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

The DSM Port and Port E are exclusive. If the design has the Port E, it can't have
the DSM port, and vice versa


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
| PA8     | SCL3                | Baro             |                                 |
| PA9     | T1CH2               | ESC              |                                 |
| PA10    |                     |                  | Free                            |
| PA11    | USB DM              |                  |                                 |
| PA12    | USB DP              |                  |                                 |
| PA13    | SWD                 |                  | Note¹                           |
| PA14    | SWD                 |                  |                                 |
| PA15    | T2CH1               | TAIL             |                                 |
|||||
| PB0     | T3CH3               | Servo 3          |                                 |
| PB1     | T3CH4               | Servo 4          | Optional²                       |
| PB2     | SPI1 NSS2           | ACC CS           | Note³                           |
| PB3     |                     |                  | Free                            |
| PB4     | T3CH1               | Servo 1          |                                 |
| PB5     | T3CH2               | Servo 2          |                                 |
| PB6     | T4CH1 / TX1 / SCL1  | AUX              |                                 |
| PB7     | T4CH2 / RX1 / SDA1  | SBUS             |                                 |
| PB8     | T10CH1              | Gyro INT         |                                 |
| PB9     | T11CH1              | Acc INT          | Note⁴                           |
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
| PC3     | ADC13               | Ibus             | Optional                        |
| PC4     | ADC14               |                  | Free for ADC use                |
| PC6     | TX6 / T8CH1         | Port B           |                                 |
| PC7     | RX6 / T8CH2         | Port B           |                                 |
| PC8     | GPIO                | Beeper           | Optional                        |
| PC9     | SDA3                | Baro             |                                 |
| PC10    | GPIO                | LED Green        |                                 |
| PC11    | GPIO                | LED Red          |                                 |
| PC12    | TX5                 | Port E           |                                 |
| PC13    | GPIO                |                  | Free⁵                           |
| PC14    | GPIO                |                  | Free⁵                           |
| PC15    | GPIO                |                  | Free⁵                           |
|||||
| PD2     | RX5                 | Port E           |                                 |


¹ For easy debugging, GND,3V3,SWDIO,SWCLK solder pads (test points) should be available on the PCB.

² The optional servo may have a solder pad on the PCB.

³ Accelerometer CS is needed with chips like BMI088

⁴ Accelerometer INT is needed with chips like BMI088

⁵ The current on this pin must not exceed 3mA


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

