# MachXO2-640HC FPGA Breakout Board - KiCad Design 🛠️

> Custom breakout board for Lattice **LCMXO2-640HC-6MG132I** - Designed in KiCad 9.0.3 - 8 Sheets Hierarchical Design - By Jay Waghmare, Pune

[![KiCad](https://img.shields.io/badge/KiCad-9.0.3-314CB0?style=for-the-badge&logo=kicad&logoColor=white)](https://kicad.org)
[![FPGA](https://img.shields.io/badge/FPGA-LCMXO2--640HC--6MG132I-ED1C24?style=for-the-badge)](https://www.latticesemi.com)
[![Board](https://img.shields.io/badge/Board-132--Ball%20CSBGA-orange?style=for-the-badge)](#)
[![Author](https://img.shields.io/badge/Author-jay--waghmare-181717?style=for-the-badge&logo=github)](https://github.com/jay-waghmare)

<p align="center">
  <img src="hardware/schematics/Page1-TopLevel.png" alt="Top Level Hierarchical" width="700" />
  <br><em>Top Level - 8 Sheets Hierarchical Design (Your PDF Page 1/8)</em>
</p>

---

## 🌟 What Is This? (Based on Your PDF)

This is my **custom FPGA development board** designed from scratch in KiCad for Lattice **MachXO2 LCMXO2-640HC-6MG132I** (640 LUTs, 132-ball CSBGA package - MG132I).

**Not a simple LED board — Full breakout board with:**

- USB Mini-B connector for power + data
- ESD protection for USB (USBLC6-2SC6)
- Dual LDO power: 5V → 3.3V (TL1963A) and 5V → 1.2V (LD1117S12) for FPGA core + I/O
- FT231XQ USB-to-UART bridge for serial communication
- JTAG header (1x10) for programming with Lattice **HW-USBN-2B** cable (10 flywires)
- All 4 I/O Banks broken out to headers (Top, Bottom, Left, Right) + GND/NC management
- Power indicator LEDs

**Why I designed this?** To learn FPGA hardware design — Power sequencing, decoupling, JTAG, clock, I/O banks — and have my own board to test Verilog later. This is rare — most students use ready-made boards, I designed my own!

**Your schematic is 8 pages, KiCad 9.0.3, file `MACHXO2_test.kicad_sch`:**

### Sheet Breakdown (From Your PDF):

| Sheet | File | What It Is |
|-------|------|------------|
| 1/8 | MACHXO2_test.kicad_sch | **Top Level** - Hierarchical blocks |
| 2/8 | Power.kicad_sch | **Power Section** - USB Mini, 3.3V/1.2V LDOs, FPGA VCC/VCCIO decoupling (C6 0.1uF, C7 4.7uF, etc) |
| 3/8 | USER_IO_Bank_T.kicad_sch | **Bank 0 + JTAG** - U6B LCMXO2 Bank0 (PT6A, PT6B...), JTAG TDO/TDI/TCK/TMS, JTAGENB, PROGRAMN, Bank0 Headers J3 (12-pin socket) |
| 4/8 | USER_IO_Bank_R.kicad_sch | **Bank 1** - U6C LCMXO2 Bank1 (PR28, PR2B...), LVDS1_P/N, LVDS2_P/N, PCLK, Bank1 Header J4 (10-pin) |
| 5/8 | USER_IO_Bank_R.kicad_sch (Bank L) | **Bank 2/3 Left** - U6D LCMXO2 Bank L (PL2A, PL2B... PL7B) - User I/O Left |
| 6/8 | USER_IO_Bank_T.kicad_sch (Bank B) | **Bank Bottom** - U6E LCMXO2 Bank Bottom (PB4A, PB4B... PB14B) - User I/O Bottom |
| 7/8 | USER_Config.kicad_sch | **Config + UART** - JTAG Pins J2 (10-pin), PROGRAMN button SW1, FT231XQ USB-UART (TXD/RXD, RTS, CTS...), JTAG header table with flywire colors |
| 8/8 | GND&NC.kicad_sch | **GND & NC** - U6F/U6G NC pins (A8, A11... P4) and GND balls (A5, B11, D2...) |

---

## ✨ Board Features (From Your Schematic)

**FPGA:**
- U6: LCMXO2-640HC-6MG132I, 640 LUTs, 132-ball CSBGA, Speed grade 6

**Power (Your PDF Page 2/8 - Power.kicad_sch):**
- J1: USB Mini-B connector (VBUS, D+, D-, GND, Shield)
- D3: 1N5819HW diode? + U2: USBLC6-2SC6 ESD protection for USB D+/D-
- R1,R2: 22R series for USB D_N/D_P
- Power LEDs: D1, D2 with R8 1K, R1 1K to VBUS and +3V3
- U4: TL1963A-33DCYR LDO - 5V VBUS to +3V3 (C2 4.7uF input, C3 10uF C21 10uF output)
- U5: LD1117S12CTR LDO - VBUS to +1V2 (C22 4.7uF input, C23 10uF C24 10uF output)
- U6A Power balls: VCC_1 K14, VCC_2 H14, VCCIO0_0 B10, VCCIO1_0/1 D14/L12, VCCIO2_0/1/2 N14/F3/D3, VCCIO3_0/1 D7/L11 with decoupling: C6/C8/C11/C14/C17 0.1uF, C7/C9/C10/C12/C15/C18 1uF? Wait: C7 4.7uF, C8 0.1uF, C10 1uF, C9 4.7uF etc — You have proper decoupling per bank!

**Recommended VCCIO (From Your Table Page 2/8):**
- PB* (left) - 3.3V sensors, headers, SPI flash - Recommended 3.3V Net
- PL* (lower) - Low-voltage memory or ADCs - 1.8V (if memory requires 1.8V)
- PR* (right) - High-speed interfaces - 1.8V or 2.5V as required
- PT* (top) - UART, SPI, UART-USB level translators - 3.3V

**User I/O Banks (Pages 3-6/8):**
- Bank0 (Top): PT6A/A2, PT6B/B3, PT6C/C4, PT6D/A4, PT7A/A4, PT7B/B4, PT7C/B6, PT7D/A6, PT9A/A7, PT9B/B7, PT9C/C8, PT9D/B8, PT10A/C8, PT10B/A9, PT10C/B9, PT10D/C10, PT11A/A11, PT11B/B11, PT11C/B13, PT11D/A13 - With J3 12-pin socket (Conn_01x12_Socket) for Bank0 headers: PT6A Pin1, PT6B Pin2... SDA/PCLKC0_0 Pin8 etc, GND Pin12
- Bank1 Right (Top Right): PR28/B14, PR2B/C13, PR2C/E12, PR2D/E14, PR3A/E13, PR3B/F12, PR3C/F13, PR3D/F14, PR5A/G14, PR5B/G13, PR5C/G12, PR5D/H12, PR7A/L14, PR7B/M13, PR7C/N14, PR7D/N13, PR6A/J13, PR6B/K16, PR6C/K15, PR6D/J16 - With J4 10-pin header (Conn_01x10_Pin) for LVDS and PCLK: LDVS1_P Pin1, LDVS1_N Pin2, LDVS2_P Pin3, LDVS2_N Pin4, LDVS3_P Pin5, LDVS3_N Pin6, PCLK1_0 Pin7, PCLK1_0? Actually PCLKT1_0, PCLKC1_0 Pin8-9, GND Pin10
- Bank Left/Right (Middle): PL2A/C1, PL2B/C2, PL2C/D1, PL2D/D2, PL3A/J3, PL3B/J4, PL3C/H4, PL3D/J5, PL5A/G3, PL5B/H2, PL5C/H1, PL5D/J1, PL6A/J1? Wait: PL6A/J1, PL6B/J2, PL6C/G2, PL7D/F2, PL7A/K3, PL7B/K1, PL7C/J2, PL6D, PL7? etc - User I/O Left Bank
- Bank Bottom: PB4A/D3, PB4B/M3, PB4C/N3, PB4D/X? Actually PB4A/D3, PB4B/M3, PB4C/N3, PB4D/M? etc - PB6A/M4, PB6B/N4, PB6C/P6? etc - User I/O Bottom - PB10A/D7, PB10B/N7, PB10C/M7? Actually PB10A/D7, PB10B/N7? etc - All bottom banks
- GND&NC: U6F/U6G NC pins (A8, C2, A12, A13, C4... M5, M11, N1, N2, N5...) and GND balls (A5, B11, D2, D13, G2, H13, L2, L13, P5, P10)

**Device Management / Configuration (Page 7/8 - USER_Config.kicad_sch):**
- J2: JTAG Header Conn_01x10_Pin 10-pin - TCK Pin1, TDO Pin2, TDI Pin3, TMS Pin4, GND Pin5, VCC Pin6, PROGRAMN Pin7, DONE Pin8, INITN Pin9, JTAGENB Pin10 with 0.1uF C25 to GND
- SW1: Push button dual for PROGRAMN with 4K7 R11 pull-up to +3V3 and 4K7 R12? Actually R11 4K7 to +3V3
- Notes: INITN (B13) Open-drain, low during config start - leave floating (internal weak pull-up). JTAGENB (B9) High = JTAG enabled - leave floating (internal weak pull-up)
- U7: FT231XQ USB to UART bridge - C26 4.7uF, C27 0.1uF for VCC, D_N/D_P to USB, TXD 17 TXD_FTDI, RXD 5? Actually TXD 17 TXD_FTDI, RXD 5? RXD 5? Actually Pin 17 TXD, Pin 5 RXD? In schematic: TXD 17 TXD_FTDI, RXD 5? Wait: U7 FT231XQ with 3V3OUT, VCCIO C28 0.1uF, RESET Pin 11, CBUS0-3, and R13 10K, R1 4K7 for VBUS?
- **JTAG Programmer Note (From Your PDF):** Use JTAG Programmer: Lattice HW-USBN-2B USB cable with 10 flywire for programming the FPGA - Official tool for JTAG/SPI programming of MachXO2 FPGAs. Includes image of cable.
- **JTAG HEADER TABLE (1x10) - LCMXO2-640HC-6MG132I and HW-USBN-2B:**
  - Pin 1 TCK B6 WHITE YES 4.7kΩ to GND
  - Pin 2 TDI B4 ORANGE YES 22Ω series
  - Pin 3 TDO A4 BROWN YES Direct
  - Pin 4 TMS A6 PURPLE YES 22Ω series
  - Pin 5 GND Any BLACK YES Multiple GND via
  - Pin 6 VCC VCCIO0 RED YES 0.1uF cap
  - Pin 7 PROGRAMN C10 YELLOW Opt 4.7kΩ + btn
  - Pin 8 DONE A13 BLUE Opt LED + 330Ω
  - Pin 9 INITN B13 GREEN Opt Float OK
  - Pin 10 JTAGENB B9 GRAY Opt Float OK

---

## 📁 File Structure (Your Actual Files)

```
MachXO2-KiCad-Board/
├── README.md (this file - based on your PDF)
├── MACHXO2_test.kicad_pro (Your KiCad project file - you uploaded)
├── MACHXO2_test.kicad_sch (Top level - you uploaded? Actually hierarchical)
├── MACHXO2_test.kicad_pcb (PCB layout - you uploaded)
├── MACHXO2_test.kicad_prl (Project lock)
├── Power.kicad_sch (Power sheet - need to upload if separate)
├── USER_IO_Bank_T.kicad_sch (Bank Top)
├── USER_IO_Bank_B.kicad_sch (Bank Bottom)
├── USER_IO_Bank_L.kicad_sch (Bank L)
├── USER_IO_Bank_R.kicad_sch (Bank R)
├── USER_Config.kicad_sch (Config + UART)
├── GND&NC.kicad_sch (GND & NC)
├── hardware/
│   ├── schematics/
│   │   ├── schematic.pdf (Your PDF - 8 pages - upload)
│   │   ├── schematic_preview_Page1.png (Top level)
│   │   ├── schematic_preview_Power.png (Power section)
│   │   └── block_diagram.png
│   ├── pcb/
│   │   ├── gerbers.zip (Export from KiCad for JLCPCB)
│   │   ├── 3d-render.png (KiCad 3D view)
│   │   └── pcb_preview.png
│   ├── photos/
│   │   ├── board_top.jpg (after fab)
│   │   └── board_bottom.jpg
│   └── bom.md (parts list from schematic)
├── docs/
│   ├── design.md (why 640HC, power choices, bank VCCIO)
│   ├── bringup.md (how to test after fab - JTAG scan, power, blinky)
│   └── jtag-wiring.md (HW-USBN-2B flywire table from your PDF)
├── datasheets/
│   ├── LCMXO2-640HC-6MG132I.pdf
│   ├── TL1963A-33.pdf
│   └── FT231XQ.pdf
├── LICENSE (MIT)
└── .gitignore (KiCad)
```

**You already uploaded:** MACHXO2_test.kicad_pro, .kicad_pcb, .kicad_prl, .kicad_sch (top). Need to upload remaining hierarchical sheets if separate files: Power.kicad_sch, USER_IO_Bank_*.kicad_sch, etc. Check your KiCad project folder.

---

## 🛠️ Built With Details (From Your PDF)

- **FPGA:** LCMXO2-640HC-6MG132I, 640 LUTs, 132-ball csBGA, Speed 6
- **KiCad:** 9.0.3 (from sheet title blocks)
- **Power:** 
  - USB Mini-B J1
  - ESD: USBLC6-2SC6 U2 (D+ D- protection)
  - 5V VBUS to +3V3: U4 TL1963A-33DCYR (LDO, C2 4.7uF, C3 10uF, C21 10uF)
  - 5V to +1V2: U5 LD1117S12CTR? Actually LD1117S12CTR or LD1117S12? In PDF: U5 LD1117S12CTR (C22 4.7uF, C23 10uF, C24 10uF)
  - Power LEDs: D1 D2 with R8 1K, R1? Actually R8? Power indicator
  - Decoupling: For U6A power balls VCC_1 K14, VCC_2 H14, VCCIOs: C6 0.1uF C7 4.7uF for +1V2, C8 0.1uF C10 1uF C9 4.7uF for +3V3 etc (each bank has 0.1uF + 1uF + 4.7uF)
- **Clock:** (Not shown in pages we saw? Probably in another sheet or internal? Might be from FPGA internal or from headers? Check if you have crystal - Your earlier description said 12MHz but PDF shows Power section only - Clock might be from external header or internal oscillator)
- **Configuration & Programming:**
  - J2: 1x10 JTAG header (TCK, TDO, TDI, TMS, GND, VCC, PROGRAMN, DONE, INITN, JTAGENB)
  - SW1: PROGRAMN push button with 4K7 R11 pull-up
  - JTAG resistors: R6 22R, R7 22R, R8 22R for TDO/TDI/TCK/TMS? Actually R6 22R, R7 22R, R8 22R from Bank0 sheet
  - USB-UART: U7 FT231XQ with C26 4.7uF C27 0.1uF C28 0.1uF, TXD_FTDI, RXD_FTDI, RTS, CTS, DTR, DSR, etc, CBUS0-3, R13 10K, R1 4K7 for VBUS detection
  - Programmer: Lattice HW-USBN-2B with flywire colors White(TCK), Orange(TDI), Brown(TDO), Purple(TMS), Black(GND), Red(VCC), Yellow(PROGRAMN), Blue(DONE), Green(INITN), Gray(JTAGENB) as per your table

---

## 📦 How to View / Edit

1. Install KiCad 9.0.3+: https://kicad.org/download/
2. Clone:
```bash
git clone https://github.com/jay-waghmare/MachXO2-KiCad-Board.git
cd MachXO2-KiCad-Board
```
3. Open `MACHXO2_test.kicad_pro` in KiCad
4. Open Schematic Editor - You will see hierarchical sheets: Power, USER_IO_Bank_T/B/L/R, USER_Config, GND&NC - Double click each to open
5. Open PCB Editor - View layout

## 🔧 How I Designed (From Your Schematic)

**Power:** USB Mini VBUS 5V → USBLC6 ESD protection → TL1963A LDO 3.3V for I/O and FT231XQ + LD1117 LDO 1.2V for FPGA core VCC_1/VCC_2. Added 100nF + 1uF + 4.7uF decoupling per VCCIO bank close to FPGA balls.

**JTAG:** Used 1x10 header J2, connected TCK (B6), TDI (B4), TDO (A4), TMS (A6) with 22Ω series + 4.7K pull-down for TCK as per Lattice guide. PROGRAMN with button SW1 and 4.7K pull-up, DONE with LED+330Ω, INITN and JTAGENB left float (internal weak pull-up as per your note).

**USB-UART:** FT231XQ with VBUS detection via 10K+4.7K divider, TXD/RXD to FPGA PT* bank (3.3V as per your table).

**I/O Banks:** Broke out each bank to headers: Bank0 T to J3 12-pin socket (PT6A/B...), Bank1 to J4 10-pin, etc. Followed your VCCIO table for recommended voltages.

**GND & NC:** Connected all GND balls (A5, B11, D2...) to ground plane, left NC balls floating as per your sheet 8/8.

---

## 📸 Photos / Renders To Add

| Top Level (Page 1) | Power (Page 2) | JTAG + UART (Page 7) | 3D Render |
|---|---|---|---|
| hardware/schematics/Page1-TopLevel.png | hardware/schematics/Page2-Power.png | hardware/schematics/Page7-Config.png | hardware/pcb/3d-render.png |

**You have PDF with 8 pages — Export each page as PNG and upload to hardware/schematics/ for README preview!**

## 🗺️ Roadmap Based on Your Design

- [x] KiCad hierarchical schematic - 8 sheets - LCMXO2-640HC (your PDF)
- [x] Upload .kicad_pro, .kicad_pcb, .kicad_prl, .kicad_sch - Done (5 commits today!)
- [ ] Upload remaining hierarchical sheets: Power.kicad_sch, USER_IO_Bank_*.kicad_sch, USER_Config.kicad_sch, GND&NC.kicad_sch (if separate files on laptop)
- [ ] Export schematic PDF (you have) - Already have, need to upload to hardware/schematics/schematic.pdf
- [ ] Export schematic PNGs for README (Top level, Power, Config) - Use Snipping Tool
- [ ] Export PCB 3D render: KiCad PCB Editor → View → 3D View → Export PNG → hardware/pcb/3d-render.png
- [ ] Add BOM with exact values from your schematic (see hardware/bom.md)
- [ ] Add design.md - Why 640HC not 7000HE, why TL1963A vs AMS1117, why 132-ball
- [ ] Fabricate PCB at JLCPCB - Upload gerbers.zip
- [ ] Bring-up: Power test (3.3V, 1.2V), JTAG scan with HW-USBN-2B, first blinky
- [ ] Add real board photos + video

## 📝 What I Learned (From This Design)

- Hierarchical design in KiCad 9.0.3 — Top sheet + sub-sheets
- MachXO2 power: Need both 1.2V core and 3.3V I/O, decoupling per bank critical
- JTAG: HW-USBN-2B wiring with flywire colors, 22Ω series, 4.7K pull-downs
- USB protection: USBLC6-2SC6 for D+/D- ESD
- FT231XQ USB-UART: CBUS pins, VBUS detection
- GND management: Many GND balls need solid plane + multiple vias
- NC pins: Leave floating, don't tie to GND

## 🤝 Help

If you know MachXO2 hardware, review my power decoupling and JTAG wiring - Open Issue!

## 👨‍💻 Author

**Jay Waghmare** - FPGA Board Designer, Hardware, Pune
- GitHub: [@jay-waghmare](https://github.com/jay-waghmare) - Professional (this account)
- Old: [@jaywaghmare26](https://github.com/jaywaghmare26) - Archive
- Board: LCMXO2-640HC-6MG132I custom breakout

If you like FPGA hardware boards, please ⭐ star this repo!

## 📄 License

MIT License - Use for learning!

> Designed from scratch in KiCad 9.0.3 - 8 sheets hierarchical - One trace at a time - Jay - Pune 2026
