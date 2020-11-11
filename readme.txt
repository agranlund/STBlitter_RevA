----------------------------------------------------------------

A simple Blitter + IDE + 256k ROM decoding board for Atari ST(M)
(c) 2019 Anders Granlund
agranlund@gmail.com

www.happydaze.se

----------------------------------------------------------------

IDE and ROM functionality is based on the work of P.Putnik:
http://atari.8bitchip.info/astide.php

----------------------------------------------------------------

This hardware is provided 'as-is', without any express
or implied warranty. In no event will the authors be held
liable for any damages arising from the use of this hardware.

----------------------------------------------------------------


Notes:

* There are no dependencies between Blitter and ROM+IDE functionality so you can easily
  make a Blitter-only board by leaving out the other parts, or vice versa.

* Bridge 2-3 on both JP1 & JP2 when blitter chip is fitted

* Bridge 1-2 on both JP1 & JP2 when no blitter chip is fitted

* Solder-jumpers SJ1 & SJ2 must always be bridged.
  These are located directly below jumpers JP1 & JP2.

* Resistor R8 is optional and should normally NOT be used.
  If it is used, you must remove Atari ST motherboard resistor R4.
  Atari resistor R4 is a pullup for blitter interrupt (INT3).
  On the ST, this is a 4K7 resistor but on STFM/Mega/STE it has a value of 1K.
  My STBlitter works fine without having to change anything on the ST motherboard,
  but if yours is not then you might want to try changing Atari resistor R4 to a 1K resistor instead.
  Alternatively, just remove it from the Atari and fit a 1K resistor as R8 on STBlitter board.

* EmuTos 0.9.11 will take a very long time to boot if there is no IDE drive is connected.
  This is not a problem on Atari TOS. Nor is it a problem on EmuTos if you have a drive connected.

* The IDE bus is NOT buffered so if you must use cables you need to keep them very short.
  I am using a CF adapter directly plugged into the board without cables and this works fast and reliable.

* The main metal shielding in your ST will probably not fit unless you cut some chunks out of it.
  Perhaps it might fit if you solder the STBlitter directly to the ST motherboard instead of using a socket,
  but I have not tried that - I just run my ST without the metal shielding.

* You will have clearance issues if you are using the 4MB RAM upgrade from Exxos (the one that clips onto the MMU)
  You can solve this by either cutting a small chunk out of the 4MB Ram PCB using a dremel.
  (Make sure to check the underside so you know where the nearest traces are and how much you can cut!)
  Or just stack 2 CPU sockets on the ST motherboard to gain some height and clearance that way.

* The PCB is made from 4 layers according to design rules of Elecrow PCB service (www.elecrow.com)
  The exported Gerbers were done so according to how Elecrow want them so they may or may not be directly
  acceptable to other PCB manufacturers.




Installation:
---------------------------------------------------------------

- Desolder CPU from Atari motherboard and fit a 0.9" DIP4 socket instead.
  CPU is instead installed on the STBlitter board

- IDE: Wire from STBlitter-ACSI10 to pin 10 of ACSI interface (J2) on Atari ST motherboard

- Blitter: Wire from STBlitter-BIRQ to pin 25 of 68901 (U11) on Atari ST motherboard (or the correct side of
  the nearby resistor R4 if you rather solder to that)

- ROM decoding:
  Wiring for 256k ROM decoding will depend on how you modded your ST to fit 256k 2chip roms.
  Of course, you can ignore this step if you don't need ROM decoding or if you already have it from another expansion board.

- Wire from STBlitter-RCEI to ROM2 or CE from Glue.

- Wire from STBlitter-RCEO to CE for the ROMS.

In My case:
RCEI to pin 8 of U68 on Atari motherboard. The pin on U68 was cut so that it no longer connects to the Atari motherboard.
RCEO to the CE-1M solder jumper on Atari motherboard

