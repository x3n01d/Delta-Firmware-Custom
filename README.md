# Delta-Firmware

Custom firmware for the **Quansheng UV-K1 / UV-K5 V3** (PY32F071), based on the
open-source **Delta** firmware.

Delta keeps the classic UV-K5 V3 feature set and adds a firmware-level **multiboot**,
so several firmware images can live in the radio at once and be selected at boot.

---

## Highlights

- **Firmware-level multiboot** — store several firmware images in the radio's external
  SPI flash (PY25Q16) and pick one at boot. Restore is done by a small RAM-resident
  stub, so the base image is never left half-erased.
- **Full Delta feature set** — channels, scanning, menus and settings are inherited
  unchanged.
- **Drop-in build** — Meson + Docker toolchain, one command to build.

## Hardware

| Parameter | Value |
|---|---|
| Radio | Quansheng UV-K1 / UV-K5 V3 |
| MCU | PY32F071 (Cortex-M0+), 128 KB flash, 16 KB RAM |
| Display | ST7565, 128×64 |
| External flash | PY25Q16 (2 MB, SPI) |

## Building

```bash
bash toolchain/compile-with-meson.sh default
```

Output: `build/deltafw.default.*.bin` — must stay within the flash limit.

## Flashing

Use your usual UV-K5 V3 flasher (UV Studio, k5tool, etc.) with the radio in
firmware-update mode. **Back up your codeplug and calibration first.**

## Multiboot

- Slots live in the external SPI flash.
- Install a slot with **UV Studio → Firmware Slots** (Display name + version).
- At boot the multiboot selector lists the installed images.
- Restore copies the selected slot back to the base address through a RAM stub.

## Credits

- **Delta** (qshosfw) — the base firmware:
  <https://github.com/qshosfw/deltafw-k1-k5v3>
- The UV-K5 / UV-K1 community and all upstream contributors.
- Multiboot port and Delta integration — x3n01d.

## Disclaimer

This is unofficial, community-made firmware. It is **not** affiliated with or endorsed
by Quansheng, Delta or any hardware manufacturer.

Flashing custom firmware **can brick your radio** and may void your warranty. You flash
at **your own risk**. Always back up your codeplug, calibration and original firmware
first, and make sure you are using the correct build for your exact hardware revision
(UV-K1 / UV-K5 V3, PY32F071). The authors take **no responsibility** for any damage,
data loss, legal issues or radio malfunction resulting from the use of this firmware.
Use it only where you are allowed to transmit.
