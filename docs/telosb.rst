*********************************
So I have 400 TelosB's, Now What?
*********************************

So you remember Travis Goodspeed right? Well, I have about 400 MSP430 boards called TelosB that should be in his hands... Hamvention 2017...

http://goodfet.sourceforge.net/hardware/telosb/

Install binutils-msp4340, gcc-msp430, msp430-libc, and msp430mcu from AUR (abdullatif).

..code-block:: sh

	pacman -S mspgcc
	pacman -S binutils-msp430
	pacman -U binutils-msp430-2.21.1a-1-x86_64.pkg.tar.xz
	pacman -U gcc-msp430-4.6.3-1-x86_64.pkg.tar.xz
	pacman -U msp430-libc-20120224-1-x86_64.pkg.tar.xz
	pacman -U msp430mcu-20120406-1-any.pkg.tar.xz

	clone git@github.com:travisgoodspeed/goodfet.git

	cd goodfet/firmware
	export board=telosb
	make clean all

	../client/goodfet.bsl --telosb -c /dev/ttyUSB0 -r -e -I -p goodfet.hex

If all went well well there should now be a `goodfet.hex` file. Bad news is the goodfet bootstrap loader (BSL) responds with BSL Exception "NAK received (wrong password?)". For now that's okay as our good friend @rho in Texas let us know that the RIOT-OS bootloader appears to work just fine.

..code-block:: sh

	git clone git@github.com:RIOT-OS/RIOT.git

	./RIOT/dist/tools/goodfet/goodfet.bsl --telosb -c /dev/ttyUSB0 -r -e -I -p goodfet/firmware/goodfet.hex 
	MSP430 Bootstrap Loader Version: 1.39-goodfet-8
	Mass Erase...
	Transmit default password ...
	Invoking BSL...
	Transmit default password ...
	Current bootstrap loader version: 1.61 (Device ID: f16c)
	Changing baudrate to 38400 ...
	Program ...
	9170 bytes programmed.
	Reset device ...

Sweet!

..code-block:: sh

	# pacman -S virtualenv2

	$ cd goodfet
	$ virtualenv2 venv
	$ source venv/bin/activate
	$ pip install pyserial

	$ /dev/ttyUSB0
	Found   CC2420
	Freq:   2405.000000 MHz
	Status: 
	(venv) [paretech@osprey goodfet]$ ./client/goodfet.spiflash info
	Ident as MISSING M25P80
	Manufacturer: ff MISSING
	Type: ff
	Capacity: ff (1048576 bytes)

	$ ./client/goodfet.ccspi info
	ON: /dev/ttyUSB0
	Found   CC2420
	Freq:   2405.000000 MHz
	Status: 

So what does this all mean? Well there is work to be done on the BSL loader that comes with goodfet but that from RIOT is immediately useful. Also, the goodfet firmware was successfully built, the goodfet client executes and is able to query the telosb hardware. Note too shabby for a few minutes of work! Some notes were even made in the process! Good job...

It's kind of interesting, the goodfet, TinyOS, and RIOT-OS all appear to be using some variation of the same BSL. They all look like really terrible python... However, the BSL from both TinyOS and RIOT-OS appear to work!



References
==========

#. http://www2.ece.ohio-state.edu/~bibyk/ee582/telosMote.pdf
#. https://github.com/travisgoodspeed/goodfet
#. http://goodfet.sourceforge.net/hardware/telosb/
#. https://github.com/RIOT-OS/RIOT
#. https://riot-os.org/
#. https://github.com/tinyos/tinyos-main
#. http://tinyos.net/
#. http://travisgoodspeed.blogspot.com/2011/03/goodfet-on-telosb-tmote-sky.html
#. http://www.pages.drexel.edu/~kws23/tutorials/motes/motes.html

The Next Chapter
================

1. Get to TelosB's talking with each other
2. Write a TelosB only BSL in Python3 (not Python2).

Making the Switch
=================

