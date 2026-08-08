# Chapter 2 — Development Environment

## 2.1 Starting point
Begin by building the legacy SDR-1000-compatible PowerSDR source tree unchanged. Establish a clean baseline before modifying code.

## 2.2 Visual Studio
Visual Studio 2015 or 2017 is a sensible starting point for compatibility testing with legacy PowerSDR code. Install the .NET desktop development tools and appropriate .NET Framework targeting packs.

## 2.3 DirectX
Some legacy PowerSDR components use DirectX/Managed DirectX-era APIs. The June 2010 DirectX SDK may be required by the particular source revision.

## 2.4 Platform
Use **x86** initially. Legacy SDR-1000 interface components such as `inpout32.dll` are 32-bit.

## 2.5 Git
Keep the original upstream source separate from experimental branches.

```text
git clone <source-repository>
git checkout -b experimental
```

## 2.6 Debug and Release
Use Debug for investigation and Release for reproducible operational builds. Record compiler, OS, source revision, and dependency versions.

## 2.7 Reproducibility
A future goal is a completely reproducible build environment with exact tool and dependency versions.
