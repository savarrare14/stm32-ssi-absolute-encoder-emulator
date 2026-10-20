# SSI Frame and Gray-Code Design

## SSI operating context

SSI is a synchronous, unidirectional encoder interface. A receiving controller normally supplies a burst of clock pulses while the encoder shifts out the position word MSB-first. A basic SSI position frame has no universally required header, address, or CRC; some device families append vendor-specific status/error bits. Field interfaces commonly use differential signalling such as RS-422 or RS-485.

This prototype uses a TIM2 PWM output as its clock source during internal bench testing rather than an external SSI master clock. See [System Architecture](system-architecture.md).

## 32-bit frame

The project frame allocation is:

| Bit range (MSB–LSB) | Width | Field |
|---|---:|---|
| 31–14 | 18 bits | Single-turn position |
| 13–2 | 12 bits | Multiturn revolution count |
| 1–0 | 2 bits | Status / error |

The fields are assembled into a 32-bit composite word, the complete word is converted to Gray code, and the result is shifted out MSB-first.

## Binary-to-Gray conversion

For an n-bit binary value B, the Gray-coded value G is:

G = B XOR (B >> 1)

The MSB remains unchanged; each subsequent Gray bit is the XOR of the corresponding binary bit and the next-more-significant binary bit.

Gray code ensures adjacent numerical values differ by one bit, which can reduce the risk of a receiver sampling a multi-bit transition as a different position.

## Position range

- Single-turn range: 2^18 = 262,144 values, from 0 through 262,143.
- Multiturn counter: 12 bits, giving 4096 values before wrapping.
- Frame width: 18 + 12 + 2 = 32 bits.

The lower 18 bits of TIM5 provide the single-turn count. Software detects the 18-bit rollover and increments the multiturn counter.

## Frame integrity check

SPI1 is configured to receive the clock/data signals and reconstruct a 32-bit frame. The receive-complete interrupt compares the received value against the corresponding value loaded for transmission, accumulating pass/fail counters.

The repository does **not** yet include consolidated pass/fail totals or a quantitative bit-error rate. Add these values from actual debug counters or serial logs rather than inferring them.
