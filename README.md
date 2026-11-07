# NOVA OS

NOVA OS is a standalone-first embedded Linux smartwatch project. Current work is **Milestone 1: hardware validation**, with the host-side engineering baseline already real and repeatable: unit-tested service cores and a Qt Quick shell that compiles and renders offscreen as committed screenshots. No bootable image or hardware test result exists yet.

## Provisional target

Radxa CM3J (RK3568J) on the Raspberry Pi Compute Module 4 IO Board is the provisional evaluation combination. Radxa documents the pairing and SD boot; upstream Linux contains a matching board device tree. This supports a serial/HDMI first-boot path, **not** a compatible 2-inch DSI panel. Exact CM3J SKU, conflicting published memory/storage options, supply, graphics userspace, and power/suspend remain to be verified before purchase.

## Engineering documents

- [Hardware comparison, decision, display procurement requirements, preliminary BOM](docs/hardware-validation.md)
- [Software architecture and Buildroot/kernel integration plans](docs/architecture.md)
- [Milestones and acceptance criteria](docs/roadmap.md)
- [Primary-source register and evidence gaps](docs/sources.md)
- [Wear OS / watchOS capability map and NOVA parity roadmap](docs/platform-feature-parity.md)
- [NOVA Intelligence and on-device model evaluation plan](docs/ai-roadmap.md)
- [Optional Mistral cloud assistant setup and privacy](docs/mistral-cloud.md)
- [NOVA visual design system](docs/ui-design-system.md)
- [Watch UI design research across brands](docs/watch-ui-research.md)
- [Verification log - what was actually executed and observed](docs/verification-log.md)
- [Evidence: rendered shell pages (offscreen Qt)](docs/evidence/)

## Status

**Implemented and verified on development hosts:** service cores (`system/` - ring buffer, power policy, diagnostics JSONL store, notification ranking core) with unit tests passing under ctest on Windows (mingw) and WSL (gcc); Qt 6 shell source (24 QML resources, including an original Canvas icon set, progress rings, a simulated liquid-glass material, an animated backdrop and shared page transitions; health, workout, sleep, notification, navigation, media, weather, calls and assistant previews; Orbit/Pulse/Liquid watch faces; session accent/glass/motion personalization; theme singleton `NovaTheme`; and `--size/--page/--ask/--screenshot` CLI) that builds offscreen in WSL and renders 17 modes with no QML binding errors. The glass is a software-rendered approximation with no real backdrop blur/refraction, and personalization is not persisted. Feature pages remain unconnected to live hardware/services. NOVA Intelligence currently runs deterministic offline commands; no generative model is integrated, and the optional Mistral cloud adapter is not live-API-tested.

**Written but never executed:** provisional serial-console Buildroot defconfig and kernel fragment (`buildroot/external`, `linux/kernel-config`), boot-chain plans (`boot/`), Buildroot `nova-shell` package. These are provisioning artifacts with explicit not-tested status in their headers.

**Not built or verified on target:** Buildroot image, U-Boot/kernel image, target boot, touchscreen/DSI, Qt runtime on the device, system services at runtime, secure boot, signed updates, or wearable power. The QML pages are a development preview, not a completed watch interface.

## Development environment

- Windows 11 (repo host, unit-test host: mingw + CMake/Ninja).
- Ubuntu under WSL2, verified working: gcc 15.2.0 / g++, CMake 4.2.3, Ninja 1.13.2, Qt 6.10.2 (Quick/QML), Noto fonts - used for the offscreen shell build and WSL ctest runs (see `docs/verification-log.md`).
- Still missing where noted: nothing for service tests; Buildroot image work requires the remaining Buildroot prerequisite set (cpio, rsync, device-tree compiler, and more) which is not yet installed or verified; Python is not installed on the Windows host, so `tests/structure-test.py` has not been executed yet.
- No physical target is connected.

