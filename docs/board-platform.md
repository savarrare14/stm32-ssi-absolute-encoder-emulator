# STM32F406G-DISC1 Platform Notes

This page summarizes the hardware platform description from the project report. The hardware identity should be checked against the physical board marking and the original CubeMX project.

## Board / MCU identified in the report

- **Development board:** STM32F406G-DISC1
- **Microcontroller:** STM32F406VGT6
- **Core:** Arm Cortex-M4 with floating-point unit
- **Core clock cited by report:** up to 168 MHz
- **Flash cited by report:** 1 Mbyte
- **RAM cited by report:** 192 Kbyte
- **On-board programmer/debugger:** ST-LINK/V2-A

The report's body consistently identifies the STM32F406G-DISC1 / STM32F406VGT6 combination, but the block-diagram heading on report page 28 says STM32F407G-DISC1. Confirm which device and board were actually used before finalizing an as-built description.

## Peripherals used for this project

| Peripheral | Use |
|---|---|
| TIM2 | PWM clock generation and CC4 compare interrupt |
| TIM5 | Free-running position counter |
| TIM6 | One-pulse inter-frame idle timer |
| SPI1 | Frame receive / verification |
| GPIO | Data output and peripheral signal mapping |
| NVIC | Timer / SPI interrupt dispatch |
| SWD through ST-LINK | Firmware flashing and debugging |

The report states that the board also provides user LEDs / buttons and other demonstration peripherals, but the project primarily uses timers, GPIO, SPI1, and the integrated debugger.

## Development tools

- STM32CubeIDE for build, flash, debug, and variable/register live-watch
- STM32CubeMX for peripheral initialization
- STM32CubeF4 HAL
- SWD over the on-board ST-LINK/V2-A
- Dual-channel digital oscilloscope for simultaneous clock/data captures

## Known hardware boundary

The experiment uses GPIO-level clock and data signals and an internal SPI receive path. The report does not describe an external SSI master or an RS-422/RS-485 differential transmitter stage. Do not treat the development-board GPIO signals as a finished cable-level industrial interface without suitable external line-driver hardware and independent validation.
