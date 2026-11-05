# Addendum — SpectroFactory brief

Detail for downstream documents (PRD, architecture), not for the brief itself.

## Technical direction from research (for architecture)
- 3D preview: React Three Fiber `STLLoader` → `BufferGeometry` mesh. Pre-generated STLs from OSF `7ewb3` (STEP also available; original CAD on Onshape).
- Parametric phone mount generator (v2 candidate): OpenSCAD-WASM, replicad or Manifold. DBS CAD is Onshape/STEP, so it would have to be re-modelled.
- In-browser capture (v2 candidate): `getUserMedia` plus Image Capture constraints (`exposureMode`, `whiteBalanceMode`). Works in Chrome on Android; iOS support is disputed, so feature-detect and fall back to photo upload.

## Source facts to carry into the PRD
- Parts DBS01–DBS25; mounts: DBS15 iPhone (SE–X), DBS16 centre camera, DBS17 USB camera, DBS25 blank plate.
- Print: PLA, 20% infill, trim burrs and raft, test-print fit, reprint binding parts in another orientation.
- Optics: film grating in DBS03, grooves horizontal; slit = DBS22 + 2× DBS23 + 2 miniature utility-knife blades.
- Calibration: CFL lines 436/546/611 nm (488 nm as fourth); recalibrate after each phone re-attachment.
- Validation exercise: CuSO₄·5H₂O 1.2 M stock, 11 serial 2× dilutions, A = −log10(P/P0) at 800 nm.
- License: CC BY-SA 4.0. Derived parts must stay share-alike with attribution to Winters et al.
- Safety notes needed: laser pointers, UV LED, copper sulfate (not yet researched).

## ESP32-CAM detector (decided 2026-11-05)
- Board: ESP32-CAM (OV2640, Wi-Fi + Bluetooth), Arduino IDE. Precedents: CloudACM ESP32-Cam Spectroscope (2023-10-28, https://www.cloudacm.com/?p=4511); Kondo, Harvard, ESP32-S3 + OV2640/OV5640 additively manufactured spectrometer (BIO Web of Conferences 2024).
- Firmware can disable AWB (`set_whitebal(s,0)`, `set_awb_gain(s,0)`), auto-exposure (`set_exposure_ctrl(s,0)`, fixed `set_aec_value`) and auto-gain (`set_gain_ctrl(s,0)`) via esp32-camera `sensor_t` (Random Nerd Tutorials; espressif/esp32-camera `ov2640.c`).
- Mounting: paper says other detectors replace the phone mount with DBS17 (USB camera) or a modified DBS25 blank plate. Adapter to be written as parametric code (OpenSCAD) so Thomas needs no CAD skills.
- Bluetooth bandwidth unsuitable for images; Wi-Fi for data, Bluetooth optional for control.
- Architecture risk: browsers normally block mixed content / private-network requests from an HTTPS site to `http://` LAN devices — candidate fixes: ESP32 serves the measurement UI (or the whole app) in AP/station mode, or the guide is served locally. Needs verification.
- Lens focus: OV2640 lens is screwed and can be turned to focus (as paper says for USB camera).
