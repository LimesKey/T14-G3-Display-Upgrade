# ThinkPad T14 Gen 3 (LCFC NM-E981) ↔ BOE NE140QDM-NX5 — eDP pinouts

Reference data for fitting a 40-pin eDP panel to a ThinkPad T14 Gen 3 / P14s Gen 3
motherboard.

## Connector identification

| | Motherboard | Panel |
|---|---|---|
| Reference | JLCD1 | — |
| Part number | I-PEX 20654-040E-01 | I-PEX 20682-040E-02 |
| Family | unlisted (see notes) | CABLINE-CA II |
| Positions | 40 signal + 12 ground tabs | 40 |
| Pitch | 0.400 mm (measured) | 0.40 mm |
| Mating type | horizontal, micro-coax receptacle | horizontal, micro-coax receptacle |
| Mating plug | unknown | `20679-0**T-01` |

Sources: NM-E981 schematic sheet 94 and the board netlist `part_number` column
for JLCD1; BOE NE140QDM-NX5 Product Specification Rev. P0 (2024-02-19), page 17
§5.1, which states the electronics interface connector is I-PEX 20682-040E-2 or
compatible.

## Pinout — both sides

Both connectors are 40-position, but **the pinouts are not the same**. A row below
shows what that pin number means on each end; it is *not* a connection.

| Pin | Motherboard JLCD1 | Panel NE140QDM-NX5 |
|---|---|---|
| 1 | GND | NC |
| 2 | EDP_TXN3 | H_GND |
| 3 | EDP_TXP3 | LANE3_N |
| 4 | GND | LANE3_P |
| 5 | EDP_TXN2 | H_GND |
| 6 | EDP_TXP2 | LANE2_N |
| 7 | GND | LANE2_P |
| 8 | EDP_TXN1 | H_GND |
| 9 | EDP_TXP1 | LANE1_N |
| 10 | GND | LANE1_P |
| 11 | EDP_TXN0 | H_GND |
| 12 | EDP_TXP0 | LANE0_N |
| 13 | GND | LANE0_P |
| 14 | EDP_AUXP | H_GND |
| 15 | EDP_AUXN | AUX_CH_P |
| 16 | GND | AUX_CH_N |
| 17 | NC | H_GND |
| 18 | VCC3LCD_R (3.3 V) | LCD_VCC (3.3 V) |
| 19 | VCC3LCD_R (3.3 V) | LCD_VCC (3.3 V) |
| 20 | VCC3LCD_R (3.3 V) | LCD_VCC (3.3 V) |
| 21 | VCC3LCD_R (3.3 V) | LCD_VCC (3.3 V) |
| 22 | LCD_SELF_TEST_ON | Bist |
| 23 | SIZE CTL | H_GND |
| 24 | GND | H_GND |
| 25 | EDP_HPD | H_GND |
| 26 | GND | H_GND |
| 27 | BACKLIGHT_ON | HPD (output) |
| 28 | PANEL_BKLT_CTRL | BL_GND |
| 29 | EPRIVACY_ON | BL_GND |
| 30 | NC | BL_GND |
| 31 | VBL15_R (backlight power) | BL_GND |
| 32 | VBL15_R (backlight power) | BL_ENABLE (3.3 V input) |
| 33 | VBL15_R (backlight power) | BL_PWM |
| 34 | -TCH_PNL_RST | NC |
| 35 | LPSS_I2C1_SCL_PNL | NC |
| 36 | LPSS_I2C1_SDA_PNL | BL_POWER (5–21 V) |
| 37 | -TCH_PNL_INT | BL_POWER (5–21 V) |
| 38 | GND | BL_POWER (5–21 V) |
| 39 | -LID_CLOSE_D | BL_POWER (5–21 V) |
| 40 | VCC3_TOUCHPANEL_F (3.3 V) | NC |

JLCD1 additionally has pins 41–52, all shell / multi-point ground tabs.

Notes:

- On both connectors the **N side of every differential pair precedes the P side**.
- Panel lane numbering runs **backwards** relative to pin order: lane 3 is nearest pin 1.
- The panel pinout is the industry-standard 40-pin, 4-lane eDP panel pinout.

