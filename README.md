This is a tweaked version of the 2.8.2 firmware for my Anycubic Kobra, which has been modified with a second belt-driven Z axis screw and had the stock extruder/hotend replaced with an Titan Aero (which is what the stock Anycubic one appears to be a clone of, anyway.)

**This specific code SHOULD NOT go on an unmodified Kobra!** The firmware's hotend max temperature has been raised higher than the stock firmware so you could damage your hotend with this firmware. Additionally, the max Z height has been reduced because of how my second Z screw mod works, so you are losing 40mm of height.

Standard disclaimer: **Any updating of your printer firmware with third party firmware could damage your printer or make it inoperable. Only do what you are comfortable with. Responsibility for your safety -- and that of your printer -- is yours alone.**

However, if anyone finds this searching for Kobra firmware modification examples, I hope it is helpful.

# Major Changes

* Hotend max temperature increased from 275 to 285 per E3D's Titan Aero Marlin instructions.
    * E3D recommends even updating it to 300 (temporarily only!) for hot tightening the nozzle, but I change nozzles too much and print at too low of temps to deal with that right now.
* Build volume Z height reduced to accomodate DIY dual Z screw mod.
* The **factory default** X/Y stepper current has been increased from 600 to 800 mA, which is what my printer shipped with in EEPROM settings. I have been printing with successfuly for a couple months like this, motors have not burned up.
	* NB: This may be something that Anycubic changed intentionally between 2.7.9 and 2.8.2; it bears investigating. 

# Minor changes

* Various version strings updated to distinguish from stock firmware
* EEPROM "factory" defaults were changed to match what I have been using, so I have a clean record of my major settings changes in git history.
	* E-steps 
	* X/Y jerk set to 10
		* NB: Now that I am in the code, I see that the firmware's factory jerk setting on 2.8.2 is set to 5 but mine shipped with it set to 20 in the EEPROM, which is definitely too much. May need to compare vs the older 2.7.9 firmware to see if this was an intentional change from Anycubic, and if so, see if 5 works even better.
	* Default acceleration for print moves set to 4000 (i.e., by default it is capped by max accel values for the various steppers.) This improves the default effective X-axis acceleration from 600 to 700 mm/s^2. 
		* There was a similar discrepancy between my as-shipped EEPROM and 2.8.2 defaults on the travel default acceleration, but because the values in 2.8.2 were still greater than any of the individual axes' max accels, I did not adjust it.
    * PID coefficients for the hotend heater updated to results of my auto tuning.
    * Aforementioned X/Y stepper current change.

# Build instructions
See https://www.reddit.com/r/anycubic/comments/y2waxu/tutorial_how_to_build_anycubic_marlin_source_code/ for detailed build instructions.

The TL;DR toolchain is:
* Win10 x64 development machine
* Keil MDK v5.36, available from Keil
* ARM::CMSIS 5.7.0 pack (**UNINSTALL** 5.8.0), installed via the Keil pack manager
* `HDSC.HC32F460.1.0.7.pack` from the chip manufacturer, XHSC.
	* Go to http://www.xhsc.com.cn/Productlist/info.aspx?itemid=1853&parent then click the 3rd tab beneath "HC32F460PETB-LQFP100. Download 
HC32F460_IDE_Rev1.0.7.zip. The pack file is inside, it can be imported in the Keil pack manager.

Once that environment is set up:
* Open `workspace/anycubic.uvprojx` in Keil uVision
* Project -> Build Target
* Resulting firmware file ends up in `workspace/firmware.bin`

# Installing firmware
* Format your SD card (FAT32) and place the resulting firmware.bin in the root, as the only file on the card.
* Power off your printer
* Insert SD card
* Turn on printer
* The printer will stall while booting on the Anycubic logo. Let it wait for a bit. It will then beep five times and then go into the updated firmware's menu.

# Troubleshooting

### When trying to install firmware, it hangs on the Anycubic logo, beeps five times, but never proceeds to the menu, even after 5 or 10 minutes.

When this happened to me, it was because I still had the USB cable connecting it to my Octopi that was probably trying to talk to it over serial and confusing it. After removing the cable and trying the process again, it flashed normally and works fine with no apparent problems.

# Special thanks
Thank you to /u/jojos38 on Reddit who provided the build instructions to allow me to do this modification.
