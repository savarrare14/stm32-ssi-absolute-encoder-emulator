# STM32F406G-DISC1 Platform Notes

This page describes the target hardware and development setup for the SSI emulator. Confirm the exact board and MCU against the physical board marking and STM32CubeMX project.

## Board and MCU identification

- **Development board:** STM32F406G-DISC1
- **Microcontroller:** STM32F406VGT6
- **Core:** Arm Cortex-M4 with floating-point unit
- **Core clock reference:** up to 168 MHz (confirm the actual project clock configuration)
- **Flash reference:** 1 Mbyte for the named MCU variant; verify the exact device datasheet
- **RAM reference:** 192 Kbyte for the named MCU variant; verify the exact device datasheet
- **On-board programmer/debugger:** ST-LINK/V2-A

The hardware identifier is inconsistent across project materials: STM32F406G-DISC1 / STM32F406VGT6 appears in the platform notes, while an architecture diagram labels STM32F407G-DISC1. Verify the physical board and CubeMX target, then use the confirmed model consistently.

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

The board also provides user LEDs, buttons, and other demonstration peripherals, but this project primarily uses timers, GPIO, SPI1, and the integrated debugger.

## Development tools

- STM32CubeIDE for build, flash, debug, and variable/register live-watch
- STM32CubeMX for peripheral initialization
- STM32CubeF4 HAL
- SWD over the on-board ST-LINK/V2-A
- Dual-channel digital oscilloscope for simultaneous clock/data captures

## Known hardware boundary

The prototype uses GPIO-level clock/data signals and an internal SPI receive path. It does not include an external SSI master or RS-422/RS-485 differential transmitter stage. Do not treat development-board GPIO signals as a finished cable-level industrial interface without suitable line-driver hardware and independent validation.
