# LG Gram laptops on Linux
This repository contains information on how to configure your LG Gram if you plan on installing Linux, as a lot of things aren't perfect.

This guide is made for the LG Gram 17" 2023, but it should work on all other models with some minor tweaking. I used Fedora (41 and 42), but all should work on other distros too.

## Kernel configurations
As seen [here](https://www.kernel.org/doc/html/latest/admin-guide/laptops/lg-laptop.html), the newer versions of the Linux kernel support a lot of drivers for the laptop.

There are 2 ways of changing these settings: using `rules.d` (and therefore applying them at boot) or by changing them manually using the terminal.

> [!WARNING]
> I noticed that these settings didn't change on Ubuntu, so the only way to apply them was by using the `rules.d` and rebooting. Which is very annoying. This was in 2024, and things may have changed since then.

### `rules.d` config
Download the rules.d folder or by downloading the zip of this entire repo.

Then, Copy the rule.d in there by moving the folder:
```bash
sudo cp rules.d/ /etc/udev/rules.d/
```

Or move them manually in one way or another. You will need root permissions of course.
Then modify them by opening them up with nano.

### Change kernel parameters at runtime
To do that, change the values using echo or nano or whatever tool you prefer. The names are on the Linux kernel web page linked above.

For instance, here I want to change the battery from 100% to 80% maximum charge:
```bash
sudo echo 80 >> /sys/class/power_supply/BAT0/charge_control_end_threshold
```

On a side note, KDE (probably Cosmic and Gnome as well) allows for a 80% max charge setting, but I have no idea if it actually works or not.

## Speakers equalization
I've made a simple equalizer to make the speakers sound a little bit better (never tried them on Windows, but I'm aware there should be an app for it). Mind the speakers are only 2W powerful so they aren't going to be very loud and you might not hear anything if there is even minor amount of noise in the room.

Just download the EasyEffects app, and configure it by loading the equalizer preset, the download is in the repo, and import it. Then, go to the Pipewire section and enable it only for the internal speakers. If you hear popping from the speakers, lower the preamp.
