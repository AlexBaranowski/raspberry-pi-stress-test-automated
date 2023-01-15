# Automated stress tests for Raspberry Pi

### **Overclocking can destroy your RPi continue on your own risk**.

This repository contains simple scripts that run stressbery (on different
overclocking parameters) and systemd unit file that starts that script. 


## Installation

```
git clone https://github.com/AlexBaranowski/raspberry-pi-stress-test-automated.git
cd raspberry-pi-stress-test-automated
# HERE YOU CAN SETUP the frequencies
# $EDITOR raspberry-stressbery
sudo cp raspberry-stressbery /sbin/
chmod 755 /sbin/raspberry-stressbery
cp raspberry-stressbery.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable raspberry-stressbery
reboot
```

## Workflow

1. The first test is run on your current setup
2. The tests on frequencies and over_voltage defined in the `frequencies_voltages` are run
3. After all defined tests runs the script will:
   - Remove arm_freq and over_voltage from the boot config
   - Disable raspberry-stressberry service
   - Remove the `/var/stressberry-run-number`
   - Reboot

After all test You should get clean state.

## Files produced

- `/var/stressberry-run-number` that is used to keep track of the run number
  ;). It should be removed after successful run.
- `/var/stressberry-run-FREQ` file produced by stressberry-run where FREQ is CPU frequency


## Hidden dependencies

The scripts requires:

1. `/usr/local/bin/stressberry-run` must be valid path for stressberry-run
2. CPU gov must be set to `performance`, as `on-demand` will produce
   missleading output file (name depends on frequency reported by the cpu0 before
   tests starts)
3. If you want to overclock beyond `over_volt` >= 8 you must add additonal config
   to the `/boot/config.txt`. Note that this is might destroy/fry/brick your
   Raspberry Pi board. I won't provide these instructions here.
4. If stress-ng is installed, and /usr/bin/stress is unavailable, the softlink `ln -s /usr/bin/stress-ng
   /usr/bin/stress` must be created.
