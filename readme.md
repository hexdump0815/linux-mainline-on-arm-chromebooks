# running linux mainline on arm chromebooks

## intro

this is about running a real mainline linux system on arm chromebooks, so no chromeos and this is not about intel chromebooks - for those please have a look at the intel related links at the bottom of this page, those should help you getting a real linux running on them.

there are most probably three (maybe even more) reasons why one wants to run a real mainline linux system on arm chromebooks:
- chromebooks will usually be supported by chromeos only for a certain time (usually between 6 to 8 years) and afterwards one will not get any new features and also no fixes for security problems, so it is not a really good idea to use them this way any longer afterwards. on the other hand they are then usually still quite useable hardware and still can be used instead of throwing them into the trash (sustainability etc.). most of the 32bit armv7 chromebooks have reached this stage meanwhile.
- even for chromebooks which are still supported by chromeos there might be a reason to use them with a real mainline linux system instead or in parallel: in case one wants to say good bye to googles interest to track its users and to collect a lot of information on how they are using their chromebook etc., which is unavoidable if used with chromeos (even after adjusting the privacy settings etc. there will still be a lof of informatoion tracked and collected).
- the third reason why this might be interesting is if one wants to run linux on a laptop with an arm processor and not an intel one. there are only a few laptops easily available with arm cpus which can be used with linux: the pinebook pro comes into mind or maybe some of those windows laptops with arm cpus - the first (while being a very nice option in general with a very vivid community around it) is quite limited performance wise (old rk3399 soc), does not have the best build quality (strange noise from the speakers for instance) and is sometimes not easy to get - the latter, while promising quite a good performance, are still very complicated to setup and usually are not really available in the lower price ranges. here the newer 64bit arm chromebooks fit in quite nicely, as they usally have relatively modern hardware, good build quality and are easily available at rather low prices (even more if one gets them used and there seems to be a large used marked for them): the mediatek mt8173 ones are quite well supported, have a performance similar to that of the pinebook pro, but there is no support for opengl acceleration via its (powervr) gpu - the mediatek mt8183 ones have a better performance and better energy efficiency than the pinebook pro, the gpu is supported by the open source panfrost driver in mesa, but they are still a bit work in progress right now - the snapdragon 7180c ones are maybe slightly faster than the mt8183 ones (but not much), they have a well suppored gpu (via the open source freedreno mesa driver), but are also still very much work in progress in general and some of their hardware seems to require quite a mess of binary blobs to work - and there are the rockchip rk3399 based ones, which use the same soc as the pinebook pro, are quite well supported (including the gpu via the open source panfrost mesa driver) and usually have a very good build quality

what you will find here is the try to support many of those arm chromebooks to some (and hopefully increasing) degree by providing disk images one can simply write to an sd card and run them from there (i.e. even while initially keeping chromeos on the device). at the beginning the focus will be to somehow cover as many of them as possible with a basic system, i.e. some useful x11 (with wayland as an option) desktop setup and extend the functionality step by step from there (i.e. trying to add things like accelerated video playback or add sound support where it is still missing etc.). in the current state most of the supported chromebooks are somehow useable for people with linux know how, but i would not yet recommend them for typical end users who expect everything to work well and out of the box (this will hopefully change over time). this is a slowly evolving spare time project, so please do not expect wonders and things will take time, but it will steadily move forward. the detailed current state for the different arm chromebooks can be found at the corresponding links below. there are also some more links to other similar projects.

i'm open for generally useful suggestions and tested pull requests. i'm not so interested in very special feature requests ("please add this very uncommon driver to the kernel" or similar) or untested suggestions. instead i would like to encourage people to for instance build their own kernels or maybe even images - its not that complicated and all information should be around spread across a few repositories here in a more or less readable format. for reports of success or failure with the images provided and everything else please create github issues. one thing is important to keep in mind: the most effort goes into trying to make sure that everything at least somehow works in the end and the least effort is required to get new ideas what can be done or added - i actually already have a very long list of things i would like to add :)

## supported devices and linux distributions

currently xubuntu 20.04 (focal) and debian 11 (bullseye) are supported.

the following chromebook types are more or less supported:

- chromebook snow:
  - samsung chromebook xe303c12 - snow - rev4 and rev5
  - https://github.com/hexdump0815/imagebuilder/blob/main/systems/chromebook_snow/readme.md
- chromebook spring:
  - hp chromebook 11 g1 - spring - wip
  - https://github.com/hexdump0815/imagebuilder/blob/main/systems/chromebook_spring/readme.md
