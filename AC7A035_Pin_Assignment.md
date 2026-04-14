# ALINX AC7A035 — Artix-7 35K FPGA Module Pin Assignment

> 🇻🇳 **Tài liệu phân công chân FPGA — HBQ Artix-7 Motion Control Card**  
> 🇬🇧 **FPGA Pin Assignment Reference — HBQ Artix-7 Motion Control Card**  
>
> Module: **ALINX AC7A035** · FPGA: **Xilinx XC7A35T-2FGG484I** (Artix-7 35K)  
> Connector: 4 × **Panasonic AXK680337YG** (80-pin, 0.4mm pitch) → CN1, CN2, CN3, CN4  
> Base board: **HBQ Artix-7 Motion Control Card** · Schematic Rev 2.0 · 01/2026

---

## 📋 Mục lục / Table of Contents

1. [Thông số module AC7A035](#1-thông-số-module-ac7a035--module-specifications)
2. [Tài nguyên FPGA XC7A35T](#2-tài-nguyên-fpga-xc7a35t--fpga-resources)
3. [Sơ đồ connector module](#3-sơ-đồ-connector--connector-layout)
4. [CN1 — Power + Bank B13/B34/B16 (GPIO + CAN/Modbus + XADC)](#4-cn1--power--b13-b34-b16)
5. [CN2 — Bank B13/B14 + USB 3.0 Data Bus 32-bit](#5-cn2--b13-b14--usb-30-data-bus)
6. [CN3 — Bank B15/B16 (Motor, Encoder, DAC, ADC, ISO, JTAG)](#6-cn3--b15-b16-motor--encoder--dac--adc--iso--jtag)
7. [CN4 — MGT GTP (PCIe) + FMC Bus + PCIe Clock](#7-cn4--mgt-gtp-pcie--fmc-bus)
8. [Phân nhóm tín hiệu theo chức năng](#8-phân-nhóm-tín-hiệu--functional-signal-groups)
9. [XDC Pin Constraints — File tham khảo](#9-xdc-pin-constraints-reference)
10. [Lưu ý thiết kế FPGA](#10-lưu-ý-thiết-kế-fpga--design-notes)

---

## 1. Thông số module AC7A035 / Module Specifications

AC7A035 là module FPGA lõi (SoM) dựa trên FPGA Xilinx Artix-7 XC7A35T-2FGG484I, tích hợp DDR3 SDRAM, QSPI Flash và nguồn điện. Module mở rộng 146 IO chuẩn 3.3V, 15 IO chuẩn 1.5V, và 4 cặp tín hiệu GTP cao tốc RX/TX.

| Thông số / Parameter | Giá trị / Value |
|----------------------|----------------|
| **Model** | ALINX AC7A035 |
| **FPGA chip** | Xilinx XC7A35T-2FGG484I |
| **Package** | FGG484 (484-pin BGA) |
| **DDR3 SDRAM** | 1GB (MT41J256M16HA-125) |
| **QSPI Flash** | 16MB |
| **Clock chính / Main clock** | 200MHz SiT9102 (differential) — hệ thống / system |
| **GTP ref clock** | 125MHz SiT9102 (differential) — cho GTP transceiver |
| **Nguồn vào / Power input** | 5V DC (qua B2B connector) |
| **Kích thước / Size** | 60mm × 60mm |
| **Nhiệt độ / Temp range** | −40°C đến +85°C (Industrial grade) |
| **Connector B2B** | 4 × Panasonic AXK680337YG (80-pin, 0.4mm pitch) |
| **IO 3.3V** | 146 chân / pins |
| **IO 1.5V** | 15 chân / pins |
| **GTP Transceiver** | 4 lanes, tối đa / max 6.6 Gbps |
| **Chứng nhận / Cert** | CE, RoHS, EMC |

---

## 2. Tài nguyên FPGA XC7A35T / FPGA Resources

| Tài nguyên / Resource | Số lượng / Quantity | Ghi chú / Note |
|-----------------------|---------------------|----------------|
| Logic Cells | 33,280 | |
| CLB Slices | 5,200 | 6-input LUT |
| CLB Flip-Flops | 41,600 | |
| Block RAM | 1,800 Kbits | 90 × 18Kb tiles |
| DSP Slices | 90 | 18×25 multiplier |
| PCIe Gen2 | 1 | Hard IP |
| XADC | 1 | 12-bit, 1Msps |
| GTP Transceivers | 4 lanes | 6.6 Gbps max |
| I/O Banks | B13, B14, B15, B16, B34, B35 | |
| PLLs / MMCMs | 5 PLLs, 5 MMCMs | |
| Speed Grade | −2 | Industrial |

### Cấu hình nguồn FPGA / FPGA Power Rails (trên module / on-module)

| Rail | Điện áp / Voltage | Chức năng / Function |
|------|-------------------|---------------------|
| VCCINT | 1.0V | FPGA core logic |
| VCCBRAM | 1.0V | Block RAM |
| VCCAUX | 1.8V | FPGA auxiliary |
| VCCO Bank 13,14,15,16 | 3.3V | User I/O (qua B2B) |
| VCCO Bank 34,35 | 1.5V (hoặc/or 3.3V) | User I/O (qua B2B) |
| VMGTAVCC | 1.0V | GTP analog core |
| VMGTAVTT | 1.2V | GTP termination |
| VCC (DDR3) | 1.5V | DDR3 SDRAM |

---

## 3. Sơ đồ connector / Connector Layout

```
                     AC7A035 Module (60×60mm, top view)
       ┌─────────────────────────────────────────────┐
       │  ┌──────┐  DDR3 SDRAM     QSPI Flash        │
       │  │XC7A  │  1GB MT41J      16MB               │
       │  │35T   │                                    │
       │  │FGG484│  200MHz CLK     125MHz GTP CLK     │
       │  └──────┘  (SiT9102)      (SiT9102)          │
       │                                              │
       │  JTAG pads   Power regulators   LED          │
       └─────────────────────────────────────────────┘
              ║          ║          ║          ║
            [CN1]      [CN2]      [CN3]      [CN4]
           80-pin     80-pin     80-pin     80-pin
          AXK680    AXK680     AXK680     AXK680
          337YG     337YG      337YG      337YG
              │          │          │          │
       ┌──────┴──────┬───┴─────┬────┴────┬────┴────┐
       │  HBQ Artix-7 Motion Control Card (Base Board)       │
       │  B13/B34     B13/B14   B15/B16   MGT/FMC   │
       └─────────────────────────────────────────────┘
```

**Kết nối module sang base board / Module-to-Baseboard mapping:**

| Connector module | Connector base board | Bank FPGA | Chức năng chính / Main Function |
|-----------------|---------------------|-----------|--------------------------------|
| CN1 (80-pin) | CN1 trên base | B13, B34, B16 | Power + GPIO + XADC + CAN/Modbus + LED |
| CN2 (80-pin) | CN2 trên base | B13, B14 | USB 3.0 32-bit Data + Bank IO |
| CN3 (80-pin) | CN3 trên base | B15, B16 | Motor/Encoder/DAC/ADC/ISO/JTAG |
| CN4 (80-pin) | CN4 trên base | MGT, B16, B34 | PCIe GTP + FMC Bus MCU |

---

## 4. CN1 — Power + B13, B34, B16

**Connector:** Panasonic AXK680337YG · 80 chân / 80-pin · Pitch 0.4mm  
**Chức năng chính:** Cấp nguồn 5V vào module + Bank B13 GPIO + B34 (XADC, đặc biệt) + B16 (CAN, Modbus, USB GPIO, PCIE reset)

### 4.1 Chân nguồn / Power Pins (CN1)

| Pin CN1 | Tín hiệu / Signal | Mô tả / Description |
|---------|------------------|---------------------|
| 1, 3, 5, 7 | 5V | Nguồn vào module / Module power input (5V main) |
| 2, 4, 6, 8 | 5V | Nguồn vào module / Module power input (5V main) |
| 9, 10 | GND | GND |
| 29, 30 | GND | GND |
| 39, 40 | GND | GND |
| 49, 50 | GND | GND |
| 69, 70 | GND | GND |
| 79, 80 | GND | GND |

> ⚠️ **VI:** Module AC7A035 chỉ cần nguồn 5V DC qua các chân nguồn trên CN1 (và các CN khác). Toàn bộ điện áp nội bộ (1.0V, 1.8V, 3.3V...) được tạo bởi mạch nguồn tích hợp trên module.  
> ⚠️ **EN:** The AC7A035 requires only 5V DC via power pins on CN1 (and other CNs). All internal voltages (1.0V, 1.8V, 3.3V...) are generated by the on-module power management circuit.

### 4.2 Bank B13 GPIO (CN1)

| Pin CN1 | Tên FPGA / FPGA Net | Bank | Chức năng / Function |
|---------|---------------------|------|---------------------|
| 21 | B13_L5_P | B13 | GPIO differential pair |
| 23 | B13_L5_N | B13 | GPIO differential pair |
| 25 | B13_L7_P | B13 | GPIO differential pair |
| 27 | B13_L7_N | B13 | GPIO differential pair |
| 31 | B13_L6_P | B13 | GPIO |
| 33 | B13_L3_P (FPGA_LED_ST) | B13 | **LED trạng thái FPGA / FPGA status LED** |
| 35 | B34_L23_P (FPGA_BUTTON) | B34 | **Nút người dùng / User button** |
| 37 | B34_L23_N | B34 | GPIO |
| 11–19 | B13_L4_P/N, B13_L1_P/N, B13_L2_P/N... | B13 | GPIO pairs |

### 4.3 Bank B34 — XADC + Special (CN1)

| Pin CN1 | Tên FPGA / FPGA Net | Chức năng / Function |
|---------|---------------------|---------------------|
| 41 | B34_L18_N | GPIO / Differential |
| 43 | B34_L18_P | GPIO / Differential |
| 45 | B34_L19_N | GPIO |
| 47 | B34_L19_P | GPIO |
| 51 | XADC_VN / OPA_DIFF_AI_FPGA_IN0− | **XADC analog input negative** |
| 53 | XADC_VP / OPA_DIFF_AI_FPGA_IN0+ | **XADC analog input positive** |
| 55 | B34_L25 | GPIO |
| 57 | B34_L24_P | GPIO |
| 59 | B34_L24_N | GPIO |

> 📌 **XADC:** Hai chân XADC_VP/VN kết nối qua op-amp vi sai (OPA) tới ngõ vào analog J7A. Dải đo: ±0.5V (VP−VN). Có thể đo điện áp bên ngoài sau khi scale qua resistor divider.  
> 📌 **XADC:** The XADC_VP/VN pins are connected through a differential op-amp to the J7A analog input. Range: ±0.5V (VP−VN) directly at XADC pins.

### 4.4 Bank B16 — CAN, Modbus, USB GPIO, PCIE Reset (CN1)

| Pin CN1 | Tên FPGA / FPGA Net | Package Pin | Chức năng / Function |
|---------|---------------------|-------------|---------------------|
| 61 | B16_L1_N (USB_GPIO2) | — | USB3 GPIO2 → FT601Q |
| 63 | B16_L1_P (USB_GPIO1) | — | USB3 GPIO1 → FT601Q |
| 65 | B16_L4_N (FPGA_MODBUS_TX) | — | **RS-485 Modbus TX → ADM3065E** |
| 67 | B16_L4_P (FPGA_MODBUS_RX) | — | **RS-485 Modbus RX ← ADM3065E** |
| 69 | — | — | GND (R226 buffer) |
| 71 | B16_L6_N (FPGA_CAN_TX) | — | **CAN TX → SN65HVD230DR** |
| 73 | B16_L6_P (FPGA_CAN_RX) | — | **CAN RX ← SN65HVD230DR** |
| 75 | B16_L8_P (FPGA_RESET75) | — | FPGA Reset |
| 77 | B16_L8_N (PCIE_RESET) | — | **PCIe PERST# reset** |
| 79 | — | — | DNP / spare |

---

## 5. CN2 — B13/B14 + USB 3.0 Data Bus

**Chức năng chính / Main function:** Kết nối FPGA với FT601Q USB 3.0 bridge qua bus dữ liệu 32-bit. Đây là connector có mật độ tín hiệu cao nhất liên quan đến USB.

### 5.1 Bank B13 IO (CN2)

| Pin CN2 | Tên FPGA / FPGA Net | Chức năng / Function |
|---------|---------------------|---------------------|
| 1 | B13_L16_P | GPIO |
| 2 | B13_L16_N | GPIO |
| 3 | B13_L15_P | GPIO |
| 5 | B13_L15_N | GPIO |
| 7 | B13_L14_P | GPIO |
| 9 | B13_L14_N | GPIO |
| 11 | B13_L13_P | GPIO |
| 13 | B13_L13_N | GPIO |
| 15 | B13_L12_P | GPIO |
| 17 | B13_L12_N | GPIO |
| 21 | B13_L11_P | GPIO |
| 23 | B13_L11_N | GPIO |
| 25 | B13_L10_P | GPIO |
| 27 | B13_L10_N | GPIO |

### 5.2 Bank B14 + USB 3.0 Data Bus 32-bit (CN2)

Tín hiệu **USB_DATA[0..31]** kết nối từ FPGA Bank B14 tới chip FT601Q-B-T (U12) qua connector CN2.

| Tên tín hiệu / Signal | FPGA Bank B14 Net | XDC Package Pin | Ghi chú |
|----------------------|------------------|-----------------|---------|
| USB_DATA0 | B14_L16_P | — | FT601Q DATA[0] |
| USB_DATA1 | B14_L16_N | — | FT601Q DATA[1] |
| USB_DATA2 | B14_L10_P → CN2 | — | FT601Q DATA[2] |
| USB_DATA3 | B14_L10_N | — | |
| USB_DATA4 | B14_L8_N | — | |
| USB_DATA5 | B14_L8_P | — | |
| USB_DATA6 | B14_L15_N | — | |
| USB_DATA7 | B14_L15_P | — | |
| USB_DATA8 | B14_L17_P | — | |
| USB_DATA9 | B14_L17_N | — | |
| USB_DATA10 | B14_L6_N | — | |
| USB_DATA11 | B14_L7_N | — | |
| USB_DATA12 | B14_L7_P | — | |
| USB_DATA13 | B14_L4_P | — | |
| USB_DATA14 | B14_L4_N | — | |
| USB_DATA15 | B14_L9_P | — | |
| USB_DATA16 | B14_L19_N | — | |
| USB_DATA17 | B14_L19_P | — | |
| USB_DATA18 | B14_L18_N | — | |
| USB_DATA19 | B14_L18_P | — | |
| USB_DATA20 | B14_L20_N | — | |
| USB_DATA21 | B14_L20_P | — | |
| USB_DATA22 | B14_L23_P | — | |
| USB_DATA23 | B14_L23_N | — | |
| USB_DATA24 | B14_IO25 | — | |
| USB_DATA25 | B14_L24_P | — | |
| USB_DATA26 | B14_IO0 | — | |
| USB_DATA27 | B14_L12_N | — | |
| USB_DATA28 | B14_L12_P | — | |
| USB_DATA29 | B14_L13_N | — | |
| USB_DATA30 | B14_L13_P | — | |
| USB_DATA31 | B14_L3_N | — | |

### 5.3 USB 3.0 Control Signals (CN2)

| Tên tín hiệu / Signal | FPGA Net | XDC Package Pin | Chức năng / Function |
|----------------------|----------|-----------------|---------------------|
| USB_TXE_N | B14_L3_P | **U17** | TXE# — FIFO có thể nhận / FIFO ready to receive |
| USB_RXF_N | B14_L21_N (?) | **U18** | RXF# — FIFO có dữ liệu / FIFO has data |
| USB_WR_N | B14_L21_P | **R19** | WR# — ghi dữ liệu / write strobe |
| USB_CLK | B14_L24_N | **Y22** | Clock từ FT601Q / Clock from FT601Q |
| USB_OE_N | B14_L22_P | — | OE# — output enable |
| USB_RD_N | B14_L22_N | — | RD# — read strobe |
| USB_SIWU_N | B16_L5_P | — | Send immediate / wake up |
| USB_WAKEUP | B16_L5_N | — | Wakeup signal |
| USB_RST | B16_L7_P | — | Reset FT601Q |
| USB_BE0 | B16_L7_N | **R16** | Byte enable [0] |
| USB_BE1 | B16_L9_P | **P15** | Byte enable [1] |
| USB_BE2 | B16_L9_N | **N17** | Byte enable [2] |
| USB_BE3 | B16_L11_P | **P17** | Byte enable [3] |
| USB_GPIO1 | B16_L1_P | — | FT601Q GPIO1 |
| USB_GPIO2 | B16_L1_N | — | FT601Q GPIO2 |

---

## 6. CN3 — B15/B16 (Motor, Encoder, DAC, ADC, ISO, JTAG)

**Connector:** AXK680337YG 80-pin  
**Chức năng chính:** Tín hiệu real-time quan trọng nhất — điều khiển động cơ, encoder, DAC/ADC SPI, cách ly DI/DO, JTAG FPGA

### 6.1 Motor Control PWM + DIR (CN3)

| Pin CN3 | Tên FPGA / FPGA Net | Bank | Chức năng / Function |
|---------|---------------------|------|---------------------|
| 1 | B15_IO0 — FPGA_MDR_DIR4 | B15 | **Motor 4 DIR → AM26LV31E** |
| 2 | B16_IO0 — FPGA_ISO_OUT1 | B16 | ISO Output 1 |
| 3 | B15_IO0 — FPGA_MDR_PWM4 | B15 | **Motor 4 PWM → AM26LV31E** |
| 4 | — FPGA_ISO_OUT2 | B16 | ISO Output 2 |
| 5 | B15_L4_P — FPGA_MDR_DIR3 | B15 | **Motor 3 DIR** |
| 6 | — FPGA_ISO_OUT3 | B16 | ISO Output 3 |
| 7 | B15_L4_N — FPGA_MDR_PWM3 | B15 | **Motor 3 PWM** |
| 8 | — FPGA_ISO_OUT4 | B16 | ISO Output 4 |
| 9 | B15_L2_N — FPGA_MDR_DIR2 | B15 | **Motor 2 DIR** |
| 11 | — FPGA_MDR_DIR1 | B15 | **Motor 1 DIR** |
| 12 | — FPGA_MDR_PWM2 | B15 | **Motor 2 PWM** |
| 13 | — FPGA_MDR_PWM1 (B15_L12_P) | B15 | **Motor 1 PWM** |
| 10 | — FPGA_ISO_OUT5 | B16 | ISO Output 5 |
| 14 | — FPGA_ISO_OUT6 | B16 | ISO Output 6 |
| 16 | — FPGA_ISO_OUT7 | B16 | ISO Output 7 |
| 18 | — FPGA_ISO_OUT8 | B16 | ISO Output 8 |

### 6.2 Encoder Input (CN3)

| Pin CN3 | Tên FPGA / FPGA Net | Bank | Chức năng / Function |
|---------|---------------------|------|---------------------|
| 23 | B15_L11_N — FPGA_ENC4B | B15 | **Encoder 4 channel B** |
| 22 | B15_L11_P — FPGA_ENC4A | B15 | **Encoder 4 channel A** |
| 25 | B15_L1_N — FPGA_ENC4Z | B15 | **Encoder 4 index Z** |
| 27 | B15_L1_P — FPGA_ENC3Z | B15 | **Encoder 3 index Z** |
| 31 | B15_L3_P — FPGA_ENC3B (B15_L5_P) | B15 | **Encoder 3 channel B** |
| 33 | B15_L5_N — FPGA_ENC3A | B15 | **Encoder 3 channel A** |
| 35 | B15_L3_N — FPGA_ENC2Z | B15 | **Encoder 2 index Z** |
| 37 | B15_L8_N — FPGA_ENC2B | B15 | **Encoder 2 channel B** (→ B15_L8_P?) |
| 39 | — FPGA_ENC2A | B15 | **Encoder 2 channel A** |
| 41 | B15_L19_P — FPGA_ENC1Z | B15 | **Encoder 1 index Z** |
| 43 | B15_L20_P — FPGA_ENC1B | B15 | **Encoder 1 channel B** |
| 47 | B15_L20_N — FPGA_ENC1A | B15 | **Encoder 1 channel A** |
| 55 | B15_L21_P — FPGA_ISO_IN6 (→ENC5B) | B15 | Encoder 5 |
| 57 | B15_L7_N/P — FPGA_ISO_IN7 (→ENC5A?) | B15 | Encoder 5/6 |
| 59 | B15_L9_N/P — FPGA_ISO_IN8 | B15 | Encoder 6 |

### 6.3 DAC SPI Interface (CN3)

| Pin CN3 | Tên FPGA / FPGA Net | XDC Package Pin | Chức năng / Function |
|---------|---------------------|-----------------|---------------------|
| 21 | B15_L11_P — DAC_MISO | **G21** | DAC SPI MISO ← DAC81404 |
| 24 | B15_L10_P — DAC_SCK | **G22** | DAC SPI Clock → DAC81404 |
| 26 | B15_L10_N — DAC_MOSI | **G20** | DAC SPI MOSI → DAC81404 |
| 28 | B15_L8_P — DAC_CS | **H20** | DAC Chip Select |
| 32 | B15_L9_P — DAC_LDAC | **H22** | DAC LDAC (load latch) |
| 34 | B15_L9_N — DAC_CLR | **J22** | DAC Clear all channels |
| 36 | B15_L13_P — DAC_RST | **K21** | DAC Reset |
| 38 | B15_L13_N — ADC_RESET | — | ADC Reset |

### 6.4 ADC SPI Interface (CN3)

| Pin CN3 | Tên FPGA / FPGA Net | Chức năng / Function |
|---------|---------------------|---------------------|
| 42 | B15_L15_P — ADC_SER_SDI | ADC SPI MOSI → AD7606C |
| 44 | B15_L15_N — ADC_SER_DOUTA | ADC SPI MISO channel A ← AD7606C |
| 46 | B15_L14_P — ADC_SER_DOUTB | ADC data out B |
| 48 | B15_L14_N — ADC_SER_DOUTC | ADC data out C |
| 50 | B15_L6_P — (ADC_SER_DOUTD?) | ADC data out D |
| 52 | B15_L13_N — ADC_SER_SCLK | ADC SPI Clock |
| 54 | B15_L13_P — ADC_SER_CS | ADC Chip Select |
| 56 | B15_L10_N — ADC_SER_FRSTDATA | ADC FRSTDATA flag |
| 58 | B15_L10_P — (ADC_CONVTS?) | ADC CONVST trigger |
| 62 | B15_L18_P — ADC_BUSY | ADC Busy flag |
| 64 | B15_L18_N — ADC_CONVTS | ADC Conversion start |

### 6.5 Isolated Digital I/O (CN3)

| Pin CN3 | Tên FPGA / FPGA Net | Bank | Chiều / Dir | Chức năng |
|---------|---------------------|------|-------------|-----------|
| 51 | B15_L14_P — FPGA_ISO_IN8 | B15 | Input | ISO DI channel 8 |
| 53 | B15_L21_P — FPGA_ISO_IN6 | B15 | Input | ISO DI channel 6 |
| 55 | B15_L21_N — FPGA_ISO_IN7? | B15 | Input | ISO DI channel 7 |
| 57 | B15_L22_P — FPGA_ISO_IN5 | B15 | Input | ISO DI channel 5 |
| 59 | B15_L22_N — FPGA_ISO_IN4? | B15 | Input | ISO DI channel 4 |
| 63 | B15_L23_N — FPGA_ISO_IN3 | B15 | Input | ISO DI channel 3 |
| 65 | B15_L22_P — FPGA_ISO_IN2? | B15 | Input | ISO DI channel 2 |
| 67 | B15_L24_N — FPGA_ISO_IN1 | B15 | Input | ISO DI channel 1 |

### 6.6 IMU Sensor Interface (CN3)

| Pin CN3 | Tên FPGA / FPGA Net | Chức năng / Function |
|---------|---------------------|---------------------|
| 66 | B15_L16_P — FPGA_IMU_CS1 | IMU SPI CS chip 1 |
| 68 | B15_L16_N — FPGA_IMU_CS2 | IMU SPI CS chip 2 |
| 70 | B15_L17_N — FPGA_IMU_INT3 | IMU interrupt 3 |
| 72 | B15_L17_P — FPGA_IMU_CS3 | IMU SPI CS chip 3 |
| 69 | — FPGA_SCL | I2C SCL |
| 73 | B15_L24_P — FPGA_SDA | I2C SDA |

### 6.7 JTAG Interface (CN3)

| Pin CN3 | Tên FPGA / FPGA Net | Chức năng / Function |
|---------|---------------------|---------------------|
| 77 | B15_L2_N — FPGA_TCK | JTAG Clock (TCK) |
| 78 | — FPGA_TDI | JTAG Data In (TDI) |
| 79 | B15_L2_P — FPGA_TDO | JTAG Data Out (TDO) |
| 80 | — FPGA_TMS | JTAG Mode Select (TMS) |

> 📌 Các tín hiệu JTAG này kết nối tới connector **CN9** trên base board qua các resistor 33Ω và ESD clamp BAS40-04.

---

## 7. CN4 — MGT GTP (PCIe) + FMC Bus

**Chức năng chính:** 4 lane GTP cho PCIe x4 · FMC parallel bus 16-bit tới MCU STM32H743 · PCIe reference clock

### 7.1 GTP Transceiver — PCIe x4 (CN4)

| Pin CN4 | Tên tín hiệu / Signal | Chức năng / Function |
|---------|----------------------|---------------------|
| 13 | MGT_TX3_P | GTP TX lane 3 + (PCIe) |
| 14 | MGT_TX3_N | GTP TX lane 3 − |
| 15 | MGT_TX1_P | GTP TX lane 1 + |
| 16 | MGT_TX1_N | GTP TX lane 1 − |
| 17 | MGT_RX3_P | GTP RX lane 3 + |
| 18 | MGT_RX3_N | GTP RX lane 3 − |
| 19 | MGT_RX1_P | GTP RX lane 1 + |
| 20 | MGT_RX1_N | GTP RX lane 1 − |
| 11/12 | MGT_TX2_P/N | GTP TX lane 2 |
| 21/22 | MGT_TX0_P/N | GTP TX lane 0 |
| 23/24 | MGT_RX2_P/N | GTP RX lane 2 |
| 25/26 | MGT_RX0_P/N | GTP RX lane 0 |
| 27/28 | MGT_TX3_P/N (alt) | TX3 routing |
| 29/30 | MGT_RX3_P/N (alt) | RX3 routing |
| 35/36 | PCIex16_CLK1_P/N | **PCIe ref clock (100MHz từ host)** |
| 37/38 | MGT_CLK1_P/N | GTP ref clock 125MHz |
| 39/40 | PCIE_BP_CLK1_P/N | Backplane PCIe clock |

> 📌 **PCIe clock**: `clk_in_clk_p` → Package Pin **R4** (DIFF_HSTL_II_18). Đây là 125MHz differential reference clock cho GTP transceivers.

### 7.2 FMC Bus — MCU Interface (CN4)

Giao tiếp song song 16-bit giữa FPGA và MCU STM32H743 qua FMC (Flexible Memory Controller).

| Tín hiệu FMC / FMC Signal | FPGA Net | B16 / B34 Pin | MCU Pin | Chức năng |
|--------------------------|----------|---------------|---------|-----------|
| FMC_A16 | B16_L5_P | CN4 pin 41 | PD11 | Address bit 16 |
| FMC_A17 | B16_L5_N | CN4 pin 42 | PD12 | Address bit 17 |
| FMC_A18 | B16_L7_P | CN4 pin 43 | PD13 | Address bit 18 |
| FMC_A19 | B16_L7_N | CN4 pin 44 | PE3 | Address bit 19 |
| FMC_A20 | B16_L9_P | CN4 pin 51 | PE4 | Address bit 20 |
| FMC_A21 | B16_L9_N | CN4 pin 53 | PE5 | Address bit 21 |
| FMC_A22 | B16_L11_P | CN4 pin 55 | PE6 | Address bit 22 |
| FMC_A23 | B16_L11_N | CN4 pin 57 | PE2 | Address bit 23 |
| FMC_DA0 | B16_L13_P | CN4 pin 41 (R) | PD14 | Data/Addr 0 |
| FMC_DA1–15 | B16_Lx | CN4 pins | PD0–PD15 | Data bus |
| FMC_NOE | B16_L15_P | — | PD4 | Output enable |
| FMC_NWE | B16_L15_N | — | PD5 | Write enable |
| FMC_NE1 | B16_L17_P | — | PD7 | Chip select |
| FMC_NWAIT | B16_L17_N | — | PD6 | Wait signal |
| FMC_NL | B16_L19_P | — | PB7 | Latch |
| FMC_NBL0 | B16_L19_N | — | PE0 | Byte lane 0 |
| FMC_NBL1 | B16_L21_P | — | PE1 | Byte lane 1 |
| FMC_CLK | B16_L21_N | — | PD3 | FMC clock |

---

## 8. Phân nhóm tín hiệu / Functional Signal Groups

### 8.1 Bảng tổng hợp tất cả tín hiệu có XDC mapping / All Confirmed XDC Mappings

Các chân sau được xác nhận trong file **`artix7_35K_pin_assignment.xdc`**:

#### USB 3.0 — FT601Q Control (LVCMOS33)

| Tên port / Port Name | Package Pin | Tín hiệu board / Board Net | Mô tả |
|---------------------|-------------|---------------------------|-------|
| `txe_n` | **U17** | USB_TXE_N | FIFO TX empty (FT601Q) |
| `rxf_n` | **U18** | USB_RXF_N | FIFO RX full (FT601Q) |
| `wr_n` | **R19** | USB_WR_N | Write strobe |
| `usb_clk` | **Y22** | USB_CLK | FT601Q clock output |
| `resetn` | **C13** | USB_RST | FT601Q reset |
| `wakeup[0]` | **V20** | USB_WAKEUP | Wakeup (PULLUP + SLOW) |

#### USB 3.0 — Data Bus data[31:0] (LVCMOS33)

| Port | Package Pin | Port | Package Pin |
|------|-------------|------|-------------|
| `data[31]` | R17 | `data[15]` | Y21 |
| `data[30]` | P16 | `data[14]` | U21 |
| `data[29]` | P20 | `data[13]` | T21 |
| `data[28]` | N15 | `data[12]` | W21 |
| `data[27]` | N14 | `data[11]` | W22 |
| `data[26]` | N13 | `data[10]` | T20 |
| `data[25]` | P14 | `data[9]` | AB18 |
| `data[24]` | R14 | `data[8]` | AA18 |
| `data[23]` | R18 | `data[7]` | AA19 |
| `data[22]` | T18 | `data[6]` | AB20 |
| `data[21]` | U22 | `data[5]` | AA20 |
| `data[20]` | V22 | `data[4]` | AA21 |
| `data[19]` | Y18 | `data[3]` | AB22 |
| `data[18]` | Y19 | `data[2]` | AB21 |
| `data[17]` | W19 | `data[1]` | W17 |
| `data[16]` | W20 | `data[0]` | V17 |

#### USB 3.0 — Byte Enable be[3:0] (LVCMOS33)

| Port | Package Pin | Board net |
|------|-------------|-----------|
| `be[3]` | **P17** | USB_BE3 |
| `be[2]` | **N17** | USB_BE2 |
| `be[1]` | **P15** | USB_BE1 |
| `be[0]` | **R16** | USB_BE0 |

#### DAC SPI — DAC81404 (LVCMOS33)

| Port | Package Pin | Board net | Mô tả |
|------|-------------|-----------|-------|
| `miso` | **G21** | DAC_MISO | DAC SPI MISO |
| `sclk` | **G22** | DAC_SCK | DAC SPI Clock |
| `mosi` | **G20** | DAC_MOSI | DAC SPI MOSI |
| `cs` | **H20** | DAC_CS | DAC Chip Select |
| `LDAC` | **H22** | DAC_LDAC | Load DAC register |
| `RST` | **K21** | DAC_RST | DAC Reset |
| `CLR` | **J22** | DAC_CLR | DAC Clear all |

#### Status LEDs (LVCMOS33)

| Port | Package Pin | Board net | Mô tả |
|------|-------------|-----------|-------|
| `vang` | **L18** | LED_YELLOW | LED vàng / Yellow LED |
| `xanh` | **N19** | LED_GREEN | LED xanh / Green LED |

#### Clock Reference

| Port | Package Pin | IOSTANDARD | Chức năng |
|------|-------------|-----------|-----------|
| `clk_in_clk_p` | **R4** | DIFF_HSTL_II_18 | 125MHz GTP reference differential clock |

---

## 9. XDC Pin Constraints Reference

File XDC hoàn chỉnh cho dự án HBQ PCIe Base Card. Copy và dùng làm template.

```tcl
# ============================================================
# HBQ Artix-7 Motion Control Card — FPGA Pin Constraints (XDC)
# FPGA: Xilinx XC7A35T-2FGG484I (ALINX AC7A035 module)
# ============================================================

# ─── CLOCK ───────────────────────────────────────────────────
# 125MHz GTP reference clock (differential, DIFF_HSTL_II_18)
set_property PACKAGE_PIN R4        [get_ports clk_in_clk_p]
set_property IOSTANDARD DIFF_HSTL_II_18 [get_ports clk_in_clk_p]

# ─── USB 3.0 FT601Q CONTROL SIGNALS (LVCMOS33) ──────────────
set_property PACKAGE_PIN U17 [get_ports txe_n]
set_property PACKAGE_PIN U18 [get_ports rxf_n]
set_property PACKAGE_PIN R19 [get_ports wr_n]
set_property PACKAGE_PIN Y22 [get_ports usb_clk]
set_property PACKAGE_PIN C13 [get_ports resetn]
set_property PACKAGE_PIN V20 [get_ports {wakeup[0]}]
set_property SLEW SLOW   [get_ports {wakeup[0]}]
set_property PULLTYPE PULLUP [get_ports {wakeup[0]}]

set_property PACKAGE_PIN P17 [get_ports {be[3]}]
set_property PACKAGE_PIN N17 [get_ports {be[2]}]
set_property PACKAGE_PIN P15 [get_ports {be[1]}]
set_property PACKAGE_PIN R16 [get_ports {be[0]}]

set_property IOSTANDARD LVCMOS33 [get_ports txe_n]
set_property IOSTANDARD LVCMOS33 [get_ports rxf_n]
set_property IOSTANDARD LVCMOS33 [get_ports wr_n]
set_property IOSTANDARD LVCMOS33 [get_ports usb_clk]
set_property IOSTANDARD LVCMOS33 [get_ports resetn]
set_property IOSTANDARD LVCMOS33 [get_ports {wakeup[0]}]
set_property IOSTANDARD LVCMOS33 [get_ports {be[3]}]
set_property IOSTANDARD LVCMOS33 [get_ports {be[2]}]
set_property IOSTANDARD LVCMOS33 [get_ports {be[1]}]
set_property IOSTANDARD LVCMOS33 [get_ports {be[0]}]

# ─── USB 3.0 DATA BUS data[31:0] (LVCMOS33) ─────────────────
set_property PACKAGE_PIN R17  [get_ports {data[31]}]
set_property PACKAGE_PIN P16  [get_ports {data[30]}]
set_property PACKAGE_PIN P20  [get_ports {data[29]}]
set_property PACKAGE_PIN N15  [get_ports {data[28]}]
set_property PACKAGE_PIN N14  [get_ports {data[27]}]
set_property PACKAGE_PIN N13  [get_ports {data[26]}]
set_property PACKAGE_PIN P14  [get_ports {data[25]}]
set_property PACKAGE_PIN R14  [get_ports {data[24]}]
set_property PACKAGE_PIN R18  [get_ports {data[23]}]
set_property PACKAGE_PIN T18  [get_ports {data[22]}]
set_property PACKAGE_PIN U22  [get_ports {data[21]}]
set_property PACKAGE_PIN V22  [get_ports {data[20]}]
set_property PACKAGE_PIN Y18  [get_ports {data[19]}]
set_property PACKAGE_PIN Y19  [get_ports {data[18]}]
set_property PACKAGE_PIN W19  [get_ports {data[17]}]
set_property PACKAGE_PIN W20  [get_ports {data[16]}]
set_property PACKAGE_PIN Y21  [get_ports {data[15]}]
set_property PACKAGE_PIN U21  [get_ports {data[14]}]
set_property PACKAGE_PIN T21  [get_ports {data[13]}]
set_property PACKAGE_PIN W21  [get_ports {data[12]}]
set_property PACKAGE_PIN W22  [get_ports {data[11]}]
set_property PACKAGE_PIN T20  [get_ports {data[10]}]
set_property PACKAGE_PIN AB18 [get_ports {data[9]}]
set_property PACKAGE_PIN AA18 [get_ports {data[8]}]
set_property PACKAGE_PIN AA19 [get_ports {data[7]}]
set_property PACKAGE_PIN AB20 [get_ports {data[6]}]
set_property PACKAGE_PIN AA20 [get_ports {data[5]}]
set_property PACKAGE_PIN AA21 [get_ports {data[4]}]
set_property PACKAGE_PIN AB22 [get_ports {data[3]}]
set_property PACKAGE_PIN AB21 [get_ports {data[2]}]
set_property PACKAGE_PIN W17  [get_ports {data[1]}]
set_property PACKAGE_PIN V17  [get_ports {data[0]}]

set_property IOSTANDARD LVCMOS33 [get_ports {data[*]}]
set_property CLOCK_DEDICATED_ROUTE FALSE [get_nets usb_clk_IBUF]

# ─── DAC SPI — DAC81404 (LVCMOS33) ──────────────────────────
set_property PACKAGE_PIN G21 [get_ports miso]   ;# DAC_MISO
set_property PACKAGE_PIN G22 [get_ports sclk]   ;# DAC_SCK
set_property PACKAGE_PIN G20 [get_ports mosi]   ;# DAC_MOSI
set_property PACKAGE_PIN H20 [get_ports cs]     ;# DAC_CS
set_property PACKAGE_PIN H22 [get_ports LDAC]   ;# DAC_LDAC
set_property PACKAGE_PIN K21 [get_ports RST]    ;# DAC_RST
set_property PACKAGE_PIN J22 [get_ports CLR]    ;# DAC_CLR

set_property IOSTANDARD LVCMOS33 [get_ports miso]
set_property IOSTANDARD LVCMOS33 [get_ports sclk]
set_property IOSTANDARD LVCMOS33 [get_ports mosi]
set_property IOSTANDARD LVCMOS33 [get_ports cs]
set_property IOSTANDARD LVCMOS33 [get_ports LDAC]
set_property IOSTANDARD LVCMOS33 [get_ports RST]
set_property IOSTANDARD LVCMOS33 [get_ports CLR]

# ─── STATUS LEDs (LVCMOS33) ──────────────────────────────────
set_property PACKAGE_PIN L18 [get_ports vang]   ;# LED Yellow
set_property PACKAGE_PIN N19 [get_ports xanh]   ;# LED Green/Blue
set_property IOSTANDARD LVCMOS33 [get_ports vang]
set_property IOSTANDARD LVCMOS33 [get_ports xanh]

# ─── TIMING CONSTRAINTS ──────────────────────────────────────
# Async clock groups (add as needed for your design)
# set_clock_groups -asynchronous \
#   -group [get_clocks clk_125mhz] \
#   -group [get_clocks usb_clk]
```

---

## 10. Lưu ý thiết kế FPGA / Design Notes

### 10.1 I/O Bank Voltage (VCCO)

| Bank | VCCO | IOSTANDARD mặc định / Default | Ghi chú |
|------|------|-------------------------------|---------|
| B13 | 3.3V | LVCMOS33 | GPIO tổng quát / General purpose |
| B14 | 3.3V | LVCMOS33 | USB 3.0 data bus |
| B15 | 3.3V | LVCMOS33 | Motor/Encoder/DAC/ADC |
| B16 | 3.3V | LVCMOS33 | Motor/ISO/FMC/CAN |
| B34 | 1.5V hoặc 3.3V | LVCMOS15 hoặc LVCMOS33 | XADC, special IO |
| B35 | — | — | Không dùng / Not used |

> ⚠️ **VI:** Tất cả IO trong cùng một bank phải dùng cùng VCCO. Không được dùng LVCMOS33 cho các chân thuộc bank VCCO=1.5V.  
> ⚠️ **EN:** All I/O within the same bank must use the same VCCO. Do not use LVCMOS33 for pins in a bank with VCCO=1.5V.

### 10.2 GTP Transceiver — Lưu ý PCIe

```tcl
# GTP PCIe constraints — thêm vào XDC khi implement PCIe IP
set_property PACKAGE_PIN R4 [get_ports pcie_refclk_p]
set_property IOSTANDARD DIFF_HSTL_II_18 [get_ports pcie_refclk_p]

# PCIe PERST# — reset từ host
set_property PACKAGE_PIN C13 [get_ports pcie_perst_n]  ;# kiểm tra lại
set_property IOSTANDARD LVCMOS33 [get_ports pcie_perst_n]
set_property PULLTYPE PULLUP [get_ports pcie_perst_n]
```

🇻🇳 PCIe Gen2 x4 dùng 4 lane GTP: MGT_TX0-3 / MGT_RX0-3. Reference clock 125MHz từ chân **R4** (differential). Dùng Vivado IP Catalog → PCIe IP Core để tự động map các chân GTP.

🇬🇧 PCIe Gen2 x4 uses 4 GTP lanes: MGT_TX0-3 / MGT_RX0-3. 125MHz reference clock from pin **R4** (differential). Use Vivado IP Catalog → PCIe IP Core for automatic GTP pin mapping.

### 10.3 Thứ tự khởi động nguồn / Power-Up Sequence

```
Thứ tự bật / Power-ON order:
  1. VCCINT (1.0V)      ─── FPGA core
  2. VCCBRAM (1.0V)     ─── Block RAM (đồng thời VCCINT / same as VCCINT)
  3. VCCAUX (1.8V)      ─── FPGA auxiliary
  4. VMGTAVCC (1.0V)    ─── GTP core (đồng thời VCCINT)
  5. VMGTAVTT (1.2V)    ─── GTP termination
  6. VCCO (3.3V/1.5V)   ─── User I/O banks (cuối cùng / last)

Thứ tự tắt / Power-OFF order: ngược lại / reverse
```

> 📌 Tất cả trình tự trên được xử lý tự động bởi mạch nguồn tích hợp trên module AC7A035. Người dùng chỉ cần cấp 5V đơn vào CN1.  
> 📌 All sequencing is handled automatically by the on-module power management. Users only need to supply 5V to CN1.

### 10.4 Clock Planning / Kế hoạch clock

| Clock nguồn / Source | Tần số / Freq | Chân FPGA / FPGA Pin | Dùng cho / Used for |
|---------------------|---------------|---------------------|---------------------|
| SiT9102 #1 (module) | 200 MHz | Differential — nội bộ DDR3 | DDR3 MIG controller, sys clock |
| SiT9102 #2 (module) | 125 MHz | **R4** (differential) | GTP PCIe reference |
| USB_CLK (FT601Q) | ~100 MHz | **Y22** | USB FIFO sync (CLOCK_DEDICATED_ROUTE FALSE) |
| PCIEx16_CLK từ J1 | 100 MHz | MGT_CLK1_P/N | PCIe host ref clock |

```tcl
# USB clock không dùng clock routing dedicated — bắt buộc phải có constraint này
set_property CLOCK_DEDICATED_ROUTE FALSE [get_nets usb_clk_IBUF]
```

### 10.5 Các constraint timing quan trọng / Key Timing Constraints

```tcl
# False paths giữa MIG ui_clk và system clocks (chuẩn Xilinx MIG flow)
set_false_path -from [get_clocks *ui_clk*] -to [get_clocks clk_out1_*]
set_false_path -from [get_clocks clk_out1_*] -to [get_clocks *ui_clk*]

# Async groups giữa các clock domain
set_clock_groups -asynchronous \
  -group [get_clocks clk_out1_*] \
  -group [get_clocks clk_pll_i]
```

---

## Tài liệu liên quan / Related Documents

| Tài liệu / Document | Mô tả / Description | Link |
|--------------------|---------------------|------|
| AC7A035 User Manual | Hướng dẫn module / Module guide | [ALINX](https://www.en.alinx.com/Product/FPGA-System-on-Modules/Artix-7/AC7A035.html) |
| XC7A35T Datasheet | Xilinx Artix-7 35T | [AMD/Xilinx](https://www.xilinx.com/products/silicon-devices/fpga/artix-7.html) |
| AXK680337YG | 80-pin B2B connector | Panasonic |
| HBQ PCIe Base Card Sch | Schematic V2.0 | `Sch_PCIEBaseCard_Rev2_0.pdf` |
| Pin Assignment XDC | File constraint | `artix7_35K_pin_assignment.xdc` |

---

## Revision History

| Rev | Ngày / Date | Nội dung / Changes |
|-----|------------|-------------------|
| 1.0 | 01/2026 | Khởi tạo từ schematic Rev2.0 và XDC file / Initial from schematic + XDC |

---

*🔧 FPGA: Xilinx XC7A35T-2FGG484I (ALINX AC7A035) · Base board: HBQ Artix-7 Motion Control Card*  
*📄 Nguồn / Source: `Sch_PCIEBaseCard_Rev2_0.pdf` + `artix7_35K_pin_assignment.xdc` + schematic CN1–CN4 images*  
*🌐 Module datasheet: https://www.en.alinx.com/Product/FPGA-System-on-Modules/Artix-7/AC7A035.html*
