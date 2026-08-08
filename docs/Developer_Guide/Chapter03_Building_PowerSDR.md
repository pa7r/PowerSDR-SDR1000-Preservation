# Chapter 3 — Building PowerSDR

## 3.1 First build
The first objective is a clean build without modifications. Open `PowerSDR.sln` in the selected Visual Studio version.

## 3.2 Configuration

```text
Configuration: Release
Platform:      x86
```

Use Debug/x86 while troubleshooting.

## 3.3 Typical projects
Depending on the source revision, projects may include PowerSDR, Console, DttSP, FlexLib, and Setup/installer projects.

## 3.4 Common problems
If DirectX assemblies are missing, verify the required legacy SDK and managed assemblies. If unsafe code is disabled, enable **Project Properties -> Build -> Allow unsafe code**. Keep 32-bit runtime DLLs with the x86 build.

## 3.5 Testing
Test a new executable in a separate directory. Verify SDR-1000 detection, receive audio, PTT/transmit operation, CAT, sound-card selection, and sample-rate configuration.

## 3.6 Development discipline
Record source revision, changed files, compiler version, OS, hardware configuration, and results for every experiment.
