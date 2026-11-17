# proprietary_vendor_xiaomi_onyx-perf

Qualcomm's perf stack (perfd: `vendor.qti.hardware.perf2-hal-service` and its feature libraries,
QLM, QAPE, the workload classifier; poweropt is left out, it crashed in a loop) and the BoostFramework jars (`QPerformance`, `UxPerformance`, `QXPerformance`)
with their system libraries, from the onyx stock dump OS3.0.303.0.WOLMIXM.

Only the DerpFest build of onyx uses it: DerpFest's frameworks/base carries BoostFramework, which
sends the framework hints (subsequent launch, pre-fling, drag, animations, rotation, FPS updates,
package names) to perfd. The Evolution X build keeps libperfmgr and never checks this repo out.

- `proprietary-files.txt`: the list, pinned by sha1.
- `setup-blobs.py <stock dump>`: copies the files and writes `Android.bp` and `onyx-perf-vendor.mk`.
- `device/xiaomi/onyx` (derp-17) inherits `onyx-perf-vendor.mk`, puts QPerformance and UxPerformance
  in `PRODUCT_BOOT_JARS` and runs Qualcomm's power HAL in place of libperfmgr.

`thermalbreakboostconfig.xml` is not shipped: without it perfd leaves mi_thermald's boost node alone,
and onyx-thermalbreak in the device tree stays its only writer.
