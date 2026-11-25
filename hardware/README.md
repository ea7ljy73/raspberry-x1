<a id="es"></a>
# Hardware — modificación TX

**Español** · [English](#english)

Documentación para habilitar la transmisión en la HamGeek Raspberry X1.

**Autor:** EA7LJY — [@ea7ljy73](https://github.com/ea7ljy73)

| Fichero | Descripción |
| --- | --- |
| [`Tx-mod-Es.md`](Tx-mod-Es.md) | **Fuente canónica** de la guía de modificación TX (español). |
| [`Tx-mod-Es.html`](Tx-mod-Es.html) | Versión HTML de presentación (español). |
| [`Tx-mod-En.md`](Tx-mod-En.md) | Versión inglesa de la guía. |
| [`Tx-mod-En.html`](Tx-mod-En.html) | Versión HTML de presentación (inglés). |
| [`Lista-compra-TX.md`](Lista-compra-TX.md) | Lista de compra de componentes para la TX ([HTML](Lista-compra-TX.html)). |

## Alcance

- Lista de componentes a añadir (etapa de potencia, filtros LPF, acoplador SWR y relés).
- Tabla de bandas, núcleos y valores de los filtros.
- Puntos críticos de montaje y puesta en marcha.
- Procedimiento recomendado de verificación con carga fantasma.

La guía es un documento **independiente y no oficial**, elaborado a partir del análisis del equipo y
de la documentación técnica del fabricante (Rev. 2.0). El diseño abierto de referencia es **MBPTRX**
(GPL-3.0): <https://github.com/ianm8/MBPTRX>.

## Aviso

Modificar el equipo implica trabajar con RF y potencias de hasta ~5 W. Respeta las precauciones ESD,
usa fuente con límite de corriente y **verifica siempre con carga fantasma y vatímetro** antes de
conectar una antena. Transmitir requiere licencia de radioaficionado.

**El autor no se hace responsable de daños o errores derivados del uso de esta documentación.**

## Licencia

[GPL-3.0](../LICENSE). Conserva la atribución (Ian Mitchell, VK7IAN — MBPTRX), el texto de la
licencia y las fuentes correspondientes, e indica los cambios. Las obras derivadas se distribuyen
bajo GPL-3.0.

---

<a id="english"></a>
# Hardware — TX modification

[Español](#es) · **English**

Documentation to enable transmission on the HamGeek Raspberry X1.

**Author:** EA7LJY — [@ea7ljy73](https://github.com/ea7ljy73)

| File | Description |
| --- | --- |
| [`Tx-mod-En.md`](Tx-mod-En.md) | **Canonical source** of the TX modification guide (English). |
| [`Tx-mod-En.html`](Tx-mod-En.html) | HTML presentation version (English). |
| [`Tx-mod-Es.md`](Tx-mod-Es.md) | Spanish version of the guide. |
| [`Tx-mod-Es.html`](Tx-mod-Es.html) | HTML presentation version (Spanish). |
| [`Lista-compra-TX.md`](Lista-compra-TX.md) | Shopping list of TX parts, Spanish ([HTML](Lista-compra-TX.html)). |

## Scope

- List of parts to add (power stage, LPF filters, SWR bridge and relays).
- Band table with cores and filter values.
- Critical assembly and bring-up points.
- Recommended verification procedure into a dummy load.

The guide is an **independent, unofficial** document based on analysis of the radio and the
manufacturer's technical documentation (Rev. 2.0). The open reference design is **MBPTRX** (GPL-3.0):
<https://github.com/ianm8/MBPTRX>.

## Notice

Modifying the radio means working with RF and up to ~5 W. Follow ESD precautions, use a
current-limited supply and **always verify into a dummy load with a power/SWR meter** before
connecting an antenna. Transmitting requires an amateur radio licence.

**The author accepts no liability for damage or errors arising from the use of this documentation.**

## Licence

[GPL-3.0](../LICENSE). Keep the attribution (Ian Mitchell, VK7IAN — MBPTRX), the licence text and
the corresponding source, and indicate changes. Derivative works are distributed under GPL-3.0.
