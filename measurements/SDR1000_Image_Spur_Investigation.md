# SDR-1000 Image/Spur Investigation

## Initial observations
Previous testing reported a recurring signal/image below center frequency, including tests with different sample rates, different sound cards, no antenna, and preamp changes.

## Items to verify
- Offset around 10–13 kHz below center in some configurations
- Persistence with more than one sound card
- Presence without an antenna
- Different behavior with preamp settings
- Band-dependent severity

## Next measurements
1. Record exact center and spur frequencies.
2. Repeat at several sample rates.
3. Record FFT size and display settings.
4. Compare RX and TX paths.
5. Test sound-card inputs with the radio disconnected.
6. Compare raw I/Q channels if accessible.
7. Test IQ gain/phase calibration.
8. Determine whether the offset follows sample rate or remains fixed.

No cause is asserted until repeatable measurements support it.
