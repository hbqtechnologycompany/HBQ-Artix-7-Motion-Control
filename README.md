# HBQ PCIe Base Card V2.0

> 🇻🇳 **Card điều khiển chuyển động & thu thập dữ liệu FPGA + MCU hiệu năng cao**  
> 🇬🇧 **High-Performance FPGA + MCU Motion Control & Data Acquisition Card**  
> Nhà sản xuất / Manufacturer: **HBQ Technology** · Rev 2.0 · 01/2026  
> 🌐 [www.hbqtechnology.com](http://www.hbqtechnology.com)

---

## 📋 Mục lục / Table of Contents

| # | Tiếng Việt | English |
|---|-----------|---------|
| 1 | [Tổng quan sản phẩm](#1-tổng-quan--product-overview) | Product Overview |
| 2 | [Bộ xử lý trung tâm](#2-bộ-xử-lý--core-processors) | Core Processors |
| 3 | [Giao tiếp máy chủ (PCIe/PXI/USB3)](#3-giao-tiếp-máy-chủ--host-interface) | Host Interface |
| 4 | [I/O Analog](#4-io-analog) | Analog I/O |
| 5 | [Điều khiển động cơ](#5-điều-khiển-động-cơ--motion-control-io) | Motion Control I/O |
| 6 | [Digital cách ly](#6-digital-cách-ly--isolated-digital-io) | Isolated Digital I/O |
| 7 | [Truyền thông công nghiệp](#7-truyền-thông-công-nghiệp--fieldbus-comms) | Fieldbus Communications |
| 8 | [Bộ nhớ](#8-bộ-nhớ--memory) | Memory |
| 9 | [Kiến trúc nguồn](#9-kiến-trúc-nguồn--power-architecture) | Power Architecture |
| 10 | [**Connector CN10 — Cấp nguồn Backplane**](#10-connector-cn10--backplane-power-supply) | CN10 Backplane Power |
| 11 | [**Connector J7A — I/O 100 chân**](#11-connector-j7a--100-pin-field-io) | J7A 100-Pin I/O |
| 12 | [Ứng dụng](#12-ứng-dụng--applications) | Applications |
| 13 | [Sơ đồ khối](#13-sơ-đồ-khối--block-diagram) | Block Diagram |
| 14 | [Lập trình & Debug](#14-lập-trình--debug) | Programming & Debug |

---

## 1. Tổng quan / Product Overview

🇻🇳 **HBQ PCIe Base Card V2.0** là card điều khiển chuyển động đa trục và thu thập dữ liệu dạng PCIe/PXI add-in, tích hợp FPGA Xilinx Artix-7 và MCU STM32H7. Card cung cấp khả năng điều khiển servo/stepper real-time, analog I/O chính xác cao, digital I/O cách ly và truyền thông công nghiệp — tất cả truy cập qua PCIe x4 hoặc USB 3.0 từ máy tính chủ.

🇬🇧 The **HBQ PCIe Base Card V2.0** is a multi-axis motion control and data acquisition PCIe/PXI add-in card combining a Xilinx Artix-7 FPGA with an STM32H7 MCU. It delivers real-time servo/stepper control, high-precision analog I/O, isolated digital I/O, and industrial fieldbus — accessible from a host PC via PCIe x4 or USB 3.0.

| Thông số / Parameter | Giá trị / Specification |
|----------------------|------------------------|
| **Form Factor** | PCIe Full-Height · PXI Hybrid Slot compatible |
| **Giao tiếp chủ / Host** | PCIe x4 Gen2 · PXI-Express · USB 3.0 |
| **FPGA** | Xilinx Artix-7 35K — XC7A35T (module ALINX AC7A035) |
| **MCU** | STM32H743VIT6 — ARM Cortex-M7 @ 480 MHz |
| **Trục động cơ / Motor Axes** | 6 × Servo/Stepper (PWM+DIR, RS-422 differential) |
| **Encoder** | 6 × Quadrature A/B/Z (RS-422 differential) |
| **Analog Input** | 8 kênh / ch, 16-bit, ±5V/±10V vi sai / differential |
| **Analog Output** | 4 kênh / ch, 16-bit, có buffer / buffered |
| **Digital cách ly / Isolated DI** | 8 kênh / ch — ISO7740, 2500 VRMS |
| **Digital cách ly / Isolated DO** | 8 kênh / ch — ISO7740, 2500 VRMS |
| **CAN FD** | 2 cổng / ports — SN65HVD230DR |
| **RS-485 / Modbus** | 2 cổng / ports — ADM3065EARZ |
| **USB 3.0** | 1 × Type-B — FT601Q, 32-bit FIFO, ~200 MB/s |
| **E-STOP** | 1 × ngõ vào dừng khẩn / Emergency Stop input (CN15) |
| **Nguồn vào / Power** | 12V qua khe PCIe hoặc backplane PXI |

---

## 2. Bộ xử lý / Core Processors

### 2.1 FPGA — Xilinx Artix-7 35K (XC7A35T)
Gắn qua module **ALINX AC7A035** trên connector **CN8** (40 chân / pin).

| Tài nguyên / Resource | Số lượng / Qty |
|-----------------------|---------------|
| Logic Cells | 33,280 |
| DSP Slices | 50 |
| Block RAM | 1,800 Kbits (90 × 18Kb) |
| GTP MGT Lanes | 4 (dùng cho PCIe x4 / used for PCIe x4) |
| XADC | Tích hợp / Built-in |

**Nhiệm vụ FPGA / FPGA Tasks:** PCIe endpoint · Đếm encoder 6 trục / 6-axis encoder counting · Tạo PWM+DIR 6 trục / 6-axis PWM generation · Giao tiếp ADC/DAC SPI · USB FIFO · ISO DI/DO · CAN/Modbus PHY

### 2.2 MCU — STM32H743VIT6
ARM Cortex-M7 @ 480 MHz, LQFP-100. Giao tiếp FPGA qua bus **FMC** 16-bit.

| Ngoại vi / Peripheral | Chức năng / Function |
|-----------------------|---------------------|
| FMC (16-bit) | Bus song song tới FPGA / Parallel bus to FPGA |
| QSPI | PSRAM ngoài APS6404L (64 Mbit) |
| SPI1, SPI2 | Cảm biến IMU #1, #2, #3 |
| I2C1 | Bus I2C IMU |
| FDCAN1/2 | CAN FD 2 cổng / 2-port CAN FD |
| UART5 | Modbus RTU |
| SWD | Debug (P1) |

---

## 3. Giao tiếp máy chủ / Host Interface

### PCIe x4
- Connector **J1** (PCIE-064-02-X-D-EMS3) — khe PCIe tiêu chuẩn / standard PCIe edge
- 4 lane MGT GTP · Ref clock từ PCIe slot / from PCIe slot
- Reset: PCIE_PERST → FPGA_RESET

### PXI Backplane
- **CN5**: PXI-XP4 Hybrid — 12V/5V/3.3V · TRIG0–7 · CLK10 · GA0–4
- **CN6**: PXI-XP3 — thêm lane tốc độ cao / additional high-speed lanes

### USB 3.0 (độc lập / independent — không cần PCIe)
- Chip: **FT601Q-B-T** (USB 3.0 ↔ 32-bit FIFO async)
- Connector: **CN11** — USB 3.0 Type-B
- Băng thông / Bandwidth: ~200 MB/s · Bus 32-bit + 4-bit byte enable
- ESD: SP3010-04UTG · USBLC6-2SC6 trên VBUS

---

## 4. I/O Analog

### ADC — AD7606C (U4) · 8 kênh đồng thời / 8-ch Simultaneous

| Thông số / Parameter | Giá trị / Value |
|----------------------|----------------|
| Kênh / Channels | 8 vi sai đồng thời / 8 differential simultaneous |
| Độ phân giải / Resolution | 16-bit |
| Dải vào / Input range | ±5V hoặc/or ±10V (chọn qua J4 / select via J4) |
| Tốc độ lấy mẫu / Sample rate | tối đa/max 200 kSPS/ch |
| Giao tiếp / Interface | SPI serial (DOUTA/B/C/D, SDI, SCLK, CS) |
| Tham chiếu / Reference | Nội 2.5V (MCP1525T) hoặc/or ngoại/external (J5) |
| Oversampling | OS0–2: ×1 đến/to ×64 |
| Nguồn / Power | 5V_ADC (qua jumper R39/R43) |

### DAC — DAC81404RHBT (U17) + TL082 buffer · 4 kênh / 4-ch

| Thông số / Parameter | Giá trị / Value |
|----------------------|----------------|
| Kênh / Channels | 4 (OUTA/B/C/D → OPA_DAC_OUT1–4) |
| Độ phân giải / Resolution | 16-bit |
| Buffer ngõ ra / Output buffer | 2× TL082 op-amp (U16, U18) |
| Giao tiếp / Interface | SPI (DAC_SCK/MOSI/MISO/CS/RST/LDAC/CLR) |
| Nguồn analog / Analog power | 12V_LDO + 5V |

---

## 5. Điều khiển động cơ / Motion Control I/O

### 5.1 Encoder Input — 6 trục / 6 Axes (RS-422 Differential)
Receiver: **AM26LV32E** × 6 IC · Termination: 120Ω on-board · Buffer: SN74LVC14A Schmitt

| Trục / Axis | Kênh A / Ch A | Kênh B / Ch B | Index Z |
|-------------|--------------|--------------|---------|
| ENC 1 | ENC_1A± (J7A 26,27) | ENC_1B± (24,25) | ENC_1Z± (76,77) |
| ENC 2 | ENC_2A± (74,75) | ENC_2B± (22,23) | ENC_2Z± (20,21) |
| ENC 3 | ENC_3A± (72,73) | ENC_3B± (70,71) | ENC_3Z± (18,19) |
| ENC 4 | ENC_4A± (16,17) | ENC_4B± (68,69) | ENC_4Z± (66,67) |
| ENC 5 | ENC_5A± (30,31) | ENC_5B± (28,29) | ENC_5Z± (78,79) |
| ENC 6 | — (via CN1–4) | — | — |

### 5.2 Motor Output — 6 trục / 6 Axes (RS-422 Differential)
Driver: **AM26LV31E** × 3 IC · ESD: NUP2105 TVS

| Trục / Axis | PWM Output | DIR Output |
|-------------|-----------|-----------|
| Motor 1 | MDR_PWM1± (J7A 11,12) | MDR_DIR1± (9,10) |
| Motor 2 | MDR_PWM2± (64,65) | MDR_DIR2± (62,63) |
| Motor 3 | MDR_PWM3± (7,8) | MDR_DIR3± (5,6) |
| Motor 4 | MDR_PWM4± (60,61) | MDR_DIR4± (58,59) |
| Motor 5 | MDR_PWM5± (via CN1–4) | MDR_DIR5± |
| Motor 6 | MDR_PWM6± (via CN1–4) | MDR_DIR6± |

---

## 6. Digital cách ly / Isolated Digital I/O

Cách ly qua / Isolated via: **ISO7740FDBQR** × 4 IC · 2500 VRMS · 150 Mbps

### Digital Input — 8 kênh / 8 channels (ISO_IN1–8)

| Kênh / Ch | Chân J7A / J7A Pin | Nguồn cách ly / Iso Power |
|-----------|-------------------|--------------------------|
| ISO_IN1 | 83 | IN_ISOV+ (33) / IN_ISOGND (34) |
| ISO_IN2 | 82 | IN_ISOV+ (33) / IN_ISOGND (34) |
| ISO_IN3 | 81 | IN_ISOV+ (33) / IN_ISOGND (34) |
| ISO_IN4 | 80 | IN_ISOV+ (33) / IN_ISOGND (34) |
| ISO_IN5 | 42 | IN_ISOV+ (33) / IN_ISOGND (34) |
| ISO_IN6 | — | IN_ISOV+ (33) / IN_ISOGND (34) |
| ISO_IN7 | 41 | IN_ISOV+ (33) / IN_ISOGND (34) |
| ISO_IN8 | 32 | IN_ISOV+ (33) / IN_ISOGND (34) |

### Digital Output — 8 kênh / 8 channels (ISO_OUT1–8)

| Kênh / Ch | Chân J7A / J7A Pin | Nguồn cách ly / Iso Power |
|-----------|-------------------|--------------------------|
| ISO_OUT1 | 37 | OUT_ISOV+ (35) / OUT_ISOGND (36) |
| ISO_OUT2 | 38 | OUT_ISOV+ (35) / OUT_ISOGND (36) |
| ISO_OUT3 | 39 | OUT_ISOV+ (35) / OUT_ISOGND (36) |
| ISO_OUT4 | 40 | OUT_ISOV+ (35) / OUT_ISOGND (36) |
| ISO_OUT5 | 84 | OUT_ISOV+ (35) / OUT_ISOGND (36) |
| ISO_OUT6 | 85 | OUT_ISOV+ (35) / OUT_ISOGND (36) |
| ISO_OUT7 | 86 | OUT_ISOV+ (35) / OUT_ISOGND (36) |
| ISO_OUT8 | 87 | OUT_ISOV+ (35) / OUT_ISOGND (36) |

> ⚠️ **VI:** IN_ISOGND và OUT_ISOGND hoàn toàn cách ly nhau và cách ly với GND board. Phải cấp nguồn riêng cho từng khối.  
> ⚠️ **EN:** IN_ISOGND and OUT_ISOGND are fully isolated from each other and from board GND. Each block requires its own power supply.

---

## 7. Truyền thông công nghiệp / Fieldbus Comms

### CAN FD — 2 cổng / 2 ports

| Cổng / Port | Transceiver | Chân J7A / Pin | Điện trở cuối / Term. |
|-------------|------------|----------------|----------------------|
| CAN1 | SN65HVD230DR (U15) | CAN1_P=57, CAN1_N=56 | 120Ω R75 (on-board) |
| CAN2 | SN65HVD230DR (U36) | CAN2_P=55, CAN2_N=54 | 120Ω R87 (on-board) |

### RS-485 / Modbus RTU — 2 cổng / 2 ports

| Cổng / Port | Transceiver | Chân J7A / Pin | Điện trở cuối / Term. |
|-------------|------------|----------------|----------------------|
| Modbus 1 | ADM3065EARZ (U7) | MB1_A=4, MB1_B=3 | 120Ω R51 (on-board) |
| Modbus 2 | ADM3065EARZ (U14) | MB2_A=2, MB2_B=1 | 120Ω R58 (on-board) |

---

## 8. Bộ nhớ / Memory

| Linh kiện / Part | Loại / Type | Dung lượng / Size | Giao tiếp / Interface | Kết nối / Connected |
|-----------------|------------|-------------------|----------------------|---------------------|
| W959D8NFYA5I (U2) | HyperRAM | 64 Mbit | HyperBus | FPGA (TXB0108 level-shift) |
| APS6404L-3SQR-SN (U5) | QSPI PSRAM | 64 Mbit | Quad-SPI | MCU (QSPI) |

---

## 9. Kiến trúc nguồn / Power Architecture

```
12V Input (PCIe J1 / PXI CN5 / Backplane CN10)
    │
    ├─ [Diode OR: D12, D44, D15 — B240Q-13-F]
    │
    ├──► MAX20006AFOC (U9) ─── 12V → 5V Buck (~4A)
    │         │
    │         ├──► MP2122AGJ-Z (U10)
    │         │         ├── 3.3V (L3) → Logic, CAN, USB, ISO, JTAG
    │         │         └── 2.5V (L2) → HyperRAM VCCQ, VREF
    │         │
    │         └──► MP2122AGJ-Z (U11)
    │                   ├── 1.8V (L4) → FPGA I/O banks
    │                   └── 1.5V (L5) → HyperRAM VCC, TXB level-shifter
    │
    └──► L78M12CDT-TR (U8) → 12V_LDO → DAC analog (VDAC), TL082 op-amp
```

| Rail | Nguồn / Source | Tải / Load | Người dùng / Consumers |
|------|---------------|-----------|------------------------|
| 12V | Ngoài / External | — | Buck input, analog supply |
| 5V | MAX20006 | ~2A | MCU, ADC, USB, encoder |
| 3.3V | MP2122 U10 | ~1A | Logic, CAN, Modbus, ISO |
| 2.5V | MP2122 U10 | ~200mA | HyperRAM VCCQ, ADC VREF |
| 1.8V | MP2122 U11 | ~400mA | FPGA I/O banks |
| 1.5V | MP2122 U11 | ~200mA | HyperRAM VCC |
| 12V_LDO | L78M12 | ~200mA | DAC analog, op-amp rails |
| 5V_ADC | 5V (jumper) | ~100mA | AD7606C AVCC |

---

## 10. Connector CN10 — Backplane Power Supply

### Mô tả / Description

🇻🇳 **CN10** là connector cấp nguồn backplane 6 chân (6-pin header), cấp điện cho card khi dùng trong chassis PXI hoặc backplane tùy chỉnh. Đây là nguồn vào thay thế/song song với khe PCIe (J1).

🇬🇧 **CN10** is a 6-pin backplane power input connector used when the card is installed in a PXI chassis or custom backplane. It provides power in parallel with / as an alternative to the PCIe slot (J1).

### Sơ đồ chân / Pin Diagram

```
       ┌─────────────────┐
  GND  │ 6    ●   ●    3 │ 12V
  GND  │ 5    ●   ●    2 │ 5V (optional)
  GND  │ 4    ●   ●    1 │ GND
       └─────────────────┘
           CN10 (top view)
        BACKPLANE POWER
```

| Chân / Pin | Tín hiệu / Signal | Mức điện áp / Voltage | Chiều / Direction | Mô tả / Description |
|-----------|------------------|----------------------|-------------------|---------------------|
| 1 | GND | 0V | Tham chiếu / REF | GND chung / Common GND |
| 2 | 5V (tùy chọn) | 5V DC | ← Input | Nguồn 5V từ backplane (tuỳ chọn) / Optional 5V from backplane |
| 3 | 12V | 12V DC | ← Input | **Nguồn chính / Main power input** |
| 4 | GND | 0V | Tham chiếu / REF | GND |
| 5 | GND | 0V | Tham chiếu / REF | GND |
| 6 | GND | 0V | Tham chiếu / REF | GND |

> 📌 **VI:** Sơ đồ trên là theo mặc định của schematic — xác minh lại bằng cách đo thực tế trước khi cấp nguồn.  
> 📌 **EN:** Pin assignment above is from schematic analysis — verify by measurement before applying power.

### Mạch bảo vệ nguồn / Power Protection Circuit

```
CN10 (12V vào) → D13 (Schottky diode) → F2 (Cầu chì 2A / 2A fuse)
                                         → C71, C72 (47µF/35V lọc / bulk filter)
                                         → C72, C73 (10µF decoupling)
                                         → VIN rail (tới MAX20006 buck)
```

- **D13**: Diode Schottky bảo vệ ngược cực / Reverse polarity protection diode
- **F2**: Cầu chì 2A bảo vệ quá dòng / 2A fuse for overcurrent protection
- **Tụ lọc / Filter caps**: 47µF/35V + 10µF chống nhiễu / bulk + decoupling

### Hướng dẫn cấp nguồn CN10 / CN10 Wiring Guide

#### Bước 1 / Step 1 — Chuẩn bị / Preparation
🇻🇳
- Dùng nguồn DC 12V ổn định, dòng tối thiểu **3A** (khuyến nghị 5A)
- Chuẩn bị cáp đôi (dây 20AWG trở lên cho 12V, 24AWG cho GND)
- Đảm bảo nguồn tắt trước khi nối / Ensure power is OFF before connecting

🇬🇧
- Use stable 12V DC supply, minimum **3A** current (5A recommended)
- Use 20AWG or thicker wire for 12V rail, 24AWG for GND
- Ensure power supply is OFF before wiring

#### Bước 2 / Step 2 — Nối dây / Wiring
```
Nguồn 12V (+) ──────── CN10 Pin 3 (12V)
Nguồn GND   (−) ──────── CN10 Pin 1 or 4 or 5 (GND)
```

🇻🇳 Nếu dùng cả 5V backplane: nối nguồn 5V vào Pin 2, GND vào Pin 4. 5V tùy chọn — có thể bỏ trống nếu không dùng.

🇬🇧 If using 5V backplane supply: connect 5V to Pin 2, GND to Pin 4. 5V is optional — can be left unconnected.

#### Bước 3 / Step 3 — E-STOP (CN15)

```
     CN15 (2 chân / 2-pin)
     ┌──────┐
  1  │  12V │ ← Nối 12V để card hoạt động / Connect 12V to enable card
  2  │  GND │ ← GND
     └──────┘
```

🇻🇳 **CN15 E-STOP**: Khi dùng nguồn ngoài (không qua khe PCIe), phải kết nối 12V vào CN15 Pin 1 và GND vào Pin 2 để card bật lên. Cắt 12V khỏi CN15 = dừng khẩn cấp phần cứng.

🇬🇧 **CN15 E-STOP**: When powering from backplane (not PCIe slot), connect 12V to CN15 Pin 1 and GND to Pin 2 to enable the card. Disconnecting 12V from CN15 = hardware emergency stop.

#### Thông số an toàn / Safety Specs

| Thông số / Parameter | Min | Typ | Max | Đơn vị / Unit |
|----------------------|-----|-----|-----|--------------|
| Điện áp vào / Input voltage | 11.4 | 12.0 | 12.6 | V DC |
| Dòng vào / Input current | — | 2.5 | 4.0 | A |
| Nhiệt độ vận hành / Operating temp | 0 | 25 | 70 | °C |
| Fuse rating (F2) | — | 2A | 2A | A |

> ⚠️ **CẢNH BÁO / WARNING**: Không cấp nguồn đồng thời từ CN10 và khe PCIe nếu không có diode OR hoặc không chắc về nguồn / Do NOT power from both CN10 and PCIe slot simultaneously unless diode OR (D12/D44/D15) is confirmed installed.

---

## 11. Connector J7A — 100-Pin Field I/O

**Part:** TE Connectivity **5787082-9** · 2×50 pin · Pitch 2.54mm · Có chốt khóa / with latch

### 11.1 Tổng quan nhóm tín hiệu / Signal Group Overview

| Nhóm / Group | Chân (Left) / Pins (L) | Chân (Right) / Pins (R) | Giao tiếp / Interface |
|-------------|----------------------|------------------------|----------------------|
| RS-485 Modbus | 1–4 | — | ADM3065EARZ half-duplex |
| Motor PWM+DIR ch1,3 | 5–12 | — | AM26LV31E RS-422 out |
| Motor PWM+DIR ch2,4 | — | 58–65 | AM26LV31E RS-422 out |
| Encoder ch4 A | 16–17 | — | AM26LV32E RS-422 in |
| Encoder ch3 Z, ch2 Z/B | 18–23 | — | AM26LV32E RS-422 in |
| Encoder ch1 B/A | 24–27 | — | AM26LV32E RS-422 in |
| Encoder ch5 B/A | 28–31 | — | AM26LV32E RS-422 in |
| CAN1, CAN2 | — | 54–57 | SN65HVD230DR |
| Encoder ch4 Z/B, ch3 B/A | — | 66–73 | AM26LV32E RS-422 in |
| Encoder ch2 A, ch1 Z | — | 74–77 | AM26LV32E RS-422 in |
| Encoder ch5 Z | — | 78–79 | AM26LV32E RS-422 in |
| ISO DI5,7,8 | 32,41,42 | — | ISO7740 cách ly / isolated |
| ISO DI1–4 | — | 80–83 | ISO7740 cách ly / isolated |
| Nguồn ISO / ISO power | 33–36 | — | User supply (5–30V) |
| ISO DO1–4 | 37–40 | — | ISO7740 cách ly / isolated |
| ISO DO5–8 | — | 84–87 | ISO7740 cách ly / isolated |
| DAC Output 1–4 | — | 88–91 | DAC81404 + TL082 buffer |
| ADC Input ch5–8 | 43–50 | — | AD7606C 16-bit diff |
| ADC Input ch1–4 | — | 93–100 | AD7606C 16-bit diff |
| GND | 51,52,53,92 | — | Board digital GND |

### 11.2 Bảng chân đầy đủ / Full Pin Table (5787082-9)

> 🇻🇳 Cột trái = pin 1–50 (left column). Cột phải = pin 51–100 (right column). Mỗi hàng là 2 pin đối diện nhau.  
> 🇬🇧 Left column = pins 1–50. Right column = pins 51–100. Each row shows two opposite pins.

**Ký hiệu loại / Signal type legend:**
`[485]`=RS-485 · `[PWM]`=Motor PWM · `[DIR]`=Motor DIR · `[ENC]`=Encoder · `[DI]`=Isolated Input · `[DO]`=Isolated Output · `[ADC]`=Analog In · `[DAC]`=Analog Out · `[CAN]`=CAN bus · `[PWR]`=Power/GND · `[ISO]`=Isolation supply

| Pin L | Tín hiệu trái / Left Signal | Tín hiệu phải / Right Signal | Pin R |
|-------|-----------------------------|------------------------------|-------|
| **1** | `[485]` MODBUS2_B | `[PWR]` GND (D11 12V — **DNP**) | **51** |
| **2** | `[485]` MODBUS2_A | `[PWR]` GND | **52** |
| **3** | `[485]` MODBUS1_B | `[PWR]` GND | **53** |
| **4** | `[485]` MODBUS1_A | `[CAN]` CAN2_N | **54** |
| **5** | `[DIR]` MDR_DIR3− | `[CAN]` CAN2_P | **55** |
| **6** | `[DIR]` MDR_DIR3+ | `[CAN]` CAN1_N | **56** |
| **7** | `[PWM]` MDR_PWM3− | `[CAN]` CAN1_P | **57** |
| **8** | `[PWM]` MDR_PWM3+ | `[DIR]` MDR_DIR4− | **58** |
| **9** | `[DIR]` MDR_DIR1− | `[DIR]` MDR_DIR4+ | **59** |
| **10** | `[DIR]` MDR_DIR1+ | `[PWM]` MDR_PWM4− | **60** |
| **11** | `[PWM]` MDR_PWM1− | `[PWM]` MDR_PWM4+ | **61** |
| **12** | `[PWM]` MDR_PWM1+ | `[DIR]` MDR_DIR2− | **62** |
| **13** | — | `[DIR]` MDR_DIR2+ | **63** |
| **14** | — | `[PWM]` MDR_PWM2− | **64** |
| **15** | — | `[PWM]` MDR_PWM2+ | **65** |
| **16** | `[ENC]` ENC_4A− | `[ENC]` ENC_4Z− | **66** |
| **17** | `[ENC]` ENC_4A+ | `[ENC]` ENC_4Z+ | **67** |
| **18** | `[ENC]` ENC_3Z− | `[ENC]` ENC_4B− | **68** |
| **19** | `[ENC]` ENC_3Z+ | `[ENC]` ENC_4B+ | **69** |
| **20** | `[ENC]` ENC_2Z− | `[ENC]` ENC_3B− | **70** |
| **21** | `[ENC]` ENC_2Z+ | `[ENC]` ENC_3B+ | **71** |
| **22** | `[ENC]` ENC_2B− | `[ENC]` ENC_3A− | **72** |
| **23** | `[ENC]` ENC_2B+ | `[ENC]` ENC_3A+ | **73** |
| **24** | `[ENC]` ENC_1B− | `[ENC]` ENC_2A− | **74** |
| **25** | `[ENC]` ENC_1B+ | `[ENC]` ENC_2A+ | **75** |
| **26** | `[ENC]` ENC_1A− | `[ENC]` ENC_1Z− | **76** |
| **27** | `[ENC]` ENC_1A+ | `[ENC]` ENC_1Z+ | **77** |
| **28** | `[ENC]` ENC_5B− | `[ENC]` ENC_5Z− | **78** |
| **29** | `[ENC]` ENC_5B+ | `[ENC]` ENC_5Z+ | **79** |
| **30** | `[ENC]` ENC_5A− | `[DI]` ISO_IN4 | **80** |
| **31** | `[ENC]` ENC_5A+ | `[DI]` ISO_IN3 | **81** |
| **32** | `[DI]` ISO_IN8 | `[DI]` ISO_IN2 | **82** |
| **33** | `[ISO]` **IN_ISOV+** ← User 5–30V | `[DI]` ISO_IN1 | **83** |
| **34** | `[ISO]` **IN_ISOGND** ← User GND | `[DO]` ISO_OUT5 | **84** |
| **35** | `[ISO]` **OUT_ISOV+** ← User 5–30V | `[DO]` ISO_OUT6 | **85** |
| **36** | `[ISO]` **OUT_ISOGND** ← User GND | `[DO]` ISO_OUT7 | **86** |
| **37** | `[DO]` ISO_OUT1 | `[DO]` ISO_OUT8 | **87** |
| **38** | `[DO]` ISO_OUT2 | `[DAC]` OPA_DAC_OUT2 | **88** |
| **39** | `[DO]` ISO_OUT3 | `[DAC]` OPA_DAC_OUT1 | **89** |
| **40** | `[DO]` ISO_OUT4 | `[DAC]` OPA_DAC_OUT4 | **90** |
| **41** | `[DI]` ISO_IN7 | `[DAC]` OPA_DAC_OUT3 | **91** |
| **42** | `[DI]` ISO_IN5 | `[PWR]` GND | **92** |
| **43** | `[ADC]` AI_IN8+ | `[ADC]` AI_IN1+ | **93** |
| **44** | `[ADC]` AI_IN8− | `[ADC]` AI_IN1− | **94** |
| **45** | `[ADC]` AI_IN7+ | `[ADC]` AI_IN2+ | **95** |
| **46** | `[ADC]` AI_IN7− | `[ADC]` AI_IN2− | **96** |
| **47** | `[ADC]` AI_IN6+ | `[ADC]` AI_IN3+ | **97** |
| **48** | `[ADC]` AI_IN6− | `[ADC]` AI_IN3− | **98** |
| **49** | `[ADC]` AI_IN5+ | `[ADC]` AI_IN4+ | **99** |
| **50** | `[ADC]` AI_IN5− | `[ADC]` AI_IN4− | **100** |

### 11.3 Hướng dẫn nối dây J7A / J7A Wiring Guide

#### ① Encoder RS-422
```
Encoder (RS-422)          J7A Connector
    A+ ──────────────────── ENC_1A+ (pin 27)
    A− ──────────────────── ENC_1A− (pin 26)
    B+ ──────────────────── ENC_1B+ (pin 25)
    B− ──────────────────── ENC_1B− (pin 24)
    Z+ ──────────────────── ENC_1Z+ (pin 77)
    Z− ──────────────────── ENC_1Z− (pin 76)
   GND ──────────────────── GND     (pin 92)
```
🇻🇳 Không cần thêm điện trở 120Ω — đã có sẵn trên board. Dùng cáp CAT5/6 tối đa 100m.  
🇬🇧 No external 120Ω termination needed — already on board. Use CAT5/6 cable up to 100m.

#### ② Motor Driver PWM+DIR (RS-422)
```
Servo Drive (RS-422)      J7A Connector
  PULSE+ ─────────────── MDR_PWM1+ (pin 12)
  PULSE− ─────────────── MDR_PWM1− (pin 11)
   DIR+  ─────────────── MDR_DIR1+ (pin 10)
   DIR−  ─────────────── MDR_DIR1− (pin 9)
   GND   ─────────────── GND       (pin 92)
```
🇻🇳 Tương thích với: Delta ASDA-B3, Mitsubishi MR-J4, Panasonic MINAS-A6, Leadshine ES-D508.  
🇬🇧 Compatible with: Delta ASDA-B3, Mitsubishi MR-J4, Panasonic MINAS-A6, Leadshine ES-D508.

#### ③ Analog Input ADC (16-bit differential)
```
Sensor/Transmitter        J7A Connector
  Signal (+) ──────────── AI_IN1+  (pin 93)
  Signal (−) ──────────── AI_IN1−  (pin 94)
  AGND       ──────────── GND      (pin 92)  ← AGND chỉ ở 1 điểm / single-point AGND
```
🇻🇳 Chọn dải ±5V hoặc ±10V qua jumper **J4** trên board. Không nối AGND với GND số ở nhiều điểm.  
🇬🇧 Select ±5V or ±10V range via jumper **J4** on board. Single-point AGND connection only.

#### ④ Isolated Digital I/O (DI/DO cách ly)
```
Nguồn PLC 24VDC           J7A Connector
  24VDC (+) ───────────── IN_ISOV+   (pin 33)  ← Cấp nguồn DI / DI supply
  24VDC (−) ───────────── IN_ISOGND  (pin 34)
  Sensor NPN ──────────── ISO_IN1    (pin 83)
  
  24VDC (+) ───────────── OUT_ISOV+  (pin 35)  ← Cấp nguồn DO / DO supply
  24VDC (−) ───────────── OUT_ISOGND (pin 36)
  Load (+)   ──────────── ISO_OUT1   (pin 37)
  Load (−)   ──────────── OUT_ISOGND (pin 36)
```
🇻🇳 Mức cách ly: **2500 VRMS**. Dải nguồn cách ly: 5–30V DC. Hai khối DI và DO có GND hoàn toàn độc lập.  
🇬🇧 Isolation: **2500 VRMS**. Isolation supply range: 5–30V DC. DI and DO blocks have fully independent grounds.

#### ⑤ CAN FD
```
CAN Bus (RS-485 level)    J7A Connector
  CANH ────────────────── CAN1_P (pin 57)
  CANL ────────────────── CAN1_N (pin 56)
  GND  ────────────────── GND    (pin 92)
```
🇻🇳 120Ω đã có trên board (R75). Nếu không phải đầu cuối bus → tháo R75.  
🇬🇧 120Ω already on board (R75). If not a bus endpoint → remove R75.

#### ⑥ RS-485 / Modbus RTU
```
RS-485 Device             J7A Connector
    A (+) ───────────────── MODBUS1_A (pin 4)
    B (−) ───────────────── MODBUS1_B (pin 3)
   GND   ───────────────── GND        (pin 92)
```
🇻🇳 Half-duplex, hướng TX/RX tự động. 120Ω termination (R51) đã có trên board.  
🇬🇧 Half-duplex, auto TX/RX direction. 120Ω termination (R51) already on board.

---

## 12. Ứng dụng / Applications

### 🤖 12.1 Robot / CNC đa trục / Multi-Axis CNC & Robot Arm
🇻🇳 Điều khiển 6 trục servo qua PCIe + FPGA interpolation real-time. Encoder feedback đến 10MHz. Modbus RTU đến VFD/HMI.  
🇬🇧 6-axis servo control via PCIe + FPGA real-time interpolation. Encoder feedback up to 10MHz. Modbus RTU to VFD/HMI.

### 📊 12.2 Thu thập dữ liệu / High-Speed DAQ
🇻🇳 8 kênh ADC 16-bit đồng thời 200kSPS. Streaming USB 3.0 ~200MB/s. DAC 4 kênh kích thích tín hiệu.  
🇬🇧 8-ch simultaneous 16-bit ADC at 200kSPS. USB 3.0 streaming ~200MB/s. 4-ch DAC for stimulus signals.

### 🏭 12.3 Tự động hoá công nghiệp / Industrial Automation (PC-PLC)
🇻🇳 8DI + 8DO cách ly 24VDC. CAN FD CANopen/CiA 402. RS-485 Modbus RTU. PREEMPT-RT Linux.  
🇬🇧 8DI + 8DO isolated 24VDC. CAN FD CANopen/CiA 402. RS-485 Modbus RTU. PREEMPT-RT Linux.

### 🚀 12.4 Thiết bị test PXI / PXI ATE Test Equipment
🇻🇳 Cài trong chassis PXI tiêu chuẩn. Trigger bus TRIG0–7. CLK10MHz đồng bộ hệ thống.  
🇬🇧 Install in standard PXI chassis. Trigger bus TRIG0–7. CLK10MHz system synchronization.

### 🎓 12.5 Nghiên cứu robot / Robotics Research
🇻🇳 FPGA hard real-time + MCU soft real-time. Cổng IMU J11 cho gyro/accelerometer. Vivado WebPACK miễn phí.  
🇬🇧 FPGA hard real-time + MCU soft real-time. IMU port J11 for gyro/accelerometer. Free Vivado WebPACK.

### 🔬 12.6 Chuyển động chính xác / Precision Motion (Semiconductor/Optics)
🇻🇳 DAC 16-bit cho piezo/galvo. ADC 16-bit đo sensor quang. PID loop FPGA >100kHz.  
🇬🇧 16-bit DAC for piezo/galvo control. 16-bit ADC for optical sensor. FPGA PID loop >100kHz.

---

## 13. Sơ đồ khối / Block Diagram

```mermaid
graph TB

  %% ─── HOST INTERFACES ───
  subgraph HOST["🖥️  HOST PC / PXI CHASSIS"]
    direction LR
    H_PCIE["PCIe x4\nGen2 Host"]
    H_USB["USB 3.0\nSuperSpeed Host"]
    H_PXI["PXI Chassis\nCN5 / CN6"]
  end

  %% ─── POWER ───
  subgraph PWR["⚡ POWER SUPPLY"]
    direction TB
    P_IN["12V Input\nCN10 / PCIe J1 / PXI"]
    P_ESTOP["E-STOP\nCN15"]
    P_5V["MAX20006\n12V → 5V Buck"]
    P_33["MP2122 U10\n5V → 3.3V + 2.5V"]
    P_18["MP2122 U11\n5V → 1.8V + 1.5V"]
    P_LDO["L78M12 U8\n12V → 12V_LDO\n(Analog clean)"]
  end

  %% ─── FPGA CORE ───
  subgraph FPGA["🔵 FPGA — Xilinx Artix-7 35K (XC7A035 on CN8)"]
    direction TB
    F_PCIE["PCIe Endpoint\nIP Core (GTP ×4)"]
    F_USB["USB FIFO\nFT601Q Interface\n32-bit bus"]
    F_ENC["Encoder Counter ×6\nA/B/Z · up to 10MHz"]
    F_PWM["PWM+DIR Gen ×6\n20kHz–200kHz"]
    F_ADC["ADC SPI Master\nAD7606C · 8ch · 16-bit"]
    F_DAC["DAC SPI Master\nDAC81404 · 4ch · 16-bit"]
    F_ISO["ISO DI/DO Logic\n8 DI + 8 DO"]
    F_CAN["CAN TX/RX\nFPGA direct path"]
    F_485["Modbus TX/RX\nFPGA direct path"]
    F_FMC["FMC Bridge\n16-bit parallel\n→ MCU"]
    F_RAM["HyperRAM\nW959D8NFY\n64Mbit"]
    F_XADC["XADC\nBoard health\nmonitor"]
  end

  %% ─── MCU ───
  subgraph MCU["🟢 MCU — STM32H743 @ 480MHz"]
    direction TB
    M_FMC["FMC Slave\n← FPGA"]
    M_CAN["FDCAN1/2\n2× CAN FD"]
    M_485["UART5\nModbus RTU"]
    M_IMU["SPI1/SPI2/I2C\nIMU ×3 (J11)"]
    M_QSPI["QSPI PSRAM\nAPS6404L\n64Mbit"]
    M_SWD["SWD Debug\nP1 + UART P2"]
  end

  %% ─── FIELD I/O via J7A ───
  subgraph FIELD["🟡 FIELD I/O — Connector J7A (100-pin · 5787082-9)"]
    direction TB
    IO_ENC["Encoder Input ×6\nRS-422 · AM26LV32E\nENC_1–6 A/B/Z±"]
    IO_MTR["Motor Output ×6\nRS-422 · AM26LV31E\nMDR_PWM1–6 ·DIR1–6±"]
    IO_ADC["Analog Input ×8\n16-bit diff · AD7606C\n±5V / ±10V"]
    IO_DAC["Analog Output ×4\n16-bit · DAC81404\n+ TL082 buffer"]
    IO_DI["Isolated DI ×8\nISO7740 · 2500VRMS\nISO_IN1–8"]
    IO_DO["Isolated DO ×8\nISO7740 · 2500VRMS\nISO_OUT1–8"]
    IO_CAN["CAN FD ×2\nSN65HVD230DR\nCAN1/2 P/N"]
    IO_485["RS-485 ×2\nADM3065EARZ\nMODBUS1/2 A/B"]
    IO_ISOPWR["ISO Supply\nIN_ISOV+ (pin33)\nOUT_ISOV+ (pin35)"]
  end

  %% ─── CONNECTIONS ───
  H_PCIE <-->|"GTP MGT lanes ×4"| F_PCIE
  H_USB  <-->|"USB 3.0 SuperSpeed"| F_USB
  H_PXI  <-->|"PCIe lanes + Triggers"| F_PCIE

  P_IN --> P_ESTOP
  P_ESTOP --> P_5V
  P_5V --> P_33
  P_5V --> P_18
  P_IN --> P_LDO

  P_5V -->|5V| FPGA
  P_33 -->|3.3V| FPGA
  P_18 -->|1.8V| FPGA
  P_5V -->|5V| MCU
  P_33 -->|3.3V| MCU
  P_LDO -->|12V_LDO| F_ADC
  P_LDO -->|12V_LDO| F_DAC

  F_ENC <-->|"FPGA_ENCx A/B/Z"| IO_ENC
  F_PWM -->|"FPGA_MDR_PWMx/DIRx"| IO_MTR
  F_ADC <-->|"SPI Serial"| IO_ADC
  F_DAC -->|"SPI + TL082"| IO_DAC
  F_ISO <-->|"ISO7740"| IO_DI
  F_ISO <-->|"ISO7740"| IO_DO
  F_CAN <-->|"CAN PHY"| IO_CAN
  F_485 <-->|"RS-485 PHY"| IO_485

  F_FMC <-->|"FMC 16-bit bus"| M_FMC
  M_CAN <-->|"FDCAN1/2 → PHY"| IO_CAN
  M_485 <-->|"UART → PHY"| IO_485
  M_IMU <-->|"SPI/I2C"| J11["IMU Port\nJ11 (30-pin)"]

  IO_ISOPWR -.->|"User supplies\n5–30V DC"| IO_DI
  IO_ISOPWR -.->|"User supplies\n5–30V DC"| IO_DO

  %% ─── STYLES ───
  style HOST fill:#1a237e,color:#e3f2fd,stroke:#3f51b5
  style PWR  fill:#1b5e20,color:#e8f5e9,stroke:#4caf50
  style FPGA fill:#0d47a1,color:#e3f2fd,stroke:#2196f3
  style MCU  fill:#1a3a1a,color:#c8e6c9,stroke:#4caf50
  style FIELD fill:#4a148c,color:#f3e5f5,stroke:#9c27b0
```

---

## 14. Lập trình & Debug / Programming & Debug

### 14.1 FPGA Programming — CN9 (JTAG)

| Thông số / Parameter | Giá trị / Value |
|----------------------|----------------|
| Connector | CN9 — 10-pin, pitch 2.54mm |
| Tín hiệu / Signals | FPGA_TCK, FPGA_TDI, FPGA_TDO, FPGA_TMS |
| ESD protection | BAS40-04 Schottky + 33Ω series (R119–R121, R150) |
| Phần mềm / Tool | Xilinx **Vivado / Vitis** (WebPACK — miễn phí / free) |
| Nạp / Bitfile | JTAG volatile · SPI Flash persistent (AC7A035 module) |

### 14.2 MCU Programming — P1 (SWD)

| Thông số / Parameter | Giá trị / Value |
|----------------------|----------------|
| SWD port | P1 — 3-pin (SWCLK, SWDIO, GND) |
| Debug UART | P2 — DBG_TX, DBG_RX |
| Boot mode | SW4 → BOOT0 high → DFU mode |
| IDE | STM32CubeIDE · OpenOCD · ST-Link V2 · J-Link |

### 14.3 Chỉ thị trạng thái / Status Indicators

| Linh kiện / Part | Chức năng / Function |
|-----------------|---------------------|
| **LED1** | Đèn trạng thái / Status LED (FPGA_LED_ST — firmware điều khiển / controlled) |
| **SW1** | Reset board → MCU NRST |
| **SW2** | DIP 8 vị trí / 8-position → MCU GPIO người dùng / user GPIO |
| **FPGA_BUTTON** | Nút người dùng → FPGA fabric |

### 14.4 Test Points / Điểm đo

| Nhóm / Group | Tín hiệu / Signals |
|-------------|-------------------|
| TP1–TP5 | ADC serial: DOUTA–D, SDI, SCLK, CS |
| TP6–TP23 | Encoder: ENC1–ENC6 A/B/Z |
| TP24–TP30 | Nguồn / Power rails: 12V, 5V, 3.3V, 2.5V, 1.8V, 1.5V, GND |
| TP31–TP35 | MCU / USB supply + debug |
| TP42–TP57 | DAC/ADC analog: OPA_DAC_OUT, ADC_VREF |
| TP87–TP94 | ADC control: BUSY, FRSTDATA, serial lines |

---

## Lịch sử phiên bản / Revision History

| Rev | Ngày / Date | Thay đổi / Changes |
|-----|------------|-------------------|
| 2.0 | 01/12/2026 | Phiên bản sản xuất hiện tại / Current production release |

---

*🏭 HBQ Technology · www.hbqtechnology.com*  
*📄 Tài liệu được tạo từ / Document generated from: `Sch_PCIEBaseCard_Rev2_0.pdf` (9 sheets)*  
*🔧 FPGA: Xilinx Artix-7 XC7A35T · MCU: STM32H743VIT6 · USB: FT601Q · ADC: AD7606C · DAC: DAC81404*
