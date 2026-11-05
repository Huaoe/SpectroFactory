---
title: 'Product Brief: SpectroFactory'
status: final
created: '2026-11-05'
updated: '2026-11-05'
inputs:
  - _bmad-output/planning-artifacts/research/technical-smartphone-spectrophotometer-guide-2026-11-05/research.md
---

# Product Brief: SpectroFactory

## Executive Summary

SpectroFactory is a React website that walks a maker, step by step, from an empty print bed to a working copy of the 3D-printable, open-source, modular double-beam visible spectrophotometer (DBS) published by Winters et al. in HardwareX (2021). The design is openly licensed (CC BY-SA 4.0) and ships as 25 printable parts plus off-the-shelf components. Its authors showed that it tracks a commercial lab spectrophotometer on a Beer's-law test.

SpectroFactory makes one deliberate change: **the smartphone is replaced by an ESP32-CAM**, an Arduino-programmable board with an OV2640 camera, Wi-Fi and Bluetooth. The paper already provides for swapping the detector by replacing the phone mount with its blank mounting plate (DBS25). The ESP32-CAM makes the instrument self-contained. Its firmware can lock white balance, exposure and gain, which a phone browser cannot do reliably, and spectra are read over Wi-Fi.

The problem is not the design but the path to building it. The material is scattered across a journal article, an OSF file repository, a spreadsheet bill of materials and an Onshape model, and none of it covers an ESP32-CAM. SpectroFactory turns this into one guided build: buy, print, fit-check, assemble, flash the firmware, connect, calibrate, measure.

This is a personal project. The first goal is for Thomas, who does not do 3D modelling, to build the instrument himself using the site as his guide. A second goal is to leave behind a public guide other makers can follow.

## The Problem

- **Scattered sources.** The build instructions are in the HardwareX paper, the STL/STEP files and BOM are on OSF (`7ewb3`), and the editable CAD is on Onshape. Nothing ties a printed part (e.g. `DBS22 Slit Mount`) to the step where it is used.
- **No ESP32-CAM path.** The paper validates phones and a USB camera. Mounting an ESP32-CAM on the DBS25 blank plate, aligning it with the grating and focusing its lens are left to the builder. Thomas cannot model an adapter himself.
- **Printer variance is left to the builder.** The authors printed in PLA at 20% infill on a MakerBot Replicator. They warn that fits vary by printer, that burrs and raft must be trimmed, and that binding parts may need reprinting.
- **Easy-to-miss failure points.** Light leaks, slit assembly with blades, grating orientation (grooves horizontal) and recalibration after any detector movement are only described in prose.

## The Solution

A guided, linear build website, plus a measurement page for the finished instrument:

1. **Overview**: what the instrument does and does not do, effort, approximate cost.
2. **Shopping list**: non-printed parts from the BOM (grating film, utility-knife blades, cuvettes, light sources) plus the AI-Thinker ESP32-CAM, its ESP32-CAM-MB USB programmer board and power supply, with a checklist.
3. **Print plan**: every DBS part except the phone mounts, plus a ready-made **ESP32-CAM adapter** for the DBS25 plate. Each part has a 3D preview, an STL download, material (black PLA/PETG), settings and orientation hints. Fit-test parts come first.
4. **Fit check**: print the small mating parts and the adapter, check them, adjust before printing the rest.
5. **Assembly**: one step per sub-assembly (slit mount, grating mount, ESP32-CAM adapter, sample holder, source adapter), each listing its parts, with a 3D view and a done checkbox.
6. **Firmware**: flash a provided Arduino sketch that locks camera settings and serves spectra over Wi-Fi. No coding required, only copy, configure Wi-Fi and upload.
7. **First light**: connect over Wi-Fi, focus the lens, run the light-leak check, and see a spectrum.
8. **Calibration and first measurement**: fluorescent-lamp calibration (436/546/611 nm lines) and the paper's copper-sulfate Beer's-law exercise.

Build progress is saved in the browser so the build can span several days. No account or server is needed for the guide itself.

## What Makes This Different

- **One instrument, done properly.** A single validated design with each part and step linked, not a catalogue of spectrometers.
- **Wireless and self-contained detector.** An ESP32-CAM with locked camera settings gives more repeatable readings than a phone, and no phone has to be clamped to the instrument.
- **No 3D skills needed.** The ESP32-CAM adapter is supplied ready to print, generated from code that can be adjusted from a few measurements.
- **Faithful to the source.** Keeps the authors' parts, numbering (DBS01–DBS25) and method, with attribution, under the same CC BY-SA license for anything derived.

No technical moat is claimed. The value is curation, sequencing and the ESP32-CAM integration.

## Who This Serves

- **Primary: Thomas.** An intermediate maker with a Bambu Lab printer, comfortable following Arduino instructions but not modelling in 3D. He wants to build the DBS from the PDF with an ESP32-CAM. Success: a working, light-tight instrument streaming a clean spectrum over Wi-Fi.
- **Secondary: other makers.** Hobbyists, teachers or students with a consumer FDM printer who want to reproduce the same instrument, in French or English.

## Success Criteria

- Thomas completes the full build using only the site, with no step requiring a return to the PDF.
- Every DBS part used and every BOM line appears in the guide with its source and the step that uses it.
- The ESP32-CAM adapter fits on the first or second print.
- A recognisable fluorescent-lamp spectrum is visible on the measurement page over Wi-Fi, with stable readings between captures (camera auto-settings off).

## Scope

**In (v1)**
- Linear step-by-step guide covering shopping, printing, fit check, assembly, firmware, first light, calibration and the copper-sulfate measurement.
- Part pages with 3D preview and STL downloads (original OSF files with attribution, plus the ESP32-CAM adapter).
- ESP32-CAM adapter for the DBS25 plate, modelled in code (parametric), printable without 3D skills.
- Arduino firmware for the ESP32-CAM: locked white balance, exposure and gain; spectrum/image access over Wi-Fi.
- Measurement page that displays the live spectrum from the ESP32-CAM.
- Bilingual content: French and English.
- Print settings tested on a Bambu Lab printer; other FDM printers covered by generic settings.

**Out (v1)**
- Smartphone detector and the DBS phone mounts.
- Other spectrometer designs.
- Bluetooth data transfer (too slow for images; possible later for control only).
- Cloud storage, user accounts, kit sales.

**Open**
- How the HTTPS guide site reaches the ESP32-CAM on the local network (browsers normally block HTTPS pages from calling local `http://` devices). Options include serving the measurement page from the ESP32 itself. To decide in architecture.
- Which Bambu Lab model (affects bed size and print-plate grouping).
- Whether the OSF files and BOM are still complete, and current part prices (paper figures date from 2021).

## Vision

If the guide works for Thomas's build, it becomes the reference build guide for an ESP32-CAM version of the DBS. Other makers can follow it, contribute adapters for other camera boards, and share calibrated spectra. The same structure could later host other modules of the design (fluorescence cover, alternative light sources) and Bluetooth control.
