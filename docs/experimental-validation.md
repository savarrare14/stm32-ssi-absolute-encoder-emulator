# Experimental Validation

This page summarizes the scope observations described in the July 2026 technical report. The source screenshots are available in [Waveform gallery](waveforms/README.md).

## Setup described in report

- Board: STM32F406G-DISC1, MCU identified in the report body as STM32F406VGT6.
- Firmware environment: STM32CubeIDE with STM32CubeMX-generated initialisation and STM32CubeF4 HAL.
- Debug/programming: onboard ST-LINK/V2-A through SWD.
- Instrument: dual-channel digital oscilloscope.
- Scope Channel A: SSI clock (red trace).
- Scope Channel B: SSI data (blue trace).
- Coupling and vertical scale: DC coupling, 2 V/division as recorded in the screenshots.
- Test configurations: nominal 10 kHz and nominal 100 kHz.

The report explains that SPI1 received the same clock/data signals during captures and compared each reconstructed 32-bit frame. The detailed pin numbers and wiring table are not included in the report text excerpt available for this repository; verify them from the original CubeMX project / board wiring before reproducing the setup.

## Qualitative observations documented

| Observation | 10 kHz configuration | Nominal 100 kHz configuration |
|---|---|---|
| Clock burst | 32 evenly spaced pulses per frame in the described capture | 32 evenly spaced pulses per frame in the described capture |
| Inter-frame gap | Visible in multi-frame views | Visible in multi-frame view |
| Data transitions | Report says transitions remain within the clock-high window | Report says transitions remain within the clock-high window |
| Successive frames | Regular burst spacing is described | Regular burst spacing is described |
| Frequency counter | No readout captured in available 10 kHz screenshot | Approx. 93.08 kHz shown in one screenshot |

These are qualitative observations as described in the report, not an independent re-test of the hardware.

## Caveat: configured vs measured clock

The report labels one capture as a nominal 100 kHz configuration, but the oscilloscope's automatic counter displays approximately 93.08 kHz. The report suggests that the counter's measurement window may include part of the inter-frame idle gap; that explanation is plausible but not conclusively demonstrated by the capture. A proper follow-up is to place time cursors between two adjacent rising edges strictly inside a single clock burst and calculate the within-burst period.

## Caveat: inter-frame idle / monoflop

The design target is described as 30 µs. However, the caption for the 10 kHz capture at 1 ms/div states a monoflop time of 65 µs, while other descriptions refer to a 30 µs gap. The report explicitly says the exact as-built monoflop value had not been consolidated. These statements are retained as a discrepancy to resolve; this README should not claim that a 30 µs measured interval has been proven.

## Caveat: SPI counters

The report says the SPI1 receive path operated concurrently with the captures and had pass/fail counters, but consolidated numerical pass counts, fail counts, and bit-error-rate statistics were not available at report time.

## Follow-up measurements

- [ ] Measure clock period inside a pulse burst (separately at each configured setting).
- [ ] Measure actual inter-frame idle time with time cursors.
- [ ] Save SPI1 pass/fail counters with run duration and number of frames.
- [ ] Repeat for an extended test duration and log any failures.
- [ ] Record exact GPIO pins, wiring, timer clocks, PSC/ARR/CCR values and CubeMX configuration.
- [ ] Test against an external SSI master if available; use appropriate differential transceivers for a field-style interface.
- [ ] Sweep upward from 100 kHz if seeking the maximum reliable frequency.

## Evidence integrity

Do not replace report screenshots with synthetic waveforms or label a design target as a measured value. Add any new measurements with the date, configuration, capture image, and method.
