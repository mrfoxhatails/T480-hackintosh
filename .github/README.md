# Thinkpad T480 Hackintosh 🍏
Welcome to the T480 Hackintosh project, aimed at enabling macOS to run on the Lenovo Thinkpad T480. 🚀
<br/><br/>

<img src="https://raw.githubusercontent.com/musamatini/T480-hackintosh/main/.github/assets/ThinkpadT480.png" alt="img" align="right" width="220px">

## Disclaimer ⚠️

This EFI is based in [pierpaolodimarzo](https://github.com/pierpaolodimarzo/ThinkPad-T480) and [valnoxy](https://github.com/valnoxy/t480-oc) repository, go and support them. This repository couldn't be done without their work.

I constantly check `config.plist`status with [sanitychecker](https://sanitychecker.ocutils.me/results/6d99f7fc-bb02-4f1b-8e30-99600eefad79), I'll recommends you to use It as the tool in the OpenCore-PKG, any suggestion is acepted to improve the files.

**What's working?**

Most of all components and services are working, take a look to [pierpaolodimarzo's list](https://github.com/pierpaolodimarzo/ThinkPad-T480/tree/main#-what-works) to make an idea about.

<br/>

## Hardware Specifications 🛠️

> **note**
>**IMPORTANT!**
>
> 1. Add **NVMeFix.kext** if you don't use any Western Digital storage device.
> 2. Generate your own **SMBIOS** or your won't be able to use Apple Services.

| Category       | Details                            |
| -------------- | ---------------------------------- |
| Display        | 1920x1080                          |
| SMBIOS         | MacBookPro15,2                     |
| CPU            | Intel Core i7-8650                 |
| GPU            | Intel UHD Graphics 620             |
| Memory         | 64GB (32x2) DDR4 3200MHz           |
| LAN            | Intel Ethernet Connection I219-V   |
| Wifi           | Intel Wi-Fi AC 8265NGW             |
| Audio          | Realtek ALC256                     |
| Bluetooth      | Bluetooth 4.2                      |
| Storage        | Western Digital Blue SN570 1TB     |
| Thunderbolt    | JHL6240 Thunderbolt 3 LP Alpine Ridge |

<br/>

## BIOS Configuration 🔧

For successful installation, configure your BIOS settings as follows:

- Security > Security Chip: **Disabled**
- Memory Protection > Execution Prevention: **Enabled**
- Virtualization > Intel Virtualization Technology: **Enabled**
- Virtualization > Intel VT-d Feature: **Enabled**
- Anti-Theft > Computrace > Current Setting: **Disabled**
- Secure Boot > Secure Boot: **Disabled**
- Intel SGX Intel SGX Control: **Disabled**
- Device Guard: **Disabled**

Make sure to set your StartUp Menu options to UEFI Only and disable `CSM Support`.

For USB settings, set `Always On USB` to **Disabled**.

In the Thunderbolt Menu, configure the settings accordingly.


<br/>

## Preparing USB 🛠️
> **note**
>
> Next steps were tought to be done in a Windows system, I'll recommend using Python3 from the Microsoft Store.

- **Python 3**
- [**ProperTree**](https://github.com/corpnewt/ProperTree)
- [**GenSMBIOS**](https://github.com/corpnewt/GenSMBIOS)

### Formating USB 🧹
1. Plug your USB device.
2. Open CMD with administrative permissions, type `diskpart`
3. Type `list disk` to see your disk id.
4. Select your pendrive by typing `select disk <diskid>`
5. Clean the pendrive and convert it to GPT. First, type `clean` and then `convert gpt`.
6. Create a new partition where we can put our files on. First, type `create partition primary`, then select the new partition with `select partition 1` and format it `format fs=fat32 quick`.
7. Finally, mount your pendrive by typing `assign`

### OpenCore 🔄
1. Download [OpenCorePkg](https://github.com/acidanthera/OpenCorePkg) as a ZIP.
2. Extract the OpenCorePkg-master.zip file.
3. With **Windows Terminal** or **CMD**, go to next path `OpenCorePkg-master\Utilities\macrecovery`
4. From the [Dortania's guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/windows-install.html#downloading-macos), use any of these commands to download macOS version I support in my EFI:

```sh
# Big Sur (11)
python3 macrecovery.py -b Mac-42FD25EABCABB274 -m 00000000000000000 download

# Monterey (12)
python3 macrecovery.py -b Mac-FFE5EF870D7BA81A -m 00000000000000000 download

# macOS Ventura (13)
python3 macrecovery.py -b Mac-4B682C642B45593E -m 00000000000000000 download
```

### Folders 📁

> **Note**
>
> There is an EFI specifically for BigSur, use It if you don't plan to use Ventura or Monterey.

1. After downloading macOS with those commands, create the folder `com.apple.recovery.boot` in the pendrive.
2. Copy `OpenCorePkg-master\Utilities\macrecovery\BaseSystem.dmg` and `Basesystem.chunklist` into `com.apple.recovery.boot` folder.
3. Download and copy the `EFI` folder of this repository and paste it into your USB.
4. Make sure you have `com.apple.recovery.boot` and `EFI` in the pendrive.

### SMBIOS  💿
> **Note**
>
> In order to use AppleID and other services, check if the serial generated has support with [Apple verification the coverage of a device](https://checkcoverage.apple.com/)

1. Start GenSMBIOS.bat and use option `1` to download MacSerial.
2. Choose option `2`, to select the path of the **config.plist** file. It will be located in `EFI -> OC` folder in your pendrive.
3. Choose option `3`, and enter `MacBookPro15,2` as the machine type.
4. Press `Q` to quit. Your config now should contain the requied serials.

## Installation 🔌
> **Note**
>
> 1.  Make sure you have an Internet connection or use Ethernet.
> 2. Boot macOS without USB is possible, you'll need to create an EFI partition during disk manager steps.
> 3. In the installation your computer may restart sometimes, keep calm and continue the process.

### Boot USB 🚀

1. Restart your computer and open Boot Menu during startup with `F12`
2. Choose your USB device and wait.
3. A Black Screen with Boot options must be showed.
4. Press the **space bar** to display hidden options.
5. Choose the `OC DEVEL (dmg)`, you could recognize like a yellow gear.

### Partitions 🗄️
1. A menu must be display, choose `Utility Disk`.
2. Touch `View` and choose the option `Show all devices`. Once done, all your storage devices will be show in the SideBar.
3. Choose the device which you'll install the system and apply. the option `erase`. Make sure to have this settings.
   - Name: 'Macintosh HD'
   - Format: APFS
   - Scheme: GUID Partition Map
> You  could name your device as you want, keep in my that It cannot be modified after the system is installed.
4. Once done, choose your storage again and select `Partitions` option.
5. Select the `+` and choose the option that says `Create partition`, then resize the new partition created to 512 mb.
   - Format: MS-DOS (fat)
> Do not select ExFat, it won't be possible to boot OC.

### System 💻
> Keep your pendrive pluged.
1. Exit `Utility Disk` and choose `Install macOS...` or `Reinstall macOS...`
2. Acept terms and conditions, now it will show you your devices options to install the system.
3. Choose the one you named steps before.
4. Now wait until the installation, this could take some time.
5. Once the installation finished, your computer could restart without any reason.
6. Keep waiting and check if in your screen shows something like "Installation process 27 minutes left".
7. Your computer could restart 2 or 3 times.

### Enjoy the system 🎓
Congratulation, now you'll see `Select Your Country or Region` interface, that means the system is now installed in your computer.


<br/>

## Booting without USB 🚀

- Mount the EFI partition of your main disk.
- Copy the EFI folder from the USB to the main disk's EFI partition.
- Unplug the USB device and reboot your laptop to boot macOS.
  
<br/>


## Troubleshooting 🛠️❗
  > [ProperTree](https://github.com/corpnewt/ProperTree) is going to be used for modifying `config.plist`. Remember to install `hombrew` and `python-tk` dependency, also you can build te app in macOS.


### Customize OpenCore
Dortania's guide has a section that shows many [themes](https://dortania.github.io/OpenCanopy-Gallery/blackosx.html#themes) made by blackosx. In GitHub there are other OpenCore themes not showed in the guide but you could inquire them.

### Samsung PM981
Most Thinkpad T480 has the `Samsung PM981` which is not compatible with macOS even using NVMeFIX.kext won´t works at all. [Reference](https://www.reddit.com/r/hackintosh/comments/evkljr/samsung_pm981_nvme_hackintosh_reboot_loop/).

### System language 
The default keyboard layout and language is Spanish. The value for English would be `en-US:0`. Check this value language [list](https://github.com/acidanthera/OpenCorePkg/blob/master/Utilities/AppleKeyboardLayouts/AppleKeyboardLayouts.txt).

To change the language modify:
- `NVRAM > Add > 7C436110-AB2A-4BBB-A880-FE41995C9F82 > prev-lang:kbd`

### Latam keyboard 
My keyboard has some troubles in the hackintosh, I have the "<>" keys at the bottom of the keyboard, this repository has fixed that key. Follow its README to install.

- [Latam-keyboard by neosergio](https://github.com/neosergio/Latam-Keyboard)

### BIOS entry
By default this is enabled in this `config.plist`. I'll recommend this by default.
#### Enable
OC writes an entry into BIOS pointing directly to OpenCore.efi and the computer's boot menu shows OC and connected disks.
- `Misc > Boot > LauncherOption = Full`
- `Misc > Boot > LauncherPath = Default`
#### Disable
Computer's boot menu shows connected disks but OC does not write its own entry into BIOS
- `Misc > Boot > LauncherOption = Disabled`
- `Misc > Boot > LauncherPath = Default`

### Hide OpenCore
In the `config.plist` modify next line to show or hide the Boot Menu, I personally do not recommend this due to some cases a macOS update could break something.
- `Misc > Boot > ShowPicker = False`
- `Misc > Boot > UsePicker = True`

### Change OpenCore background color
We have to modify with hex valor the backgrounds color. Modify and change the value with the color you want (e.g. BFBFBF)
- `NVRAM > Add > 4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14 > DefaultBackgroundColor > <000000>`

### Change NVRAM size
By default It is recommend to use 1.5Gb, even macbook Pro use the same size, but If you are who likes to using more, read next Reddit post about It.

- [Whatevergreen iGPU VRAM allocation via framebuffer-unifiedmem](https://www.reddit.com/r/hackintosh/comments/gp07ko/whatevergreen_igpu_vram_allocation_via/?utm_source=share&utm_medium=android_app&utm_name=androidcss&utm_term=1&utm_content=2)

### Hide EFI partitions
During the bootloader, EFI partitions may be showed. Hiding this is easiest as adding `.contentVisibility` file into **BOOT** and **OC** folders. By default this is enabled in this configuration.

### Disable smoothing font
Smoothing font can exhaust the eyes in 1366x768 display, disable It with next command.
```sh
# disable font smoothing
defaults -currentHost write -g AppleFontSmoothing -int 0

# enable default smoothing. You can also use "1" for light smoothing and "3" value for strong smoothing.
defaults -currentHost write -g AppleFontSmoothing -int 2
```


<br/>

## Special Thanks 🙌

A big thank you to the following references and projects that made this hackintosh project possible:

- [Dortania Guide](https://dortania.github.io/OpenCore-Install-Guide/)
- [pierpaolodimarzo](https://github.com/pierpaolodimarzo/ThinkPad-T480)
- [valnoxy](https://github.com/valnoxy/t480-oc)

Your support and contribution are greatly appreciated! 👏
