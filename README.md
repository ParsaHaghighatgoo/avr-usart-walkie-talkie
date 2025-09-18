# AVR USART Walkie-Talkie

Two-node “walkie-talkie” built on **ATmega32**: channelized **USART** link with a fixed **SYNC** byte, per-channel **XOR encryption**, **LCD** UI (HD44780), **debounced buttons** (short/long press for 6 messages), **heartbeat Stand-By** after 1 minute idle, and **sleep (IDLE)** between events. Includes Proteus design and LaTeX report.

## Features
- Potentiometer → channel select (baud + XOR key)
- SYNC check (green LED on success, red on failure)
- 6 user messages (H/O/R + A/B/Y), plus auto **Stand-By ‘S’**
- Tx/Rx **mode toggle** (PD5 long-press)
- **Power ON/OFF** (PD4) that disables Timer1/ADC/USART and shows `OFF`
- Works in **Proteus**; firmware is AVR-GCC / PlatformIO friendly

## Hardware (per node)
- ATmega32 @ 8 MHz  
- 16×2 LCD (8-bit; data: PORTC, ctrl: PA0=RS, PA1=RW, PA2=EN)  
- Potentiometer on PA3/ADC3  
- Buttons: PD2, PD3, PB2 (messages), PD5 (mode), PD4 (power)  
- Bi-color LED (common-anode): PB3=RED, PB4=GREEN

## Channel map
| CH | Baud   | XOR key |
|---:|-------:|:-------:|
|0|  4800|0x11|
|1|  9600|0x22|
|2| 19200|0x33|
|3| 38400|0x44|
|4| 57600|0x55|
|5| 76800|0x66|
|6|115200|0x77|
|7|250000|0x88 (sim only / 16 MHz on HW)|

## Build & Run
### AVR-GCC
```bash
avr-gcc -mmcu=atmega32 -DF_CPU=8000000UL -Os -o main.elf main.cpp
avr-objcopy -O ihex -R .eeprom main.elf main.hex
# flash (example)
avrdude -p m32 -c usbasp -U flash:w:main.hex
```

[env:atmega32]

platform = atmelavr

board = ATmega32

framework = arduino

board_build.f_cpu = 8000000L

upload_protocol = custom
