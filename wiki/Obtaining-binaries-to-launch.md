# Where can I find binaries to launch?

You can find the binaries to launch in many places, and I will teach you the most common: 
* M5Burner
* GitHub project release pages
* Compiling your own projects
* [Downloading from here](https://bmorcelli.github.io/Launcher/m5lurner.html)

## M5Lurner (the M5Burner repository outside M5Burner)
Access this [Page](https://bmorcelli.github.io/Launcher/m5lurner.html) and browse the firmwares available to download
Don't forget to rename the files to better organize yourself.

## Using M5Burner
* Download M5Burner from its site: https://docs.m5stack.com/en/download

![image](https://github.com/bmorcelli/Launcher/assets/104320209/d810c52d-108b-49c5-93be-86fe369ecec1)

* Unzip it and run "M5Burner.exe"
* Choose the App you want to use and click "Download"

![image](https://github.com/bmorcelli/Launcher/assets/104320209/38811259-e6bd-47db-b077-b706a09c230c)

* In the folder where "M5Burner.exe" is, enter in "packages" and "firmware", your binary will be there with a strange name

![image](https://github.com/bmorcelli/Launcher/assets/104320209/0a885f82-a614-4358-8ed5-31b49431bd9b)

* Rename it and copy into your SD Card

## Using GitHub project Releases
* In the github page of the project you like, sometimes the dev shares the precompiled binaries
 for example, enter in my fork of Nemo Firmware: https://github.com/bmorcelli/m5stick-nemo
* Look for "releases"

![image](https://github.com/bmorcelli/Launcher/assets/104320209/a33434e8-d0ea-437e-afde-5f54c26e4a0b)

![image](https://github.com/bmorcelli/Launcher/assets/104320209/bb88b5d0-2330-4f5d-9a9f-3cd131b07078)

* Download the file from there and copy to SD Card

## Compiling your own projects
### Arduino IDE Users

* Open your Your_Project_Name.ino file
* Setup the board you will use
* On menu, choose: Sketch -> Export Compiled Binaries

![image](https://github.com/bmorcelli/Launcher/assets/104320209/94fc37bd-e40e-4c12-ad86-8073c772b525)

* If it compile your project without errors, you will find a folder called "build" inside your project folder.

* Inside this folder you will find some binary files, you will copy the "Your_Project_Name.ino.bin" to your SD Card

![image](https://github.com/bmorcelli/Launcher/assets/104320209/4ff4dae4-c754-4994-8fff-4d6a5fd89072)

### VSCode and PlatformIO/ESP-IDF
* It is similar to the steps of Arduino IDE, after you build your project, the binary will be in the "build" folder.
