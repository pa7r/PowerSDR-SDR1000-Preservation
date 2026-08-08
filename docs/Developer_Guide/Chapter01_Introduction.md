# Chapter 1 — Introduction to the FlexRadio SDR-1000

## 1.1 Purpose
The FlexRadio SDR-1000 was an early commercially successful amateur-radio software-defined radio. It used a PC sound-card interface and software DSP as major parts of its radio architecture.

## 1.2 Why it matters
The SDR-1000 demonstrated that a general-purpose computer could perform sophisticated amateur-radio signal processing in real time. PowerSDR became the principal user interface and control application associated with the platform.

## 1.3 High-level architecture

```text
RF Front End -> I/Q Conversion -> Sound Card -> PowerSDR/DttSP -> Audio/UI
                                      ^                 |
                                      |                 v
                                      +---- Control --- SDR-1000
```

## 1.4 Preservation project
This project preserves technical knowledge scattered across old software releases, forums, mailing lists, manuals, and individual experiments. It also documents reproducible development and measurements.

## 1.5 Documentation principle
Historical facts, source-code observations, measurements, and hypotheses should be clearly distinguished.
