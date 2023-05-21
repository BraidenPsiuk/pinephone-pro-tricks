# pinephone-pro-tricks
Some simple commands to solve various issues with the PinePhone Pro.
Some tricks may also work on the OG PinePhone but have not been tested.
Tricks that no longer work and are need of an update will be marked with a red "❌" (x) symbol.
Need more tricks? See the list of links at the end of this file.

Enjoy!

---

### (ANY) Changing power warnings and shutdown threshold
- Issue
  - You may want to change what the phone consideres "low", "critical", and "immediate action" battery percentage thresholds. This is especially useful to prevent your battery from becoming "flat". If this happens, the battery can be extremely difficult to charge up again. This is more of a preventative fix, but it may prove to be very helpful if you don't always keep a close eye on your battery level.
- Fix
  - Edit your UPower configuration:
    ```shell
    sudo nano /etc/UPower/UPower.conf
    ```
  - Modify the values for **"PercentageLow"**, **"PercentageCritical"**, and **"PercentageAction"**. In my case I used 20, 10, and 6 respectively.
  - At the end of the file, change **"CriticalPowerAction"** to **"PowerOff"**

### (PHOSH) Disabling the touchscreen ( ❌ )
- Issue
  - It might be helpful to disable the touchscreen if you have a bad digitizer which causes unwanted swipe/tap events. Use this fix and then Plug in a keyboard and mouse or use SSH instead until you can get your screen fixed.
- Fix
  - If you are using SXMO this option exists already, simply toggle it from the menu. Otherwise, SSH in and run:
  - ```shell
    echo 'SUBSYSTEM=="usb", ATTRS{idVendor}=="0416", ATTRS{idProduct}=="0486", ATTR{authorized}="0"' | sudo tee /etc/udev/rules.d/100-touchscreen.rules > /dev/null && sudo udevadm control --reload-rules && sudo udevadm trigger
    ```

### (SXMO) Preventing 3 presses of volume-down from killing your open windows
- Issue
  - Sometimes, pressing volume-down just one or two times kills windows unintentially. It's like you've pressed the key three times, even though you know you didn't. (Don't worry, you're not going insane! Well maybe just a bit, since you've forced yourself to use SXMO...) This may be due to improper debouncing configuration (I'm not sure what a proper fix is for this). But this fix will prevent unwanted window kills.
- Fix
  - Edit your three-button-touchscreen inputhandler hook:
  - ```shell
    sudo nano /usr/share/sxmo/default_hooks/three_button_touchscreen/sxmo_hook_inputhandler.sh
    ```
  - Find the comment **"standard handling"**, and then look for the case **"voldown_three"**.
  - Comment out **"sxmo_killwindow.sh"**

---

## Get more tricks, tweaks, and scripts!
- [Mobian Tweaks](https://wiki.mobian-project.org/doku.php?id=tweaks) (mobian-project.org)
- [milky-sway's PinePhone Scripts](https://github.com/milky-sway/pinephone-scripts) (github.com)
- [Max1220’s PinePhone scripts and tweaks](https://forum.level1techs.com/t/max1220s-pinephone-scripts-and-tweaks/172293) (level1techs.com)
- [Dejvino's PinePhone scripts for Sway](https://github.com/Dejvino/pinephone-sway-poc/tree/master/usr/local/bin) (github.com)
- [JonathanTippy's (snek100's) PinePhone helper scripts](https://github.com/JonathanTippy/pinephone_helpers) (github.com)
