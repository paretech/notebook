**********
Arch Linux
**********

TO-DO List
==========

#. Restore laptop image to VM
#. Install Printer
#. Install and configure RAID
#. Sound Proxy
#. Dotfile Management
#. Font management
#. Spotify Client
#. xrandr dual monitor configuration
#. i3-WM configuration files
#. terminal color theme
#. sublime text
#. Install (and share) printer
#. System Fonts
#. Syslinux linux and configure mkinitcpio
#. timezone
#. install virtualbox
#. i3 dmenu customization
#. pycharm
#. timezone (locale)
#. Alt+Tab i3 module to return to previous focus
#. Media buttons for volume control and song control.
#. Switch from SYSLINUX to EFISTUB
#. Implement Automatic copy of vmlinuz-linux to vmlinuz-linux on linux update.
#. Need some simple redshift like program.
#. alias ll="ls -la --color=auto"


Initial RAMDisk Environment (mkinitcpio)
============================

.. WARNING::
	Don't take a kernel update if mkinitcpio is not sorted or if not ready to reboot immediately. System instability is likely to occur. 
	
If using EFI boot system (as opposed to BIOS) then you must create an EFI System Partition. Arch documentation often refers to this partition as ``esp``.

After creating the EFI System Partition, you must choose how it will be mounted. The simplest option is to mount it at /boot or bind mount it to /boot since this allows pacman to directly update the kernel that the EFI firmware will read. If you elect for this option, continue to #Booting EFISTUB [#arch_efistub_systemd]_.

Note: You can keep kernel and initramfs out of ESP if you use a boot manager which has a file system driver for the partition where they reside, e.g. rEFInd.


mkinitcpio is the next generation of initramfs creation. mkinitcpio is a Bash script used to create an initial ramdisk environment [#arch_mkinitcpio]_. 

From the mkinitcpio(8):

    The initial ramdisk is in essence a very small environment (early userspace) which loads various kernel modules and sets up necessary things before handing over control to init. This makes it possible to have, for example, encrypted root file systems and root file systems on a software RAID array. mkinitcpio allows for easy extension with custom hooks, has autodetection at runtime, and many other features.

The kernel, once it is loaded, finds init in sbin and executes it. When init starts, it becomes the parent or grandparent of all of the processes that start up automatically on your Linux system. The first thing init does, is reading its initialization file, /etc/inittab. This instructs init to read an initial configuration script for the environment, which sets the path, starts swapping, checks the file systems, and so on. Basically, this step takes care of everything that your system needs to have done at system initialization: setting the clock, initializing serial ports and so forth [#tldp_boot_process]_.

mkinitcpio is a modular tool for building an initramfs CPIO image [#arch_mkinitcpio]_. The ramdisk environment produced by mkinitcpio uses BusyBox for common UNIX utilities.

By default, the mkinitcpio script generates two images after kernel installation or upgrades: a default image, and a fallback image that skips the autodetect hook thus including a full range of mostly-unneeded modules. This is accomplished via the preset files which most kernel packages install in /etc/mkinitcpio.d (e.g. /etc/mkinitcpio.d/linux.preset for linux). 

A preset is a predefined definition of how to create an initramfs image instead of specifying the configuration file and output file every time. The -p switch specifies a preset to utilize. For example, mkinitcpio -p linux selects the preset provided by the linux package.

An additional configuration file is located at /etc/mkinitcpio.conf and is used to specify options global to all presets. The --allpresets switch specifies that all presets should be utilized when regenerating the initramfs after a mkinitcpio.conf change. 

Users may create any number of initramfs images with a variety of different configurations. The desired image must be specified in the respective boot loader configuration file. 

WARNING: Preset files are used to automatically regenerate the initramfs after a kernel update; be careful when editing them.

There are a couple of options [#arch_efistub_systemd]_:
	1. Blind mount ``esp`` to ``/boot`` so that the boot manager can access the kernel and intitramfs(s) (primary and fallback).
	2. Use a boot manager that has file system driver so that the kernel and initramfs can be accessed.
	3. Copy boot files (kernel and initramfs) to ``esp`` and use some other mechanism (systemd event triggers, mkinitcpio hook)to keep the files up to date as kernel updates become available.
	   
For now, a method of ``mkinitcpio`` hook is used that rights the boot files directly to ``esp``.

.. [#arch_mkinitcpio] https://wiki.archlinux.org/index.php/mkinitcpio
.. [#tldp_boot_process] http://www.tldp.org/LDP/intro-linux/html/sect_04_02.html
.. [#arch_efistub_systemd] https://wiki.archlinux.org/index.php/EFISTUB#Using_systemd

The Terminal
============

After being frustrated with configuration of xterm looked into other terminals (urxvt, st). Ultimately settled on Termite after its high reviews, lauded as being ideal for tiling windows manager (WM) and VIM like nature. So far very happy. The config is managed via a text file and can be reloaded at runtime! All bit ++++! 

Slant.co has a nice comparison going on that was instrumental to discovering Termite! https://www.slant.co/versus/2445/2462/~xterm_vs_termite

Configuration instructions from Arch Termite page were more than enough to get started. Configured to usable point in minutes. Installed Anonymous FFT Font via pacman. See ``pacman -Ss font`` and ``fc-list`` to make obtaining and configuring fonts easier than past experiences. See also ```pacman -Ss font | grep -i mono -B 1```.

After some experimentation, settled on Inconsolata. Though there are some odd renderings in Sublime Text-3. Does not appear to be maintaining monospace. In particular, the ``y`` appears to be colapsing and ``J`` is even worse....

Python on Arch
==============
If you must use pip, use a virtual environment (python -m venv) or with pip install --user to avoid conflicting with packages in /usr. It is always preferred to use pacman to install software. To help keep your system managed as intended, do not even install python-pip via pacman to minimize accidental out venv package installs. Once a python venv environment is activated (source ./bin/activate) pip will be available in the local virtualized context. 

Manual Sections
===============

The number corresponds to what section of the manual that page is from; 1 is user commands, while 8 is sysadmin stuff. The man page for man itself (man man) explains it and lists the standard ones:

MANUAL SECTIONS
    The standard sections of the manual include:

    1      User Commands
    2      System Calls
    3      C Library Functions
    4      Devices and Special Files
    5      File Formats and Conventions
    6      Games et. al.
    7      Miscellanea
    8      System Administration tools and Daemons

    Distributions customize the manual section to their specifics,
    which often include additional sections.

Source: https://unix.stackexchange.com/questions/3586/what-do-the-numbers-in-a-man-page-mean

Customizing the Terminal
========================

A nice and rather lengthing explanation http://www.futurile.net/2016/06/15/xterm-256color-themes-molokai-terminal-theme/

Coupled with https://wiki.archlinux.org/index.php/Xterm


UEFI Boot
=========
Lots has been previously led, so much so that the information should be rested and sorted. Afterall, it's hard to keep a journal in a sectioned document. Arch was initially installed using if `/sys/firmware/efi` exists, then the kernel has booted in EFI mode.


PDF Viewing
===========

#. Zathura
#. Evince
#. 

Printing
========

Adding print queue to CUPS:
Device URI: hp:/usb/hp_LaserJet_1320_series?serial=00CNHC62Q0H1
Queue name: HP_LASERJET_1320
PPD file: lsb/usr/HP/hp-laserjet_1320-ps.ppd.gz
Location: office
Information: 

Reading Later
=============

#. https://kyechou.github.io/blog/2017/01/26/my-i3-wm-on-archlinux/
#. 
