
# DURA_MIDI
## for MOTU classic Macintosh serial RS422 MIDI interfaces, to keep them going. 

DURA - Dead Unicorn Resurrection Adapter. <br><br>
Bring all those dead MOTUs with 9-pin mini din connectors back to life! All (most?) of those interfaces speaking the same MOTU serial MIDI dialect, even Opcodes.<br>

Hardware, class compliant multi-cable (x8) USB MIDI adapter for large family of old MOTU MIDI interfaces that become obsolete with elimination of serial mini-din ports on old MACs. Opcodes will come alive with MOTUs as well! (Because Opcodes have MOTU emulation.)
<br><img width="400" alt="DURA MIDI" src="https://github.com/user-attachments/assets/67851090-fab5-4048-a1de-60eea7536f9c" />

<br>First draft firmware of mostly fully functional adapter is available (see release).<br>
You need to make some hardware. Ideally - proper RS422 interface with phy chip. But, I initially was running it with just a bunch of resistors to bring signals to recognizable levels. See Poor Man's schematic. It can be cheap alternatives for those who has limited budget. It can be made for under $5.<br><br>

## First release firmware
<br>
Appeared to be fully functional. Some notes:<br>
1. It runs at "fast", aka "x4" interface speed, that is 125KHz. Make sure you set your interface to "fast" and not to 1MHz. I am still trying to get 1MHz going, but it has complex handshake that I did not figured out yet. Any help is appreciated. "Fast" mode does not need handshake, it just works.<br>
2. Some interfaces, like original/classic MIDI Express do not have "fast" mode. If you have one of those - you may only use "Legacy" firmware for transparent one cable (MIDI Port) setup. <br>
3. Firmware fully support and applies "Running Status" to all outgoing messages. It will properly apply and strip "Running Status" as necessary when translating between serial and USB.<br>
4. It will strip "Active Sensing" messages when translating from serial to USB.<br>
5. I chose pin-out for crossover cables. Most cables nowadays sold on Amazon are straight pin-for-pin. To be sure it is crossover - make your own. PoorMan's schematic is for plug, so it is straight pin-out.<br>
6. It will pass SysEx to each port as intended. I do not know if there is any practical limit to the length of SysEx. Most interfaces do choke at some point. Would be interesting to test and see. It may not have a limit at all as it handles MIDI as a stream. Presently I do not parse SysEx messages intended for the interface itself. It will be needed to implement ClockWork like functionality. Sometime in the future. <br>
7. For highly technical crowd: It appeared that all interfaces I have to try are able to receive asynchronous communications. That 1MHz synchronous clock apparently for the Mac and not for the interfaces. So far I see no problem (no communication errors) running 100% asynchronous UART. Let's keep an eye on it.<br>

<br>
The driver: It may prompt for MOTU driver. Please manually select generic "Composite Device".
<br><br>
Enjoy it and let me know how it works for you.

______________________________________________

### V3: 
- Added heartbeat with port routing reset: if no MIDI message sent for over 4 seconds - cable routing will reset to default, that is any message without cable number header will be delivered to all ports. This behavior is observed on all MOTU devices from that era.
- To be able to converse with attached MOTU or OpCode, cable 8 now has a SysEx filter that will pass SysEx to the interface without the cable ID header. And for following 2 seconds it will return any response unaltered. After that it will be forced to reset to default state with heartbeat (above). This also mean that MIDI device connected to port 8 will not be able to receive SysEx, it will be stripped. The rest of MIDI traffic on port 8 is unaffected. In the future maybe able to make it selective for MOTU and OpCode only. For now, any SysEx dependent device must use ports 1 through 7.

______________________________________________
### Some testing:
- All testing is done in "fast" async mode, that is 125KHz. I am not able to see 1MHz rate under any conditions. I have (re)build MAC system 7.6.1 and tap conversation of two networked MTP_AVs that are on USB, nowhere I can see handshake and 1MHz communication. 125KHz is the max I see. I can see 1M clock, but data is always at fraction of it. What am I missing? Was 1MHz data rate on MAC's serial ever real?
- Originally implemented 2-layer "running status" works great on original MTP, but not so well on OpCode and MTP_AV. Lower layer is a MIDI standard defined one, and upper layer is a cable number header that works in very similar manner. That old MTP is able to track those two layers without any glitches. It significantly reduces serial traffic under stress and improves latency. BUT, later MOTUs and OpCodes appeared to flatten "running status" and treat any change of either layer as running status reset. There is also an aggressive timeout of only ~60mSec that also resets running status by requiring full headers for all messages. V3 firmware not yet has settings to disable running status for newer (20 years old instead of 30) devices. This is to do.
- 125KHz is x4 MIDI speed. With typical MIDI data, realistically this will suffice for 5-6 saturated MIDI channels. But for most use cases most folks probably will notice nothing.
- Actual latency from USB to MIDI for single messages is mostly below 2mSec, negligible for most use cases and inline with best interfaces.
- While I do have some MTP_AV and OpCode, presently most of the testing is done with original MTP.
__________________________________
### Serial Cables
The most sure way to have a crossover / AppleTalk cable is to make one by yourself. But! Most of cheap minidin-8 connectors available are the same kind and the problem is that plastic shroud is covering too much of the engaging part. Most MOTUs constructed in such way that those connectors are barely able to reach jack's contacts, making interface unreliable. I just cut about 2mm of plastic shroud, that in turn remove latches that holds connectors together. That is remedied with couple drops of cyanoacrylate glue. 
For async communications at 125KHz only two signal pairs and ground are used. So 5-wire cable will suffice. Supposedly for 1MHz need a handshake and that is two extra pins. And to complete there is another supposedly unused pin. So you may as well go all the way with 8-wire crossover cable.
Absolute majority of the cables on Amazon and elsewhere are straight pin-for-pin cables. 


