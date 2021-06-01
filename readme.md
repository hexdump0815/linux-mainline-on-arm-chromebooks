# running linux mainline on arm chromebooks

## intro

this is about running a real mainline linux system on arm chromebooks, so no chromeos and this is not about intel chromebooks - for those please have a look at the intel related links at the bottom of this page, those should help you getting a real linux running on them.

there are most probably three (maybe even more) reasons why one wants to run a real mainline linux system on arm chromebooks:
- chromebooks will usually be supported by chromeos only for a certain time (usually between 6 to 8 years) and afterwards one will not get any new features and also no fixes for security problems, so it is not a really good idea to use them this way any longer afterwards. on the other hand they are then usually still quite useable hardware and still can be used instead of throwing them into the trash (sustainability etc.). most of the 32bit armv7 chromebooks have reached this stage meanwhile.
- even for chromebooks which are still supported by chromeos there might be a reason to use them with a real mainline linux system instead or in parallel: in case one wants to say good bye to googles interest to track its users and to collect a lot of information on how they are using their chromebook etc., which is unavoidable if used with chromeos (even after adjusting the privacy settings etc. there will still be a lof of informatoion tracked and collected).
- the third reason why this might be interesting is if one wants to run linux on a laptop with an arm processor and not an intel one. there are only a few laptops easily available with arm cpus which can be used with linux: the pinebook pro comes into mind or maybe some of those windows laptops with arm cpus - the first (while being a very nice option in general with a very vivid community around it) is quite limited performance wise (old rk3399 soc), does not have the best build quality (strange noise from the speakers for instance) and is sometimes not easy to get - the latter, while promising quite a good performance, are still very complicated to setup and usually are not really available in the lower price ranges. here the newer 64bit arm chromebooks fit in quite nicely, as they usally have relatively modern hardware, good build quality and are easily available at rather low prices (even more if one gets them used and there seems to be a large used marked for them): the mediatek mt8173 ones are quite well supported, have a performance similar to that of the pinebook pro, but there is no support for opengl acceleration via its (powervr) gpu - the mediatek mt8183 ones have a better performance and better energy efficiency than the pinebook pro, the gpu is supported by the open source panfrost driver in mesa, but they are still a bit work in progress right now - the snapdragon 7180c ones are maybe slightly faster than the mt8183 ones (but not much), they have a well suppored gpu (via the open source freedreno mesa driver), but are also still very much work in progress in general and some of their hardware seems to require quite a mess of binary blobs to work - and there are the rockchip rk3399 based ones, which use the same soc as the pinebook pro, are quite well supported (including the gpu via the open source panfrost mesa driver) and usually have a very good build quality

what you will find here is the try to support many of those arm chromebooks to some (and hopefully increasing) degree by providing disk images one can simply write to an sd card and run them from there (i.e. even while initially keeping chromeos on the device). at the beginning the focus will be to somehow cover as many of them as possible with a basic system, i.e. some useful x11 (with wayland as an option) desktop setup and extend the functionality step by step from there (i.e. trying to add things like accelerated video playback or add sound support where it is still missing etc.). in the current state most of the supported chromebooks are somehow useable for people with linux know how, but i would not yet recommend them for typical end users who expect everything to work well and out of the box (this will hopefully change over time). this is a slowly evolving spare time project, so please do not expect wonders and things will take time, but it will steadily move forward. the detailed current state for the different arm chromebooks can be found at the corresponding links below. the snapdragon 7180c based ones are not yet supported by the imagebuilder framework used here yet (as i do not have such hardware as of now - donations are welcome :) ), but there is some support for them in cadmium - a project with a similar but also slightly different focus on getting mainline linux onto arm chromebooks - see the link at the bootom of this page. there are also some more links to other similar projects.

## supported devices and linux distributions

currently xubuntu 20.04 (focal) is supported and debian 11 (bullseye) is planned to be supported a bit after it is officially released.

the following chromebook types are more or less supported:

- chromebook snow:
  - samsung chromebook xe303c12 - snow - rev4 and rev5
  - hp chromebook 11 g1 - spring - wip
  - https://github.com/hexdump0815/imagebuilder/blob/main/systems/chromebook_snow/readme.md
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
  - https://github.com/hexdump0815/imagebuilder/blob/main/systems/chromebook_nyan/readme.md
- chromebook oak:
  - lenovo chromebook n23 - hana
  - acer chromebook r13 - elm
  - most probably many more - see page below
  - https://github.com/hexdump0815/imagebuilder/blob/main/systems/chromebook_oak/readme.md
- chromebook gru:
  - asus chromebook c101 - bob
  - samsung chromebook plus - kevin - untested
  - should run with the oak version above for now - maybe an own version will come later ...
- chromebook kukui:
  - lenovo ideapad duet 10.1 chromebook - krane - wip
  - most probably many more in the future - see page below
  - https://github.com/hexdump0815/imagebuilder/blob/main/systems/chromebook_kukui/readme.md
- chromebook trogdor:
  - will maybe be added at some time in the future ... meanwhile check the cadmium project link at the bottom of this page

## enabling developer mode

in order to boot anything else than chromeos on a chromebook one needs to enable developer mode for it. a few things related to enabling developer mode are important to know: first - in the process all data saved locally on the chromebook will be deleted, so important data should be backed up first - and second - in developer mode some of the advanced security features of chromeos are not enabled anymore. some more information about the developer mode and how to enable it on different devices can be found here: https://chromium.googlesource.com/chromiumos/docs/+/master/developer_mode.md

on a normal chromebook with a build in keyboard the following procedure will initiate the switch to developer mode (see: https://chromium.googlesource.com/chromiumos/docs/+/master/debug_buttons.md#firmware-keyboard-interface):

- esc + refresh (the round circle button) and press power (the power on button on the right)
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

now the chromebook should be able to boot one of the bootable images provided here for the corresponding device type and wirtten properly to an sd card or usb disk. for writing disk images to such a medium there is plenty information available on the web for how to do it on the different operating systems.

## setting gbb flags and enabling ccd

coming soon ...

## different boot options: u-boot, kpart

coming soon ...

## using and adjusting the bootable images

coming soon ...

## other related projects

for arm chromebooks:

- https://github.com/Maccraft123/Cadmium
- https://github.com/eballetbo/chromebooks
- https://github.com/SolidHal/PrawnOS

for intel chromebooks:

- https://mrchromebox.tech/
- https://galliumos.org/
