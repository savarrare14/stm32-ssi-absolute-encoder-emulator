# Oscilloscope Waveform Gallery

These four oscilloscope screenshots are extracted from the technical report and committed in this folder. They are report evidence, not new or synthetic captures.

- **Channel A (red):** SSI clock.
- **Channel B (blue):** serial data.
- **Vertical scale:** report states 2 V/division and DC coupling for both channels.
- **Test settings:** 10 kHz and nominal 100 kHz configurations.

## Figure 8.1 — 10 kHz, 400 µs/div

The report describes repeated 32-pulse clock bursts with visible inter-frame idle intervals and data changes during the clock-high window.

## Figure 8.2 — 10 kHz, 1 ms/div

The report says three successive frame bursts are visible. Its caption labels the monoflop time as 65 µs. This differs from the 30 µs design target described elsewhere, and must be resolved using a direct measurement.

## Figure 8.3 — Nominal 100 kHz, 100 µs/div

The report describes 32-pulse clock bursts and data transitions confined within the clock-high window.

## Figure 8.4 — Nominal 100 kHz configuration, frequency counter capture

The on-screen automatic frequency counter reads about 93.08 kHz. The report suggests that the measurement window may include the inter-frame gap; the screenshot alone does not prove this explanation. Measure the period between adjacent clock edges inside a single burst to confirm the within-burst rate. The report caption also labels this figure 30 µs/div while the on-screen scale appears to show 40 µs/div; verify against the original instrument settings if precision is important.

## How to extend this gallery

When adding new measurements, include the configuration, timebase, vertical scale, probe points, date, and what the image demonstrates. Keep raw captures separate from annotated copies so the evidence remains auditable.
