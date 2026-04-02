# DIEGO 500 (WIP)

Turn your A500 into an A2000 with this sidecar backplane

![PCB](/IMAGES/5-SLOT-3D.png)


## What's this?  

DIEGO 500 gives you an A2000 compatible CO-PROCESSOR slot along with a full set of 5 ZORRO2 slots all madepossible by the onboard Bluster IC.  Bluster is a remake of the classic A2000 Buster by LIV2 although it is a custom version for DIEGO to handle inverted clocks and provide some further functions.  

DIEGO 500 has been tested extensively with various CPU and Zorro cards, a list of tested cards is provided below.  If you have anything to add to this list, please get in touch.  

DIEGO 500 is also intended to power your A500 via the passthrough 4 pin connector adjacent to the main 24 pin ATX connector.  


## Ordering

There are currently 2 versions of DIEGO 500, a full 5 slot version and a smaller 3 slot version.  

Gerber, BOM and CPL files are provided for both in the JLCPCB folder.  

All passives on each version are located on the bottom side.  All logic and electrolytic caps are located on the top side.  

Both versions of DIEGO 500 are 4 layer boards.  HASL finish is fine for this project, inner layers can be left 0.5oz.  

All signals are routed on the top and bottom layers but there are some power traces on each internal layer along with solid grounds.  

Due to the amount of passives I recommend having your PCB manufacturer fit the bottom side, then you as builder complete the top side.  

While all components are available at JLC several top side parts are getting expensive!  



## Assembly

Assuming you got your boards made with the bottom side completed you may fit out the top side to suit yourself.  I recommend fitting and programming the CPLD first and finish all SMD work before doing the slots as they'll just get in the way!  

NOTE: U11 is only required if you're using something in the COPROCESSOR slot that needs a 28mhz clock.  There doesn't seem to be that many (if any) cards that do require this so you may leave this off if you prefer. 

FURTHER NOTE: You MUST use LS and ALS logic as per the BOM.  DIEGO 500 was originally built with HCT logic but this caused issues with several boards.  

For the 86pin slot to the A500 you will need to bend the pins in to meet the pads on DIEGO 500.  

For programming the CPLD, the required JED file is available in the CPLD folder or you may get it from LIV2s GitHub.  I use a Raspberry PI to programme CPLDs as per LIV2s guide but use whatever method you are comfortable with.  Just remember if using a PI and if powering 3.3v from the PI, disconnect all other power supplies from DIEGO 500!

Speaking of power, DIEGO 500 is intended to also power your Amiga but the voltage rails are not connected at the side car slot. Rather you must build and connect the passthrough cable to the 4 pin connector beside the main 24 pin ATX connector.  Voltage rails are labelled at this passthrough.  Pay attention to your pinout, don't blame me if you blow up your A500.



## Using your DIEGO 500

For the most part DIEGO 500 is simply plug and play but there are a few things to keep in mind.
- Firstly there is a single jumper to configure.  At the top of the board beside the COPROCESSOR slot you need to select which 7mhz clock to use.  If you are using a Rev3 or 5 A500 you must select CPLD clock.  If you are using a Rev6 A500 you can use either but if using A500 clock you need to close JP6 on the A500s motherboard.  If you are using an A500+ you can pick either.  The CPLD clock is derived from _CCK XNOR _CCQK within the CPLD.    
- There is one more pin header adjacent to the above labelled CFGIN.  If you have any expansions in your A500 on the autoconfig chain you MUST connect said expansions config out to this config in.  If you don't you'll most likely get a yellow screen or just no zorro cards detected!
- Insert all cards as if this was a real A2000 i.e. the front of DIEGO is the front of an A2000.  Most modern Zorro expansions (especially half length cards) will have an arrow pointing to front or back.  Pay attention to this! If you plug something in wrong expect a dead card and possibly dead Amiga! 
- Zorro 2 cards will autoconfigure themselves just like they would in any Amiga.  To confirm this hold both mouse buttons on start up to access the early start-up menu and select "expansion board diagnostic".  
- If you're using a true A2000 CPU card in the COPROCESSOR slot (like the N2630) simply insert the card and Bluster will tristate the internal A500 CPU. (this is achieved by Bluster monitoring _BOSS and when asserted, Bluster asserts _BR to the internal CPU)  
- If you are using an A500 type accelerator in the COPROCESSOR slot such as TF536 or a PISTORM (including PISTORM 2000), you MUST open the A500 and remove the internal CPU.  Even if you manually assert _BOSS, the E clocks will clash and it won't work.  There is NO fix for this!  A2000 accelerators work as they monitor E and sync to it or generate it as necessary.  



## Testing and Results

With thanks to GADGETUK, SPARX, CATHERS and ANDI@HBR we've been able to test and confirm that most boards 
work fine in DIEGO 500.  The below table lists those boards tested and confirmed as working or not.


| Card Type | Name | Status |
|---|---|---|
| SCSI | Commodore A2090A | Working but very fussy.  Only bluescsi V2 and only in raw mode. |
| SCSI | Commodore A2091 | Working |
| SCSI | GVP Series II HC+8 | Working |
| IDE |	Ripple IDE | Working |
| IDE |	AT-Bus clone | Working |
| IDE |	Buddha | Working |
| RAM |	Amigakit ZORRAM | Working |
| RAM |	Gottagofastram 2000 | Working |
| Sound | Prelude | Working |
| RTG |	GBAPII | Working |
| Multi-feature | LAN-IDE-Clock | Working/Working/Not tested |
| CPU |	A2630 | Working |
| CPU |	N2630 | Working |
| CPU |	TF520 | Working |
| CPU |	TF536 | Working |
| CPU |	TK2 | Working |
| CPU |	Pistorm | Working |
| CPU |	Pistorm 2k | Working |


If you can add to this table please get in touch!



## Case

Still a WIP and I'm very much open to suggestions!



## FAQ

Several people have asked about adding ISA slots, a video slot or making this remote with a ribbon cable to the A500.  

ISA slots - DIEGO 500 has been designed so that you could in theory add ISA slots on a separate board, the Zorro slots 
in relation to the edge of the PCB are positioned to allow this.  I have not yet designed an ISA board but it's on the list.
Video slot - There are no plans to add a video slot.  Very few of the required signals are on the A500s side car so it would need a multicore ribbon from DENISE.  
Ribbon Cable - If you want to try a ribbon cable you can simply solder one onto the edge connector at the side of DIEGO but I don't expect this to work without further buffering! (like in the bodega bay).



## License

This project represents countless hours of work by not just me but LIV2 and everyone involved in testing.  
It is however released under a CERN Open Hardware Licence v2 for the community to enjoy.  

While I cannot put any direct stipulations into this I would ask that it should only be built and sold at cost plus time and if you plan on forking or using this as the basis for any other works, all attributions as presented on DIEGO 500 should be maintained and carried through.


## Credits

PCB by CRG(Glen)

Bluster by LIV2

Artwork by Brick Studios

Testing by GadgetUK, Sparx, Cathers and Andi@HBR


Why is it called Diego 500?  Well... this project started solely as a Zorro 2 expansion and to implement that I first had
to understand how the Zorro bus worked.  You might say I had to unmask Zorro.  The character of Zorro unmasked is called Don Diego so Diego 500. 










