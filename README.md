# asus-tuf-15-opencore
OpenCore files for Asus TUF 15

# Asus TUF 15 FX506L OpenCore OSX EFI

This repo was created after I was able to boot with the desktop files for Comet Lake platform.
Most of the work to get this to work was done by MaLd0n, which can be found here:
https://www.olarila.com/topic/12233-eblogexitbsstart-error-opencore-big-sur-111/?tab=comments#comment-138034
All credits for the DSDT file go to him.

[![license](https://img.shields.io/badge/license-Anti%20996-blue.svg)](https://github.com/996icu/996.ICU/blob/master/LICENSE)

**macOS Version: 11.1 Big Sur **

**OpenCore Version: [0.6.4 Offical](https://github.com/acidanthera/OpenCorePkg/releases/tag/0.6.4)**

MacOS on Asus FX506L

 MacOS Big Sur 11.1
 :-------------------------:

 
 ## Updates

 hopefully coming .... :)
 
- 2021-01-15:

  * Initial release .

 ## Guide
 
 * Go to [GUIDE](https://dortania.github.io/vanilla-laptop-guide/)(**Official Guide**)

 ## ## System Information 💻
 
 | Part | Functional | Model | 
 | --- | --- | --- |
 | Machine | ✅ | Asus TUF 15 FX506LI |
 | BIOS | ✅ | 3.06 |
 | CPU | ✅ | Intel(R) Core(TM) i5-10300H CPU @ 2.50GHz |
 | RAM | ✅ | 24GB DDR4 SODIMM |
 | SSD | ✅ | 512GB NVMe |
 | iGPU | ✅ | Intel UHD Graphics 630 1536 MB |
 | WLAN | ✅ | Intel AX201 Wifi 6 Card |
 | Bluetooth | ✅ | Intel Bluetooth |
 | Ethernet | ✅ | Realtek 8111 Gigabit Ethernet |
 | Webcam | ✅ | Integrated 720P Webcam |
 | Audio | ✅ | Realtek HDA |
 | Microphone | ✅  | Integrated Microphone |
 | Internal Screen | ✅ | 15.6" 1920x1080 144Hz |
 | Trackpad | ✅/🚫 | I2C ELAN1201 |
 | Keyboard | ✅ | - |
 | dGPU | 🚫 | NVIDIA GTX 1650 4GB GDDR6 |
 
## Perfectly Working Features

- [x] Native Hardware NVRAM
- [x] Intel UHD630
- [x] Screen Brightness Control
- [x] Screen Brightness Memoriztion After Reboot
- [x] Native Screen Refresh Rate Settings
- [x] USB 3.1 Gen 1
- [x] Web Camera
- [x] Battery Percentage
- [x] Sleep & Wake

- [x] ...

> 1. The above functions are only tested and passed in AN715-51.
> 1. Whether the native refresh rate adjustment is available depends on the model and production batch of the screen.
 
 
 ## ## Issues & Solutions
 ### macOS
 * [Hackintool: The Swiss army knife of vanilla Hackintoshing.](https://github.com/headkaze/Hackintool)
 * [How to download a full ‘Install macOS’ app with software update in TERMINAL](https://scriptingosx.com/2019/10/download-a-full-install-macos-app-with-softwareupdate-in-catalina/)
 
 ## Generate your own SMbios

   Platforminfo

   For setting up the SMBIOS info, we'll use CorpNewt's GenSMBIOS and ProperTree application. https://github.com/corpnewt/GenSMBIOS https://github.com/corpnewt/ProperTree

   Because of the Coffee Lake Plus(9th Gen), we'll choose the MacBookPro16,1 SMBIOS:

   Run GenSMBIOS, pick option 1 for downloading MacSerial and Option 3 for selecting out SMBIOS. This will give us an output similar to the following:

   Type: MacBookPro16,4

   Serial: C02WXAY2HV2B

   Board Serial: C02826303CDHRPC8B

   SmUUID: 88AA1336-8DF9-477A-A39F-03D016ED0807

   The order is Product | Serial | Board Serial (MLB)

   The Type part gets copied to Generic -> SystemProductName.

   The Serial part gets copied to Generic -> SystemSerialNumber.

   The Board Serial part gets copied to Generic -> MLB.

   The SmUUID part gets copied toto Generic -> SystemUUID.

   We set Generic -> ROM to either an Apple ROM (dumped from a real Mac), your NIC MAC address, or any random MAC address (could be just 6 random bytes, for this guide we'll use 11223300 0000. After install follow the Fixing iServices page on how to find your real MAC Address)

   Reminder that you want either an invalid serial or valid serial numbers but those not in use, you want to get a message back like: "Invalid Serial" or "Purchase Date not Validated"

   Apple Check Coverage page https://checkcoverage.apple.com/cn/zh/

### Monitor
* [Intel® Power Gadget](https://software.intel.com/en-us/articles/intel-power-gadget)
* [IO Registry Explorer](https://download.developer.apple.com/Developer_Tools/Additional_Tools_for_Xcode_11/Additional_Tools_for_Xcode_11.dmg)
* [iStat Menus](https://bjango.com/mac/istatmenus/)
* [HWSensors](https://github.com/kozlek/HWSensors)

### NTFS Writer
* [Mounty](http://enjoygineering.com/mounty/)
 
 ### Audio
 * KEXT required to enable Audio support : `AppleALC.kext`
 * Make sure you inject audio `layout-id = 11` in OpenCore 
 
 ### ELAN Trackpad (TPAD)
 * Using the latest VoodooI2C as of now 2.6.3 with VoodooI2CHID
 * The touchpad works mostly fine, but the left button mostly works for dragging, sometimes for clicking
 * right button is not working
 * occassionally there is some delay/tear 
 
 ### Wifi & Bluetooth
 * In order to get Bluetooth and Wifi working, you can use the drivers from https://github.com/OpenIntelWireless/itlwm 
 * For bluetooth https://github.com/OpenIntelWireless/IntelBluetoothFirmware
 * For the UI, there is HeliPort: https://github.com/OpenIntelWireless/HeliPort
 * I like to hide the wireless icon for the builtin card, dozer is a great tool for that: https://github.com/Mortennn/Dozer
 * add heliport to your startup items under login and hide the orginal with dozer and it's hard to see the difference
 
 ### GPU
 ##### iGPU

 * HDMI Port :
    * Long story short, it won't work. Why? Because all display output is hard wired to the NVIDIA GPU. You can confirm this by going into NVIDIA controler panel in Windows and see PhysX, and you can see all display output is wired to the NVIDIA card, while the eDP in screen display is wired to the iGPU. Therefore, since NVIDIA card won't work, also Optimus won't work, the HDMI port or USB-C display output just won't work because the display output is not wired to the iGPU. Not to mention you disabled dGPU in `config.plist/-wegnoegpu`or custom `SSDT-DDGPU`.
    
 ##### dGPU
 * NVIDIA GTX1650 is not supported (for now only in High Sierra it seems) and is disabled 
 * [Apple and Nvidia Are Over: NVIDIA drops CUDA support for macOS.](https://gizmodo.com/apple-and-nvidia-are-over-1840015246)
 * Currently, there is nothing we can do. Let's hope Apple and NVIDIA work together again. 
 
 ### Power Management
 * seems ok, can be improved by
   [CPUFriendFriend](https://github.com/corpnewt/CPUFriendFriend)

   Instructions on how to build a power mangaement profile for any other CPU types can be found here:

   https://github.com/PMheart/CPUFriend/blob/master/Instructions.md
 
 Intel Power Gadget
 :-------------------------:
 ![alt text](screenshots/IPG.png)

## Credits

- [acidanthera](https://github.com/acidanthera) for providing almost all kexts and drivers
- [alexandred](https://github.com/alexandred) for providing VoodooI2C
- [headkaze](https://github.com/headkaze) for providing the very useful [Hackintool](https://www.tonymacx86.com/threads/release-hackintool-v2-8-6.254559/)
- [daliansky](https://github.com/daliansky) for providing the awesome hotpatch guide [OC-little](https://github.com/daliansky/OC-little/) and the always up-to-date hackintosh solutions [XiaoMi-Pro-Hackintosh](https://github.com/daliansky/XiaoMi-Pro-Hackintosh) [黑果小兵的部落阁](https://blog.daliansky.net/)
- [RehabMan](https://github.com/RehabMan) for providing numbers of [hotpatches](https://github.com/RehabMan/OS-X-Clover-Laptop-Config/tree/master/hotpatch) and hotpatch guides
- [corpnewt](https://github.com/corpnewt/CPUFriendFriend) for CPUFriendFriend
- And all other authors that mentioned or not mentioned in this repo
 
 ## License
 * This work is issued under the [996 License](https://github.com/996icu/996.ICU/blob/master/LICENSE) and [MIT License](https://opensource.org/licenses/MIT).
