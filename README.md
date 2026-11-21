# SKY130 CMOS Well and Substrate Tap Cells (`ntap` & `ptap`)

Custom physical layout implementations of N-well tap (`ntap`) and P-substrate tap (`ptap`) cells designed in **Magic VLSI** using the open-source SkyWater SKY130 PDK.

## Overview
In bulk CMOS processes, parasitic PNP and NPN bipolar junction transistors form a four-layer PNPN thyristor structure between `Vdd` and `Vss`. Without low-impedance body ties placed at regular intervals, substrate or well currents can forward-bias these junctions and trigger **latch-up**. 

These tap cells provide dedicated ohmic contacts to anchor the N-well to `Vdd` and the P-substrate to `Vss`, stabilizing body potentials across standard-cell rows and analog layout blocks.

---

## Cell Architectures & Layer Stack

### 1. N-Well Tap Cell (`ntap`)
* **Supply Tie:** `Vdd`[1]
* **Function:** Establishes a low-resistance ohmic connection between the N-well and the `Vdd` power rail to reverse-bias the N-well/P-substrate junction.
* **Layer Stack (Bottom to Top):**
  1. **N-Well (`nwell`):** Encloses the entire tap structure to define the n-type well region[1].
  2. **N-Tap Implant (`ntap` / `nsd`):** Heavily doped $n^+$ region inside the N-well for ohmic contact formation[1].
  3. **Diffusion-to-LI Contact (`licon`):** Connects the $n^+$ tap region upward to Local Interconnect[1].
  4. **Local Interconnect (`li1`):** SKY130 local routing layer bridging `licon` and `mcon`[1].
  5. **LI-to-Metal1 Via (`mcon`):** Connects `li1` to Metal 1[1].
  6. **Metal 1 (`met1`):** Routes upward from the contact stack to the `Vdd` pin/rail[1].

### 2. P-Substrate Tap Cell (`ptap`)
* **Supply Tie:** `Vss`[2]
* **Function:** Establishes a low-resistance ohmic connection between the P-type substrate/P-well and the `Vss` ground rail.
* **Layer Stack (Bottom to Top):**
  1. **P-Substrate / P-Well:** Sits outside `nwell` in the native p-type substrate[2].
  2. **P-Tap Implant (`ptap` / `psd`):** Heavily doped $p^+$ region for low-resistance substrate contact[2].
  3. **Diffusion-to-LI Contact (`licon`):** Connects the $p^+$ tap region to Local Interconnect[2].
  4. **Local Interconnect (`li1`):** Bridges `licon` to `mcon`[2].
  5. **LI-to-Metal1 Via (`mcon`):** Connects `li1` to Metal 1[2].
  6. **Metal 1 (`met1`):** Routes downward from the contact stack to the `Vss` pin/rail[2].

---

## Layer Summary Table

| Layer Name (Magic) | Purpose | Used in `ntap` | Used in `ptap` |
| :--- | :--- | :---: | :---: |
| `nwell` | Defines the N-well region[1] | Yes[1] | No[2] |
| `ntap` (`nsd`) | $n^+$ well-tie diffusion/implant[1] | Yes[1] | No |
| `ptap` (`psd`) | $p^+$ substrate-tie diffusion/implant[2] | No | Yes[2] |
| `licon` | Contact between tap diffusion and `li1` | Yes[1] | Yes[2] |
| `li1` | Local Interconnect routing layer | Yes[1] | Yes[2] |
| `mcon` | Via between `li1` and `met1`[1, 2] | Yes[1] | Yes[2] |
| `met1` | Metal 1 routing to `Vdd` / `Vss` pins[1, 2] | Yes[1] | Yes[2] |
