# sky130pdk_Tap-Cells
# SKY130 CMOS Well and Substrate Tap Cells (`ntap` & `ptap`)

Custom physical layout implementations of N-well tap (`ntap`) and P-substrate tap (`ptap`) cells designed in **Magic VLSI** using the open-source **SkyWater SKY130 PDK (`sky130A`)**.

## Overview
In bulk CMOS processes, parasitic PNP and NPN bipolar junction transistors form a four-layer PNPN thyristor structure between `Vdd` and `Vss`. Without low-impedance body ties placed at regular intervals, substrate or well currents can forward-bias these junctions and trigger **latch-up**. 

These tap cells provide dedicated ohmic contacts to anchor the N-well to `Vdd` and the P-substrate to `Vss`, stabilizing body potentials across standard-cell rows and analog layout blocks.

---

## Cell Architectures & Layer Stack

### 1. N-Well Tap Cell (`ntap`)
* **Supply Tie:** `Vdd`[cite: 1]
* **Function:** Establishes a low-resistance ohmic connection between the N-well and the `Vdd` power rail to reverse-bias the N-well/P-substrate junction.
* **Layer Stack (Bottom to Top):**
  1. **N-Well (`nwell`):** Encloses the entire tap structure to define the n-type well region[cite: 1].
  2. **N-Tap Implant (`ntap` / `nsd`):** Heavily doped $n^+$ region inside the N-well for ohmic contact formation[cite: 1].
  3. **Diffusion-to-LI Contact (`licon`):** Connects the $n^+$ tap region upward to Local Interconnect[cite: 1].
  4. **Local Interconnect (`li1`):** SKY130 local routing layer bridging `licon` and `mcon`[cite: 1].
  5. **LI-to-Metal1 Via (`mcon`):** Connects `li1` to Metal 1[cite: 1].
  6. **Metal 1 (`met1`):** Routes upward from the contact stack to the `Vdd` pin/rail[cite: 1].

### 2. P-Substrate Tap Cell (`ptap`)
* **Supply Tie:** `Vss`[cite: 2]
* **Function:** Establishes a low-resistance ohmic connection between the P-type substrate/P-well and the `Vss` ground rail.
* **Layer Stack (Bottom to Top):**
  1. **P-Substrate / P-Well:** Sits outside `nwell` in the native p-type substrate[cite: 2].
  2. **P-Tap Implant (`ptap` / `psd`):** Heavily doped $p^+$ region for low-resistance substrate contact[cite: 2].
  3. **Diffusion-to-LI Contact (`licon`):** Connects the $p^+$ tap region to Local Interconnect[cite: 2].
  4. **Local Interconnect (`li1`):** Bridges `licon` to `mcon`[cite: 2].
  5. **LI-to-Metal1 Via (`mcon`):** Connects `li1` to Metal 1[cite: 2].
  6. **Metal 1 (`met1`):** Routes downward from the contact stack to the `Vss` pin/rail[cite: 2].

---

## Layer Summary Table

| Layer Name (Magic) | Purpose | Used in `ntap` | Used in `ptap` |
| :--- | :--- | :---: | :---: |
| `nwell` | Defines
