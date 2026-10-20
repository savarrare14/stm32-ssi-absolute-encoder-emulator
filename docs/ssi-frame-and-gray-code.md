# SSI Frame and Gray-Code Design

## SSI context from the report

The report describes SSI as a synchronous, unidirectional encoder interface in which a receiving controller usually supplies a burst of clock pulses and the encoder shifts out the position word MSB-first. The basic SSI position frame has no required header, address, or CRC; some device families append manufacturer-specific status/error bits. The report references differential RS-422 or RS-485 signalling for field use.

The emulator described in this project uses its own TIM2 PWM output during the documented test setup, rather than an external master clock. See [System Architecture](system-architecture.md).

## 32-bit frame

The report's frame allocation is:

| Bit range (MSB–LSB) | Width | Field |
|---|---:|---|
| 31–14 | 18 bits | Single-turn position |
| 13–2 | 12 bits | Multiturn revolution count |
| 1–0 | 2 bits | Status / error |

The fields are assembled into a 32-bit composite word. The report then converts the entire 32-bit word to Gray code, rather than Gray-encoding the fields independently, and shifts the result MSB-first.

## Binary-to-Gray conversion

For an n-bit binary value B, the Gray-coded value G is:

G = B XOR (B >> 1)

The MSB remains unchanged; each subsequent Gray bit is the XOR of the corresponding binary bit and the next-more-significant binary bit.

The report's engineering rationale is that adjacent Gray-code values differ by one bit, reducing the chance that a receiver interprets a multi-bit intermediate transition as a different position.

## Position range

- Single-turn range: 2^18 = 262,144 values, from 0 through 262,143.
- Multiturn counter: 12 bits, giving 4096 values before wrapping.
- Frame width: 18 + 12 + 2 = 32 bits.

The report says the lower 18 bits of TIM5 naturally provide the single-turn count; software detects its 18-bit rollover and increments the multiturn counter.

## Frame integrity check

SPI1 is configured to receive the clock/data signals and reconstruct a 32-bit frame. The receive-complete interrupt compares the received value against the corresponding value loaded for transmission, accumulating pass/fail counters.

The report does **not** provide a consolidated numerical count of all passes/failures or a quantitative bit-error rate. That evidence should be added from the actual debug counters or serial logs rather than inferred.
