# Hardware checklist

Run after flashing both halves from the same build. Standard mode means
no RawTouch app running (quit it, or use another computer); RawTouch mode
means the app is running with Accessibility granted and the menu shows
the keyboard as connected.

## Standard mode

1. **Keys**: type on both halves; the Magic, Keypad and SymbolNav layers
   work as on stock firmware.
2. **Right pad, pointer**: the pointer follows the finger in every
   direction, at a speed comparable with stock. Hold or toggle layer 2
   (SymbolNav): the pointer moves faster.
3. **Right pad, tap**: a quick tap left-clicks; a slow press or a drag
   does not.
4. **Left pad, scroll**: a vertical drag scrolls vertically, a horizontal
   drag horizontally, in a view that scrolls both ways, in the same
   directions as stock firmware on the same computer.
5. **Left pad, tap**: a quick tap right-clicks; a drag does not.
6. **Left pad on the Factory layer**: activate the Factory layer (hold
   the Magic key, press the right end of the Z row); the left pad moves
   the pointer and a tap left-clicks. The stock Factory layer has no
   exit key: power-cycle the left half to leave it.
7. **Both transports**: repeat 1 to 6 over USB (left half plugged in;
   after selecting a Bluetooth profile, switch output back to USB with
   Magic + `` ` ``) and over Bluetooth after the
   [forget-and-pair](../README.md#first-connection).

## RawTouch mode

8. **Discovery**: the RawTouch menu lists the keyboard; **Settings →
   Keyboards** (menu bar icon → Settings…) shows the firmware version for
   the keyboard and, after a touch on the right pad, for the right half
   too.
9. **Scrolling**: a drag on the left pad scrolls both axes and
   diagonals with the finger; lift during a flick to coast; touch the
   pad again to stop the coast. A quick tap still right-clicks; a dab
   that stops a coast must not.
10. **Pointer unchanged**: the right pad still points and taps exactly
    as in Standard mode while the app runs.
11. **Return to Standard mode**: quit the app; wheel scrolling on the
    left pad resumes at once. Force-quit it instead: scrolling resumes
    within 30 seconds.
12. **Both transports**: repeat 8 to 11 over USB and over Bluetooth,
    switching output as in item 7.
    Unplug USB mid-drag with Bluetooth paired: the gesture ends cleanly
    and the next drag scrolls over Bluetooth.

## Split link

13. **Wired**: with the halves cabled, right-pad pointer motion is
    smooth, with no periodic hitching.
14. **Wireless (if used)**: switch both halves off, unplug the cable
    between them, and switch them on; repeat 2, 3 and 8 to 10. Switch
    both off again before plugging the cable back in. If the right half
    stays dead without the cable, the halves are not paired with each
    other in these roles, which happens when they were last paired with
    the right half as the central: do MoErgo's configuration factory
    reset on both halves, then pair the computer again.

## Verified on hardware

In November 2026, on one Go60 with MoErgo ZMK v26.09, module 0.1 and
driver 0.1.0:

- **With a keymap exported from MoErgo's Layout Editor:** over USB and
  over Bluetooth, with the halves cabled and with the wireless split:
  both modes, taps, pointing, the return to Standard mode when RawTouch
  quits, and the SymbolNav and Factory pad behaviour (items 2 and 6).
- **With the stock keymap:** over USB: both modes, taps, the return to
  Standard mode, sleep and wake with and without a finger on a pad, and
  power-cycling the halves.
- **Standard-mode speeds**, measured against MoErgo's stock firmware in
  raw HID counts per full-height swipe: wheel 90 against 91, in the same
  direction; pointer 2,276 against 2,281.

Reports from other Go60s are welcome.
