# PIC18F4550 Incubator Temperature Control System

Embedded C project for the PIC18F4550 microcontroller implementing an incubator-inspired temperature monitoring and control system. The system reads an analog temperature sensor through the ADC module, displays the measured temperature on an LCD, indicates the thermal state using an RGB LED, and controls a heater output through PWM.

This project was developed as an academic embedded systems practice and later organized as part of a technical portfolio focused on biomedical engineering, microcontroller programming, sensor acquisition, and temperature control systems.

## Features

* Reads an analog temperature sensor using the PIC18F4550 ADC module.
* Converts ADC readings into voltage and estimated temperature.
* Displays real-time temperature values on a 16x2 LCD.
* Shows temperature status labels such as `COLD`, `NORMAL`, `WARM`, and `HOT`.
* Uses an RGB LED to indicate thermal status.
* Configures a PWM heater output using CCP1 and Timer2.
* Includes a basic PID-style control function.
* Includes keypad support files for future user input or setpoint configuration.
* Uses modular source files for ADC functions, LCD control, and keypad handling.

## Technologies Used

* C
* PIC18F4550 microcontroller
* MPLAB X / XC8
* ADC module
* PWM / CCP1
* Timer2
* 16x2 LCD display
* RGB LED
* Analog temperature sensor
* Keypad input support
* PID control logic

## Project Structure

```text
.
├── main.c              # Main temperature monitoring and heater control logic
├── functions.c         # ADC helper functions
├── functions.h         # Constants, pin definitions, and function declarations
├── keypad.c            # Keypad scanning functions
├── keypad.h            # Keypad map and function declarations
├── LCD_PORTD.c         # LCD control functions
├── LCD_PORTD.h         # LCD function declarations and pin definitions
├── LCD_commands.h      # LCD command definitions
└── config.h            # PIC18F4550 configuration bits
```

## How It Works

1. The PIC18F4550 internal oscillator is configured to 8 MHz.
2. The LCD is initialized and displays a temperature interface.
3. The ADC module is configured to read the temperature sensor.
4. The microcontroller continuously reads the analog value from the selected sensor channel.
5. The ADC value is converted into voltage.
6. The voltage is converted into an estimated temperature value.
7. The LCD displays the temperature in degrees Celsius.
8. The RGB LED changes color according to the measured temperature range.
9. The heater output is controlled using PWM.
10. A PID-style function calculates the temperature error relative to the desired temperature.

## Temperature Status Logic

| Temperature Condition              | Status | RGB Indicator |
| ---------------------------------- | ------ | ------------- |
| Below desired temperature - 2 °C   | Cold   | Blue          |
| Near desired temperature           | Normal | Green         |
| Slightly above desired temperature | Warm   | Yellow        |
| Above desired temperature + 3 °C   | Hot    | Red           |

## Hardware Mapping

| Component             | PIC18F4550 Pin / Port |
| --------------------- | --------------------- |
| Temperature sensor    | AN0                   |
| LCD data lines        | PORTD                 |
| LCD control pins      | PORTE                 |
| RGB LED red channel   | RE2                   |
| RGB LED green channel | RE1                   |
| RGB LED blue channel  | RE0                   |
| Heater PWM output     | CCP1 / RC2            |

## Main Constants

| Constant       | Description                                       |
| -------------- | ------------------------------------------------- |
| `_XTAL_FREQ`   | Oscillator frequency set to 8 MHz                 |
| `PWM_FREQ`     | PWM frequency set to 2.5 kHz                      |
| `DESIRED_TEMP` | Default desired temperature, 38 °C                |
| `SENSOR`       | ADC channel used for the temperature sensor       |
| `DEGREE`       | LCD character code for the degree symbol          |
| `Tau`          | Sampling period used in the PID-style calculation |

## Main Functions

| Function          | Description                                                                  |
| ----------------- | ---------------------------------------------------------------------------- |
| `main()`          | Initializes LCD, ADC, RGB pins, heater PWM, and runs the main control loop.  |
| `configADC()`     | Configures the ADC module and analog input channels.                         |
| `analogRead()`    | Reads a 10-bit ADC value from the selected analog channel.                   |
| `heatingstatus()` | Updates RGB LED status according to the measured temperature.                |
| `updateLCD()`     | Displays temperature and status information on the LCD.                      |
| `configHeater()`  | Configures Timer2 and CCP1 for heater PWM control.                           |
| `setHEATER_DC()`  | Sets the PWM duty cycle for the heater output.                               |
| `PID()`           | Calculates a proportional-integral control value based on temperature error. |
| `updateHEAT()`    | Updates heater control based on the measured temperature.                    |
| `checkCols()`     | Checks keypad column input.                                                  |
| `check_rows()`    | Reads keypad row input and returns the selected key.                         |

## Temperature Conversion

The ADC reading is converted into voltage using:

```c
volts = 5.0 * adc_value / 1023.0;
```

The estimated temperature is calculated using:

```c
temp = volts * 10.018 - 0.15;
```

This equation depends on the sensor calibration used during the practice.

## PID-Style Control

The project includes a basic proportional-integral control function:

```c
double err = t_objective - temp;
double P = Kp * err;
I = I + Ki * err * (Tau / 1000.0);
return P + I;
```

This logic is intended to calculate a control response based on the difference between the desired temperature and the measured temperature.

## Learning Outcomes

Through this project, I practiced:

* Embedded C programming for PIC microcontrollers.
* ADC-based temperature sensor acquisition.
* LCD-based real-time monitoring.
* RGB LED status indication.
* PWM configuration using CCP1 and Timer2.
* Basic heater control logic.
* PID-style feedback control concepts.
* Modular embedded software organization.
* Keypad scanning logic for future user input.
* Biomedical device-inspired embedded system design.

## Possible Improvements

* Rename the repository from `incubadora2` to `pic18f4550-incubator-temperature-control`.
* Add a circuit diagram or simulation screenshot.
* Add comments explaining the sensor calibration equation.
* Add comments explaining the PWM duty cycle calculation.
* Use the PID output directly to update the heater duty cycle.
* Prevent heater duty cycle overflow or underflow.
* Add keypad-based setpoint configuration.
* Add a `.gitignore` for MPLAB/XC8 generated files.
* Organize files into `src/`, `include/`, and `docs/` folders.
* Add photos or screenshots of the LCD output and simulation.

## Notes

This project was originally developed as an academic embedded systems practice. It is intended for learning purposes and is not a certified medical device, neonatal incubator, or clinical temperature control system.
