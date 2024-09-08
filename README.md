# MFUltra-chameleon-fw
Firmware for ChameleonMini RevE that works to emulate a Mifare Ultralight EV1

## Why?
The Iceman ChameleonMini RevE repository (https://github.com/iceman1001/ChameleonMini-rebooted) is essentially abandoned, the actual code has some bugs: MF_DETECTION does not work properly and the Mifare Ultralight EV1 164B emulation has one-way counters that do not work.  
The only firmware that works was the one found on the lab401 website (https://lab401.com/en-it/blogs/academy/chameleon-mini-reve-rebooted-flashing-to-latest-firmware), but it still has the one-way counters implementation that partially works.    
I am only interested in the Mifare Ultralight EV1 emulation so i fixed it.

What i did was:
1. Upload the lab401 code to the ChameleonMini. In the GUI, i found that the commit ID was 7420671
2. Go to the Iceman repo commit page (https://github.com/iceman1001/ChameleonMini-rebooted/commits/master/) and search for the one with the same ID
3. Go to that commit page (https://github.com/iceman1001/ChameleonMini-rebooted/tree/74206715e9415dd6370432d1845090bfa692d6b4) and download the ZIP file
4. Change the Firmware/ChameleonMini/Application/MifareUltralight.c lines 388-389 with
```C
uint8_t CounterId = Buffer[1];
uint32_t Addend = ((uint32_t)Buffer[2]) | ((uint32_t)Buffer[3] << 8) | ((uint32_t)Buffer[4] << 16);
```
5. Use linux to compile the firmare with the instructions present here https://github.com/iceman1001/ChameleonMini-rebooted/wiki/Compiling-Linux-(Unix)
6. Upload the firmware on the ChameleonMini by following the instructions present here https://github.com/iceman1001/ChameleonMini-rebooted/wiki/Getting-started

IInstead of doing all this, i put the .eep and .hex files to upload in this repository, so you can avoid all these steps.

## Dump
You need to upload a dump of a Mifare Ultralight EV1 created with Proxmark3 (in that dump you will find the signature, version and counter values).  
TODO: Is it possible to obtain the same dump with libnfc, try to create a script for that.
