<a id="es"></a>
# Avisos y atribuciones

**Español** · [English](#english)

## Independencia

**X1 Custom** es un proyecto independiente de **EA7LJY** ([@ea7ljy73](https://github.com/ea7ljy73)).
No está afiliado, patrocinado ni respaldado por HamGeek, por el diseñador de la Raspberry X1 ni por
Raspberry Pi Ltd. Las marcas «HamGeek», «Raspberry Pi» y «Raspberry X1» pertenecen a sus titulares y
solo se usan para identificar el equipo.

## Contenido de este repositorio

- **Firmware X1 Custom:** se distribuye compilado (`.uf2`) en [`firmware/`](firmware/). © 2026 EA7LJY, todos los
  derechos reservados. Puedes usarlo y copiarlo gratis; no puedes modificarlo ni venderlo.
- **Documentación de hardware** ([`hardware/`](hardware/)): bajo [GPL-3.0](LICENSE).
- **Capturas** ([`docs/img/`](docs/img/)): pantallas reales del firmware.

## Componentes de terceros en el firmware

El binario incluye estos componentes, cada uno con su licencia:

| Componente | Autor | Licencia |
| --- | --- | --- |
| ft8_lib | Kārlis Goba | MIT |
| kiss_fft | Mark Borgerding | BSD-3-Clause |
| TFT_eSPI | Bodmer | FreeBSD / MIT |
| arduino-pico | Earle F. Philhower III | LGPL-2.1 |
| Raspberry Pi Pico SDK | Raspberry Pi Ltd | BSD-3-Clause |

Los textos completos de las licencias van en el paquete de cada versión. Como exige la LGPL-2.1, si
quieres volver a enlazar el firmware con otra versión del core arduino-pico, pídele al autor los
ficheros objeto.

## Fuentes de la documentación de hardware

- Documentación técnica del fabricante de la Raspberry X1 (Rev. 2.0) y BOM de producción V2,
  consultadas como referencia; no se redistribuyen.
- **MBPTRX** de Ian Mitchell (VK7IAN), GPL-3.0 — diseño abierto de referencia:
  <https://github.com/ianm8/MBPTRX>
- **uSDX / (tr)uSDX** — referencia para el bobinado del acoplador de ROE.

## Exención de responsabilidad

Todo se ofrece **«tal cual»**, sin garantía. **El autor no se hace responsable de daños, pérdidas o
errores** derivados de su uso.

---

<a id="english"></a>
# Notices and attribution

[Español](#es) · **English**

## Independence

**X1 Custom** is an independent project by **EA7LJY** ([@ea7ljy73](https://github.com/ea7ljy73)). It is
not affiliated with, sponsored by or endorsed by HamGeek, the Raspberry X1 designer or Raspberry Pi
Ltd. "HamGeek", "Raspberry Pi" and "Raspberry X1" are trademarks of their owners and are used only to
identify the radio.

## Contents of this repository

- **X1 Custom firmware:** distributed compiled (`.uf2`) in [`firmware/`](firmware/). © 2026 EA7LJY,
  all rights reserved. You may use and copy it free of charge; you may not modify or sell it.
- **Hardware documentation** ([`hardware/`](hardware/)): under [GPL-3.0](LICENSE).
- **Screenshots** ([`docs/img/`](docs/img/)): real firmware screens.

## Third-party components in the firmware

The binary includes these components, each under its own licence:

| Component | Author | Licence |
| --- | --- | --- |
| ft8_lib | Kārlis Goba | MIT |
| kiss_fft | Mark Borgerding | BSD-3-Clause |
| TFT_eSPI | Bodmer | FreeBSD / MIT |
| arduino-pico | Earle F. Philhower III | LGPL-2.1 |
| Raspberry Pi Pico SDK | Raspberry Pi Ltd | BSD-3-Clause |

The full licence texts ship with every release. As LGPL-2.1 requires, if you want to relink the
firmware against another version of the arduino-pico core, ask the author for the object files.

## Sources for the hardware documentation

- The Raspberry X1 manufacturer's technical documentation (Rev. 2.0) and V2 production BOM,
  consulted as a reference; not redistributed.
- **MBPTRX** by Ian Mitchell (VK7IAN), GPL-3.0 — open reference design:
  <https://github.com/ianm8/MBPTRX>
- **uSDX / (tr)uSDX** — reference for the SWR bridge winding.

## Disclaimer

Everything is provided **"as is"**, without warranty. **The author is not liable for any damage,
loss or error** arising from its use.