- chromebook peach:
  - samsung chromebook xe503c12 - peach-pit
  - samsung chromebook xe503c32 - peach-pi - untested
  - https://github.com/hexdump0815/imagebuilder/blob/main/systems/chromebook_peach/readme.md
- chromebook veyron:
  - medion chromebook s2013 - jaq
  - medion chromebook s2015 - mighty
  - asus chromebook c201 - speedy
  - asus chromebook c100 - minnie - wip
  - most probably many more - see page below
  - https://github.com/hexdump0815/imagebuilder/blob/main/systems/chromebook_veyron/readme.md
- chromebook nyan:
  - acer chromebook cb5 311 - nyan big
  - acer chromebook 13 c810 - nyan big
  - hp chromebook 14 g3 - nyan blaze
  - https://github.com/hexdump0815/imagebuilder/blob/main/systems/chromebook_nyan/readme.md
- chromebook gru:
  - asus chromebook c101 - bob
  - samsung chromebook plus - kevin
  - https://github.com/hexdump0815/imagebuilder/blob/main/systems/chromebook_gru/readme.md
- chromebook oak:
  - lenovo chromebook n23 - hana
  - acer chromebook r13 - elm
  - most probably many more - see page below
  - https://github.com/hexdump0815/imagebuilder/blob/main/systems/chromebook_oak/readme.md
- chromebook kukui:
  - lenovo ideapad duet 10.1 chromebook - krane
  - acer chromebook spin cp311-3h - juniper
  - most probably many more - see page below
  - https://github.com/hexdump0815/imagebuilder/blob/main/systems/chromebook_kukui/readme.md
- chromebook trogdor:
  - acer chromebook spin sp513-1h - lazor
  - most probably many more - see page below
  - https://github.com/hexdump0815/imagebuilder/blob/main/systems/chromebook_trogdor/readme.md

## enabling developer mode

in order to boot anything else than chromeos on a chromebook one needs to enable the developer mode for it. a few things related to enabling the developer mode are important to know: first - in the process all data saved locally on the chromebook will be deleted, so important data should be backed up first - and second - in developer mode some of the advanced security features of chromeos are not enabled anymore. some more information about the developer mode and how to enable it on different devices can be found here: https://chromium.googlesource.com/chromiumos/docs/+/master/developer_mode.md

