# Hackintosh-Gigabyte-Z390-GAMING-X-i7-9700k
Procedure and some source file including EFI used when I install hackintosh Catalina 10.15.1

## Configuration

### Hardware 

|   |  Type |
| :------------: | :------------: |
| Montherboard | Gigabyte Z390 Gaming X  |
| CPU |  I7-9700K |
| AMD Graphics Card | Sapphire RX590 8G |
| RAM |  16GB 3200MHz DDR4 X 2 |
| Solid State Drives | SAMSUNG 970 EVO NVMe M.2 500GB |


### Operating System
macOS Catalina 10.15.1

### BIOS Version
- 

### Current Issues
- Cannot shut down normally. Coredump when shut down

## Steps to install

### BIOS Setting
1. Load __Optimized Defaults__
2. Disable __VT-d__
3. Close __CSM Support__
4. Disable __Secure Boot Mode__
5. Close __IO Serial__
6. Enable __XHCI Handoff__
7. Set OS type to __Other OS__
8. Disable *__intel graphics__*
9. Save and Exit

### Create a Bootable USB and Install
1. Download `dmg` from [here](https://blog.daliansky.net/macOS-Catalina-10.15.1-19B88-Release-version-with-Clover-5098-original-image-Double-EFI-Version.html).
2. Using `etcher` to create a bootable USB with above `dmg`. If the initial EFI cannot work for you, try to change it with `Mine-EFI`. I have tried a lot, this is the version with whom I successed. I have also provided other versions of EFI I have tried in this repo to offer you a chance to try again. __Inserting the `Bootable USB` in USB 2.0 slot may contribute to a successful installation.__
3. *If you stall at the last two minutes of 1st installation phase. Try delete `AptioMemoryFix.efi` or use `v1`, `v2`, `v3`, `free200`, etc. It's maybe wrong of EFI about memory. According to [this](http://bbs.pcbeta.com/viewthread-1799596-2-1.html), try `OsxAptionFix3Drv-64.efi` or `OsxAptioFixDvr-64.efi` instead of `AptioMemoryFix.efi`. __I solved this problem by deleting `OsxAptionFixDrv-64.efi`__. Even if it still stalls at last two minutes of 1st installation phase, wait enough time and reboot physically, I have entered into MacOS successfully.* 
4. After installation successed, replace the EFI floder of `Hackintosh HD`'s EFI partion with that of `Bootable USB`'s. `Clover Configration` app can ease this step. _Remember to make a backup before replacement and don't change content of `Bootable USB`._

## Dual System With Ubuntu
__The `Bootable USB` in above steps still needed__
1. Install Ubuntu
- Install with `something else` method
- Install `Boot loader` on the MacOS's EFI partition
- Add a partition to mount point `/`

2. Recover MacOS EFI
- After Ubuntu installation finished, reboot and boot from previous `Bootable USB`. Check the changed files in MacOS's EFI partion by compare the size of files in `Bootable USB`'s and `Hackintosh HD`'s. Through my practice, replace `BOOT` floder in `Hackintosh HD`'s EFI partition with that of `Bootable USB`'s.

3. *The last thing is to chang the boot order in BIOS to boot from `clover`*
4. If the `clover` GUI cannot see Ubuntu, enter into MacOS and use `clover configuration` to modify `config.plist` checking the `gui`>`scan`>`linux`. Then save and reboot to see ubuntu in `clover` GUI.

## Ohter Method
I previously successed with the following method but failed to do it again. I guess that I havn't biult the EFI partition correctly.
According to this [website](https://www.tonymacx86.com/threads/how-to-create-a-macos-catalina-public-beta-installation-usb.278188/):
After download macOS Catalina in a real Mac,
1. Insert the USB drive
2. Open `/Applications/Utilities/Disk Utility`
3. Highlight the USB drive in left column
4. Click on the Partition tab
5. Click Current and choose 1 Partition
6. Click Options...
7. Choose GUID Partition Table
8. Under Name: type USB (You can rename it later)
9. Under Format: choose Mac OS Extended (Journaled)
10. Click `Apply` then Partition
11. Open `/Applications/Utilities/Terminal`
12. Run
```bash
sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/USB /Applications/Install\ macOS\ Catalina.app --nointeraction
```
13. Download the most recent standalone `Clover` installer from the download section.
14. Install `UEFI` or Legacy Clover version using the USB (Install macOS Catalina) as the target.
15. Navigate to `/EFI/CLOVER/` and replace `config.plist` with example `config.plist` (attached below)
16. Navigate to `/EFI/CLOVER/kexts/Other/` and add `FakeSMC.kext`
17. (Optional) Navigate to /EFI/CLOVER/kexts/Other/ and add your ethernet kext
18. (Optional) Navigate to /EFI/CLOVER/kexts/Other/ and add NullCPUPowerManagement.kext
