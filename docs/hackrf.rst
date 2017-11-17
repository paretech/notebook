******************************************
Starting from Scratch with the HackRF One!
******************************************

Introduction
============
I was one of the original backers of the HackRF One radio. My order was fulfilled but for a variety of life reasons, I never really got into it.

I recall long ago that I did update the firmware and that I was able to tune a local FM radio station. A couple years later and that sounds like a decent place to start up again.

The following sections outline (perhaps detail) the process targeted for an Arch Linux Environment.

As of 2017-11-02, the latest HackRF "release" was 2017-02-01 and the last commit was on 2017-09-29. Are these facts signs of a mature platform or a loss of interest? Signal processing is cool and all...

Installation:

.. code-block:: sh

	$ pacman -S hackrf
	$ hackrf_info

	hackrf_info version: 2017.02.1
	libhackrf version: 2017.02.1 (0.5)
	Found HackRF
	Index: 0
	Serial number: 0000000000000000457863c825391b1f
	Board ID Number: 2 (HackRF One)
	Firmware Version: 2015.07.2 (API:1.00)
	Part ID Number: 0xa000cb3c 0x007d4f44

Follow instructions for updating HackRF Firmware [#hackrf_upfirmware]_. When download latest release, us `tar -xJf <if>` to extract if *.tar.xz. All lights with exception of RX and TX are on and steady.''


.. code-block:: sh

	$ hackrf_info

	hackrf_info version: 2017.02.1
	libhackrf version: 2017.02.1 (0.5)
	Found HackRF
	Index: 0
	Serial number: 0000000000000000457863c825391b1f
	Board ID Number: 2 (HackRF One)
	Firmware Version: 2017.02.1 (API:1.02)
	Part ID Number: 0xa000cb3c 0x007d4f44

Install some more software...''

.. code-block:: sh

	pacman -S gnuradio gnuradio-osmosdr
	pacman -S gnuradio-companion


Trying to run the example FM Radio Receiver [hackrf_gettingstarted]_. The resulting window is displaying samples and the HackRF appears to be operating in RF mode. These are both good and healthy signs. Even better, when using an appropriate antenna and tuning to a strong station even get decent sounding audio. Note if sound is not heard there is a fair bit of information on the subject [gnur_ALSAPulseAudio]_. 


.. [#hackrf_upfirmware] https://github.com/mossmann/hackrf/wiki/Updating-Firmware
.. [#hackrf_gettingstarted] https://github.com/mossmann/hackrf/wiki/Getting-Started-with-HackRF-and-GNU-Radio
.. [#gnur_ALSAPulseAudio] https://wiki.gnuradio.org/index.php/ALSAPulseAudio


Resources
=========

1. https://github.com/mossmann/hackrf/wiki/Getting-Started-with-HackRF-and-GNU-Radio
2. https://wiki.archlinux.org/index.php/GNU_Radio
3. https://github.com/mossmann/hackrf
4. https://github.com/mossmann/hackrf/wiki/Operating-System-Tips
5. http://hackrfblue.com/getting-started/


Additional Reading
==================

1. http://greatscottgadgets.com/sdr/
2. https://github.com/mossmann/hackrf/wiki/Software-with-HackRF-Support
3. https://github.com/jopohl/urh
4. http://zadig.akeo.ie/
5. https://grymoire.wordpress.com
6. http://www.briandorey.com
7. http://www.briandorey.com
8. https://www.mouser.com/Search/ProductDetail.aspx?qs=y9%2FpVxnqej94SfQhw%252bqKcA%3D%3D
9. http://dangerousprototypes.com/blog/tag/hackrf/
10. https://z4ziggy.wordpress.com/2015/05/17/sniffing-gsm-traffic-with-hackrf/
11. https://github.com/scateu/kalibrate-hackrf

