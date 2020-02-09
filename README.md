# KeyGoose
###### Hacked By Me [Video 1]((http://KeyGoose.HackedBy.Me "Video 1") Code
------------
##### KeyGoose is a fun implementation of a BadUSB, where a keyboard is "hacked" by wiring a microcontroller with custom firware to the inside of the keyboard. The microcontroller is designed to randomly deliver a payload (in the form of the [Desktop Goose Game](https://samperson.itch.io/desktop-goose "Desktop Goose Game")) to unsuspecting users in only 2.25 seconds.
------------
### What's Included in this Repository?
- A KeyGoose Parts List
- Verbal instructions as to how to create your own KeyGoose
- A technical explanation of how KeyGoose works
- The KeyGoose code (both the Driveby and Keyboard implementation) with which to flash the microcontroller
- The powershell script fetched and run by the KeyGoose upon payload injection

## Parts List
- A USB Keyboard
	- I recommend getting the cheapest keyboard you can find on Amazon. Not only will this bring-down the cost, but the cheaper keyboards tend to be more maleable and made of a weaker plastic, making the keyboard easier to manipulate when it comes to squeezing-in the microcontroller.
	- I also found that getting a keyboard that has a battery option was ideal, as it left a nice, open area in which to embed the microcontroller.
- A MicroController with USB Support
	- Any microcontroller should do, as long as it is small-enough and supports USB. In this project, I used a *Beetle ATmega32u4 Development Board*
- General Tools
	- Raspberry Pi with GPIO wires
		- You may use any other preferred method to flash the microcontroller, this is simply the option I went with, and will be explaining here.
	- Solder/Soldering Iron
	- Screw Driver
	- File Set
	- 26 AWG Wire
	- Wire Stripper
