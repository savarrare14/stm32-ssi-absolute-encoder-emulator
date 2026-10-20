# Firmware Source (to be added)

The uploaded technical report describes firmware built with STM32CubeIDE, STM32CubeMX-generated peripheral initialization, and STM32CubeF4 HAL. The PDF does not include the original source tree, so this directory currently documents what should be uploaded rather than pretending to contain the original implementation.

## Please add the original files when available and approved for sharing

- STM32CubeIDE project files
- CubeMX .ioc file
- Application .c / .h sources
- HAL configuration and interrupt handlers
- Peripheral setup (TIM2 PWM + CC4, TIM5 counter, TIM6 one-pulse, SPI1 receive)
- Readme with build instructions, board pin map, timer clock calculation, and tested package versions
- Reproducible frame-verification logs

Do not commit generated build output, credentials, private configuration, or any material you are not authorized to publish.

## Reconstructed algorithm vs. original firmware

Any future standalone examples added to this folder should be labelled as illustrative/reconstructed unless they are taken from the actual project source and verified against the build.
