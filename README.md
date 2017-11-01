
macOS on DELL XPS13 (9350)
====================================
This project targets at giving the relatively complete functional macOS for XPS13 9350. Before you start, there's a brief introduction of how to finish powering up macOS on your laptop:

Need BETA Testers
----------------
Please Read: https://github.com/ZombieTheBest/XPS9350-10.13-HighSierra/issues/6

Before installing macOS
----------------
Since the original WiFi module is not compatible with macOS, you need to buy a DW1560 (recommended) or DW1830 module.
Then follow this [guide](https://www.ifixit.com/Teardown/Dell+XPS+13+Teardown/36157) to swap it.

macOS installation
----------------
1. Create a vanilla installation disk (USB or other removable disk). (Google how to)
2. Install Clover with UEFI only and UEFI64Drivers to the installation disk just created.
3. Replace the original CLOVER folder with the one under my Git/XPS9350-macOS/CLOVER.
4. Change BIOS settings to the following
  * Disable Secure Boot
  * Set SATA Operation to AHCI
  * Disable FastBoot
5. Boot to USB and hit F4 and/or Fn-F4 to capture ACPI tables to the USB
6. Install macOS but do not reboot until you have finished steps 7 and 8, below.
7. Install Clover with UEFI only and UEFI64Drivers to SSD.
8. Replace the original CLOVER folder on the SSD with the one from the USB drive under my Git/XPS9350-macOS/CLOVER.
9. Once you finish installation of macOS, you can do the following steps to finish the post installation of macOS


How to use deploy.sh?
----------------

Download the latest version installation package/directory by entering the following command in a terminal window:

```sh
git clone https://github.com/ZombieTheBest/XPS9350-10.13-HighSierra.git
```
This will download the whole installation directory to your current directory(./) and the next step is to change the permissions of the file (add +x) so that it can be run.


```sh
cd XPS9350-macOS
chmod +x ./Deploy.sh
```


Run the script in a terminal windows by(Note: You should dump the ACPI tables by pressing F4/Fn+F4 under Clover first and then execute the following command lines):

```sh
./Deploy.sh
```

Reboot your macOS to see the change. If you have any problem about the script, try to run deploy in DEBUG mode by 
```sh
./Deploy.sh -d
```

Note:
- ```VoodooPS2Controller``` is recommended on macOS ```Sierra``` and ```High Sierra``` (10.```12+```)
- ```AppleSmartTouchPad``` users(recommend on 10.```11-```): for two finger scrolling you need to change the speed of the Scrolling once to get it work and also have to enable them in Trackpad preferences. 

Known Issues
----------------
- Minor Bluetooth / HandOff issues (need more feedback)
- SD Card Reader not working on Hackintosh (no solution at the moment)

TODO List
----------------
- Nothing to do

Change Log
----------------
2017-11-01
- Fixed SSDTs compile error caused by new Clover version (c) @squash- @ZombieTheBest

2017-10-29 - ```High Sierra STABLE```
- High Sierra STABLE Deploy
- Fixed Backlight issue

2017-10-26 
- Fixed Deploy.sh code (Thank you @chenyisen!)
- Removed SSDT-USBX + SSDT-EC as they were already patched in original Deploy.

2017-10-24 - ```High Sierra BETA Update```
- Updated Deploy.sh
- Corrected the PixelClock Patch for QHD Screens on High Sierra. Old patch causes Clipboard Crash, restore Vanilla CoreDisplay.framework to fix that issue.
- Added SSDT-EC + SSDT-USBX for correct USB Power Management
- Added SSDT-ALC256.aml 
- Updated config.plist with new High Sierra Patches
- Fixed High Sierra Bluetooth issue by renaming AirPortBrcmNIC-MFG.kext in AirPortBrcmNIC-MFG.bak


2017-09-04

- Fixed Wi-Fi slow issue after sleep by correcting ```_UPC``` value from ```0x03``` to ```0xFF``` (c) @ZombieTheBest 
- Added ```-disablegfxfirmware``` to avoid firmware load into endless loop
- Added working combo jack for ALC256 on XPS9350/XPS9360 (c) @MuntashirAkon
- Added drivers to fix keyboard with FileVault (c) @vaites

2017-03-13

- Correct 2finger tap value (c) syscl
- Mute VoodooPS2 spam log (c) syscl
- Correct tochpad resolution for ```XPS 13(Broadwell/Skylake/Kabylake)``` (c) syscl
- Sync to @icedman latest VoodooPS2 source 
- Use SSDT to power up USB, deprecate kernel extension injector (c) syscl
- Added Apple ```Super Drive``` support for each USB port (c) syscl
- ```darkwake``` to fix lid wake require extra key press issue 
- Disable ```autopoweroff```

2017-02-22

- ```X86PlatformPluginInjector``` ready (c) syscl
- Prevent patching RecoveryHD per call (c) syscl
- Sync fixUSB to latest version: 2016-12-08 with speed optimization 

2017-02-21

- X86PlatformPluginInjector method is ready for both ```Kabylake``` and ```Skylake``` XPS 13 model (c) syscl
- Updated port injector to support ```MacBook8,1```, ```MacBookPro13,x``` (c) syscl
- Updated sleep: add deep sleep & deep idle properties as per Pike R. Alpha

2017-02-11

- HWP full power management(CPU + iGPU) is ready credit syscl, Pike
- Tiny SSDT-pr for all XPS 13(Skylake/Kabylake) to active HWP is ready credit dpassmor, we don't need to use ssdtPRGen anymore
- Dell SMBIOS truncate issue fixed credit David Passmore

2017-02-03

- Inject more Audio properties(see ```System Report``` -> ```PCI```) for ALC3246 credit @Mirone
- Inject ```ARPT``` and deprecate ```PXSX``` for RP05(Wi-Fi device on XPS 13 9350/9360)
- Cleanup and minor bugs fixed in deploy

2017-01-27

- Inject Device(USBX) with properties and _DSM method rewrite credit syscl
- Fix shutdown become reboot issue #29
- Added _SSD, GTF0 method credit syscl
- Added IONVMeFamily Preferred Block Size 0x10 -> 0x01 credit Pike R. Alpha implement by syscl 
- Updated port0000 setting for 0x19260004 credit syscl

2017-01-25

- Enable IOPCIFamily to set tolerance latency for PCI devices (c) syscl
- Prepare for @icedman 's VoodooPS2Controller(tuned by syscl)
- Fixed RP0X rename issue

2017-01-24

- Inject SsdtS3 from MacBook credit syscl's stripple down
- Added port injector for XHC/EHC credit syscl

2017-01-22

- Inject NVMe power management properties credit Pike R. Alpha("use-msi", "nvme-LPSR-during-S3-S4") syscl("deep-idle")

2017-01-17

- Use refs.txt to try to fix issue #18 credit @x4080 who reminded to use refs.txt
- Updated rebuild kernel cache command

2016-12-19

- Remove _PRW, _PSW in PWRB(PNP0C0C)
- RP09::ARPT->RP09::SSD0 (c) syscl
- Remove pnp0c14
- _T_2 ->T_2 
- Rename definition block

2016-12-18

- Fixed update/sync file issue

2016-12-17

- Rename HDAS->HDEF for layout to inject successfully
- PBTN->PWRB

2016-12-16

- Updated FaceTimeHD keys from MacBookPro13,2
- Cleanup redundant kexts
- Sort kexts and sync 10.11's kexts
- _PWR val fixed
- PXSX to ARPT

2016-12-15

- HWP is ready
- Boot issue should go away; xh_rvp07 must be included

2016-12-14

- Fixed audio injection bug causes by Clover
- Updated 3 cpu models i5-6200, i7-6500, i7-6560 SSDT-pr
- Resolve hp distortion and static noise
- Optimization CC credit syscl
- Updated BT driver for DW1830
- Fixed stuck of some function in deploy
- Fixed SSDT-pr installation issue

2016-12-13

- Auto correct reset val and address from FACP
- Added HDMI property
- Updated DisplayLink Driver to 2.6.0

2016-12-12

- Fixed/refined DSDT patch
- Reading FACP to correct reset value and reset address credit syscl
- Remove Glan patch

2016-12-11

- Fix MDBG issue credit syscl
- Fix word field mismatch issue credit syscl
- Fix brightness key function credit azlvda, tdmsn

2016-12-10

- Refined lid wake issue: 0a -> 0f for 0x19260004 credit syscl

2016-12-08

- Sync fixUSB to latest version: 2016-9-17

2016-12-07

- Fixed the Sunix camera issue I mentioned before credit syscl

2016-12-04

- Fixed boot issue by ```AddClockID``` and ```FixOwnership```
- Fixed syntax error 
- Correct frame buffer name for SKL

2016-12-01

- Inject PMCR then macOS will combine PPMC together to make AppleIntelPCHPMC load correctly credit syscl

2016-11-29

- Fixed reboot unstable issue and speed up reboot progress ```ResetAddress = 0xB2``` and ```ResetValue = 0x73``` credit syscl

2016-11-22

- Added lspci to list all pci devices

2016-11-20

- Correct SKL laptops' internal display connector type from LVDS to eDP credit syscl
- Added lid wake auto patches for deploy.sh
- Inject SSDT-XPS13SKL.aml

2016-11-19

- PPMC -> PMCR
- Insert DMAC for macOS credit syscl
- Make Device (MATH) load correctly credit syscl
- SBTN -> SLPB with _STA 0x0B credit syscl
- Eliminate SgRef

2016-11-18

- Fixed HD520 glitches on Sierra credit Pikeralpha 
- Eliminate redundant acpi table & patches

2016-11-17

- Fixed brightness save issue: ```EmuVariableUefi-64``` + ```RC.Script```

2016-11-15

- Fixed screen blank when resume from sleep credit syscl/lighting/Yating Zhou

2016-11-12

- First commit 

