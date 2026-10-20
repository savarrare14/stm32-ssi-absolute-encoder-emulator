# Experimental Setup and Reproduction Notes

## Hardware and instruments

- STM32F406G-DISC1 development board
- STM32 MCU board; confirm the exact model on the physical board before reproducing the setup
- Onboard ST-LINK/V2-A debugger/programmer
- USB Micro-AB connection for board power/programming
- Dual-channel digital oscilloscope
- Oscilloscope probes on the SSI clock and data GPIO header signals

The bench setup uses TIM2 to generate the clock and SPI1 on the same MCU for internal frame verification. It does not use an external SSI master, differential line driver, or external encoder interface card.

## Software

- STM32CubeIDE
- STM32CubeMX for peripheral initialization
- STM32CubeF4 HAL
- SWD debugging through the onboard ST-LINK
- Register / variable live-watch during bring-up

## Peripheral setup as described

- TIM5: free-running 32-bit counter; 18-bit rollover is detected in software.
- TIM2: one PWM channel for clock and CC4 in compare/interrupt mode for shifting the data.
- TIM6: basic timer in one-pulse mode.
- SPI1: slave receive configuration with interrupt-based reception; DMA is not used.
- GPIO alternate-function pin mapping: configured through CubeMX; exact signal pin numbers must be transcribed from the actual project.

## Bench-test procedure

1. Configure TIM2 for the 10 kHz test setting and program the board.
2. Confirm frame generation using the firmware's available indicator or debug output.
3. Probe clock on Channel A and data on Channel B, both DC-coupled at 2 V/division.
4. Capture a wide-timebase view of repeated frames and a closer view of edges / bursts.
5. Configure the nominal 100 kHz setting and repeat the capture.
6. Monitor the SPI1 verification counters during the observations.
7. Store the actual source project, configuration, pin map, and measurement logs alongside any future reproducibility package.

## Details still needed for reproducibility

The original source tree is not yet included in this repository. Before treating the project as a turnkey build, add:

- Original .ioc configuration and STM32CubeIDE project files
- All source and header files
- Exact MCU package / board silkscreen verification
- Exact GPIO and alternate-function pin mapping
- Timer input-clock calculation and actual PSC/ARR/CCR values
- SPI1 data size / clock phase-polarity / frame setting details from CubeMX
- Build version and required STM32CubeF4 package version
- Reproducible SPI counter logs and measured idle interval