## Required mapping

A working cable must implement this. It is **not** 1:1.

| Board JLCD1 | Signal | Panel pin |
|---|---|---|
| 2 | EDP_TXN3 | 3 |
| 3 | EDP_TXP3 | 4 |
| 5 | EDP_TXN2 | 6 |
| 6 | EDP_TXP2 | 7 |
| 8 | EDP_TXN1 | 9 |
| 9 | EDP_TXP1 | 10 |
| 11 | EDP_TXN0 | 12 |
| 12 | EDP_TXP0 | 13 |
| 14 | EDP_AUXP | 15 |
| 15 | EDP_AUXN | 16 |
| 4, 7, 10, 13, 16 | GND | 5, 8, 11, 14, 17 |
| 18–21 | VCC3LCD_R (3.3 V) | 18–21 |
| 22 | LCD_SELF_TEST_ON | 22 (Bist) |
| 24, 26 | GND | 23–26 |
| 25 | EDP_HPD | 27 |
| 27 | BACKLIGHT_ON | 32 (BL_ENABLE) |
| 28 | PANEL_BKLT_CTRL | 33 (BL_PWM) |
| 31, 32, 33 | VBL15_R | 36–39 (BL_POWER) |
| any GND | GND | 28–31 (BL_GND) |
| 1, 17, 23, 29, 30, 34–37, 39, 40 | leave open | — |
| — | — | 1, 34, 35, 40 leave open |

Two consequences:

1. The eDP block is offset by exactly one position (board *n* → panel *n+1*) across
   board pins 2–16.
2. From board pin 25 onward the two pinouts diverge completely. **A 1:1 adapter wired
   directly between JLCD1 and the panel shorts the backlight rail to ground**: board
   pin 31 is VBL15_R, panel pin 31 is BL_GND. That is a dead short through F9402 (3 A).
   It also drives that rail into panel pins 32 and 33, which are 3.3 V logic inputs to
   the panel's LED driver.

This warning applies only to an adapter bridging JLCD1 *directly* to the panel. A
genuine Lenovo 40-pin cable already performs this remap internally, so a pitch-only
adapter at the panel end of a correct cable is a different situation.

## Connector specifications — panel side

I-PEX CABLINE-CA II, per product specification PRS-2163-14EN.

| Parameter | Value |
|---|---|
| Type | Wire-to-board micro-coax, horizontal mating, fully shielded, mechanical lock |
| Contact pitch | 0.4 mm |
| Receptacle | `20682-0**E-#2#` (newer family `21095-0**E-02`) |
| Mating plug | `20679-0**T-01` (newer `21066-0**T-01`) |
| Plug sub-parts | Housing `20680-0**1`, lock-bar assembly `20681-0**T-01` |
| Accepted cable | Micro-coax AWG 44–36, discrete wire AWG 36/34, twinax AWG 42/40 |
| Data rate class | 20 Gbps/lane (HBR2 at 5.4 Gbps/lane is well within spec) |

An FPC alternative exists: **CABLINE-CA IIF**, shell assembly `20856-040T-01`, which
I-PEX states mates to CABLINE-CA II receptacles `20682-0**E-02`. It requires a
*shielded* FPC with a contact-area thickness of 0.25 mm (+0.02/−0.03), roughly double
a standard 2-layer flex.

## Connector specifications — motherboard side

I-PEX 20654-040E-01. The part number is confirmed twice within the board package
(schematic text line and netlist `part_number` column), but it does not appear in
I-PEX's public English catalog, on LCSC, or on DigiKey, so no official datasheet or
mating-plug number is available.

Verified by measurement from the netlist pad coordinates (mils):

| Parameter | Value |
|---|---|
| Pitch | pin 1 x = 2677, pin 40 x = 3291 → 614 mil / 39 = 15.744 mil = **0.3999 mm** |
| Stagger | none; all 40 signal pads share y = 5352 |
| Ground row | pads 46–51 at y = 5389, i.e. 0.94 mm behind the signal row, ~2.4 mm spacing (multi-point ground) |
| End anchors | x = 2573 and x = 3395 → 822 mil = 20.88 mm span, body width ≈ 22 mm |
