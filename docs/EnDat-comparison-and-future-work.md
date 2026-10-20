# SSI and EnDat 2.2: Comparison and Future Work

This summary follows the comparison and future-work framing in the project report. It is a conceptual comparison, not an EnDat implementation.

## Comparison in the report

| Attribute | SSI | EnDat 2.2 |
|---|---|---|
| Data direction | Unidirectional encoder-to-master data | Bidirectional data line with turnaround |
| Communication model | Fixed-format position frame | Mode-command based |
| CRC | Base SSI has no general mandatory CRC; optional vendor-specific status/error bits may be present | Report describes CRC-based error detection |
| Diagnostics | Limited to optional status/error information | Supports richer parameter and diagnostic access |
| Implementation complexity | Lower for a fixed position word | Higher: turnaround timing and command decoding |
| Ecosystem | Multi-vendor convention | HEIDENHAIN-defined ecosystem |
| Project relevance | Implemented protocol and timing prototype | Future comparison / possible extension |

## Future work identified by the report

1. Add a CRC or parity scheme if the frame format is extended and its compatibility implications are explicitly documented.
2. Quantify SPI1 pass/fail counts and bit-error statistics over extended runs.
3. Measure the actual TIM6 idle interval and reconcile it with the documented 30 µs design target and the waveform caption that reports 65 µs.
4. Use time cursors to measure within-burst clock period; do not rely solely on a counter window that might span the gap.
5. Sweep the clock above 100 kHz to establish the practical limit of interrupt-driven bit updates.
6. Consider DMA-based frame handling if needed for higher rates or multiple channels; the described implementation did not use DMA.
7. Add an appropriate RS-422/RS-485 differential transceiver stage before any cable-based field deployment.
8. For an actual EnDat 2.2 implementation, obtain and follow the authoritative Interface Specification and implement bidirectional line turnaround, mode-command decoding, and the required protocol integrity behavior.

## Boundary of current work

The report's EnDat discussion is architectural/documentary only. It does not claim that an EnDat 2.2 physical layer or command protocol was implemented.
