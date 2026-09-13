## Hi there 👋

<!--
**chrislafave/chrislafave** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
# Project Portfolio — Chris LaFave

github.com/chrislafave · chris.r.lafave@gmail.com · linkedin.com/in/chris-lafave

Current, self-directed bench work. Full write-ups, schematics, firmware source, and photos in the linked repos.

---

## Project 1 — ESP32-S3 Four-Channel Bench Data Logger (2025 – Present)

**Problem.** Needed long-duration logging of low-frequency analog signals on the bench (thermal drift, supply rail sag, sensor output) without tying up an oscilloscope, and wanted results viewable from another room.

**Approach.**
- Analog front end: ADS1115 16-bit ADC, four single-ended channels, RC anti-alias filtering, protective input clamping.
- Controller: ESP32-S3 running ESP-IDF, firmware in C, sampling and networking split across FreeRTOS tasks with a queue between them.
- Interfaces: on-board HTTP server with a live-updating chart, MQTT publish to a local broker, CSV to microSD as the fallback record.
- Hardware: 2-layer PCB designed in KiCad — separated analog and digital ground pours joined at a single point, decoupling cap placed at each supply pin.

**Verification.**

| Test | Method | Result |
|---|---|---|
| DC accuracy | Compared against calibrated reference, 0–4.096 V in 0.25 V steps | ±0.3% of reading |
| Noise floor | Shorted input, 10,000 samples | 1.2 LSB RMS |
| Wi-Fi dropout recovery | Forced AP power cycle | Reconnect and resume logging in under 8 s, no sample loss to SD |

**What I learned.** The first board revision shared a ground pour and placed the switching regulator too close to the ADC input traces — the noise floor was about four times worse. Relocating the regulator and reworking the analog return path fixed it. Both revisions are kept in the repo to show the before/after measurement.

**Artifacts.** Schematic and layout (KiCad), firmware source, bring-up checklist, test procedure and results, photos of both board revisions.

---

## Project 2 — 2.4 GHz Antenna Selection and Matching Study (2025)

**Problem.** Which antenna option actually performs best on a small ESP32 board, and how much does matching matter in practice?

**Approach.** Built one target board with three populated antenna options (PCB trace, ceramic chip, external whip via U.FL/SMA). Measured return loss on each, tuned a pi-network match on the chip antenna, then ran repeatable range tests: fixed AP location, marked distances, 500 pings per point, logging RSSI and packet loss.

**Results.** Matching improved usable range by roughly 35% over the untuned chip antenna; the external whip outperformed both PCB options by a further margin, at the cost of an extra connector and assembly step.

**What I learned.** The mechanical and BOM cost of a connector is a real trade-off against RF gain — the "best" antenna depends on the enclosure and product volume, not just S11. Fixture repeatability dominated the measurement until positions were clamped and marked.

**Artifacts.** Return-loss plots, matching network design notes, range-test data and logging script.

---

## Project 3 — Linear Bench Supply Repair and Characterization (2024)

**Problem.** A dead 0–30 V linear bench supply, no output, no documentation beyond a service manual.

**Approach.** Traced from the output back through the regulation loop, isolated a shorted pass transistor and two out-of-spec filter capacitors, sourced replacements, and reworked the board. Characterized the repaired unit rather than assuming it was fine.

**Results.** Load regulation 0.02% from no-load to 2 A; output ripple reduced to 4 mV p-p, measured with a short-ground-spring probe.

**What I learned.** Probe technique determines what you measure — an early ripple reading came in roughly 3x high because of loop-inductance pickup in the probe ground lead. The corrected measurement and the mistake are both documented.

**Artifacts.** Fault-isolation notes, before/after ripple scope captures, bill of materials for the repair.