on a normal chromebook with a built in keyboard the following procedure will initiate the switch to developer mode (see: https://chromium.googlesource.com/chromiumos/docs/+/master/debug_buttons.md#firmware-keyboard-interface):

- esc + refresh (the round circle button) and press power button
- ctrl d
- enter (to accept)
- it will take around 10 minutes to do the switch

for more tablet style chromebooks without a fixed keyboard (like the lenovo duet for instance) the following procedure will initiate the switch to developer mode (see: https://chromium.googlesource.com/chromiumos/docs/+/master/debug_buttons.md#firmware-menu-interface):

- press the volume - and volume + buttons at the same time and then press the power button to turn on the device
- a prompt saying something like "please insert a recovery usb disk" will appear
- press the volume + button, now a menu should appear with options like "show debug info", "cancel", "power off", and "language"
- press the volume - and volume + buttons at the same time again, now the option "show debug info" should change to "confirm disabling os verification"
- select this option using the volume buttons and confirm it via the power button
- it will take around 10 minutes to do the switch

afterwards preparing everything for booting from sd card or usb looks like this by going to the command prompt (see: https://chromium.googlesource.com/chromiumos/docs/+/master/developer_mode.md#shell):

- ctl alt ->
- login as user chronos (no password required)
- sudo su (to become root)
- crossystem dev_boot_usb=1 dev_boot_signed_only=0
- reboot
- ctrl u (to boot from sd card at the first initial screen after reboot)

now the chromebook should be able to boot one of the bootable images provided here for the corresponding device type and wirtten properly to an sd card or usb disk. for writing disk images to such a medium there is plenty of information available on the web about how to do it on the different operating systems.

## setting gbb flags, enabling ccd and the magic suzyqable

it is possible to set parameters like the above to enable booting from usb also as default in the chromebook firmware via the so called gbb flags. this is definitely recommended as soon as one wants to install a regular linux onto the internal emmc storage of a chromebook (and wiping the chromeos on it). the reason for this is that at least some chromebooks tend to drain the battery if not used for a longer time and settings like the above for allowing usb booting and the enabled developer mode get lost if the battery gets completely empty in such cases. so one might end up in a situation where booting from usb/sd card is no longer possible as the corresponding flags were reset to their default to not allow booting from those devices. as there is no chromeos anymore on the emmc it is not possible to change the flags to enable booting from usb/sd again and the only way out is to restore cromeos via restore image onto the internal emmc storage wiping the regular linux installation there (and loosing all of its data) which is not really what one wants. due to this it is in the end highly recommended to set those flags to default to allow usb/sd booting and enabled developer mode, so that it will still work even if the battery gets completely drained. in case one gets into the above described situation that the usb booting flags get lost due to battery drain there might be some last option to get access to the installed linux on emmc back without having to restore chromeos to it by using a special hand crafted restore image as described here: https://www.chromium.org/chromium-os/developer-information-for-chrome-os-devices/workaround-for-battery-discharge-in-dev-mode/ - i'm not sure if this procedure is still working for the latest chromeos and firmware versions, but it might be worth a try at least in such cases.

the problem with this is that it is not really that easy as chromebooks have by default some form of write protection enabled for the area of the firmware where those gbb flags are stored as part of the chromeos security setup. disabling this write protection requires opening the chromebook and removing a certain screw (in most cases) or metallic sticker (for instance in case of the snow chromebook), which is bridging some contacts to keep the write protection active. a search on the internet should bring up a lot of information about where to find this special screw for different chromebook types and how this screw usually looks like (for the ones without explicit documentation). this method to enable/disable the write protection was used on all older chromebooks, but all newer ones do it in a different way which does not longer require the device to be opened, but requires a special usb-c cable to disable the write protection - the suzyqable - instead. this cable has on top of that a few other nice use cases: it provides access to the serial console of the chromebook, to the ec firmware console and to the so called cr50 console. all this together is called 'ccd' for closed case debugging. if a certain chromebook supports ccd can be seen in the last column of this table: https://www.chromium.org/chromium-os/developer-information-for-chrome-os-devices - more information about how to disable the write protection via suzyqable on supported systems can be found here: https://wiki.mrchromebox.tech/Firmware_Write_Protect#Disabling_WP_on_CR50_Devices_via_CCD

after following the mrchromebox guide from above to open the ccd, the following commands given at the ccd prompt will ensure maximum openness and least risk of not being able to get out of a bricked situation (but please be aware that this will give quite a bit of power to everyone having physical access to the device):
- wp false
- wp false atboot
- ccd reset factory
- ccd testlab enable (requires pressing the power button a few times when requested to prove presence)

and finally some more detailed links around those topics:

- https://chromium.googlesource.com/chromiumos/platform/ec/+/cr50_stab/docs/case_closed_debugging_cr50.md
- https://chromium.googlesource.com/chromiumos/docs/+/master/write_protection.md
- https://github.com/jackrosenthal/ec-zephyr-build/blob/master/docs/case_closed_debugging_cr50.md
- https://wiki.mrchromebox.tech/Unbricking
- https://chromium.googlesource.com/chromiumos/platform/ec/+/cr50_stab/docs/case_closed_debugging_cr50.md#Option-2_OpenNoDevMode-and-OpenFromUSB-are-set-to-Always

back to the gbb flags ... lets assume the write protection has been disabled in one way or another. the following are the steps to set the gbb flags in the desired way:

- the usual disclaimer:
  - all the below steps are done at your own risk
  - only do the below steps if you have at least a rough idea what they are doing
  - if something goes wrong it might brick your chromebook in the worst case - the chance is very very low, but the risk is there if flashing the low level firmware
- make sure the chromebook is properly charged and the power suply is connected properly
- make sure the chromebook is in developer mode (otherwise see above)
- disable the flash write protection (see above - this disables the first part of the write protection - the hardware write protection)
- boot chromeos
- ctl alt ->
- login as user chronos (no password required)
- sudo su (to become root)
- go somewhere, where a "touch testfile" does not give an error, because the dir is read-only - 'cd /tmp' should work
- check the second part of the write protection - the software write protection - via the comman 'flashrom --wp-status'
- it will most probably show enabled
- disable it via 'flashrom --wp-disable'
- re-check it via 'flashrom --wp-status'
- it should be disabled now (this will only work if the hardware write protection has been disabled beforehand)
- read the firmware from the flash into a file 'bios.bin' via the command 'flashrom -r bios.bin'
- it is a good idea to copy this file to a safe place outside of the chromebook now (sd card, usb stick etc.) to have a copy of the original unmodified firmware around just in case ...
- note: in case the boot screen should be changed as well (see below) it might be a good idea to do this first to avoid resetting the changed gbb settings again then
- set the desired gbb flags via the command '/usr/share/vboot/bin/set_gbb_flags.sh 0x19'
- the meaning of the flags is (their sum is 0x19):
  - GBB_FLAG_DEV_SCREEN_SHORT_DELAY 0x00000001 - initial boot screen only for 2 seconds instead of the default 30 seconds and no beep afterwards
  - GBB_FLAG_FORCE_DEV_SWITCH_ON 0x00000008 - keep developer mode enabled by default
  - GBB_FLAG_FORCE_DEV_BOOT_USB 0x00000010 - keep the possibility to boot from usb/sd card enabled by default
- re-enable the software write protection via 'flashrom --wp-enable'
- in case you get an error for that command speaking about --wp-range (which seems to happen on newer chromeos versions), then please do the following (see: https://chromium.googlesource.com/chromiumos/docs/+/master/write_protection.md#Enabling-write-protect)
  - run the command 'fmap_decode bios.bin'
  - note down the range for WP_RO - usually it is 0x00000000 to 0x00200000 (or 0x00400000), but better double check
  - then rerun the failed command as (with the range noted down above) - for example 'flashrom --wp-enable --wp-range 0x00000000 0x00200000'
- re-check the software write protect via 'flashrom --wp-status' - it should be enabled again now
- if everything looks good, you can shutdown your system now
- put the flash write protect screw/sticker back or enable it again via ccd if desired

on older chromebooks (at least the 32bit versions) it is also possible to replace the initial developer mode sceen with a simple black screen with the small text 'developer mode warning' in the middle of the screen by doing the following steps before re-enabling the write protection above (make sure you have saved a copy of your initial bios.bin file somewhere outside of your chromebook beforehand):
- create an empty boot bitmap file via 'touch nothing'
- write this empty boot bitmap into the flash file via 'gbb_utility -s --bmpfv=nothing bios.bin'
- write back the modified flash file via 'flashrom -w bios.bin'
- to be on the safe side maybe redo the gbb flag settings from above as it might have been undone in case the bios.bin file was read before the gbb flags were set
- now you can continue with the re-enabling of the write protection as described above

it seems to be possible to do a similar cleanup of the intial developer mode screen on newer devices as well according to https://www.reddit.com/r/chromeos/comments/hbajeg/bios_bitmaps_question/fv8uncy/ - i tried it on an elm chromebook and it worked using the following commands:

- cd /tmp
- flashrom -p host -r bios.bin
- cp bios.bin bios.bin.org
- cbfstool bios.bin extract -n vbgfx.bin -f vbgfx.bin.org
- cbfstool bios.bin remove -n vbgfx.bin
- flashrom -p host -w bios.bin

it is a good idea to put the bios.bin.org and vbgfx.bin.org to a safe place in case one wants to revert the changes one day (i did not really like the result and actually reverted it back to the original state by flashing back the bios.bin.org). maybe it is even possible to replace the default bitmaps with custom own ones similar to how it is described at https://jcs.org/2016/08/26/openbsd_chromebook for the old gbb bitmaps.


## different boot options: u-boot, kpart

there are two boot strategies used in the arm chromebook bootable images: chainloaded u-boot and native chromeos kpart kernels. actually there is the third options to install an self built low level libreboot or coreboot bootloader, but this options is quite complicated, risky and not supported for many arm chromebooks and thus not used and discussed here.

booting via chainloaded u-boot is used by images for the 32bit armv7l chromebooks. here the low level bootloader is loading u-boot (another very common bootloader for arm systems) instead of the linux kernel and u-boot then will load the actual kernel. the advantage of this setup is that it is possible to create boot menus for selecting different kernels etc.

the native chromeos kpart booting works by packaging the used mainline kernel in the same way the regular chromeos kernel is packaged (kpart - i.e. the format for the kernel partition) and putting it this way into the kernel partition, so that the chromebook will boot it just like a regular chromeos kernel. this approached is used for the 64bit aarch64 chromebooks as there is no mainline u-boot available for them, which could be chainloaded. the disadvantages of this approach are that the resulting kernel image can be at max 32mb in size (16mb for 32bit armv7l systems) which can be quite small for systems with initrd and that the kernel cmdline args are fixed built into this kernel image and thus it is not very flexible. the advantage of this approach is that such a kernel image can contain multiple dtb files for different hardware and the proper one will be selected automatically at boot. an important feature of chromeos kernel partitions is that there might be multiple of them and they can be given a priority and a counter for failed boots which to some degree gives the opportunity for relatively failsafe test booting of new kernels without the risk of completely bricking a system. the bootable images discussed here for this reason have two kernel partitions of which only the first is currently used right now - the second one os there for test booting other kernels if done with the proper settings.

## using and adjusting the bootable images

the bootable images discussed here can either be bootable directly without any further changes after writing the image to an sd card or usb disk (important side note: not all chromebooks can boot from both media - some can only boot from sd card, on some not all usb connectors are bootable and not all chromebooks have an sd card slot at all). for some of the images some extra steps are required to adjust the image for a specific model of the supprted hardware - those additional steps are usually described in the system specific readme and they are usually one or both of the following: adjusting the dtb file in the extlinux.conf boot configuration file or installing the proper u-boot version. the extlinux.conf file can be found in the "extlinux" directory of the third partition (boot) of the written image and is a simple text file which usually contains some comments on what might require adjustment. alternative u-boot versions can be found in the "extra" directory of the same partition and are written with the dd command to the first partition (the primary kernel partition).

## storing chromebooks and avoiding battery drain

maybe the following situations seem familiar when dealing with chromebooks: a chromebook was fully charged and not used for a while and afterwards the battery is already fully discharged - or - a chromebook is turned off and put into a shelf, maybe on top of another laptop or similar device and a moment later it turned on again on its own or a day later the battery is completely discharged.

the reason for the first situation is that even if a chromebook is turned off the embedded controller is still running in some low power mode and thus draining the battery slowly until it is empty after a few weeks or months usually. this happens as chromebooks usually do not have those small extra bios batteries other systems often have to keep components like an embedded controller or real time clock alive when they are powered off.

the second situation comes from the fact that chromebooks often have small magnetic sensors for sensing the opening of the lid (which has a corresponding small magnet on it) to power them on in this case. those magnetic sensors can easily get confused by other strong magnets (for instance from another laptop) and as a result the chromebook will power on even if the lid has not really been opened. if this stays unnoticed it might run then until the battery gets drained in the worst case.

a way to avoid such situations when storing a chromebook for a longer time while not using it is a special mode most newer chromebooks can be put into to avoid such behaviour: the battery disconnect mode or battery cutoff mode. it looks like it seems to be available for most chromebooks with a built in keyboard built after around 2014/2015 - older models do not seem to support it. to enter this mode the refresh button (the one in the top row with the round circle) has to be pressed for a few seconds together with the power button while the charger is connected and then the charger has to be removed while still pressing those buttons together. afterwards the chromebook will not power on anymore neither by pressing the power button nor by opening the lid until a charger is connected. it can then be stored for a very long time with only very minimal battery drain. in theory it should work by pressing the power together with the volume up button and removing the power supply for 5 seconds for tablet style chromebooks without a builtin keyboard like the lenovo duet (krane), but it did not work for me for this device. what kind of worked for the lenovo duet was to set "crossystem battery_cutoff_request=1" as root in chromeos in developer mode while there is no power supply connected and reboot - this way it did not seem to respond to lid open events, but can still be powered on using the power button. not sure yet, if this can somehow also be initiated from linux without having to run chromeos. it looks like on older chromebooks sometimes "ectool batterycutoff" can put the chromebook into battery cutoff mode (tested on a hp chromebook 14 g1 - nyan blaze).

some usefule links regarding those topics:

- https://support.google.com/chrome/a/answer/9139543?hl=en
- https://chromium.googlesource.com/chromiumos/docs/+/HEAD/debug_buttons.md
- https://community.acer.com/en/kb/articles/23-chromebook-may-turn-on-automatically-when-placed-on-top-of-another-chromebook
- https://chromium.googlesource.com/chromiumos/platform/vboot_reference/+/aee6bd69fefac653cfc4a5679eb387d7c3280d14
- https://chromium.googlesource.com/chromiumos/platform/memento_softwareupdate/+/3c7204287bbc8f4341f546857216389bdff58e51/battery_cutoff/battery_cutoff.sh#213

## other related projects

for arm chromebooks:

- https://github.com/Maccraft123/Cadmium
- https://github.com/eballetbo/chromebooks
- https://github.com/SolidHal/PrawnOS
- https://github.com/amstan/PKGBUILDs/commit/linux-chromeos

for other arm laptops:

- https://github.com/aarch64-laptops/debian-cdimage - snapdragon 845 (lenovo yoga c630 [not the c630 chromebook!], lenovo flex 5g)
- https://github.com/aarch64-laptops/build - snapdragon 835 (asus novago tp370ql, hp envy x2, lenovo miix 630) - not much happening there anymore
- https://wiki.pine64.org/wiki/Pinebook_Pro - rockchip rk3399 based
- https://wiki.pine64.org/wiki/Pinebook- allwinner a64 based
- https://wiki.pine64.org/wiki/PineTab - allwinner a64 based

for intel chromebooks:

- https://github.com/hexdump0815/linux-mainline-on-intel-chromebooks/
- https://mrchromebox.tech/
- https://galliumos.org/
