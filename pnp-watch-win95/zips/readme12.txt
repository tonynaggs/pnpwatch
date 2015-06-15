Original announcement in demon.announce
Text recovered from https://groups.google.com/forum/#!topic/demon.archives.announce/KeK9x-fYHZU


Date: Sun, 22 Nov 1998 22:07:25 +0000
From: Anthony Naggs <a...@ubik.demon.co.uk>
 
File Name:      pnpwat12.zip
File Size:      27979 bytes
Platform:       Windows 95/NT
 
Short: Free utility (with source code) to watch Windows (95 / NT)
       Plug'n'Play events, and report details of such things as PCMCIA
       cards or CD-ROMs being inserted or removed.
 
Suggested Location:
ftp://ftp.demon.co.uk/pub/ibmpc/win95/programming/pnpwat12.zip

Description:
This is a small Windows 95/98/NT utility, that reports Plug'n'Play
events as they happen, e.g.: CD-ROMs being inserted or removed; PCMCIA
cards being inserted or removed; or the Ethernet, modem etc... functions
on the card being configured.

It also has value as example code for doing useful things with Windows
Plug'n'Play events.  See the readme.txt and watch.c files within the
archive for guidance on copyright and how the program may be used and
distributed.

Version 1.2 is an update that has checks added to help debug other
applications that broadcast PnP events.  E.g. checking the pointer in
the broadcast is non-NULL, and the data structures are sufficiently
large.


Best regards,
Anthony Naggs