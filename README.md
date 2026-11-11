# Go60 configuration with RawTouch

[![Build](https://github.com/kalakris/go60-rawtouch-config/actions/workflows/build.yml/badge.svg?branch=main)](https://github.com/kalakris/go60-rawtouch-config/actions/workflows/build.yml)
[![License: MIT](https://img.shields.io/github/license/kalakris/go60-rawtouch-config)](LICENSE)

<img src="https://raw.githubusercontent.com/kalakris/rawtouch/main/docs/images/demo.gif" align="right" width="200" alt="A finger on a Go60's built-in trackpad scrolls a web page through RawTouch: the page follows the finger, coasts after a flick, and stops when the finger touches down again">

Firmware for the MoErgo Go60 that works with
[RawTouch](https://github.com/kalakris/rawtouch), a macOS app that makes
the keyboard's trackpad scroll the way a Mac trackpad does: content
follows the finger on both axes, coasts after a flick, and stops when
you touch down again.

Pad roles stay as on stock firmware: the left pad scrolls, the right pad
points. Without the app, the keyboard points and wheel-scrolls by itself
(Standard mode). While the app runs, it turns the left pad's touches
into trackpad-style scrolling (RawTouch mode); quitting it returns the
keyboard to Standard mode.

**What you need:** a Go60, a USB-C cable, and a Mac running macOS 13 or
later for the app. Standard-mode scrolling works on any computer. Paths
B and C below also need a free GitHub account. Not a Go60? See
[Which repository?](#which-repository)

## Choose your path

| You want | Do |
|---|---|
| To try it with MoErgo's stock keymap | [A. Prebuilt firmware](#a-prebuilt-firmware) |
| To keep the keymap you made in MoErgo's Layout Editor | [B. Your Layout Editor keymap](#b-your-layout-editor-keymap) |
| To edit the keymap file yourself | [C. Edit the keymap yourself](#c-edit-the-keymap-yourself) |

Every path ends with the same two files, one per half:

| File | Flash to | Role |
|---|---|---|
| `go60_lh-zmk.uf2` | **Left** half | Split central: connects to the computer over USB and Bluetooth |
| `go60_rh-zmk.uf2` | **Right** half | Split peripheral |

### A. Prebuilt firmware

1. Download `go60_lh-zmk.uf2` and `go60_rh-zmk.uf2` from the
   [latest release](https://github.com/kalakris/go60-rawtouch-config/releases/latest)'s
   assets.
2. [Flash](#flash-the-firmware) both halves and continue from there.

This puts MoErgo's stock keymap on the keyboard, in place of whatever
keymap it has now.

### B. Your Layout Editor keymap

MoErgo's [Layout Editor](https://my.moergo.com/go60/) cannot build
firmware with the RawTouch module, but a layout made in it can be built
here, entirely on GitHub's website:

1. **Export your layout.** In the Layout Editor, open it and choose
   **Export**, then **Download** under **.keymap ZMK Format**.
2. **Fork this repository** on GitHub (the **Fork** button at the top of
   its page). Then open your fork's **Actions** tab and click the button
   that enables workflows: GitHub disables them on a fresh fork.
3. **Replace the keymap.** In your fork, open `config/go60.keymap`, click
   the pencil icon, delete everything in the file, paste in the contents
   of the file you exported, and click **Commit changes**.
4. **Get the firmware.** The commit starts a build. In the **Actions**
   tab, open the **Build** run, wait for it to finish, then download the
   `go60-rawtouch-firmware` artifact from the run's summary page and
   unzip it.
5. [Flash](#flash-the-firmware) both halves and continue from there.

Keys, layers, combos, macros and custom behaviours come from your
layout. Its **trackpad settings do not**: pointer and scroll speeds,
tap-to-click on or off, a layer that turns on while the pointer moves,
and scroll direction all start out as on stock firmware. To set them
again, see
[Trackpad settings from a Layout Editor layout](#trackpad-settings-from-a-layout-editor-layout).

Settings the Layout Editor keeps outside the keymap are not in the
exported file either; if you set any, add them to `config/go60.conf`.

After a later change in the Layout Editor, repeat steps 1 and 3 to 5.

### C. Edit the keymap yourself

1. Fork this repository and enable workflows, as in B, step 2.
2. Edit `config/go60.keymap` as you would on stock ZMK firmware; both
   halves build from it. Every push builds, and **Run workflow** on the
   **Build** workflow builds on demand.
3. Download the artifact as in B, step 4, and
   [flash](#flash-the-firmware) both halves.

Pushing a tag that starts with `v` (for example `v1`) to your fork also
publishes both files as a release of the fork.

## Flash the firmware

Flash **both halves from the same build**: the right half's firmware
changes what it sends to the left, and the left half's changes how it
reads it. Flash one half at a time, with the cable plugged into the half
you are flashing.

1. **Connect the right half** to the computer with a USB-C cable.
2. **Put it into its bootloader.** Switch the half off. Hold its **T3**
   and **C3R3** keys and switch it back on, still holding them. T3 is
   the thumb key nearest the middle of the keyboard; C3R3 is the
   home-row key in the third column from the inside edge (Alt + K on
   the stock layout). These are positions, so they are the same on any
   keymap. The light next to the power switch pulses red slowly.
3. **Copy the file.** A drive named `GO60RHBOOT` appears. Copy
   `go60_rh-zmk.uf2` onto it. When the copy finishes, the drive
   disappears and the half restarts with the new firmware.
4. **Flash the left half the same way**, with Ctrl + D (T3 + C3R3 on the
   left half) at power-on. The drive is `GO60LHBOOT` and the file
   `go60_lh-zmk.uf2`.

**Do not hold T3 + C3R2** (Ctrl + E or Alt + I, one row up) at
power-on. That is MoErgo's configuration factory reset, not the
bootloader: it erases the half's Bluetooth pairings and settings. Its
one use here is under [First connection](#first-connection).

While the stock keymap is running, **Magic + Tab** (left half) and
**Magic + `\`** (right half) also enter the bootloader. Magic is the
left end of the Z row. This needs both halves running and connected to
each other; the power-on method always works. To leave the bootloader
without flashing, switch the half off and on. MoErgo's
[Go60 user guide](https://docs.moergo.com/go60-user-guide/customizing-key-layout/)
describes both methods.

## First connection

The key combinations in this section are the stock keymap's, where Magic
is the left end of the Z row. On your own keymap, use the keys you bound
to the same functions.

**USB:** plug in the **left** half, the split central. Keys, both pads,
and taps should work immediately.

**Bluetooth:** the firmware adds a second HID service, so every host
that was paired with other firmware holds a stale record. For each
profile you use:

1. Forget the keyboard on the host.
2. On the keyboard, select the profile (Magic + X, C, V or B selects
   profile 0 to 3), then clear it: hold Magic and press `=` (the left end
   of the number row).
3. Pair again.

Selecting a profile also switches the keyboard's output to Bluetooth, so
it keeps typing over Bluetooth even while the left half is plugged in.
To switch back to USB, hold Magic and press `` ` `` (the left end of the
left half's bottom row).

If keys work after pairing but a pad does not, open the Mac's Bluetooth
settings, disconnect the keyboard and connect it again. A future
firmware that changes the HID reports needs the same forget-and-pair;
the module's release notes say when.

**Without the cable between the halves:** if the right half does nothing
once the cable is out, the halves are not paired with each other. Do
MoErgo's
[configuration factory reset and re-pairing](https://docs.moergo.com/go60-user-guide/troubleshooting/)
on both halves, then pair the computer again. Switch both halves off
before plugging or unplugging that cable.

## Check Standard mode before installing RawTouch

With no RawTouch app running, confirm:

- Typing on both halves.
- Right pad: pointer movement; tap left-clicks; faster pointer on the
  SymbolNav layer. On the stock keymap, hold the left half's outermost
  thumb key for SymbolNav, or double-tap it to stay there and press it
  again to leave.
- Left pad: vertical and horizontal wheel scrolling; tap right-clicks.
- The same over Bluetooth if you use it.

If something here fails, the cause is in the firmware or its
configuration, not the app; see [Troubleshooting](#troubleshooting). The
[hardware checklist](docs/hardware-checklist.md) has the full list,
including the RawTouch-mode and split-link checks.

## Install RawTouch

Follow RawTouch's [Get started](https://github.com/kalakris/rawtouch#get-started)
from step 2: install the app, grant Accessibility, and drag on the left
pad. Quit the app and the left pad's wheel scrolling returns.

## What changes from stock

| | Stock Go60 | This configuration |
|---|---|---|
| Keymap and layers | MoErgo's default | Unchanged; `config/go60.keymap` is byte-identical and included by a keymap per half |
| Left pad | Scrolls; tap right-clicks | Scrolls on both axes in both modes; in Standard mode at stock's rate and direction (measured). Tap right-clicks; a touch that stops RawTouch's coasting does not click. |
| Left pad on the Factory layer | Points | Points at stock's speed (scaler set from a measured ratio); tap left-clicks |
| Right pad | Points; tap left-clicks; faster on SymbolNav (layer 2) | Same, at stock's speeds (scalers set from a measured ratio) |
| Taps | Detected by the pads | Detected in firmware by the module |
| Pad driver | Pete Johanson's Cirque driver, relative mode | Zephyr's Pinnacle driver in absolute mode ([`cirque-input-module`](https://github.com/kalakris/cirque-input-module), `config/west.yml`) |
| USB | One HID interface | Two: the second carries touch samples for RawTouch |
| Bluetooth | One HID service | Two, for the same reason. Hosts must pair again after the first flash |
| Split roles | Left half central | Unchanged: plug the **left** half into USB |
| Wired split link | Polled about every 22.5 ms | About every 5 ms (see [Tuning notes](#tuning-notes)) |

No new layer or key binding is needed.

## Trackpad settings

Pad behaviour (pointer speed, scroll direction, taps) lives in
`config/raw-touch-central.dtsi`, not in the keymap. The
`&cirque_rh_listener` and `&cirque_lh_listener` blocks in `go60.keymap`
have no effect: `raw-touch-central.dtsi` is included after them and
replaces both listeners, so make pad changes there.

Two of its blocks apply on one layer each, by the layer's name in the
keymap: the right pad's faster pointer on `LAYER_SymbolNav`, and the
left pad pointing on `LAYER_Factory`. A keymap without a layer of that
name builds without that block. To have one apply on another layer, put
that layer's name in the block's `#ifdef` and `layers = <...>` lines;
a Layout Editor export names each layer `LAYER_` followed by the name
you gave it.

### Trackpad settings from a Layout Editor layout

A layout's trackpad settings are not carried over; the pads behave as
described under [What changes from stock](#what-changes-from-stock)
until you edit `config/raw-touch-central.dtsi`. The settings people most
often set again:

- **A layer that turns on while the pointer moves:** add
  `<&zip_temp_layer LAYER_Mouse 200>,` to the right pad's
  `input-processors`, on the line before its idle filter, with your
  layer's name and timeout.
- **No tap-to-click:** delete `tap-click;` from that pad's node, and for
  the left pad `tap-click-while-scrolling;` as well.
- **Scroll direction without RawTouch:** the left pad scrolls as on
  stock firmware. If your layout inverted it, add
  `<&zip_scroll_transform INPUT_TRANSFORM_Y_INVERT>,` after
  `<&zip_xy_to_scroll_mapper>,` in the left pad's `input-processors`.
  RawTouch's own direction is set in the app and is not affected.
- **Speeds:** see the tuning notes below.

### Tuning notes

- **Pointer speed** is set by the `zip_xy_scaler` values in
  `config/raw-touch-central.dtsi`. Motion derived from absolute
  positions is about 4.2 times stock's relative-mode motion (measured on
  a Go60), so each stock ratio is multiplied by 5/21 to match stock's
  speed: `5 7` where stock has `3 1` (right pad, left pad on the Factory
  layer) and `15 7` where stock has `9 1` (right pad on SymbolNav).
- **Standard-mode scroll speed** is set by the left pad's
  `zip_scroll_scaler 1 34` in the same file: a full-height swipe gives
  the same wheel count as stock (measured on a Go60). A larger second
  number scrolls slower. RawTouch mode does not use it.
- **Wired split polling** runs about every 5 ms instead of stock's
  22.5 ms, set by the two `CONFIG_ZMK_SPLIT_WIRED_HALF_DUPLEX_*` lines in
  `config/go60_lh.conf`. Over the cable, the right half's keys and pad
  arrive sooner and without bursts. More frequent polling may cost
  battery on both halves (not measured); delete those two lines for
  stock timing. It makes no difference over a wireless split.
- **Standard-mode scroll direction** matches stock: the left pad's
  wheel chain in `config/raw-touch-central.dtsi` has no transform, like
  the stock chain. For the opposite vertical direction, add
  `<&zip_scroll_transform INPUT_TRANSFORM_Y_INVERT>` after
  `<&zip_xy_to_scroll_mapper>`. RawTouch-mode direction is set in the
  app, separately.
- **Pad sensitivity** keeps the board's stock `1x` on both pads. If the
  pointer jitters under a sustained touch, raise it for that pad only by
  appending this after the includes in `config/go60_rh.keymap` (right
  pad) or `config/go60_lh.keymap` (left pad):

  ```dts
  &glidepoint {
      sensitivity = "2x";
  };
  ```

  Editing the shared `config/raw-touch-glidepoint.dtsi` would change both
  pads. Rebuild and flash that half, then check light touches as well as
  jitter.

## Going back to stock firmware

This firmware is built from the keymap in this repository. A layout you
made in MoErgo's Layout Editor is not carried over by itself; to keep
it, see [B. Your Layout Editor keymap](#b-your-layout-editor-keymap).

To return to MoErgo's firmware, build it in the
[Layout Editor](https://my.moergo.com/go60/) (its default layout is the
stock keymap) and [flash](#flash-the-firmware) both halves the same way.
Then forget and pair Bluetooth hosts again, since the HID services
change back. RawTouch can stay installed; it ignores a keyboard without
the module.

## Troubleshooting

| Symptom | Check |
|---|---|
| The Actions build fails | Open the failed step's log. Common causes: a typo in a keymap edit; custom devicetree in a Layout Editor layout that sets properties on the trackpad (`&glidepoint`), which belong to the stock driver; a `revision` in `config/west.yml` that does not exist; `CONFIG_USB_HID_DEVICE_COUNT` set below 2. A build of an unmodified fork should pass; if it does not, open an issue with the log. |
| No `GO60RHBOOT` or `GO60LHBOOT` drive appears | Use a cable that carries data, plug it into the half you are flashing, and use the power-on method. |
| Keys on the right half do nothing | Flash both halves from the same build. If they still do not talk to each other, which happens when they were last paired with each other in other roles, follow MoErgo's [configuration factory reset and re-pairing](https://docs.moergo.com/go60-user-guide/troubleshooting/) for both halves; it also clears Bluetooth pairings. |
| A pad does not respond, while keys work | Switch that half off and on. Rarely, a pad does not respond after power-on. If it stays dead in Standard mode, the cause is in the firmware or hardware, not the app. |
| Scrolling works over USB but not Bluetooth | Do the [Bluetooth forget-and-pair](#first-connection) for that profile, including clearing it on the keyboard. |
| RawTouch shows no keyboard | Check that both halves run this firmware and the left half is connected; then see RawTouch's [troubleshooting](https://github.com/kalakris/rawtouch#troubleshooting). |
| RawTouch lists the keyboard but the left pad scrolls in steps | RawTouch needs Accessibility permission, and **Use RawTouch Scrolling** must be on in its menu. |

## Which repository?

| You have | Start here |
|---|---|
| A MoErgo Go60 | [go60-rawtouch-config](https://github.com/kalakris/go60-rawtouch-config) (this repository): prebuilt firmware, or a configuration to fork |
| Another ZMK keyboard with a Cirque Pinnacle trackpad | [zmk-raw-touch](https://github.com/kalakris/zmk-raw-touch): the firmware module and its setup guide |
| Firmware with the module, and a Mac | [RawTouch](https://github.com/kalakris/rawtouch): the macOS app |
| A question about the trackpad driver | [cirque-input-module](https://github.com/kalakris/cirque-input-module): the absolute-mode Pinnacle driver the module is tested with |

## Adopting into an existing configuration

The integration is five small commits on top of MoErgo's template, one
concern each. Cherry-pick them all, or read them; they are small enough
to apply by hand. The series builds only once commits 1, 3 and 4 are
all applied.

| Order | Commit | Subject | Files |
|---|---|---|---|
| 1 | `963d41e` | `west: pin MoErgo ZMK v26.09, add the RawTouch module and Cirque driver` | `config/west.yml` |
| 2 | `222453b` | `build: pin the ZMK build workflow, name the artifact, release on tags` | `.github/workflows/build.yml` |
| 3 | `0f3a30a` | `config: Kconfig for the RawTouch module on both halves` | `config/go60.conf`, `config/go60_lh.conf`, `config/go60_rh.conf` |
| 4 | `3064c64` | `keymap: RawTouch pad configuration on top of the stock keymap` | `config/go60_lh.keymap`, `config/go60_rh.keymap`, `config/raw-touch-*.dtsi` |
| 5 | `e59f906` | `config: faster wired-split polling on the central` | `config/go60_lh.conf` |

From your configuration repository:

```sh
git remote add rawtouch-config https://github.com/kalakris/go60-rawtouch-config.git
git fetch rawtouch-config
git cherry-pick 963d41e 222453b 0f3a30a 3064c64 e59f906
```

Commit 2 is recommended: ZMK's `main` build workflow targets newer
trees, so it pins the workflow to `v0.3`, names the artifact, and
publishes a release with both firmware files when you push a `v*` tag.
Commit 5 is optional: it speeds up wired-split polling (see
[Tuning notes](#tuning-notes)). In a customized configuration, expect
conflicts and adapt:

- **`config/west.yml`:** merge the `kalakris` remote and the two
  projects into your lists rather than replacing the file. The driver
  project must keep the name `cirque-input-module`, so that it replaces
  the one MoErgo's ZMK imports. The module is tested against MoErgo's
  `v26.09` (ZMK 0.3 / Zephyr 3.5); a configuration on another MoErgo
  revision or another ZMK needs its own build check.
- **`.conf` files:** append the settings if the files already exist.
  Do not set `CONFIG_USB_HID_DEVICE_COUNT` below 2.
- **Keymaps:** the per-half keymaps here include the stock `go60.keymap`
  and then the RawTouch configuration. If you already have per-half
  keymaps, keep your own bindings and includes. Add
  `#include "raw-touch-central.dtsi"` after them in `go60_lh.keymap`, and
  `#include "raw-touch-peripheral.dtsi"` in `go60_rh.keymap`. The
  central-side file assumes the stock listener layout (left pad local
  and scrolling, right pad relayed through `cirque_split` and
  pointing); its per-layer blocks apply only if your keymap defines
  `LAYER_SymbolNav` or `LAYER_Factory`. It replaces the
  `&cirque_rh_listener` and `&cirque_lh_listener` nodes, with anything
  your keymap set on them, so carry any pad changes you made there into
  it. If your listener chains differ, rebuild them on
  the module's rules: scroll marker first in any scrolling chain, one
  idle-filter instance per listener placed last, and no processor that
  consumes `INPUT_BTN_0` (such as a `zip_button_behaviors` mapping it
  to `&none`), which would swallow firmware taps. The module README's
  [setup](https://github.com/kalakris/zmk-raw-touch#setup) and
  [split](https://github.com/kalakris/zmk-raw-touch#split-keyboards)
  sections walk through each piece.
- **Swapped split roles:** if your right half is the central, every
  per-half file swaps sides. `raw-touch-central.dtsi` and the
  central-only settings in `go60_lh.conf` (`CONFIG_ZMK_RAW_TOUCH`,
  `CONFIG_USB_HID_DEVICE_COUNT` and the wired-split polling) go to the
  right half, with the right pad local and the left pad relayed.
  `raw-touch-peripheral.dtsi` and the Bluetooth transmit buffers in
  `go60_rh.conf` go to the left half. The module README's
  [split section](https://github.com/kalakris/zmk-raw-touch#split-keyboards)
  explains what each side needs.

## Dependency versions

| Dependency | What it is | Where |
|---|---|---|
| `moergo-sc/zmk` | MoErgo's ZMK with the Go60 board: release `v26.09` (ZMK 0.3 / Zephyr 3.5) | `config/west.yml` |
| `kalakris/zmk-raw-touch` | The RawTouch firmware module, at a numbered release | `config/west.yml` |
| `kalakris/cirque-input-module` | The absolute-mode Cirque driver, at the release the module's release notes name | `config/west.yml` |
| ZMK build workflow | `v0.3`, matching the ZMK tree | `.github/workflows/build.yml` |
| RawTouch app | Its [latest release](https://github.com/kalakris/rawtouch/releases/latest) | Not part of the firmware build |

The module and the driver each publish numbered release tags and a
moving `stable` tag that points at their newest release. In
`config/west.yml`, choose one:

- **Follow releases:** set both the module's and the driver's revision
  to `stable`. GitHub Actions fetches the newest release on every build.
- **Pin a release:** take a module tag from its
  [releases](https://github.com/kalakris/zmk-raw-touch/releases) and the
  driver tag that release's notes name, and set both. The build stays
  reproducible and changes only when you edit the tags.

Avoid `main`, which carries unreleased work. When a build picks up a new
release, flash both halves and run
[the hardware checklist](docs/hardware-checklist.md), and check the
release notes for HID report changes that need Bluetooth hosts to forget
and pair the keyboard again.

## License

The RawTouch additions are MIT-licensed (see [LICENSE](LICENSE)). Files
that come from MoErgo's Go60 template, whose history this repository
keeps, remain MoErgo's under the template's own terms; the stock keymap,
`config/go60.keymap`, carries The ZMK Contributors' MIT notice.