Installation might be harder or easier if you are using a 3rd party 2chip TOS upgrade board.
Check with the supplier of your board where you can hijack and replace the CE signal.

I do not have one so I couldn't tell for sure, but it looks like the STFM Dual-Tos board from Exxos would make
installation very easy as it appear to have a pin for an external CE signal.



Parts:
---------------------------------------------------------------

-- Common --
C3, C4        : 0.1uF
CPU Socket    : 2x 0.9" DIP64, digikey: ED90061-ND
ST connector  : 2x 32pin lathed male headers for connecting to socket on ST motherboard
                I use these, but you can of course find them on Digikey or elsewhere:
                https://www.electrokit.com/en/product/male-header-1x40p-lathed-breakable/
Pin headers   : Standard 2.52mm male headers for JP1, JP2 and (ACSI,RCEO,RECI,BIRQ) connector.
                (or ignore this and just solder instead)
Pin Jumper    : 2p jumper for JP1 & JP2 each    
                (or ignore this and just solder instead)

-- Blitter --

R8            : - DO NOT USE -
R1, R2        : 4K7
C1, C2        : 0.1uF
Socket        : PLCC68, digikey: AE10059-ND
Blitter       : You need to find a blitter from somewhere, for example from Exxos:
                https://www.exxoshost.co.uk/atari/last/storenew/

-- IDE & ROM decoding --

74LS03V       : DIP14 package
ATF16V8       : DIP20 package
C5, C6        : 0.1uF
R3            : 4K7
R4, R7        : 2K2
R5            : R390
R6            : R100
LED           : 3mm size
Header        : 2x44-2mm, Digikey: WM18853-ND





CUPL logic for ATF16V8:
----------------------------------------------

Name     STBlitter ;
PartNo   00 ;
Date     2019-09-03 ;
Revision 01 ;
Designer Anders Granlund ;
Company  HappyDaze ;
Assembly None ;
Location  ;
Device   g16v8 ;

/*
 A23     I [ 1     20 ] VCC 
 A22     I [ 2     19 ] T*    RCEI
 A21     I [ 3     18 ] T     RCEO
 A20     I [ 4     17 ] T     DTACK
 A19     I [ 5     16 ] T     IDE_WR
 A18     I [ 6     15 ] T     IDE_RD
 A17     I [ 7     14 ] T     IDE_CS1
 A16     I [ 8     13 ] T     IDE_CS0
 A5      I [ 9     12 ] T*    RW
       GND [ 10    11 ] I     AS
*/


/************ Inputs ************/

pin 1 = A23;
pin 2 = A22;
pin 3 = A21;
pin 4 = A20;
pin 5 = A19;
pin 6 = A18;
pin 7 = A17;
pin 8 = A16;
pin 9 = A5;
pin 11 = AS;
pin 12 = RW;
pin 19 = RCEI; 

/************ Outputs ************/

pin 13 = !IDE_CS0;
pin 14 = !IDE_CS1;
pin 15 = !IDE_RD;
pin 16 = !IDE_WR;
pin 17 = DTACK;
pin 18 = !RCEO;

/************ Logic ************/

DTACK = (!AS & A23 & A22 & A21 & !A20 & !A19 & !A18 & RW) # (A23 & A22 & A21 & A20 & !A19 & !A18 & !A17 & !A16);

RCEO = (!AS & A23 & A22 & A21 & !A20 & !A19 & !A18 & RW) # !RCEI;

IDE_CS0 = (A23 & A22 & A21 & A20 & !A19 & !A18 & !A17 & !A16 & !A5);
IDE_CS1 = (A23 & A22 & A21 & A20 & !A19 & !A18 & !A17 & !A16 & A5);

IDE_RD = (!AS & A23 & A22 & A21 & A20 & !A19 & !A18 & !A17 & !A16 & RW);
IDE_WR = (!AS & A23 & A22 & A21 & A20 & !A19 & !A18 & !A17 & !A16 & !RW);


