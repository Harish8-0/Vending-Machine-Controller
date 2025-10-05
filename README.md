<div align="center">
   <img src='https://github.com/user-attachments/assets/9f11bef9-e1f1-4f73-ad2a-65e0317aa016' />


<h3 align="center">SURE ProEd (SURE Trust) - Skill Upgradation for Rural-youth Empowerment Trust</h3>
  <h2> M Harish G1-25 Integerated VLSI </h2>
</div>
<h2 🎰 Vending Machine Controller - Digital IP Core </h2>

<div align="center">

[![Verilog](https://img.shields.io/badge/Language-Verilog-blue.svg)](https://en.wikipedia.org/wiki/Verilog)
[![Status](https://img.shields.io/badge/Status-Complete-success.svg)]()

**A parameterized, synthesizable Verilog RTL design for flexible vending machine controller IP**

[Features](#-key-features) • [Architecture](#️-architecture) • [Getting Started](#-getting-started) • [Configuration](#️-configuration-guide) • [Simulation](#-simulation)

</div>

---

## 📋 Table of Contents
- [🎯 Overview](#-overview)
- [✨ Key Features](#-key-features)
- [🏛️ Architecture](#️-architecture)
- [📐 Design Specifications](#-design-specifications)
- [🧩 Module Hierarchy](#-module-hierarchy)
- [🚀 Getting Started](#-getting-started)
- [⚙️ Configuration Guide](#️-configuration-guide)
- [🧪 Simulation](#-simulation)
- [🔮 Future Enhancements](#-future-enhancements)
---

## 🎯 Overview

This project implements a **configurable vending machine controller** as a production-grade digital IP core designed for seamless integration into various vending machine systems. The design supports multiple items with different prices, handles asynchronous user inputs, and provides a robust APB configuration interface for system setup.

**Project developed as part of:** SURE ProEd - Integrated VLSI Internship (Front-End Design/Verification)

---

## ✨ Key Features

### 🎯 Core Specifications
| Feature | Specification | Notes |
|---------|--------------|-------|
| ⚡ Operating Frequency | 100 MHz | High-performance system clock |
| 📦 Max Items | 1024 | Fully parameterized design |
| 💵 Currency Support | 5, 10, 15, 20, 50, 100 Rs | Six denominations |
| ⏱️ Transaction Latency | <10 clock cycles | Currency to dispense |
| 🔢 APB Clock | 50 MHz | Configuration interface |
| 📶 Input Frequency Range | 10 KHz - 50 MHz | Asynchronous inputs |

### 🛡️ Advanced Capabilities
- 📊 **Real-Time Inventory**: Live tracking of dispensed and available items
- 🔁 **Smart Change Return**: Automatic calculation and dispense of exact change
- 🚫 **Error Handling**: Immediate refund for out-of-stock or invalid transactions
- 🔐 **Industry Standard**: AMBA APB protocol compliance
- 🌉 **Clock Domain Crossing**: Safe CDC for multi-clock operation
- 🧊 **Metastability Protection**: 2-stage synchronizers on all async inputs

---

## 🏛️ Architecture

### 🏗️ High-Level Block Diagram

![Image](https://github.com/Harish8-0/Vending-Machine-Controller-/blob/main/Architecture/Basic_Block_Diagram.jpg)

### 🔄 Operating Modes

| Mode | Signal Condition | Description | Icon |
|------|-----------------|-------------|------|
| **RESET** | `rstn == 0` | Initialize all hardware registers | 🔄 |
| **CONFIG** | `rstn == 1 && cfg_mode == 1` | Load item details via APB | ⚙️ |
| **OPERATION** | `rstn == 1 && cfg_mode == 0` | Normal vending operation | 🎰 |

---

## 📐 Design Specifications

### ⏱️ Timing Requirements
| Parameter | Value | Tolerance |
|-----------|-------|-----------|
| 🔵 System Clock | 100 MHz | ±50 ppm |
| 🟢 APB Clock | 50 MHz | ±50 ppm |
| 🟡 User Input Frequency | 10 KHz - 50 MHz | Asynchronous |
| ⚡ Latency | <10 clocks | Worst case |

### 🔌 Interface Specifications

#### 🎛️ General Control Signals
| Signal | Width | Direction | Clock Domain | Description |
|--------|-------|-----------|--------------|-------------|
| `clk_fsm` | 1 | Input | - | 100MHz system clock |
| `clk_apb` | 1 | Input | - | 50MHz APB clock |
| `rstn` | 1 | Input | - | Active-low async reset |
| `cfg_mode` | 1 | Input | FSM | Configuration mode enable |

#### 💰 User Input Interface (Asynchronous)
| Signal | Width | Direction | Description |
|--------|-------|-----------|-------------|
| `currency_valid_async` | 1 | Input | 💵 Currency insertion pulse |
| `currency_value_async` | log₂(MAX_NOTE_VAL)+1 | Input | 💰 Denomination value |
| `item_select_valid_async` | 1 | Input | 🛒 Item selection pulse |
| `item_select_async` | log₂(MAX_ITEMS) | Input | 🔢 Selected item number |

#### 📝 APB Configuration Interface
| Signal | Width | Direction | Description |
|--------|-------|-----------|-------------|
| `paddr` | 15 | Input | 📍 APB address |
| `psel` | 1 | Input | ✅ Slave select |
| `pwrite` | 1 | Input | ✏️ Write enable |
| `pwdata` | 32 | Input | 📥 Write data |
| `prdata` | 32 | Output | 📤 Read data |
| `pready` | 1 | Output | ⏳ Transfer complete |

#### 📤 Output Interface
| Signal | Width | Direction | Description |
|--------|-------|-----------|-------------|
| `item_dispense_valid` | 1 | Output | ✅ Dispense valid pulse |
| `item_dispense` | log₂(MAX_ITEMS) | Output | 📦 Item number dispensed |
| `currency_change` | 8 | Output | 💸 Change amount |

---

## 🧩 Module Hierarchy

```
📦 vending_top (Top-Level Integration)
│
├── 🔄 input_cdc (Asynchronous Input Synchronization)
│   ├── pulse_sync (currency_valid)
│   └── pulse_sync (item_select_valid)
│
├── ⚙️ main_fsm (Core State Machine)
│   └── States: IDLE → WAIT_FOR_MONEY → DISPENSE/EMPTY
│
├── 💾 item_memory (Dual-Interface Memory)
│   ├── FSM interface (100 MHz)
│   └── Config interface (50 MHz)
│
└── 🔌 apb_cdc (APB Clock Domain Crossing)
    └── Safe APB to FSM domain bridge
```

### 📚 Module Descriptions

| Module | Responsibility | Key Features |
|--------|---------------|--------------|
| **🏠 vending_top** | Top-level integration | Connects all submodules |
| **🔄 input_cdc** | Input synchronization | 2-stage pulse synchronizers, metastability protection |
| **⚙️ main_fsm** | Core control logic | Item selection, currency accumulation, change calculation |
| **💾 item_memory** | Data storage | Stores cost, available count, dispensed count |
| **🔌 apb_cdc** | Configuration bridge | Safe APB to FSM clock domain crossing |

---

## 🚀 Getting Started

### 📋 Prerequisites
- 🖥️ Verilog simulator (ModelSim/VCS/Icarus Verilog)
- 📊 Waveform viewer (GTKWave/ModelSim)
- 🧠 Basic knowledge of Verilog and digital design

### ⚡ Quick Start

#### 1️⃣ Clone the Repository
```bash
git clone https://github.com/yourusername/vending-machine-controller.git
cd vending-machine-controller
```

#### 2️⃣ Compile the Design
```bash
# 🟦 Using Icarus Verilog
iverilog -o vending_sim src/*.v tb/tb_top.v

# 🟥 Using VCS
vcs -full64 -sverilog +v2k -timescale=1ns/1ps \
    src/*.v tb/tb_top.v -o vending_sim
```

#### 3️⃣ Run Simulation
```bash
# ▶️ Execute simulation
./vending_sim +TEST_NAME=directed_test +DEBUG

# 📈 View waveforms
gtkwave dump.vcd
```

---

## ⚙️ Configuration Guide

### 📍 APB Register Map

#### Base Address: `0x4000_0000`

| Address | Register | Access | Description |
|---------|----------|--------|-------------|
| `0x4000_0000` | 🎯 `no_of_items` | RW | Number of active items |
| `0x4000_0004` | 📦 `item_cfg[0]` | RW | Item 0 configuration |
| `0x4000_0008` | 📦 `item_cfg[1]` | RW | Item 1 configuration |
| ... | ... | ... | ... |
| `0x4000_xxxx` | 📦 `item_cfg[N-1]` | RW | Item N-1 configuration |

### 📋 Item Configuration Register Format (32 bits)

```
┌──────────────┬──────────────┬──────────────┐
│   [31:24]    │   [23:16]    │   [15:0]     │
│  📊 disp_    │  📦 avail_   │  💰 item_    │
│  items (RO)  │  items (RW)  │  val (RW)    │
└──────────────┴──────────────┴──────────────┘
```

| Field | Bits | Access | Description |
|-------|------|--------|-------------|
| **📊 disp_items** | 31:24 | Read-Only | Number of items dispensed |
| **📦 avail_items** | 23:16 | Read-Write | Available item count |
| **💰 item_val** | 15:0 | Read-Write | Item cost in Rs |

### 💡 Configuration Example

```systemverilog
// 🎯 Configure 8 items
apb_write(0x0000, 32'd8);  // Set no_of_items = 8

// 📦 Configure Item 0: Cost=11 Rs, Available=13 units
apb_write(0x0004, {8'd0, 8'd13, 16'd11});

// 📦 Configure Item 1: Cost=22 Rs, Available=7 units
apb_write(0x0008, {8'd0, 8'd7, 16'd22});
```

---

## 🧪 Simulation

### 📊 Available Test Cases

| Test Name | Type | Description | Icon |
|-----------|------|-------------|------|
| **write_read_test** | Functional | Verifies APB read/write functionality | ✅ |
| **directed_test** | Directed | Tests complete vending flow with known scenarios | 🎯 |
| **random_test** | Random | Randomized transaction testing | 🎲 |

### ▶️ Running Tests

```bash
# 🎯 Run specific test
./vending_sim +TEST_NAME=directed_test +DEBUG

# 🔇 Run without debug messages
./vending_sim +TEST_NAME=write_read_test

# 🎲 Run random test
./vending_sim +TEST_NAME=random_test
```

### 📤 Expected Output

```
********************************
 📦 Item No: 1, 💰 Cost: 22, 💸 Change: 3
---------------------------------
💵 Driving Note interface: 20
💵 Driving Note interface: 5
✅ Output: Item Dispensed Item_no: 1, Change: 3
✅ PASS: Exp Item = 1, Actual Item = 1
✅ PASS: Exp change = 3, Actual change = 3
---------------------------------
```

### 📈 Waveform Analysis

**Key signals to observe:**
- 🔄 `current_state` - FSM state transitions
- 💰 `current_credit` - Accumulated currency
- ✅ `item_dispense_valid` - Output valid pulse
- 💸 `currency_change` - Calculated change

---

## 🔮 Future Enhancements

### 🚀 Planned Features
- [ ] 🧪 UVM-based verification environment
- [ ] 🔌 Wishbone bus interface option
- [ ] 🔋 Power management modes (sleep/idle)
- [ ] 💳 Credit card payment interface
- [ ] 📺 LCD display controller integration
- [ ] 🌡️ Temperature sensor for product monitoring
- [ ] 🌐 Network connectivity for remote monitoring
- [ ] 🌍 Multi-currency support (USD, EUR, GBP)

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. 🍴 Fork the repository
2. 🌿 Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. 💾 Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. 📤 Push to the branch (`git push origin feature/AmazingFeature`)
5. 🔀 Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 👨‍💻 Author

**Your Name**
- 🐙 GitHub: [@Harish8-0](https://github.com/Harish8-0)
- 💼 LinkedIn: [Harish M](www.linkedin.com/in/mondem-harish)
- 📧 Email: mondemharish08@gmail.com

---

## 🙏 Acknowledgments

- **SURE ProEd** - For project specification and guidance
- **Project Mentor**: Mr. Satish Devarapalli (Emulation Verification at Apple)
- **Course**: Integrated VLSI Internship - Front-End Design/Verification

---

## 📊 Project Statistics

| Metric | Value |
|--------|-------|
| 📝 Lines of Code | ~800 (RTL + Testbench) |
| 🧩 Modules | 6 core modules |
| ✅ Test Cases | 3 comprehensive tests |
| ⏰ Clock Domains | 2 (APB, System) |
| ⚙️ Parameterized | Fully configurable |

---

<div align="center">

### 🌟 If you found this project useful, please consider giving it a star! 🌟

**Made with ❤️ for the Digital Design Community**

[⬆ Back to Top](#-vending-machine-controller---digital-ip-core)

</div>
