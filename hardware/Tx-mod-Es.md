# Modificación TX para Raspberry X1 (no oficial)

Guía para habilitar la **transmisión** en la HamGeek Raspberry X1, un SDR HF de 160–10 m que sale de
fábrica **solo como receptor**.

> 🌐 **Español** · [English](Tx-mod-En.md) — [versión HTML](Tx-mod-Es.html) — [lista de compra](Lista-compra-TX.md)

**Autor:** EA7LJY — [@ea7ljy73](https://github.com/ea7ljy73)

- **Estado:** borrador revisable. Valores contrastados con el esquemático Rev. 2.0, la BOM de
  producción V2 y una unidad real (foto interior sin TX). Pendiente de montaje y mediciones.
- **Base:** documentación técnica del fabricante Rev. 2.0 (esquemáticos) y BOM de producción V2,
  contrastadas con el proyecto abierto **MBPTRX** (GPL-3.0):
  <https://github.com/ianm8/MBPTRX>.
- **Naturaleza:** documento independiente y no oficial.
- **Licencia:** [GPL-3.0](../LICENSE).

> **Aviso.** La modificación implica RF y potencias de hasta ~5 W. **El autor no se hace
> responsable de daños o errores derivados del uso de esta información.** Trabaja con carga
> fantasma y vatímetro antes de conectar una antena, usa fuente con límite de corriente y respeta
> las precauciones ESD. Transmitir requiere licencia de radioaficionado.

## Índice

1. [Resumen](#1-resumen)
2. [Estado de una unidad de serie](#2-estado-de-una-unidad-de-serie)
3. [Antes de empezar](#3-antes-de-empezar)
4. [Componentes a añadir](#4-componentes-a-añadir)
5. [Filtros paso bajo (LPF)](#5-filtros-paso-bajo-lpf)
6. [Etapa de potencia y transformadores](#6-etapa-de-potencia-y-transformadores)
7. [Acoplador de ROE/SWR](#7-acoplador-de-roeswr)
8. [Alimentación de la etapa de TX](#8-alimentación-de-la-etapa-de-tx)
9. [Puesta en marcha y verificación](#9-puesta-en-marcha-y-verificación)
10. [Puntos a verificar](#10-puntos-a-verificar)
11. [Fuentes](#11-fuentes)

---

## 1. Resumen

La placa base está diseñada para recepción. Para transmitir hay que **poblar el transistor final,
los transformadores, los filtros paso bajo, el acoplador de ROE y los seis relés**, y quitar el
puente de recepción que trae de fábrica. Los relés los controla el firmware a través del expansor
I²C **PCA9555**.

Cadena de TX: señal de RF del FST3253 (`TXSIG`) → **Q5** µPC1678G (preamplificador) → **Q6**
RD01MUS1 (driver) → **T4** → **Q4** RD16HHF1 (final) → **T3** → **RELAY-6** (TX/RX) → filtro LPF de
la banda (**RELAY-1…5**) → acoplador **T1/T2** → antena.

En recepción, **RELAY-6** lleva la antena (ya filtrada por el LPF de la banda) a la entrada del
mezclador (`RXFST` → D1 → FST3253).

## 2. Estado de una unidad de serie

Revisión visual de una unidad de serie (placa *MAIN UNIT v2.0*, versión solo RX):

| Bloque | Montado | Falta |
| --- | --- | --- |
| Final | almohadilla térmica de Q4, C75, R50, R51, C82, FB5, C80, C86, R55 | **Q4** RD16HHF1, **T3**, **C76** (100 µF) |
| Driver | Q6 RD01MUS1, R56–R59, C83, C87, C88, FB6, R49 | **T4** |
| Preamplificador | Q5 µPC1678G, R52–R54, C79, C84, C85, FB4 | — |
| Conmutación | U11 78M09, Q2 SI2301, Q3 2N7002, R47, R48, C72–C74, C77, C78 | **RELAY-6** |
| Filtros LPF | condensadores C47–C70 | **RELAY-1…5**, **L1…L15** |
| Acoplador ROE | D3, D4, R37–R46, C63–C66, ADS1115 (U10) | **T1**, **T2** |

> ⚠️ **Puente de recepción.** De fábrica, un **cable rojo** une el conector de antena (BNC) directamente
> con la entrada del receptor (D1), saltándose relés y filtros. **Hay que retirarlo al montar
> RELAY-6 y los relés de los LPF.** Si se transmite con él puesto, la potencia entra en el mezclador
> de recepción y destruye el FST3253.
>
> Consecuencia para la recepción: mientras exista ese puente el receptor **no tiene ningún filtro de
> RF** delante del mezclador.

## 3. Antes de empezar

**Herramientas y equipo recomendados**

- Estación de soldadura (aire caliente y punta fina), flux y lupa/microscopio.
- Multímetro, comprobador de continuidad y, si es posible, osciloscopio y analizador de espectro.
- **Carga fantasma**, **vatímetro/medidor de ROE** y fuente de alimentación con límite de
  corriente ajustable.
- Pulsera y superficie ESD.

**Comprobaciones previas**

- Identifica la **revisión de tu placa**. Esta guía se basa en la Rev. 2.0 (*MAIN UNIT v2.0*).
- Revisa qué componentes trae realmente tu unidad con la tabla del [§2](#2-estado-de-una-unidad-de-serie).
- Verifica que la fuente entrega 13,8 V y al menos 2 A antes de conectar la etapa de potencia.

## 4. Componentes a añadir

La lista para comprar, con cantidades y repuestos, está en [`Lista-compra-TX.md`](Lista-compra-TX.md).

### 4.1. Semiconductores

| Ref. | Componente | Cant. | Función |
| --- | --- | --- | --- |
| Q4 | **RD16HHF1** (TO-220) | 1 | Final de potencia (~5 W). La aleta es la fuente (masa): no necesita aislante. |

Q5 (µPC1678G), Q6 (RD01MUS1), Q2 (SI2301), Q3 (2N7002) y los diodos D3/D4 (1N5711WS) **ya vienen
montados**. La BOM de producción lista `PT9926-SOP8` para Q5: es un error de la BOM (un MOSFET
doble); el componente montado es el **µPC1678G** del esquemático.

### 4.2. Relés

| Ref. | Componente | Cant. | Función |
| --- | --- | --- | --- |
| RELAY-1…5 | **Hongfa HFD4/5-SR** (SMD, DPDT, bobina 5 V) | 5 | Selección de filtro LPF |
| RELAY-6 | **Hongfa HFD4/5-SR** | 1 | Conmutación TX/RX |

El esquemático los llama `G6K-2Y-Y`, pero la **huella de la placa es `RELAY-SMD_HFD4/5-SR`** y la
BOM de producción indica **HFD4/5-S** (LCSC C64399). Un Omron **G6K-2F-Y DC5** es equivalente
eléctrico (bobina 5 V, ~21 mA); comprueba que encaja en la huella antes de comprarlo.

Las bobinas se alimentan directamente desde las salidas del PCA9555 (~21 mA cada una). Comprueba que
todos los relés conmutan con firmeza al cambiar de banda.

### 4.3. Núcleos y bobinados

| Ref. | Núcleo | Cant. | Uso |
| --- | --- | --- | --- |
| L1–L3, L7–L9 | **T37-2** (rojo) | 6 | Bobinas de los LPF de 160 y 80 m |
| L4–L6, L10–L15 | **T37-6** (amarillo) | 9 | Bobinas de los LPF de 40 a 10 m |
| T3, T4 | **FT37-43** | 2 | Transformadores bifilares (huella `FT37-43 BIF`) |
| T1, T2 | **FT23-43** | 2 | Acoplador de ROE |

### 4.4. Pasivos

| Ref. | Componente | Cant. | Notas |
| --- | --- | --- | --- |
| C76 | Electrolítico 100 µF / 25 V, radial Ø5 mm paso 2 mm | 1 | Desacoplo de +13,8 V del final |

Los condensadores de los filtros (C47–C70) ya vienen montados. Si tuvieras que reponer alguno, usa
**C0G/NP0**.

## 5. Filtros paso bajo (LPF)

Cada filtro es un paso bajo de 7 elementos (4 condensadores a masa y 3 bobinas). La banda de cada uno
se deduce de su frecuencia de corte, calculada con los valores del esquemático
(T37-2: A<sub>L</sub> ≈ 4,0 nH/vuelta²; T37-6: A<sub>L</sub> ≈ 3,0 nH/vuelta²):

| Relé | Línea PCA9555 | Banda | Núcleo y vueltas | Inductancia | Condensadores | Corte aprox. |
| --- | --- | --- | --- | --- | --- | --- |
| RELAY-1 | BAND1 | **160 m** | T37-2: L1 32, L2 37, L3 32 | 4,1 / 5,5 / 4,1 µH | C47 820p, C48 2,2n, C49 2,2n, C50 820p | 2,9 MHz |
| RELAY-3 | BAND2 | **80 m** | T37-2: L7 22, L8 27, L9 22 | 1,9 / 2,9 / 1,9 µH | C55 470p, C56 1,2n, C57 1,2n, C58 470p | 5,4 MHz |
| RELAY-5 | BAND3 | **60–40 m** | T37-6: L13 21, L14 24, L15 21 | 1,3 / 1,7 / 1,3 µH | C67 270p, C68 680p, C69 680p, C70 270p | 9,3 MHz |
| RELAY-2 | BAND4 | **30–20 m** | T37-6: L4 16, L5 17, L6 16 | 0,77 / 0,87 / 0,77 µH | C51 180p, C52 390p, C53 390p, C54 180p | 17 MHz |
| RELAY-4 | BAND5 | **17–10 m** | T37-6: L10 10, L11 11, L12 10 | 0,30 / 0,36 / 0,30 µH | C59 120p, C60 220p, C61 220p, C62 120p | 36 MHz |

> La numeración de relés **no sigue el orden de las bandas**. El firmware X1 Custom ya usa este mapeo
> (LPF1 = 160 m, LPF2 = 80 m, LPF3 = 40 m, LPF4 = 30–20 m, LPF5 = 17–10 m).
>
> *Corrección respecto a versiones anteriores de esta guía, que desplazaban las bandas una posición
> (80/40/30/20/10 m).*

**Bobinado:** hilo esmaltado de **0,4 mm** (AWG 26), espiras repartidas por todo el toroide. Una
«vuelta» es cada paso del hilo por el agujero. Si tienes analizador (NanoVNA), **mide cada filtro**:
pérdida de inserción < 0,5 dB en la banda y rechazo del 2.º armónico > 40 dB. Ajusta abriendo o
juntando espiras.

## 6. Etapa de potencia y transformadores

- **Q5 → Q6 → Q4** forman la cadena de amplificación. Q4 es el final (RD16HHF1, ~5 W) y Q6 su
  driver (RD01MUS1).
- **Q4** va atornillado (M3) sobre su almohadilla térmica contra el chasis/disipador, con pasta
  térmica. Disipa varios vatios: no transmitas sin él bien fijado.
- **T3 y T4** son transformadores **bifilares** sobre FT37-43 (la huella es `FT37-43 BIF`, de 4
  patillas; las conexiones las fija la placa). El esquemático no da vueltas. Valores de partida
  (estimados, no verificados): **8–10 vueltas bifilares** de hilo 0,3–0,4 mm en cada uno.
  Con la conexión 1:4 que sugiere el esquemático, el drenador de Q4 ve ~12,5 Ω, compatible con unos
  5 W a 13,8 V.

Mantén los conductores de RF cortos y con retorno de masa próximo.

### 6.1. Polarización

- La polarización de Q6 la fija **R49** y la de Q4 **R50**, ambas alimentadas desde los +9 V
  conmutados (solo presentes en TX).
- Antes del primer encendido en TX, gira R50 (y R49) al **mínimo de tensión de puerta**.
- Con **carga fantasma**, fuente limitada a **1,5 A** y **sin señal de excitación**, activa TX y sube
  R50 poco a poco hasta una corriente de reposo de Q4 de unos **100–200 mA** sobre el consumo en
  RX. Haz lo mismo con R49 para Q6 (unos 20–50 mA).
- La tensión de puerta debe quedar por debajo de unos 4 V (RD16HHF1, V<sub>th</sub> típica 2–3 V).
  Si la corriente se dispara o Q4 se calienta en reposo, corta y revisa el montaje.

Referencia: procedimiento del proyecto MBPTRX para su etapa con 2× RD16HHF1
(<https://github.com/ianm8/MBPTRX/blob/main/PCB/README.md>), que hay que escalar a un solo final.
No validado en la X1.

## 7. Acoplador de ROE/SWR

T1 y T2 forman un **acoplador tándem**. Las resistencias de carga en paralelo (R41∥R42 y R43∥R44,
2 × 100 Ω = 50 Ω) indican el diseño clásico:

| Transformador | Primario | Secundario |
| --- | --- | --- |
| T1 | 1 paso (el conductor de antena atraviesa el toroide) | 10 vueltas |
| T2 | 1 paso | 10 vueltas |

Acoplamiento ≈ −20 dB. Los detectores son D3/D4 (1N5711WS) y la lectura la digitaliza el
**ADS1115** (FWD en AIN0, REF en AIN1). Calibra con carga de 50 Ω y una carga desadaptada conocida.

*Versiones anteriores de esta guía proponían 1,5/10 y 4/30 vueltas; se sustituyen por el tándem
simétrico, coherente con las resistencias de carga de la placa.*

## 8. Alimentación de la etapa de TX

Según el esquemático (hoja 3):

1. **U11** (78M09) genera **+9 V** a partir de +13,8 V. También alimenta el amplificador de audio
   LM386, así que siempre está montado.
2. `TXENABLE` (PCA9555) activa **Q3** (2N7002), que abre **Q2** (SI2301).
3. Q2 entrega los **+9 V conmutados** al drenador de Q6 (a través de T4) y a los trimmers de
   polarización R49/R50. En RX no hay tensión: los transistores quedan cortados.
4. El drenador de **Q4** recibe +13,8 V a través de T3 de forma permanente (sin polarización no
   conduce).
5. **Q5** (µPC1678G, ya montado) se alimenta de la red **`+5V_B`** (Vcc y salida, a través de FB4). Esa red no
   aparece en ninguna otra hoja del esquemático.

> ⚠️ **Antes de la primera TX, mide con el polímetro de dónde sale `+5V_B`.** El µPC1678G
> admite como máximo **6 V**: no debe recibir los 9 V. Comprueba también si `+5V_B` está presente en
> RX o solo en TX.

## 9. Puesta en marcha y verificación

1. **Inspección visual:** soldaduras, puentes, orientación de transistores y relés. **Cable rojo de
   antena retirado.**
2. **Continuidad:** sin cortos entre +13,8 V, +9 V, +5 V y masa.
3. **Alimentación sin RF:** enciende con límite de corriente bajo y comprueba +5 V, +9 V, los +9 V
   conmutados (solo en TX) y `+5V_B` (≤ 6 V).
4. **Relés:** cambia de banda desde el firmware y confirma que cierra el relé del filtro correcto
   (tabla del [§5](#5-filtros-paso-bajo-lpf)) y que RELAY-6 conmuta en TX.
5. **RX:** comprueba que la recepción sigue funcionando a través de RELAY-6 y del filtro de la banda.
6. **Polarización** según el [§6.1](#61-polarización).
7. **TX en carga fantasma:** primero a baja potencia y durante pocos segundos. Vigila corriente,
   temperatura de Q4 y ROE.
8. **Ajuste:** sube potencia progresivamente, comprueba armónicos con analizador si dispones de él
   y valida la lectura de ROE con una carga desadaptada.
9. **Antena:** solo después de validar todo lo anterior.

**Nunca transmitas sin carga o antena conectada.** El firmware X1 Custom mantiene la TX bloqueada
hasta activar «Etapa TX instalada» en el menú, y aplica tiempo máximo de TX y corte por ROE alta.
La TX del firmware **no se ha probado todavía en el aire**.

## 10. Puntos a verificar

- **`+5V_B`:** origen y tensión antes de alimentar Q5 (máx. 6 V). Ver [§8](#8-alimentación-de-la-etapa-de-tx).
- **R49/R50:** vienen montados. El esquemático indica 4,7 kΩ, pero la referencia de la BOM
  (3296W-1-**501**) es de **500 Ω**; tenlo en cuenta al ajustar la polarización.
- **Q5 en la BOM:** `PT9926-SOP8` es un error; el correcto es µPC1678G.
- **Relés:** huella HFD4/5-SR. Verifica patillaje y tensión de bobina antes de soldar.
- **Vueltas de T3/T4 y T1/T2:** estimadas, no medidas en este equipo.
- **Polarización de Q4/Q6:** requiere procedimiento y medida.
- **Disipación:** verifica la temperatura de Q4 a potencia sostenida.
- **Revisión de placa:** esta guía asume Rev. 2.0.

## 11. Fuentes

- **MBPTRX** de Ian Mitchell (VK7IAN), GPL-3.0 — diseño abierto de referencia (esquemático, BOM,
  Gerbers y firmware): <https://github.com/ianm8/MBPTRX>
- Documentación técnica del fabricante, Rev. 2.0 (esquemáticos) y BOM de producción V2 —
  consultadas como referencia.
- Ficha del producto de HamGeek (la TX es de montaje propio, hasta 5 W):
  <https://www.hgeek.com/products/hamgeek-raspberry-x1-full-mode-sdr-multi-band-phased-receiver-built-in-cw-codec>
- Datos del relé Omron G6K-2F-Y DC5 (bobina 5 V, 237 Ω): <https://uk.rs-online.com/p/signal-relays/3650361>
- Hojas de datos públicas de RD16HHF1, RD01MUS1, µPC1678G, 1N5711, HFD4, SI2301 y 2N7002.

---

*Documento bajo [GPL-3.0](../LICENSE). Atribución: Ian Mitchell (VK7IAN) — MBPTRX. © 2026 EA7LJY. Si
lo modificas, indica los cambios y publica las fuentes.*

🌐 [English version](Tx-mod-En.md) · [HTML](Tx-mod-Es.html) · [Lista de compra](Lista-compra-TX.md)
