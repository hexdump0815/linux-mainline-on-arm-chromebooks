# running linux mainline on arm chromebooks

## intro

this is about running a real mainline linux system on arm chromebooks, so no chromeos and this is not about intel chromebooks - for those please have a look at the intel related links at the bottom of this page, those should help you getting a real linux running on them.

there are most probably three reasons why one wants to run real mainline linux system on arm chromebooks:
- chromebooks will usually be supported by chromeos only for a certain time (usually between 6 to 8 years) and afterwards one will not get any new features and also no fixes for security problems, so it is not a really good idea to use them this way any longer. on the other hand they are then usually still quite useable hardware and still can be used instead of throwing them into the trash (sustainability etc.). most of the 32bit armv7l chromebooks have reached this stage meanwhile.
- even for chromebooks still supported by chromeos there might be a reason to use them with a real mainline linux system instead or in parallel: in case one wants to say good bye to googles interest to track a lot of information on how you are using your chromebook etc., which is unavoidable if you are using a chromebook with chromeos (even if you adjust the privacy settings etc. there will ibe still a lof of informatoion collected).
- the third reason why this might be interesting is if one wants to run linux on a laptop with an arm processor and not an intel one. there are only a few laptops easily available with arm cpus which can be used with linux: the pinebook pro comes into mind or maybe some of those windows laptops with arm cpus - the first (while being a very nice option in general with a very vivid community around it) is quite limited performance wise (old rk3399 soc), does not have the best build quality (strange noise from the speakers for instance) and is sometimes not easy to get - the latter, while promising a quite good performance, are still very complicated to setup and usually are not really available in the lower price ranges. here the newer 64bit arm chromebooks fit in quite nicely: the mediatek mt8173 ones are quite well supported, have a performance similar to that of the pinebook pro, but there is no support for opengl acceleration via its (powervr) gpu - the mediatek mt8183 ones have a better performance and better energy effeiciency than the pinebook pro, the gpu is supported by the open source panfrost driver in mesa, but they are still a bit work in progress right now - the snapdragon 7180c ones are maybe slightly faster than the mt8183 ones, they have a well suppored gpu (via the open source freedreno mesa driver), but are also still very much work in progress in general and some of their hardware seems to require quite a mess of binary blobs to work - and there are the rockchip rk3399 based ones, which use the same soc as the pinebook pro, are quite well supported (including its gpu via the open source panfrost mesa driver) and usually have a very good build quality

what you will find here is the try to support many of those arm chromebooks to some (and hopefully increasing) degree by proving disk images one can simply write to an sd card and run them from there (i.e. even while initially keeping chromeos on the device). at the beginning the focus will be to somehow cover as many of them with a basic system, i.e. some useful x11 (with wayland as an option) desktop setup and extend the functionality step by step from there (i.e. tring to add things like accelerated video playback or add sound support where it is still missing etc.). in the current state most of the supported chromebooks are useable for people with linux know how, but i would not yet recommend them for typical end users who expect everything to work well and out of the box (this will hopefully change over time). this is a slowly evoloving spare time projects, so please do not expect wonders and things will take time, but they will steadily move forward. the detailed current state for the different arm chromebooks can be found at the corresponding links below. the snapdragon 7180c based ones are not yet supported by the image builder framework yet (as i do not have any of those yet), but there is some support for them in cadmium - a project with a similar but also slightly different focus on getting mainline linux onto arm chromebooks - see the link at the bootom of this page. there are also some more links to other similar projects.

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
  - will maybe be added at some time in the future ...

## enabling developer mode

coming soon ...

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
