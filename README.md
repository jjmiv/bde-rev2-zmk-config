# BDE Rev2 ZMK Config

## Instructions

1. You will need to [sign up for a GitHub account](https://github.com/signup), and [fork this repository](https://docs.github.com/en/get-started/quickstart/fork-a-repo#forking-a-repository) so you can edit it.
2. Navigate to the **Actions** tab and click the "I understand my workflows, go ahead and run them" button to enable builds.
   ![Actions tab with "I understand my workflows" button](https://i.imgur.com/B7cTAE6.png)
3. Edit the [config/bde_rev2.conf](config/bde_rev2.conf) to enable/disable features. Edit [config/bde_rev2.keymap](config/bde_rev2.keymap) to change the keymap. Lastly, make sure the [build.yaml](build.yaml) file has your board in the "boards" list.
4. After committing your changes, your firmware will begin compiling. Assuming there are no typos or other problems, it will eventually be [downloadable from the Actions tab](https://zmk.dev/docs/user-setup#installing-the-firmware).
5. [Flash the firmware](https://zmk.dev/docs/user-setup#flashing-uf2-files) that matches the board you're using, e.g. `bde_rev2-nice_nano_v2-zmk.uf2` for a nice!nano v2.

## Common Questions/Problems

### There was an error and the build didn't finish.

#### There is probably an error in your keymap.

Carefully double-check the last changes you made. ZMK syntax is very different from QMK syntax. Note that when editing keymaps, each ZMK code is generally preceded by a reference categorizing what that code does.

For example: the letter "Z" is categorized as a **k**ey **p**ress. To add the letter "Z" to a keymap, it must be written `&kp Z`.

A few more examples below:

| QMK | ZMK |
| --- | --- |
| `KC_A` | `&kp A` |
| `QK_REBOOT` | `&reset` |
| `LT(1, KC_SPACE)` | `&lt 1 SPACE` |

There are many, many more differences. Don't forget to [check the ZMK docs](https://zmk.dev/docs/features/keymaps).

### The OLED/RGB/encoder(s) don't work.

#### Double-check the features in the [config/bde_rev2.conf](config/bde_rev2.conf) file.

And double-check the Kconfig file in the build results to make sure what you've enabled is really enabled. As of mid-July 2022, [there are also some issues regarding default board/shield configurations taking precedence over user configurations](https://github.com/zmkfirmware/zmk/issues/1382).

#### The OLED can be a bit finicky.

- [There is a known issue with OLEDs not being re-initialized after power off](https://github.com/zmkfirmware/zmk/issues/674); please check that pull request for any updates and/or possible workarounds.
- If using a nice!nano v2 without a battery, its `RAW` pin does not always deliver consistent voltage. This can result in the OLED turning off immediately after boot, or failing to turn on. Unfortunately, the only guaranteed fix is to add a battery; occasionally it will work after a reset.

Try the following:

1. Wait at least one minute and press the physical reset button on the board once.

### After disconnecting the keyboard from Bluetooth, it won't reconnect.

#### Forget the Bluetooth connection on both the BDE and the device, then try re-pairing.

If you don't remember which profile on the BDE was used to connect to a specific device, this may mean cycling through each (`&bt BT_PRV`/`&bt BT_NXT`) and clearing them (`&bt BT_CLR`) one by one. [See ZMK's documentation for more info](https://zmk.dev/docs/behaviors/bluetooth#bluetooth-pairing-and-profiles).

As a last resort, try flashing the `settings_reset` firmware for your board; this will force-clear everything.

## Further Troubleshooting Resources

- [ZMK's troubleshooting page](https://zmk.dev/docs/troubleshooting)
- Stop by #keyboard-help in [MechWild's Discord](https://discord.gg/nfxHnsm)
- Ask the ZMK experts in [ZMK's Discord](https://zmk.dev/community/discord/invite)
