# CMOS Differential Amplifier with Active Load (GPDK180)

An end-to-end analog IC design, simulation, full-custom layout, and physical verification of a single-stage CMOS Differential Amplifier with an Active PMOS Current-Mirror Load, implemented in Cadence Virtuoso using the GPDK180 (180 nm) technology node.

---

## Technical Specifications & Performance Summary

| Parameter | Target Spec | Pre-Layout (Schematic) | Post-Layout (PEX) | PVT Variation Range |
| :--- | :--- | :--- | :--- | :--- |
| **Supply Voltage ($V_{DD}$)** | 1.8 V | 1.8 V | 1.8 V | 1.62 V – 1.98 V |
| **Differential Gain ($A_d$)** | > 30 dB | **34.81 dB** | **34.81 dB** | 24.86 dB – 36.83 dB |
| **-3dB Bandwidth** | > 500 kHz | **1.059 MHz** | **1.055 MHz** | 830.3 kHz – 2.817 MHz |
| **Gain Bandwidth (GBW)** | — | **36.86 MHz** | **36.72 MHz** | 29.49 MHz – 70.03 MHz |
| **Common-Mode Gain ($A_{cm}$)**| — | -26.03 dB | — | — |
| **CMRR** | > 50 dB | **60.84 dB** | — | — |
| **Quiescent Power** | Low Power | **118 µW** | 118 µW | 106.2 µW – 128.5 µW |
| **Peak-to-Peak Output Swing**| — | **1.50 V** (83% $V_{DD}$) | — | — |

---

## Circuit Architecture & Device Sizing

The amplifier consists of a 5-transistor OTA topology with an additional NMOS diode-connected reference device for tail current biasing ($I_{BIAS} = 50\text{ }\mu\text{A}$).

| Device | Role | Type | Width ($W$) | Length ($L$) | $W/L$ Ratio | Quiescent Current ($I_D$) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **M1, M2** | Differential Input Pair | NMOS | 6.5 µm | 360 nm | 18.06 | 23.71 µA |
| **M3, M4** | Active Mirror Load | PMOS | 1.5 µm | 360 nm | 4.17 | 23.71 µA |
| **M5** | Tail Current Source | NMOS | 3.6 µm | 360 nm | 10.00 | 47.42 µA |
| **M6** | Reference Current Diode | NMOS | 3.6 µm | 360 nm | 10.00 | 50.00 µA |

---

## Design Flow & Verification Steps

1. **DC Operating Point Analysis:** Confirmed all six transistors operate strictly in the saturation region (Region 2).
2. **AC Small-Signal Analysis:** Verified differential gain ($A_d$), common-mode rejection ratio (CMRR), and dominant output pole position.
3. **Transient Response:** Evaluated large-signal step response with a 200 mV peak-to-peak differential input step, demonstrating an output swing between 0.29 V and 1.79 V without ringing or overshoot.
4. **PVT Corner Analysis:** Verified performance across **46 total corners** combining 5 process corners (FF, FS, NN, SF, SS), 3 supply voltages (1.62 V, 1.80 V, 1.98 V), and 3 temperatures (-40°C, 27°C, 90°C). All transistors stayed in saturation across all corners.
5. **Monte Carlo Mismatch Analysis:** Evaluated random process mismatch impact across sample runs (Mean $A_d = 31.98\text{ dB}$, $\sigma = 3.95\text{ dB}$).
6. **Full-Custom Layout & Verification:** 
   * **DRC (Design Rule Check):** Clean (0 violations).
   * **LVS (Layout Versus Schematic):** Clean (0 net/device/pin/parameter mismatches).
   * **PEX (Parasitic Extraction):** Post-layout AC response matched pre-layout within **< 0.4% degradation**.

---

## Repository Structure

```text
DIFF_AMPLIFIER/
├── Differential_Amp/     # Cadence Virtuoso schematic, layout, & maestro views
├── diff_amp_layout.gds   # Exported GDSII layout file
├── cds.lib               # Cadence library configuration file
├── drc.sum               # DRC physical verification summary
├── lvs.sum               # LVS matching summary
├── .gitignore            # Excludes log files, locks, and temporary run directories
└── README.md             # Project documentation
