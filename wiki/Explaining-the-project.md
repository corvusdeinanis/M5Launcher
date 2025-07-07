# M5Stick-Launcher

This application was made for those, like me, who want to be able to switch programs in your device quickly and anywhere, without the need to plug into a computer and do many steps.

M5StickCs and Cardputer are awesom devices, and i always felt stuck in one application only, needing to go home or plug the device into some PC to change its application.

# How does it works
Imagine M5Launcher as the TWRP is for android devices, an application that runs everytime you turn your device on, and would not show its true colors unless it is demanded.

So it is allocated in a separated part of your device, and run every time you turn it on (restarting the device from software won't make ir run, will explain it later), letting you choose: continue to what you have installed or install a new application in it place (not the launcher, the app you previously installed using the launcher).

The M5Launcher can be divided into three major problems: the Partitioning, the Bootloader and the app itself.

## Application
M5Launcher uses the "Update.h" library, from espressif, to install binary files placed on the SD Card into an OTA partition, as if it was installing Over The Air (OTA), but from SD card. 

There is nothing special with this process when we are trying to install binaries that get from your Arduino IDE projects, or PlatformIO, or ESP-IDF. These binaries are the compiled program (~project/build/your-board-name/project.ino.bin), starting with the Magic Numbers [E9] (use HxD software to open a .bin file and these should be the first tree numbers you will see in Hexadecimal).

The real problem is shown when we try to install the files downloaded from the internet, such as Github projects (like this one) and M5Burner.

These files can be one out of two things: A _Simple Merged Binary_ or a _memory dump_.

### Simple merged binary
These files are the composition of the bootloader.bin (starting from 0x1000), partitions.bin (starting from 0x8000) and apllication.bin (starting in 0x10000).
The space from 0x0 until 0xFFF (Except in ESP32-s3 - Cardputer), and the spaces in between the bootloader endfile and 0x7FFF, is filled with 0xFF, and the application will start from the 0x10000 offset, and you can see that with HxD at that position, and the file will end at the end of the application.bin no matter what is its size.

### Memory Dump
These files are 4Mb (or 8Mb, 16Mb) sharp in size, and the spaces in between 0x0 and bootloader, partitions, application will be filled with what was on the device it was dumped.

So here we have a huge problem! there are no ways to knoow for sure what is the real size of the application inside the partition...

What should we do, now?!? easy.... We copy the whole thing until the end of the partition.

For that we look deep into the .bin file and extract a few key positions (0x804A and 0x804B), that will tell us the size of the partition.

And we will "crop" the original binary from 0x10000 (app start position) until the end of the partition.

### Common problem
To M5Stack-SD-Updater work properly, the application can't be larger than the partition it will be installed on, for that I neede to setup a custom partition table to allocate the most common and desired apps, such as Nemo (Huge Apps) as Marauder (min spiffs with 2 OTA).


## Custom Partitioning
In order to make the launcher work, we must take good care of the partition table.

To allocate the most desired applications I made as follows:

4Mb partition Table

| # Name   |   Type | SubType  |  Offset  |   Size   | Size  | For What     |
| -------- | ------ | -------- | -------- | -------- | ----- | ------------ |
|  nvs     |   data |  nvs     |   0x9000 |  0x5000  |   -   | System       |
| otadata  |   data |  ota     |   0xe000 |  0x2000  |   -   | System       |
| test     |    app |   test   |  0x10000 | 0x170000 | 1,47Mb| Launcher App |
| app0     |    app |   ota_0  | 0x180000 | 0x260000 | 2,4Mb | Application  |
| spiffs   |   data |  spiffs  | 0x3E0000 | 0x20000  | 128kB | Obs2         |

8Mb partition Table

| # Name   |   Type | SubType  |  Offset  |   Size   | Size  | For What     |
| -------- | ------ | -------- | -------- | -------- | ----- | ------------ |
|  nvs     |   data |  nvs     |   0x9000 |  0x5000  | 32kb  | System       |
| otadata  |   data |  ota     |   0xe000 |  0x2000  | 8kb   | System       |
| test     |    app |   test   |  0x10000 | 0x180000 | 1,53Mb| Launcher App |
| app0     |    app |   ota_0  | 0x190000 | 0x4E0000 | 4,9Mb | Application  |
| vfs      |   data |  FAT     | 0x670000 | 0x80000  | 512kb | Obs1         |
| spiffs   |   data |  spiffs  | 0x6F0000 | 0x100000 | 1Mb   | Obs2         |
| coredump |   data | coredump | 0x7F0000 | 0x10000  | 64kb  | System       |


16Mb partition Table

| # Name   |   Type | SubType  |  Offset  |   Size   | Size  | For What     |
| -------- | ------ | -------- | -------- | -------- | ----- | ------------ |
|  nvs     |   data |  nvs     |   0x9000 |  0x5000  | 32kb  | System       |
| otadata  |   data |  ota     |   0xe000 |  0x2000  | 8kb   | System       |
| test     |    app |   test   |  0x10000 | 0x1F0000 | 1,9Mb | Launcher App |
| app0     |    app |   ota_0  | 0x200000 | 0x800000 | 8,0Mb | Application  |
| sys      |   data |  FAT     | 0xA00000 | 0x100000 | 1Mb   | Obs1         |
| vfs      |   data |  FAT     | 0xB00000 | 0x200000 | 2Mb   | Obs1         |
| spiffs   |   data |  spiffs  | 0xD00000 | 0x2F0000 | 2,9Mb | Obs2         |
| coredump |   data | coredump | 0x7F0000 | 0x10000  | 64kb  | System       |

Obs1: FAT partition to allow micropython apps (UIFlow, CircuitPython and MicroHydra) work.
Obs2: SPIFFSs partition, to work with applications such as OrcaOne.

Look that the `app0` partition:
* it is 4,9MbMb in size on Cardputer and M5StickCPlus2, to accommodate Huge apps like Cardputer Demo app
* 2,4Mb in size on M5StickC and M5StickCPlus, that can acommodate almost all firmwares in the M5Burner 

Look to that `test` partition, there is where our launcher resides, look that it have limited size in 4Mb devices (M5StickC and Plus), Loucher is using 99% of its capacity, so the Launcher will not have new functionalities there.

## changing Partitions
Now you can change the partitions settings on Cardputer and StickCs. to do that you must:
- Navigate to CFG (4th icon to the Right)
- Choose "Part Change"
- Choose the partition scheme you desire

Options:
StickC Plus 2 and Cardputer (8Mb flash)
- Default
- Doom (6.4Mb of APP partition)
- UIFlow (4,5Mb App partition, 1Mb FAT sys and 0,9Mb FAT vfs)

StickC and StickC Plus (4Mb flash)
- Default
- Orca (2Mb APP partition and 512kb Spiffs)

Core and Core2 (16Mb flash)
- Default
* Doesn't need any other option atm.

## Bootloader
I've programmed a custom bootloader to read the "reboot_reason" and then choose what it will do:
* If reboot reason is `POWERON_RESET`, meaning the it was just turned on, bootloader will choose "test" partition, that will show the splash screen and, if nothing is done, it will do ESP.restart() and restart the device
* It leeds us into `SW_CPU_RESET` reboot reason, that will let bootloader choose the last OTA valid partition with app.

These checks can be done by the application, but is impossible to return to the Launcher unless you change the other application to it, what we don't want to do.





