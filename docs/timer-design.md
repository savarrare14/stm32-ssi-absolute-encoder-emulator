# Timer Design and Timing Budget

The following roles and timing formula are described in the project's technical report. Exact register values depend on the actual CubeMX clock tree and firmware configuration; they are not specified here because the original .ioc / source project has not yet been uploaded.

## TIM2 PWM clock generation

TIM2 uses a hardware PWM channel to produce the SSI clock. The design target is a 50% duty-cycle waveform. Hardware PWM generates clock edges without requiring an ISR to toggle the clock pin, separating clock-edge timing from interrupt execution latency.

The report gives this timer-frequency relation:

f_CLK = f_TIMxCLK / ((PSC + 1) × (ARR + 1))

where PSC is the prescaler register, ARR is the auto-reload value, and f_TIMxCLK is the effective timer input clock. Compute f_TIMxCLK from the actual RCC / APB clock tree; the report notes the STM32 timer-clock doubling behavior when the APB prescaler is not unity.

The report describes testing two clock configurations: 10 kHz and nominal 100 kHz.

## TIM2 CC4 — bit update timing

A second TIM2 channel, CC4, is configured for an output-compare interrupt with a phase offset inside the PWM clock-high interval. On each compare interrupt, the firmware presents the current MSB of the Gray-coded shift register on the GPIO data line, shifts the register, and updates the bit index.

The report's design rationale is to update the data line after the rising clock edge but before the following falling edge. The data transition should therefore remain within the clock-high window and settle before the next sampling edge.

## TIM5 — single-turn position

TIM5 is used as a free-running 32-bit up-counter. Only the lower 18 bits are used for single-turn position:

- Position modulus: 2^18 = 262,144 counts.
- Valid single-turn values: 0 through 262,143.
- At the 18-bit rollover, the software multiturn count increments.

The report says rollover is detected in software against the 18-bit mask; a normal TIM5 auto-reload interrupt is not required for the counter's continuous operation.

## Multiturn count and frame preparation

A 12-bit software counter tracks revolution count, providing 4096 values before wrapping. A 2-bit status/error field is added. The full 32-bit composite is then converted to Gray code and loaded into the shift register.

The report states that the status/error bits are intended to represent internal consistency / fault indicators. This repository does not infer meanings for individual status-bit patterns because a complete assignment table is not documented in the available report text.

## TIM6 one-pulse mode — frame gap

After the 32nd bit has been transmitted, the TIM2 CC4 handler stops the clock output and triggers TIM6 one-pulse mode. At the TIM6 update interrupt, the next frame is prepared and TIM2 transmission restarts.

- **Design target cited in report:** 30 µs.
- **Reported discrepancy:** Figure 8.2's caption labels the idle / monoflop time as 65 µs.
- **Status:** exact as-built idle time was not quantitatively consolidated in the report.

Measure the clock-low gap with time cursors and document the result before claiming a validated interval.

## SPI1 receive path

SPI1 is used as an interrupt-driven receive path with the shared clock and data signals. After 32 bits arrive, the receive-complete ISR reconstructs the frame and compares it with the transmit-side frame value. The report says software pass/fail counters were accumulated, but it does not provide consolidated totals or a bit-error rate.

## Recommended quantitative checks

1. Record RCC/APB/TIM2 clock tree, PSC, ARR, CCR / duty-cycle configuration.
2. Measure within-burst clock period with cursors, excluding inter-frame gap.
3. Measure actual TIM6 idle interval independently.
4. Log frames sent, received, passes, failures, and duration for each clock setting.
5. Increase the clock gradually above 100 kHz only if needed, monitoring ISR timing margin and error counts.
