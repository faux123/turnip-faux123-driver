# Adreno support matrix

This page lists which Adreno GPUs turnip_faux123 supports, which ones I have tested, and what each test covered. It applies to [v1.2.1](https://github.com/faux123/turnip-faux123-driver/releases/tag/v1.2.1), the latest release.

## Tested devices

| Device | GPU | Generation | SoC | GMEM | Tested release | What was tested |
|---|---|---|---|---|---|---|
| AYN Odin 3 | Adreno 830 | A8xx | CQ8725S | 12 MiB | v1.2.1 | All 115 Vulkan-Samples scenes, against the build before it: no scene lost, no scene slower; FauxVkTest driver check and speed test on [FauxVkDB](https://fauxvkdb.canonspike.com/report/1552) |
| | | | | | v1.2.0 | All 115 Vulkan-Samples scenes, against v1.1.20: no scene lost, no scene slower; `compute_nbody` 11% faster |
| | | | | | v1.1.20 | All 115 Khronos Vulkan-Samples scenes, against v1.1.9: no scene lost, no scene slower |
| | | | | | v1.1.9 | FauxVkTest driver check, 2026-10-25: one problem, the `compat.nested_map` crash, which v1.1.20 fixes |
| Retroid Pocket Duo | Adreno 722 | A7xx | CQ7790S | | v1.2.1 | All 115 Vulkan-Samples scenes, against the build before it: no scene lost, no scene slower; Pokémon Ultra Sun, Z-A and Azahar fixes checked in game; driver check on [FauxVkDB](https://fauxvkdb.canonspike.com/report/1558) |
| Samsung Galaxy S23 | Adreno 740 | A7xx | Snapdragon 8 Gen 2 (SM8550) | 3 MiB | v1.2.1 | All 115 Vulkan-Samples scenes, against the build before it: no scene lost; one scene slower in both runs (`fragment_shading_rate`); driver check on [FauxVkDB](https://fauxvkdb.canonspike.com/report/1551) |
| | | | | | v1.2.0 | All 115 Vulkan-Samples scenes, against v1.1.20: no scene lost, no scene slower; `hdr` 22% faster |
| | | | | | v1.1.20 | All 115 Vulkan-Samples scenes, against v1.1.9: no scene lost, no scene slower |
| Samsung Galaxy S22 | Adreno 730 | A7xx | Snapdragon 8 Gen 1 (SM8450) | 2 MiB | v1.2.1 | All 115 Vulkan-Samples scenes, against the build before it: no scene lost; four scenes slower in one run each, not in the other; driver check on [FauxVkDB](https://fauxvkdb.canonspike.com/report/1556) |
| | | | | | v1.2.0 | All 115 Vulkan-Samples scenes, against v1.1.20: no scene lost, no scene slower; `hdr` 21% faster |
| | | | | | v1.1.20 | All 115 Vulkan-Samples scenes, against v1.1.9: no scene lost, no scene slower; `fragment_density_map` fixed |
| Samsung Galaxy S21+ | Adreno 660 | A6xx | Snapdragon 888 (SM8350) | 1.5 MiB | v1.2.1 | All 115 Vulkan-Samples scenes, against the build before it: no scene lost; one scene slower in both runs (`hpp_instancing`); driver check on [FauxVkDB](https://fauxvkdb.canonspike.com/report/1555) |
| | | | | | v1.2.0 | All 115 Vulkan-Samples scenes, against v1.1.20: no scene lost, no scene slower; `compute_nbody` 17% faster |
| | | | | | v1.1.20 | All 115 Vulkan-Samples scenes, against v1.1.9: no scene lost, no scene slower; `afbc` fixed |
| Samsung Galaxy S20+ | Adreno 650 | A6xx | Snapdragon 865 (SM8250) | | v1.2.1 | All 115 Vulkan-Samples scenes, against the build before it: no scene lost; one scene slower (`hpp_push_descriptors`, +5.8%); driver check on [FauxVkDB](https://fauxvkdb.canonspike.com/report/1557) |
| Samsung Galaxy S10 | Adreno 640 | A6xx | Snapdragon 855 (SM8150) | | v1.2.1 | All 115 Vulkan-Samples scenes, against the build before it: no scene lost, no scene slower; driver check on [FauxVkDB](https://fauxvkdb.canonspike.com/report/1563) |

The Vulkan-Samples runs are capability sweeps: each scene runs on both drivers in turn, and any scene that looks slower is run again five times to confirm. The FauxVkTest driver check and speed test were run on v1.2.1 on every device above.

## Supported GPUs

turnip_faux123 starts only on the A6xx, A7xx, and A8xx generations. On any other generation the driver does not start.

A GPU listed below has an entry in the driver's GPU table, so the driver recognizes it. "Tested" means I ran it on my own device; every other GPU here is expected to work but has not been tested. If you run one of them, please report how it went.

- **A6xx:** Adreno 605, 608, 610, 612, 613, 615, 616, 618, 619, 620, 621, 623, 630, 640 (tested), 643, 644, 650 (tested), 660 (tested), 663, 680, 690, 702, 704, 7c+ Gen 3, 7c+ Gen 3 Lite, 8c Gen 3
- **A7xx:** Adreno 722 (tested), 725, 730 (tested), 735, 740 (tested), 740v3, 750, X1-45, X1-85, FDA32
- **A8xx:** Adreno 810, 829, 830 (tested), 830v1, 840, X2-85, X2-90

## Not supported

Adreno 2xx, 3xx, 4xx, and 5xx. turnip does not support these generations.

## Report a result

If you test turnip_faux123 on a GPU that is not marked as tested, open an Issue with your device, GPU, Android version, the release you used, and what worked or failed. You can also post in the [FauxApps Discord](https://discord.gg/ceyPguKNbu).
