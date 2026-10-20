# SSI Protocol Background

This page captures the SSI protocol description and engineering context discussed in the project report. It is a summary of the report, not a replacement for vendor specifications.

## Why an absolute encoder?

The report frames absolute encoders as position-feedback devices used in servo drives, CNC axes, robotic joints, rotary tables, and related automation systems. An absolute encoder reports a position code directly, including after power-up; an incremental encoder typically reports relative motion through pulse trains and depends on counting / reference procedures.

The project's frame combines single-turn position with a multiturn revolution count.

## Working model described in the report

SSI is a synchronous, unidirectional data interface in which the receiving controller normally supplies the clock. The encoder shifts out the current position word, typically MSB-first. A transaction consists of a clock burst for the required number of bits, followed by an idle interval before a new position is loaded for the next transaction.

The report describes the usual encoder-side sequence as:

1. The first falling clock edge latches the current position into the output register.
2. The following rising edge presents the MSB.
3. Subsequent clock edges advance the output register one bit at a time.
4. After the expected number of bits, the master stops clocking.
5. The clock remains idle for the specified monoflop interval before the next transaction.

The project report's own validation setup generated the clock internally with TIM2 and observed the signal on a scope; it did not use a separate external SSI master. This distinction matters when interpreting the experimental evidence.

## Electrical interface

The report describes field SSI wiring as a differential interface commonly implemented with RS-422 or RS-485 transceivers, using a clock pair and a data pair (Clock+, Clock−, Data+, Data−). Differential signalling is used to improve tolerance to common-mode interference in electrically noisy industrial environments.

The development board used in the reported setup does not contain the external differential line-driver stage required for a typical cable-based field interface. The report explicitly excludes a field-ready line-driver design from scope.

## Timing parameters

The report describes the following timing concepts:

- **Clock period T:** time between clock edges / cycles.
- **Propagation delay tv:** time from a clock edge to the corresponding data transition.
- **Monoflop / idle time tm:** required idle interval between frames.

The report states the design should keep data transitions within the clock-high window and gives the constraint tv < T/2 as a timing rationale. The implemented firmware places the TIM2 CC4 compare event within the clock-high portion of the PWM period so data is updated away from the clock edge.

A 30 µs monoflop is stated as the design target, but the report contains a 65 µs caption for one 10 kHz capture. The actual idle interval remains a follow-up measurement, not a settled quantitative result.

## Cable-length example reproduced in the report

The report reproduces the following recommended values from the cited SICK SSI description:

| Cable length | Recommended baud rate in report |
|---|---:|
| Less than 50 m | Less than 400 kHz |
| Less than 100 m | Less than 300 kHz |
| Less than 200 m | Less than 200 kHz |
| Less than 400 m | Less than 100 kHz |

These are quoted as the report's cited vendor guidance. Check the current encoder manufacturer's documentation for a real installation.

## Strengths and limitations

The report lists the main SSI strengths as low protocol overhead, simple fixed-frame operation, multi-vendor adoption, and differential-line robustness. Limitations noted include the absence of a generally mandatory CRC in base SSI, optional/vendor-specific error fields, unidirectional data, and reduced maximum baud rate as cable length increases.

For the project's comparison with bidirectional EnDat 2.2, see [SSI and EnDat comparison](EnDat-comparison-and-future-work.md).
