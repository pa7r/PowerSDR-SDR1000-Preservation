# Chapter 4 — PowerSDR Source Code Architecture

## 4.1 Purpose

This chapter documents the architecture of the KE9NS PowerSDR v2.8 source tree and provides a starting point for developers who want to build, debug, or modify the software.

KE9NS describes the project as based on FlexRadio's GPL PowerSDR 2.7.2 source. The project documentation identifies five components: DttSP (C), PowerMate (C++), PowerSDR (C#), Inno Setup, and Advanced Installer. KE9NS's current source documentation describes a Visual Studio 2022 / .NET Framework 4.5.2 build environment.

## 4.2 High-level architecture

```text
                 +----------------------+
                 |      PowerSDR        |
                 |        C#            |
                 | GUI / configuration  |
                 | radio control / CAT  |
                 +----------+-----------+
                            |
                            v
                 +----------------------+
                 |        DttSP         |
                 |          C           |
                 | DSP / FFT / filtering|
                 | demodulation / AGC   |
                 +----------+-----------+
                            |
                            v
                 +----------------------+
                 |    Sound-card I/O    |
                 |       ADC / DAC      |
                 +----------+-----------+
                            |
                            v
                 +----------------------+
                 |      SDR-1000        |
                 |    RF / I-Q hardware |
                 +----------------------+
```

This is a conceptual architecture; exact call paths must be verified against the source revision being documented.

## 4.3 Main components

### DttSP
DttSP is the native C DSP component. It provides the computational signal-processing layer used by PowerSDR, including filtering, FFT processing, demodulation, AGC, and I/Q processing. The principal runtime output is `DttSP.dll`.

### PowerSDR
PowerSDR is the main C# application. It provides the GUI and coordinates radio configuration, frequency and mode control, audio configuration, user settings, meters, panadapter/waterfall display, CAT/external control, and interaction with the native DSP layer and radio hardware.

### PowerMate
PowerMate is the C++ component associated with Griffin PowerMate support. KE9NS documents this support as integrated into PowerSDR v2.8.

### Installers
KE9NS documents Inno Setup for the full installer and Advanced Installer for the incremental installer. These are distribution components, not part of the real-time DSP path.

## 4.4 Receive path

```text
Antenna
   |
RF / analog hardware
   |
I/Q baseband
   |
Sound-card ADC
   |
PowerSDR input path
   |
DttSP
   |
Filtering / demodulation / AGC / DSP
   |
Audio output / GUI
```

The SDR-1000 differs from modern networked SDRs because the PC sound card is a central part of the I/Q data path. Consequently, sound-card configuration can affect sample rate, bandwidth, latency, channel alignment, I/Q balance, noise, and spurious responses.

## 4.5 Transmit path

```text
Microphone / digital audio
          |
     PowerSDR input
          |
        DttSP
          |
      TX DSP
          |
      I/Q output
          |
     Sound-card DAC
          |
       SDR-1000
          |
     RF amplification
          |
        Antenna
```

Transmit control also includes PTT/MOX, band selection, sequencing, and radio control. A working PTT signal therefore does not by itself prove that the TX audio/DSP path is functioning.

## 4.6 Control path

```text
User action
    |
PowerSDR GUI
    |
Application state
    |
Radio-control layer
    |
SDR-1000 hardware
```

Examples include frequency, mode, PTT/MOX, band selection, hardware configuration, CAT commands, and configuration changes.

## 4.7 CAT and external control

For troubleshooting CAT, separate the problem into:
1. command received;
2. command parsed;
3. internal state changed;
4. hardware command issued;
5. hardware response observed.

This prevents CAT failures from being incorrectly diagnosed as DSP failures.

## 4.8 Native/managed boundary

```text
C# PowerSDR
      |
      | managed/native interface
      v
C DttSP
      |
      v
DSP processing
```

Problems at this boundary can involve incorrect parameters, buffer-size mismatches, calling conventions, native memory corruption, or DSP errors.

## 4.9 Real-time processing

```text
Sound card
   |
buffer arrives
   |
DSP processes buffer
   |
output buffer
   |
repeat
```

Long-running operations such as file access, network access, database queries, extensive logging, or expensive GUI work should not be inserted into time-critical callbacks without careful testing.

## 4.10 Where a developer should start

1. Build the exact source revision without modifications.
2. Run the application and verify runtime dependencies.
3. Trace one simple GUI operation, such as changing frequency.
4. Trace receive audio from sound-card input through DttSP.
5. Trace transmit audio from microphone input to sound-card output.
6. Only then begin modifying DttSP.

## 4.11 KE9NS source markers

KE9NS documents `//ke9ns add` for code added by KE9NS and `//ke9ns mod` for modifications to the original 2.7.2 source. These markers are useful for source archaeology, but Git history and comparison against the original source are preferable when available.

## 4.12 Build outputs

KE9NS documentation identifies:
- `PowerSDR.exe`
- `PowerSDR.config.exe`
- `DttSP.dll`
- `PowerMate.dll`

It also notes that `vcruntime140.dll` may be required on the target system. Additional DLLs can depend on the exact source revision and enabled features.

## 4.13 Future source-level documentation

Future revisions should map important classes, native functions, C#/C interop declarations, audio callbacks, DSP entry points, CAT handlers, SDR-1000 hardware-control functions, PTT/MOX handling, configuration/database classes, and thread creation/synchronization.

## 4.14 Verification rule

This project should distinguish architecture that has been verified from hypotheses. Before publishing a statement such as “Function X calls Function Y”, verify it against the exact source revision.

An explicitly documented unknown is better than an incorrect but plausible description.

## References

- KE9NS PowerSDR source/build documentation: https://ke9ns.com/flexpage.html
- KE9NS revision history: https://www.ke9ns.com/revision.html
- KE9NS PowerSDR source repository: https://github.com/ke9ns/PowerSDR_ke9ns_v2.8.0/
