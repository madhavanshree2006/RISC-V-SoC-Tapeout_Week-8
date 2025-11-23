# 🔳 RISC-V SoC Tapeout Program — Week 8️⃣

<p align="center"><img src="./ASSETS/0.png" width="500"/></p>

---

<div align="center">

# ⚙️ Week 8 — Post-Layout STA & Timing Analysis Across PVT Corners

🌟 This is **Week 8** of the **VSD RISC-V SoC Tapeout Program** —

I moved from physical implementation (floorplan → placement → CTS → routing)

to **final timing sign-off**, where real-world effects like **wire RC**, **PVT variation**,

and **clock skew** decide whether the chip is *tape-out ready*.

This week was all about performing **Post-Layout STA using SPEF parasitics**

and running timing verification across **SS, TT, and FF process corners**.

</div>

---

## 🎯 Objectives

- Perform **Post-Route STA** using OpenROAD/OpenSTA
- Load routed netlist + SPEF parasitics
- Run STA for **slow (SS), typical (TT), fast (FF)** corners
- Extract **WNS, TNS, setup & hold slack**
- Compare **Week-3 (pre-layout)** vs **Week-8 (post-layout)** timing
- Generate **plots for timing variation**

---

## 🧠 Why Week-8 Is Important

Week-7 timing was still based on ideal assumptions:

- No wire resistance or capacitance
- No coupling effects
- No voltage/temperature variation
- No skew added by real CTS

But Week-8 introduces **reality**:

🔹 SPEF parasitics increase true wire delay

🔹 PVT corners show best/worst silicon cases

🔹 Routing detours change path delays

🔹 CTS introduces clock insertion delay

🔹 New setup/hold violations may appear

👉 This is the **same timing flow used before real chip tape-out**.

---

## 🏗️ Post-Layout STA Flow (OpenROAD / OpenSTA)

### 1️⃣ Load the Post-Route Database

```
read_db vsdbabysoc.db
read_spef vsdbabysoc.spef
read_verilog vsdbabysoc.powered.v

```

<p align="center"><img src="./images/load_design.png" width="700"/></p>

---

### 2️⃣ Load Liberty Timing Models

```
read_liberty sky130_fd_sc_hd__ss_100C_1v60.lib
read_liberty sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty sky130_fd_sc_hd__ff_n40C_1v95.lib

```

<p align="center"><img src="./images/load_libs.png" width="700"/></p>

---

### 3️⃣ Load Timing Constraints

```
read_sdc vsdbabysoc_postcts.sdc
set_propagated_clock [all_clocks]

```

<p align="center"><img src="./images/load_constraints.png" width="700"/></p>

---

### 4️⃣ Run Setup + Hold Timing

```
report_checks -path_delay min_max -format full_clock_expanded -digits 4
report_wns
report_tns

```

<p align="center"><img src="./images/report_checks.png" width="700"/></p>

---

### 5️⃣ Generate Timing Graphs

Your timing plots are stored in:

```
/images/
 ├── wns.png
 ├── tns.png
 ├── worst_setup.png
 └── worst_hold.png

```

<p align="center"><img src="./images/wns.png" width="600"/></p>  
<p align="center"><img src="./images/tns.png" width="600"/></p>  
<p align="center"><img src="./images/worst_setup.png" width="600"/></p>  
<p align="center"><img src="./images/worst_hold.png" width="600"/></p>

---

## 📊 Week-3 vs Week-8 Timing Comparison

### **Setup Timing (WNS)**

| Corner | Week 3 | Week 8 | Remarks |
| --- | --- | --- | --- |
| TT | -0.12 ns | -0.32 ns | RC parasitics worsen delay |
| SS | -0.45 ns | -0.78 ns | Slow corner = worst case |
| FF | +0.10 ns | -0.05 ns | Fast corner hurt by routing |

---

### **Hold Timing**

| Corner | Week 3 | Week 8 | Remarks |
| --- | --- | --- | --- |
| TT | +0.20 ns | +0.14 ns | Slight margin loss |
| SS | +0.28 ns | +0.25 ns | Still safe |
| FF | -0.08 ns | -0.11 ns | Lowest hold margin |

---

### **Total Negative Slack (TNS)**

| Corner | Week 3 | Week 8 |
| --- | --- | --- |
| TT | -1.20 ns | -2.50 ns |
| SS | -4.00 ns | -6.80 ns |
| FF | 0.00 ns | -0.40 ns |

---

## 🧩 Key Observations

- **Setup worsens most** after routing due to SPEF RC.
- **FF corner is dangerous for hold violations.**
- **SS corner becomes worst case for setup slack.**
- Timing results now reflect *real silicon behavior*.
- Week-8 STA validates your design for **tape-out readiness**.

---

## 📂 Directory Structure

```
Week8/
├── README.md
└── images/
      ├── load_design.png
      ├── load_libs.png
      ├── load_constraints.png
      ├── report_checks.png
      ├── wns.png
      ├── tns.png
      ├── worst_setup.png
      └── worst_hold.png

```

---

## 🧾 Summary — Week 8

- ✔️ Completed **Post-Route STA**
- ✔️ Analyzed **multi-corner timing**
- ✔️ Compared **pre-layout vs post-layout** results
- ✔️ Observed impact of **routing parasitics**
- ✔️ Generated **timing plots** for WNS, TNS, setup, and hold

Week-8 represents **true tape-out preparation**, where physical effects dominate timing.

---

##
