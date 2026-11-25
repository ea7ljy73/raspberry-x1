# TX modification for the Raspberry X1 (unofficial)

Guide to enable **transmission** on the HamGeek Raspberry X1, a 160–10 m HF SDR that ships
**receive-only**.

> 🌐 [Español](Tx-mod-Es.md) · **English** — [HTML version](Tx-mod-En.html) — [shopping list (ES)](Lista-compra-TX.md)

**Author:** EA7LJY — [@ea7ljy73](https://github.com/ea7ljy73)

- **Status:** draft for review. Values cross-checked against the Rev. 2.0 schematic, the V2
  production BOM and a real unit (interior photo, no TX). Assembly and measurements pending.
- **Basis:** manufacturer's technical documentation Rev. 2.0 (schematics) and V2 production BOM,
  cross-checked with the open **MBPTRX** project (GPL-3.0):
  <https://github.com/ianm8/MBPTRX>.
- **Nature:** independent, unofficial document.
- **Licence:** [GPL-3.0](../LICENSE).

> **Notice.** This modification involves RF and up to ~5 W. **The author accepts no liability for
> damage or errors arising from the use of this information.** Test into a dummy load with a
> power/SWR meter before connecting an antenna, use a current-limited supply and follow ESD
> precautions. Transmitting requires an amateur radio licence.

## Contents

1. [Summary](#1-summary)
2. [State of a production unit](#2-state-of-a-production-unit)
3. [Before you start](#3-before-you-start)
4. [Parts to add](#4-parts-to-add)
5. [Low-pass filters (LPF)](#5-low-pass-filters-lpf)
6. [Power stage and transformers](#6-power-stage-and-transformers)
7. [SWR bridge](#7-swr-bridge)
8. [TX stage supply](#8-tx-stage-supply)
9. [Bring-up and verification](#9-bring-up-and-verification)
10. [Points to verify](#10-points-to-verify)
11. [Sources](#11-sources)

---

## 1. Summary

The main board is built for receive. To transmit you must **fit the final transistor, the
transformers, the low-pass filters, the SWR bridge and the six relays**, and remove the factory
receive jumper. The relays are driven by the firmware through the **PCA9555** I²C expander.

TX chain: RF from the FST3253 (`TXSIG`) → **Q5** µPC1678G (pre-amp) → **Q6** RD01MUS1 (driver) →
**T4** → **Q4** RD16HHF1 (final) → **T3** → **RELAY-6** (TX/RX) → band LPF (**RELAY-1…5**) →
**T1/T2** bridge → antenna.

On receive, **RELAY-6** routes the antenna (already filtered by the band LPF) to the mixer input
(`RXFST` → D1 → FST3253).

## 2. State of a production unit

Visual inspection of a production unit (board *MAIN UNIT v2.0*, RX-only version):

| Block | Fitted | Missing |
| --- | --- | --- |
| Final | Q4 thermal pad, C75, R50, R51, C82, FB5, C80, C86, R55 | **Q4** RD16HHF1, **T3**, **C76** (100 µF) |
| Driver | Q6 RD01MUS1, R56–R59, C83, C87, C88, FB6, R49 | **T4** |
| Pre-amp | Q5 µPC1678G, R52–R54, C79, C84, C85, FB4 | — |
| Switching | U11 78M09, Q2 SI2301, Q3 2N7002, R47, R48, C72–C74, C77, C78 | **RELAY-6** |
| LPF | capacitors C47–C70 | **RELAY-1…5**, **L1…L15** |
| SWR bridge | D3, D4, R37–R46, C63–C66, ADS1115 (U10) | **T1**, **T2** |

> ⚠️ **Receive jumper.** From the factory a **red wire** connects the antenna connector (BNC) straight
> to the receiver input (D1), bypassing relays and filters. **Remove it when fitting RELAY-6 and the
> LPF relays.** Transmitting with it in place feeds the power into the receive mixer and destroys
> the FST3253.
>
> Consequence for receive: while that jumper is there the receiver has **no RF filtering at all**
> in front of the mixer.

## 3. Before you start

**Recommended tools and equipment**

- Soldering station (hot air and fine tip), flux and loupe/microscope.
- Multimeter, continuity tester and, if possible, oscilloscope and spectrum analyser.
- **Dummy load**, **power/SWR meter** and a power supply with adjustable current limit.
- ESD wrist strap and mat.

**Preliminary checks**

- Identify your **board revision**. This guide is based on Rev. 2.0 (*MAIN UNIT v2.0*).
- Check which parts your unit actually has against the table in [§2](#2-state-of-a-production-unit).
- Make sure the supply delivers 13.8 V and at least 2 A before connecting the power stage.

## 4. Parts to add

The shopping list, with quantities and spares, is in [`Lista-compra-TX.md`](Lista-compra-TX.md)
(Spanish).

### 4.1. Semiconductors

| Ref. | Part | Qty | Function |
| --- | --- | --- | --- |
| Q4 | **RD16HHF1** (TO-220) | 1 | Power final (~5 W). The tab is the source (ground): no insulator needed. |

Q5 (µPC1678G), Q6 (RD01MUS1), Q2 (SI2301), Q3 (2N7002) and diodes D3/D4 (1N5711WS) **are already
fitted**. The production BOM lists `PT9926-SOP8` for Q5: that is a BOM error (a dual MOSFET); the
fitted part is the **µPC1678G** in the schematic.

### 4.2. Relays

| Ref. | Part | Qty | Function |
| --- | --- | --- | --- |
| RELAY-1…5 | **Hongfa HFD4/5-SR** (SMD, DPDT, 5 V coil) | 5 | LPF selection |
| RELAY-6 | **Hongfa HFD4/5-SR** | 1 | TX/RX switching |

The schematic calls them `G6K-2Y-Y`, but the **board footprint is `RELAY-SMD_HFD4/5-SR`** and the
production BOM gives **HFD4/5-S** (LCSC C64399). An Omron **G6K-2F-Y DC5** is electrically
equivalent (5 V coil, ~21 mA); check that it fits the footprint before buying it.

The coils are driven directly from the PCA9555 outputs (~21 mA each). Check that every relay
switches firmly when changing band.

### 4.3. Cores and windings

| Ref. | Core | Qty | Use |
| --- | --- | --- | --- |
| L1–L3, L7–L9 | **T37-2** (red) | 6 | 160 and 80 m LPF inductors |
| L4–L6, L10–L15 | **T37-6** (yellow) | 9 | 40 to 10 m LPF inductors |
| T3, T4 | **FT37-43** | 2 | Bifilar transformers (footprint `FT37-43 BIF`) |
| T1, T2 | **FT23-43** | 2 | SWR bridge |

### 4.4. Passives

| Ref. | Part | Qty | Notes |
| --- | --- | --- | --- |
| C76 | Electrolytic 100 µF / 25 V, radial Ø5 mm, 2 mm pitch | 1 | +13.8 V decoupling of the final |

The filter capacitors (C47–C70) are already fitted. If you ever replace one, use **C0G/NP0**.

## 5. Low-pass filters (LPF)

Each filter is a 7-element low-pass (4 shunt capacitors and 3 inductors). The band of each one is
derived from its cut-off frequency, calculated from the schematic values
(T37-2: A<sub>L</sub> ≈ 4.0 nH/turn²; T37-6: A<sub>L</sub> ≈ 3.0 nH/turn²):

| Relay | PCA9555 line | Band | Core and turns | Inductance | Capacitors | Approx. cut-off |
| --- | --- | --- | --- | --- | --- | --- |
| RELAY-1 | BAND1 | **160 m** | T37-2: L1 32, L2 37, L3 32 | 4.1 / 5.5 / 4.1 µH | C47 820p, C48 2.2n, C49 2.2n, C50 820p | 2.9 MHz |
| RELAY-3 | BAND2 | **80 m** | T37-2: L7 22, L8 27, L9 22 | 1.9 / 2.9 / 1.9 µH | C55 470p, C56 1.2n, C57 1.2n, C58 470p | 5.4 MHz |
| RELAY-5 | BAND3 | **60–40 m** | T37-6: L13 21, L14 24, L15 21 | 1.3 / 1.7 / 1.3 µH | C67 270p, C68 680p, C69 680p, C70 270p | 9.3 MHz |
| RELAY-2 | BAND4 | **30–20 m** | T37-6: L4 16, L5 17, L6 16 | 0.77 / 0.87 / 0.77 µH | C51 180p, C52 390p, C53 390p, C54 180p | 17 MHz |
| RELAY-4 | BAND5 | **17–10 m** | T37-6: L10 10, L11 11, L12 10 | 0.30 / 0.36 / 0.30 µH | C59 120p, C60 220p, C61 220p, C62 120p | 36 MHz |

> Relay numbering **does not follow band order**. The X1 Custom firmware already uses this mapping
> (LPF1 = 160 m, LPF2 = 80 m, LPF3 = 40 m, LPF4 = 30–20 m, LPF5 = 17–10 m).
>
> *Correction to earlier versions of this guide, which shifted the bands by one position
> (80/40/30/20/10 m).*

**Winding:** **0.4 mm** enamelled wire (AWG 26), turns spread around the whole toroid. One "turn" is
each pass of the wire through the hole. If you have an analyser (NanoVNA), **measure each filter**:
insertion loss < 0.5 dB in band and 2nd-harmonic rejection > 40 dB. Adjust by spreading or
compressing turns.

## 6. Power stage and transformers

- **Q5 → Q6 → Q4** form the amplifier chain. Q4 is the final (RD16HHF1, ~5 W) and Q6 its driver
  (RD01MUS1).
- **Q4** is screwed (M3) onto its thermal pad against the chassis/heatsink, with thermal paste. It
  dissipates several watts: never transmit without it properly fixed.
- **T3 and T4** are **bifilar** transformers on FT37-43 (footprint `FT37-43 BIF`, 4 pins; the board
  sets the connections). The schematic gives no turns. Starting values (estimated, not verified):
  **8–10 bifilar turns** of 0.3–0.4 mm wire on each. With the 1:4 connection the schematic suggests,
  Q4's drain sees ~12.5 Ω, consistent with about 5 W at 13.8 V.

Keep RF leads short with a nearby ground return.

### 6.1. Bias

- Q6 bias is set by **R49** and Q4 bias by **R50**, both fed from the switched +9 V (present only
  on TX).
- Before the first TX power-up, turn R50 (and R49) to **minimum gate voltage**.
- Into a **dummy load**, with the supply limited to **1.5 A** and **no drive**, key TX and raise R50
  slowly to a Q4 idle current of about **100–200 mA** above the RX current. Do the same with R49
  for Q6 (about 20–50 mA).
- Gate voltage should stay below about 4 V (RD16HHF1, typical V<sub>th</sub> 2–3 V). If the current
  runs away or Q4 heats at idle, cut power and check the assembly.

Reference: the MBPTRX procedure for its 2× RD16HHF1 stage
(<https://github.com/ianm8/MBPTRX/blob/main/PCB/README.md>), to be scaled to a single final. Not
validated on the X1.

## 7. SWR bridge

T1 and T2 form a **tandem coupler**. The parallel load resistors (R41∥R42 and R43∥R44,
2 × 100 Ω = 50 Ω) point to the classic design:

| Transformer | Primary | Secondary |
| --- | --- | --- |
| T1 | 1 pass (the antenna conductor goes through the toroid) | 10 turns |
| T2 | 1 pass | 10 turns |

Coupling ≈ −20 dB. The detectors are D3/D4 (1N5711WS) and the reading is digitised by the
**ADS1115** (FWD on AIN0, REF on AIN1). Calibrate with a 50 Ω load and a known mismatched load.

*Earlier versions of this guide proposed 1.5/10 and 4/30 turns; they are replaced by the symmetric
tandem, consistent with the board's load resistors.*

## 8. TX stage supply

According to the schematic (sheet 3):

1. **U11** (78M09) makes **+9 V** from +13.8 V. It also feeds the LM386 audio amplifier, so it is
   always fitted.
2. `TXENABLE` (PCA9555) turns on **Q3** (2N7002), which opens **Q2** (SI2301).
3. Q2 delivers the **switched +9 V** to Q6's drain (through T4) and to the bias trimmers R49/R50.
   On RX there is no voltage: the transistors are cut off.
4. **Q4**'s drain gets +13.8 V through T3 permanently (without bias it does not conduct).
5. **Q5** (µPC1678G, already fitted) is fed from the **`+5V_B`** net (Vcc and output, through FB4). That net does not
   appear on any other schematic sheet.

> ⚠️ **Before the first TX, find out with a meter where `+5V_B` comes from.** The µPC1678G
> takes at most **6 V**: it must not get the 9 V. Also check whether `+5V_B` is present on RX or
> only on TX.

## 9. Bring-up and verification

1. **Visual inspection:** joints, bridges, orientation of transistors and relays. **Red antenna wire
   removed.**
2. **Continuity:** no shorts between +13.8 V, +9 V, +5 V and ground.
3. **Power without RF:** power up with a low current limit and check +5 V, +9 V, switched +9 V (TX
   only) and `+5V_B` (≤ 6 V).
4. **Relays:** change band from the firmware and confirm the right filter relay closes (table in
   [§5](#5-low-pass-filters-lpf)) and that RELAY-6 switches on TX.
5. **RX:** check that receive still works through RELAY-6 and the band filter.
6. **Bias** as in [§6.1](#61-bias).
7. **TX into a dummy load:** low power first and for a few seconds. Watch current, Q4 temperature
   and SWR.
8. **Tuning:** raise power gradually, check harmonics with an analyser if you have one and validate
   the SWR reading with a mismatched load.
9. **Antenna:** only after all of the above.

**Never transmit without a load or antenna connected.** The X1 Custom firmware keeps TX locked until
«Etapa TX instalada» (TX stage fitted) is enabled in the menu, and applies a TX timeout and a high-SWR
cut. The firmware TX **has not been tested on air yet**.

## 10. Points to verify

- **`+5V_B`:** source and voltage before powering Q5 (max. 6 V). See [§8](#8-tx-stage-supply).
- **R49/R50:** already fitted. The schematic says 4.7 kΩ, but the BOM part number (3296W-1-**501**)
  is **500 Ω**; keep it in mind when setting the bias.
- **Q5 in the BOM:** `PT9926-SOP8` is an error; the correct part is the µPC1678G.
- **Relays:** HFD4/5-SR footprint. Check pinout and coil voltage before soldering.
- **T3/T4 and T1/T2 turns:** estimated, not measured on this radio.
- **Q4/Q6 bias:** needs a procedure and measurement.
- **Dissipation:** check Q4 temperature at sustained power.
- **Board revision:** this guide assumes Rev. 2.0.

## 11. Sources

- **MBPTRX** by Ian Mitchell (VK7IAN), GPL-3.0 — open reference design (schematic, BOM, Gerbers and
  firmware): <https://github.com/ianm8/MBPTRX>
- Manufacturer's technical documentation, Rev. 2.0 (schematics) and V2 production BOM — consulted as
  reference.
- HamGeek product page (TX is DIY, up to 5 W):
  <https://www.hgeek.com/products/hamgeek-raspberry-x1-full-mode-sdr-multi-band-phased-receiver-built-in-cw-codec>
- Omron G6K-2F-Y DC5 relay data (5 V coil, 237 Ω): <https://uk.rs-online.com/p/signal-relays/3650361>
- Public datasheets of RD16HHF1, RD01MUS1, µPC1678G, 1N5711, HFD4, SI2301 and 2N7002.

---

*Document under [GPL-3.0](../LICENSE). Attribution: Ian Mitchell (VK7IAN) — MBPTRX. © 2026 EA7LJY. If
you modify it, state the changes and publish the source.*

🌐 [Versión en español](Tx-mod-Es.md) · [HTML](Tx-mod-En.html)
