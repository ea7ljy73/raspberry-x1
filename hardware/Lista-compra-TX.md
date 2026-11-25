# Lista de compra — modificación TX de la Raspberry X1

Lo que hay que comprar para habilitar la transmisión en una unidad de serie (solo RX). Detalles y
montaje en la [guía](Tx-mod-Es.md). Las cantidades incluyen **repuestos** razonables.

## Imprescindible

| Componente | Para | Necesarios | Comprar | Notas |
| --- | --- | --- | --- | --- |
| **RD16HHF1** (Mitsubishi, TO-220) | Q4, final | 1 | **2** | Compra a un distribuidor fiable: abundan falsificaciones |
| **Relé Hongfa HFD4/5-SR** (SMD, DPDT, 5 V) | RELAY-1…6 | 6 | **7** | LCSC C64399. Alternativa: Omron G6K-2F-Y DC5 (comprobar huella) |
| **Toroide T37-2** (rojo) | L1–L3, L7–L9 | 6 | **8** | Micrometals o Amidon |
| **Toroide T37-6** (amarillo) | L4–L6, L10–L15 | 9 | **11** | |
| **Toroide FT37-43** | T3, T4 | 2 | **3** | Ferrita mix 43 |
| **Toroide FT23-43** | T1, T2 | 2 | **3** | Ferrita mix 43 |
| **Hilo esmaltado 0,4 mm** (AWG 26) | Bobinas LPF y T3/T4 | ~5 m | **10 m** | |
| **Hilo esmaltado 0,25–0,3 mm** (AWG 30) | Secundarios de T1/T2 | ~1 m | **3 m** | |
| **Condensador electrolítico 100 µF / 25 V** (radial Ø5 mm, paso 2 mm) | C76 | 1 | **2** | |
| **Pasta térmica** y **tornillo M3** con arandela | Fijación de Q4 | 1 | 1 | |

## Componentes equivalentes

| Original | Equivalente | Nota |
| --- | --- | --- |
| RD16HHF1 | RD15HVF1 | Mismo encapsulado y patillaje (compruébalo). Evita el IRF510: la aleta es el drenador y el patillaje es otro |
| G6K-2Y-Y (esquema) | Hongfa HFD4/5-SR, HFD4/5-S, Omron G6K-2F-Y DC5 | Bobina de 5 V. La huella de la placa es HFD4/5-SR |
| FT37-43 | Fair-Rite 5943000301 | Mismo núcleo |
| FT23-43 | Fair-Rite 5943000101 | Mismo núcleo |
| T37-2 / T37-6 | Micrometals o Amidon T37-2 / T37-6 | No los sustituyas por ferrita |
| C76 100 µF / 25 V | Cualquier electrolítico radial Ø5 mm, paso 2 mm, ≥ 25 V | Mejor de 105 °C |
| 1N5711WS (D3, D4) | BAT54WS, SD101AWS | Ya vienen montados; solo para reponer |

## Equipo de medida (si no lo tienes)

- **Carga fantasma** de 50 Ω y al menos 10 W.
- **Vatímetro / medidor de ROE** para HF.
- **Fuente de 13,8 V** con límite de corriente ajustable (≥ 2 A).
- Opcional: **NanoVNA** para ajustar los filtros y el acoplador de ROE.

## Ya vienen montados (no comprar)

Q5 µPC1678G, Q6 RD01MUS1, C75, U11 78M09, Q2 SI2301, Q3 2N7002, D3/D4 1N5711WS, ADS1115, R49, R50 y los
condensadores de los filtros (C47–C70).
