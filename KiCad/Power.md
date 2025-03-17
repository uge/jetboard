# Power Supply Architecture

## Summary

The original motivation for this design was to include a small rechargable battery in the Traquito/Jetpack system. A Traquito which is powered only by solar cells obviously only works in conditions where there is sufficient solar energy to power the device consistently for long periods of time (assumping extended telemetry spanning multiple WSPR messages).

## Specs

### Solar Input

The solar input uses a TLV61070 which supports an input voltage range of 0.5V to 5.5V and converts to a voltage of 4.4V. 

### USB Input

A USB-C receptacle is configured to accept a nominal 5V USB supply voltage when connected to a USB host.

### Battery Manager

The TI BQ25185 is a combination of a single cell lithium battery charger and a power path controller. The BQ25185 will supply from 3.0V to 4.2V to the system depending on the battery, solar input, and USB voltages. The BQ25185 has supports a NTC thermistor to control behavior depending on battery temperature. A resistor statically configures the battery manager for 4.2V/3.0V battery range and an input current limit of 100mA.

### Battery

The battery holder is sized for a LIR2450 cell. A typical LIR2450 cell has 120mAH of capacity but the capacity and maximum voltage decrease significantly at low ambient temperature. Most LIR2450 cells are rated to minimum temperature of -20 degrees C so finding a battery variety which is tolerant of even colder temperature is important.

## Funtional Description

### Alarm Clock Subsystem

An STM8 small microcontroller is included in the design to permit the RP2040 and related systems to be completely powered off for longer periods of time by disabling the 3.3V DC/DC converter. The STM8 receives power from a power mux which supplies power either from the battery directly (typical situation) or Solar/USB (only when available). The current consumption of the STM8, especially in low power modes, should not meaningfully drain the battery during until the next solar input cycle begins.

### RP2040 

The design follows the Raspberry Pi Pico board since it is used for the Jetpack/Traquito system. 

#### QSPI

The QSPI component is specified to be the same as the component on the Pico board.

#### Boot Mode 

A small tactile switch allows the user to force the RP2040 to firmware load mode as it does on the Pico Board.

#### AVDD

The AVDD supply to the ADC has limited filtering.

#### Crystal 

The Pico board had a 12 MHz crystal which was qualified to -25 C. We'll use a similar crystal but find one specified to -40 C.
