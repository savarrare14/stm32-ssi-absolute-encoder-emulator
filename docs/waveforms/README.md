# Oscilloscope Waveform Gallery

This gallery collects four oscilloscope captures from the SSI emulator bench tests. They are original measurement images, not synthetic waveforms.

- **Channel A (red):** SSI clock.
- **Channel B (blue):** serial data.
- **Vertical scale:** 2 V/division and DC coupling for both channels, as indicated in the capture notes.
- **Test settings:** 10 kHz and nominal 100 kHz configurations.

## Figure 8.1 — 10 kHz, 400 µs/div

![Figure 8.1: 10 kHz SSI clock and data at 400 microseconds per division](fig-8-1-10khz-400us-div.png)

The capture shows repeated 32-pulse clock bursts, visible inter-frame idle intervals, and data transitions during the clock-high window.

## Figure 8.2 — 10 kHz, 1 ms/div

![Figure 8.2: 10 kHz SSI clock and data at 1 millisecond per division](fig-8-2-10khz-1ms-div.png)

Three successive frame bursts are visible. The capture caption labels the monoflop time as 65 µs, which differs from the 30 µs design target. Measure the actual gap to resolve the discrepancy.

## Figure 8.3 — Nominal 100 kHz, 100 µs/div

![Figure 8.3: nominal 100 kHz SSI clock and data at 100 microseconds per division](fig-8-3-100khz-100us-div.png)

The capture shows 32-pulse clock bursts and data transitions within the clock-high window.

## Figure 8.4 — Nominal 100 kHz configuration, frequency counter capture

![Figure 8.4: scope capture with automatic frequency counter reading 93.08 kHz](fig-8-4-config-100khz-readout-93-08khz.png)

The automatic frequency counter reads about 93.08 kHz. The measurement window may include the inter-frame gap, but the capture alone cannot establish this. Measure adjacent clock edges inside one burst to confirm the within-burst rate. The caption says 30 µs/div while the on-screen scale appears to show 40 µs/div; check the original instrument settings if precise time-scale metadata is needed.

## How to extend this gallery

When adding new measurements, include the configuration, timebase, vertical scale, probe points, date, and what the image demonstrates. Keep raw captures separate from annotated copies so the evidence remains auditable.
