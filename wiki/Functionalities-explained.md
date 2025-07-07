What does the options and selections do whan I press something?

# SD
Here you can navigate through your SD files and take the following actions:
- Install (if it is a .bin file)
- New Folder: Create a new folder 
- Rename: rename a file or folder
- Copy: Saves the file path into memoy to paste when demanded
- Delete: deletes a file or folder
- Paste: create a copy of the pervious file selected into another folder

# OTA (Over the Air)
- Browse through the firmwares available on M5Burner servers, once selected, you
- Browse through firmware versions, here you can choose between Install directly or Download the binary to your SD Card.

# WUI (Web User Interface)
- Here you can Manage the files in your SD Card
  - Create folder
  - Delete Files and folders
  - Rename Files and Folders
  - Download Files
  - Upload files (multiple files)
  - Install Binaries you have in your SD Card
- OTA Install: Install binaries you have in your computer or smartphone without SD card.. UIFlow won't work, yet.

# Settings:
- Brightness: ajust the bright of the screen
- Only Bins & All Files: Set the files you can see in the SD menu
- Avoid & Ask Spiffs: When installing a firmware, it will ask or not if you want ot update the SPIFFS/LittleFS partition
- Part Change: Change the Partitions of the device, to install huge apps or UIFlow, in case of Cardputer and StickCPlus2 (Obs.: Restart is needed to apply the new partition scheme)
- Part List: Check the actual partitions
- Clear FAT: Erase all FAT Partitions.
- Bkp SPIFFS: Save a copy of your SPIFFS partition into the /bkp/spiffs.bin file
- Bkp FAT vfs: Save a copy of your FAT vfs partition into the /bkp/fat_vfs.bin file (This is where the UiFlow and MicroHydra flash files are stored)
- Restore SPIFFS: Option to recover a previous SPIFFS backup file (you can choose)
- Restore FAT FS: Option to recover a previous FAT backup file (you can choose)
- Restart: Soft reset.

