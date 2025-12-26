# LG Gram laptops on Linux
This repository contains information on how to configure your LG Gram if you plan on installing Linux, as a lot of things aren't perfect.

This guide is made for the LG Gram 17" 2023, but it should work on all other models with some minor tweaking. I used Fedora (41 and 42), but all should work on other distros too.

# Download the configurations
Download the configurations from the release page, and unzip the file. You can then modify all the files to your liking, and move them to the folders mentioned below (you need root).

## Kernel configurations
As seen [here](https://www.kernel.org/doc/html/latest/admin-guide/laptops/lg-laptop.html), the newer versions of the Linux kernel support a lot of drivers for the laptop.

There are 2 ways of changing these settings: using `rules.d` (and therefore applying them at boot) or by changing them manually using the terminal.

> [!WARNING]
> I noticed that these settings didn't change on Ubuntu, so the only way to apply them was by using the `rules.d` and rebooting. Which is very annoying. This was in 2024, and things may have changed since then.

### `rules.d` config
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

## Performance fixes
The LG Gram laptops, being very lightweight, suffer from having an insanely small heatsink and very restrictive thermal configuration. By default, whenever the CPU goes above 70°C (even on Windows) it thermal throttles and this means incredibly limited performance.

To give you an idea, the laptop could potentially use upwards of 30-35W of power between CPU and iGPU. But when the limiters are in place, it drops all the way down to 12W which makes it incredibly slow even for mundane tasks. It took me months of working on it for hours a day to figure out how to fix it. Insane.

Thankfully it's possible to bypass this by using a custom `thermald` configuration. I already made one that works, and you can just drag and drop it in the `/etc/thermald/` folder.

No your laptop will not explode, Intel has set a maximum operating temperature of 95°C that cannot be exceeded or changed even on the BIOS (it resets at boot). So your laptop will be safe, it will just operate better.

## Speakers equalization
I've made a simple equalizer to make the speakers sound a little bit better (never tried them on Windows, but I'm aware there should be an app for it). Mind the speakers are only 2W powerful so they aren't going to be very loud and you might not hear anything if there is even minor amount of noise in the room.

Just download the EasyEffects app, and configure it by loading the equalizer preset, the download is in the repo, and import it. Then, go to the Pipewire section and enable it only for the internal speakers. If you hear popping from the speakers, lower the preamp.

## PageUp/Home and PageDown/End keys are the other way around
They are inverted, and you gotta hold the FN key to use them, which gets in the way when programming. If you want to fix it, I recommend you install an app called Input Remapper on your distro.

## Other oddities
### BIOS settings
You can get into the Advanced section by holding `CTRL+ALT+SHIFT+F7`.

The BIOS options, when holding the arrow keys down, scroll much slower than on Windows. I have no idea how this is even possible.

Also, there's millions of options but only a couple of them stick when rebooting? Most of them just don't, and sometimes they reset for no reason (the Performance tab for Power profiles is one of them. Also it doesn't work, it limits at 25W even when you push it higher).

### Performance
Other oddities are what I explained in the performance tab. To give more context, games like Genshin Impact (yeah I was playing that at the time don't judge) would drop from 40ish FPS to like... 12? Even lower sometimes. All because of this ridiculously low temperature setting. I've never seen any other computer with such low temperature thresholds. That's just insanely low considering that CPUs can hit even more than 100°C before throttling nowadays.
