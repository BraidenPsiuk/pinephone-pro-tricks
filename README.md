# pinephone-pro-tricks
Some simple commands to solve various issues with the PinePhone Pro.
Some tricks may also work on the OG PinePhone but have not been tested.
Tricks that no longer work and are need of an update will be marked with a red X (❌).

Enjoy!

---

### Changing power warnings and shutdown threshold
- Issue
  - You may want to change what the phone consideres "low", "critical", and "immediate" battery percentage thresholds.
- Fix
    Edit your UPower configuration
  - ```shell
    sudo nano /etc/UPower/UPower.conf
    ```
  - Modify the values for **"PercentageLow"**, **"PercentageCritical"**, and **"PercentageAction"**. In my case I used 20, 10, and 6 respectively.
  - At the end of the file, change **"CriticalPowerAction"** to **"PowerOff"**

### Disabling the touchscreen (❌)
- Issue
  - It might be helpful to disable the touchscreen if you have a bad digitizer which causes unwanted swipe/tap events. Use this fix and then Plug in a keyboard and mouse or use SSH instead until you can get your screen fixed.
- Fix
    If you are using SXMO this option exists already, simply toggle it from the menu. Otherwise, SSH in and run:
  - ```shell
    echo 'SUBSYSTEM=="usb", ATTRS{idVendor}=="0416", ATTRS{idProduct}=="0486", ATTR{authorized}="0"' | sudo tee /etc/udev/rules.d/100-touchscreen.rules > /dev/null && sudo udevadm control --reload-rules && sudo udevadm trigger
    ```
