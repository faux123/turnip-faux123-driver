# turnip-faux123-driver

Mesa **Turnip**, the open source Vulkan driver for Qualcomm Adreno GPUs, built for
Android and packaged for [AdrenoTools](https://github.com/bylaws/libadrenotools)
driver loading. No root required.

I apply fixes that are not upstream in mesa yet, and I verify every one of them
on my own hardware before I release it. Each release lists what it changes.

**Everything here is verified on one device: my AYN Odin 3, the 12 GB model,
Adreno 830.** It should load on 600 and 700 series parts too, but I do not own
one, so nothing here is tested there and I can make no promises. See
[Adreno 600 and 700 series](#adreno-600-and-700-series-not-tested-no-guarantees).

Grab the latest `.adpkg.zip` from [Releases](../../releases) and import it in
GameNative, Winlator, or any app with an AdrenoTools driver picker.

---

## Where the driver comes from

Nothing here is a repackaged binary from somewhere else. It is built from source,
and you can rebuild it yourself.

| | |
|---|---|
| Source | [`gitlab.freedesktop.org/mesa/mesa`](https://gitlab.freedesktop.org/mesa/mesa), branch `main` |
| Toolchain | Android NDK r29, meson cross build, `aarch64` |
| Packaging | AdrenoTools `.adpkg.zip`: `libvulkan_freedreno.so` plus `meta.json` |

Builds track mesa `main`, so the base moves. **Every release names the exact
mesa commit it was built from**, in its release notes and in the `meta.json`
inside the package.

The build container, the cross file, the patch scripts and a recreate guide live
in a companion repository. Ask in an issue if you want it opened up.

### Why upstream `main` and not a fork

Most Adreno 8xx Turnip builds you can download are cut from a community fork,
because upstream support for the newer parts arrived later. I started there too.
Upstream turned out to be the better base once the display bug below was fixed:
it is faster on tile-heavy work, and it runs ray queries that the fork cannot
finish.

---

## How it was tested

Everything below was measured on **one device**: my AYN Odin 3, the 12 GB model,
Adreno 830, Android 15, rootless.

The test rig is the Khronos [Vulkan-Samples](https://github.com/KhronosGroup/Vulkan-Samples)
app, rebuilt with AdrenoTools linked in so it loads a chosen driver directly. No
Wine, no DXVK, no Box64, no emulation. One variable: the driver `.so`.

Before a release I launch all 86 samples one at a time and check every one for
crashes, against the driver the new build replaces. Each release says what that
run found.

Frame times, minimum of three rounds, round robin between drivers with a
throttle gate between every run, against the stock Qualcomm driver. Under 1.00
means Turnip is faster:

| sample | ratio vs Qualcomm |
|---|---|
| `instancing` | **0.32x** |
| `oit_depth_peeling` | 0.55x |
| `terrain_tessellation` | 0.56x |
| `subpasses` | 0.83x |
| `hdr` | 0.87x |
| `oit_linked_lists` | 1.06x |

Turnip is faster on five of six. Your mileage will differ by workload, and a
game running through a translation layer is usually limited by the CPU rather
than the driver, so do not expect these ratios in a game.

## Adreno 600 and 700 series: not tested, no guarantees

Turnip covers the 600, 700 and 800 series and my builds are no different, so
they should load on those parts. I cannot tell you it works, because **I do not own
one.** Nothing here has been run on a 6xx or 7xx device. Not the crash sweep,
not the frame times, not the fixes.

Concretely, on 600 and 700 series hardware:

- Every fix I ship was written for the hardware I have. On a different part it
  may be unnecessary, and it may not be harmless.
- A bug I fixed may never have happened there, and bugs I have never seen may
  be waiting.
- Nothing on this page was measured on that hardware, so none of the numbers
  apply to it.
- I cannot reproduce what you report, so a bug there may stay open.

Use it if you want to, but that is the honest state of it. If you do try it, a
[device report](../../issues/new/choose) is the most useful thing you can send
me, and it is how those parts get supported properly instead of by assumption.

It would be nice to have a 700 series device so I could test and fix that
generation rather than guess at it. I do not have one at the moment.

## Installing

1. Download `turnip_faux123_<version>.adpkg.zip` from [Releases](../../releases).
   Do not unzip it.
2. Import it in your app's driver manager:
   - **GameNative:** Settings, then Emulation, then Driver Manager, then import.
     The imported driver appears under **Graphics Driver Version**, not
     **Graphics Driver**.
   - **Winlator and forks:** the Turnip or graphics driver picker in container
     settings.
3. Select it, then restart the container or game.

The driver has to be loaded by an app that opts in. Android does not let you
replace the system Vulkan driver for arbitrary apps without root, so a normal
Play Store game cannot use this.

---

## What works and what does not

**Not exposed by Turnip on the Adreno 830**, so samples needing them refuse to start
with a clear message rather than failing:

`VK_EXT_mesh_shader`, ray tracing pipelines (ray *queries* do work),
`VK_EXT_descriptor_heap`, `VK_EXT_shader_object`, 16-bit storage arithmetic,
`VK_KHR_pipeline_binary`, fragment shader barycentric, shader quad control.

That list is a roadmap, not a defect list.

---

## Reporting a bug

Open an [issue](../../issues/new/choose). The bug report template asks for the
device, the driver version, the app, and a log, because without those I cannot
do anything with it.

The most useful thing you can attach is `adb logcat` output covering the moment
it went wrong. If the app dies, the lines beginning `F DEBUG` are the ones that
matter.

---

## Credits

- **Mesa and the freedreno team** wrote Turnip. This repository is a build of
  their work with a handful of patches on top. All the hard parts are theirs.
- **[bylaws](https://github.com/bylaws/libadrenotools)** for AdrenoTools, which
  is the only reason a custom Vulkan driver can be loaded without root.
- **[MrPurple666](https://github.com/MrPurple666/purple-turnip)** and
  **[whitebelyash](https://github.com/whitebelyash)** for the Adreno 8xx build
  and packaging work the community has been using, which is where I started.
- **Khronos** for Vulkan-Samples, which turned out to be a far better driver
  test bench than any game.

## License

Turnip is [MIT licensed](https://gitlab.freedesktop.org/mesa/mesa/-/blob/main/docs/license.rst),
and so are the patches in this repository. See [LICENSE](LICENSE).

This project is not affiliated with or endorsed by Qualcomm, Google, the Khronos
Group, or the Mesa project.
